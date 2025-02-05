---
title: Orchestrations externes dans Fonctions durables - Azure
description: Découvrez comment implémenter des orchestrations externes à l’aide de l’extension Fonctions durables pour Azure Functions.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 99eabf3bc91887ff19b3a0bc9cf6647d32fa6750
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787562"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Orchestrations externes dans Fonctions durables (Azure Functions)

Les *orchestrations externes* sont des fonctions d’orchestrateur qui ne se terminent jamais. Elles sont utiles si vous souhaitez utiliser [Fonctions Durable](durable-functions-overview.md) pour des agrégateurs et tout scénario qui nécessite une boucle infinie.

## <a name="orchestration-history"></a>Historique de l’orchestration

Comme expliqué dans la section [Création de points de contrôle et réexécution](durable-functions-checkpointing-and-replay.md), l’infrastructure des tâches durables effectue le suivi de l’historique de chaque orchestration de fonction. Cet historique augmente sans cesse tant que la fonction d’orchestrateur continue de planifier une nouvelle tâche. Si la fonction d’orchestrateur entre dans une boucle infinie et planifie sans cesse des tâches, cet historique risque de devenir très volumineux et de provoquer d’importants problèmes de performances. Le concept *d’orchestration externe* a été conçu pour limiter ces types de problèmes dans les applications nécessitant des boucles infinies.

## <a name="resetting-and-restarting"></a>Réinitialisation et redémarrage

Au lieu d’utiliser des boucles infinies, les fonctions d’orchestrateur réinitialisent leur état en appelant la méthode [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_). Cette méthode n’utilise qu’un seul paramètre JSON sérialisable, qui devient la nouvelle entrée de la prochaine génération de la fonction d’orchestrateur.

Lorsque la méthode `ContinueAsNew` est appelée, l’instance garde un message dans sa propre file d’attente avant de se fermer. Le message redémarre l’instance avec la nouvelle valeur d’entrée. Le même ID d’instance est conservé, mais l’historique de la fonction d’orchestrateur est tronqué.

> [!NOTE]
> L’infrastructure des tâches durables conserve le même ID d’instance, mais crée en interne un nouvel *ID d’exécution* pour la fonction d’orchestrateur réinitialisée par `ContinueAsNew`. Cet ID d’exécution n’est généralement pas exposé en externe, mais il peut être utile d’en tenir compte lors du débogage de l’exécution d’orchestration.

## <a name="periodic-work-example"></a>Exemple de travail périodique

Voici un cas d’utilisation des orchestrations externes : un code a besoin d’effectuer un travail périodique indéfiniment.

### <a name="c"></a>C#

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (Functions 2.x uniquement)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("DoCleanup");

    // sleep for one hour between cleanups
    const nextCleanup = moment.utc(context.df.currentUtcDateTime).add(1, "h");
    yield context.df.createTimer(nextCleanup.toDate());

    context.df.continueAsNew(undefined);
});
```

La différence entre cet exemple et une fonction déclenchée par un minuteur est que les heures de déclenchement du nettoyage ne sont pas ici basées sur une planification. Par exemple, une planification CRON qui exécute une fonction toutes les heures se produira à 1 h 00, 2 h 00, 3 h 00 etc., et risque d’entraîner des problèmes de chevauchement. Mais dans cet exemple, si le nettoyage prend 30 minutes, il sera alors planifié à 1 h 00, 2 h 30, 4 h 00, etc., et il n’existe aucun risque de chevauchement.

## <a name="exit-from-an-eternal-orchestration"></a>Fermeture d’une orchestration externe

Si une fonction d’orchestrateur doit se terminer, il vous suffit de ne *pas* appeler `ContinueAsNew` et de laisser la fonction se terminer.

Si une fonction d’orchestrateur entre dans une boucle infinie et doit être arrêtée, utilisez dans ce cas la méthode [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_). Pour plus d’informations, consultez [Gestion d’instance](durable-functions-instance-management.md).

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Apprendre à implémenter des orchestrations de singleton](durable-functions-singletons.md)
