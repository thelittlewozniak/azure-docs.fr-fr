---
title: Singletons pour l’extension Fonctions durables - Azure
description: Découvrez comment utiliser des singletons dans l’extension Durable Functions pour Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: c032ba046668310ff71d067d22a805fc6446667c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683803"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Orchestrateurs de singleton dans l’extension Fonctions durables (Azure Functions)

Pour les travaux en arrière-plan, vous avez souvent besoin de vérifier qu’une seule instance d’un orchestrateur spécifique est exécutée à la fois. Vous pouvez le vérifier grâce à l’extension [Fonctions durables](durable-functions-overview.md), en affectant un ID d’instance spécifique à un orchestrateur lors de sa création.

## <a name="singleton-example"></a>Exemple de singleton

Les exemples C# et JavaScript suivants illustrent une fonction de déclencheur HTTP qui crée une orchestration singleton de travail en arrière-plan. Le code garantit que seule une instance existe pour un ID d’instance spécifié.

### <a name="c"></a>C#

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (Functions 2.x uniquement)

Voici le fichier function.json :
```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}/{instanceId}",
      "methods": ["post"]
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

Voici le code JavaScript :
```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionName;

    // Check if an instance with the specified ID already exists.
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance) {
        // An instance with the specified ID doesn't exist, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

Par défaut, les ID d’instance sont des identificateurs globaux uniques générés de manière aléatoire. Dans ce cas, cependant, l’ID d’instance est passé dans les données d’itinéraire à partir de l’URL. Le code appelle [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) (C#) ou `getStatus` (JavaScript) pour vérifier si une instance ayant l’ID spécifié est déjà en cours d’exécution. Si ce n’est pas le cas, une instance est créée avec cet ID.

> [!WARNING]
> Quand vous développez localement dans JavaScript, vous devez définir la variable d’environnement `WEBSITE_HOSTNAME` sur `localhost:<port>`, par exemple `localhost:7071` pour utiliser les méthodes sur `DurableOrchestrationClient`. Pour plus d’informations sur cette configuration, consultez le [problème GitHub](https://github.com/Azure/azure-functions-durable-js/issues/28).

> [!NOTE]
> Il existe une condition de concurrence potentielle dans cet exemple. Si deux instances de **HttpStartSingle** s’exécutent simultanément, les deux appels de fonction signalent la réussite de l’opération, mais en réalité une seule instance d’orchestration démarre. Selon vos besoins, cela peut avoir des effets secondaires indésirables. Pour cette raison, il est important de s’assurer l’impossibilité que deux demandes puissent exécuter cette fonction de déclencheur simultanément.

Les détails liés à l’implémentation de la fonction d’orchestrateur ne sont pas importants. Il peut s’agir d’une fonction d’orchestrateur classique, qui démarre et se termine, ou d’une fonction qui s’exécute sans s’arrêter (une [orchestration externe](durable-functions-eternal-orchestrations.md)). Ce qui importe, c’est qu’une seule instance s’exécute à la fois, systématiquement.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [En savoir plus sur l’appel d’orchestrations secondaires](durable-functions-sub-orchestrations.md)
