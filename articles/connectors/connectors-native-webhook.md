---
title: Créer des actions ou des workflows basés sur les événements – Azure Logic Apps | Microsoft Docs
description: Automatiser des actions ou des workflows basés sur les événements avec des Webhooks et Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.topic: article
tags: connectors
ms.date: 07/21/2016
ms.openlocfilehash: c3047000843e054e71ec1a80313118a25e7c4905
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60447195"
---
# <a name="create-event-based-workflows-or-actions-by-using-webhooks-and-azure-logic-apps"></a>Créer des actions ou des workflows basés sur les événements avec des Webhooks et Azure Logic Apps

Avec le déclencheur et l’action webhook, vous pouvez déclencher, suspendre et reprendre les flux afin d’effectuer les tâches suivantes :

* Créer un déclenchement à partir d’[Azure Event Hub](https://github.com/logicappsio/EventHubAPI) lorsqu’un élément est reçu
* Attendre une approbation avant de poursuivre un flux de travail.

En savoir plus sur la [création d’API personnalisées qui prennent en charge un webhook](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Utilisation du déclencheur webhook

Un [*déclencheur*](../connectors/apis-list.md) est un événement qui démarre un flux de travail d’application logique. Le déclencheur de webhook est basé sur un événement et ne repose pas sur l’interrogation de nouveaux éléments. Lorsque vous enregistrez votre application logique avec un déclencheur de webhook, ou lorsque vous modifiez votre application logique en la faisant passer du statut Désactivée au statut Activée, le déclencheur de webhook *s’abonne* au service ou point de terminaison spécifié en inscrivant une *URL de rappel* avec ce service ou point de terminaison. Le déclencheur utilise ensuite cette URL pour exécuter l’application logique en fonction des besoins. Comme le [déclencheur de requête](connectors-native-reqres.md), l’application logique se déclenche immédiatement lorsque l’événement attendu se produit. Le déclencheur *annule l’abonnement* si vous supprimez le déclencheur et que vous enregistrez votre application logique, ou lorsque vous modifiez votre application logique en la faisant passer du statut Activée au statut Désactivée.

Voici un exemple de configuration d’un déclencheur HTTP dans le concepteur d’application logique. Ces étapes supposent que vous avez déjà déployé ou que vous accédez à une API qui suit [le modèle d’abonnement et de résiliation d’abonnement au webhook dans les applications logiques](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). 

**Pour ajouter le déclencheur webhook**

1. Ajoutez le déclencheur **HTTP Webhook** en tant que première étape dans une application logique.
2. Renseignez les paramètres pour les appels d’abonnement et de résiliation d’abonnement au webhook.

   Cette étape suit le même modèle que le format de [l’action HTTP](connectors-native-http.md).

     ![Déclencheur HTTP](./media/connectors-native-webhook/using-trigger.png)

3. Ajoutez au moins une action.
4. Cliquez sur **Enregistrer** pour publier l’application logique. Cela permet d’appeler le point de terminaison d’abonnement avec l’URL de rappel nécessaire pour déclencher cette application logique.
5. À chaque fois que le service effectue un `HTTP POST` à l’URL de rappel, l’application logique se déclenche et inclut toutes les données transmises dans la requête.

## <a name="use-the-webhook-action"></a>Utilisation de l’action webhook

Une [*action*](../connectors/apis-list.md) est une opération qui est définie et exécutée par le workflow de votre application logique. Quand une application logique exécute une action de webhook, cette action *s’abonne* au service ou point de terminaison spécifié en inscrivant une *URL de rappel* à ce service ou point de terminaison. Ensuite, l’action de webhook attend que ce service appelle l’URL avant que l’application logique reprenne son exécution. L’application logique annule l’abonnement au service ou point de terminaison dans les cas suivants : 

* Lorsque l’action de webhook s’est terminée avec succès.
* Si l’exécution de l’application logique est annulée en attendant une réponse.
* Avant que la logique d’application arrive à expiration.

Par exemple, l’action [**Envoyer un e-mail d’approbation**](connectors-create-api-office365-outlook.md) est un exemple d’action de webhook qui suit ce modèle. Vous pouvez étendre ce modèle à n’importe quel service à l’aide de l’action webhook. 

Voici un exemple de configuration d’une action webhook dans le concepteur d’application logique. Ces étapes supposent que vous avez déjà déployé ou que vous accédez à une API qui suit [le modèle d’abonnement et de résiliation d’abonnement au webhook utilisé dans les applications logiques](../logic-apps/logic-apps-create-api-app.md#webhook-actions). 

**Pour ajouter une action webhook**

1. Choisissez **Nouvelle étape** > **Ajouter une action**.

2. Dans la zone de recherche, tapez « webhook » pour rechercher l’action **HTTP Webhook**.

    ![Sélectionner l’action de requête](./media/connectors-native-webhook/using-action-1.png)

3. Renseignez les paramètres pour les appels d’abonnement et de résiliation d’abonnement au webhook

   Cette étape suit le même modèle que le format de [l’action HTTP](connectors-native-http.md).

     ![Terminer l’action de requête](./media/connectors-native-webhook/using-action-2.png)
   
   Lors de l’exécution, l’application logique appelle le point de terminaison d’abonnement après avoir atteint cette étape.

4. Cliquez sur **Enregistrer** pour publier l’application logique.

## <a name="technical-details"></a>Détails techniques

Voici plus de détails sur les déclencheurs et les actions que webhook prend en charge.

## <a name="webhook-triggers"></a>Déclencheurs Webhook

| Action | Description |
| --- | --- |
| HTTP Webhook |Permet d’abonner une URL de rappel à un service qui peut appeler l’URL pour déclencher l’application logique lorsque nécessaire. |

### <a name="trigger-details"></a>Détails du déclencheur

#### <a name="http-webhook"></a>HTTP Webhook

Permet d’abonner une URL de rappel à un service qui peut appeler l’URL pour déclencher l’application logique lorsque nécessaire.
Une * signifie que le champ est obligatoire.

| Nom d’affichage | Nom de la propriété | Description |
| --- | --- | --- |
| Méthode d’abonnement* |method |Méthode HTTP à utiliser pour la demande d’abonnement |
| URI d’abonnement* |URI |URI HTTP à utiliser pour la demande d’abonnement |
| Méthode de résiliation d’abonnement* |method |Méthode HTTP à utiliser pour la demande de résiliation d’abonnement |
| URI de résiliation d’abonnement* |URI |URI HTTP à utiliser pour la demande de résiliation d’abonnement |
| Corps d’abonnement |body |Corps de la demande HTTP pour s’abonner |
| En-têtes de l’abonnement |headers |En-têtes de la demande HTTP pour s’abonner |
| Authentification de l’abonnement |Authentification |Authentification HTTP à utiliser pour s’abonner. [Connecteur HTTP](connectors-native-http.md#authentication) pour plus d’informations |
| Corps de résiliation d’abonnement |body |Corps de la demande HTTP de résiliation d’abonnement |
| En-têtes de résiliation d’abonnement |headers |En-têtes de la demande HTTP de résiliation d’abonnement |
| Authentification de la résiliation d’abonnement |authentication |Authentification HTTP à utiliser pour la résiliation d’abonnement. Voir [Connecteur HTTP](connectors-native-http.md#authentication) pour plus d’informations |

**Détails des résultats**

Requête Webhook

| Nom de la propriété | Type de données | Description |
| --- | --- | --- |
| headers |objet |En-têtes de requête Webhook |
| body |objet |Objet de la requête Webhook |
| Code d’état |int |Code d’état de la requête Webhook |

## <a name="webhook-actions"></a>Actions de webhook

| Action | Description |
| --- | --- |
| HTTP Webhook |Permet d’abonner une URL de rappel à un service qui peut appeler l’URL redémarrer une étape du flux de travail lorsque nécessaire. |

### <a name="action-details"></a>Détails de l’action

#### <a name="http-webhook"></a>HTTP Webhook

Permet d’abonner une URL de rappel à un service qui peut appeler l’URL redémarrer une étape du flux de travail lorsque nécessaire.
Une * signifie que le champ est obligatoire.

| Nom d’affichage | Nom de la propriété | Description |
| --- | --- | --- |
| Méthode d’abonnement* |method |Méthode HTTP à utiliser pour la demande d’abonnement |
| URI d’abonnement* |URI |URI HTTP à utiliser pour la demande d’abonnement |
| Méthode de résiliation d’abonnement* |method |Méthode HTTP à utiliser pour la demande de résiliation d’abonnement |
| URI de résiliation d’abonnement* |URI |URI HTTP à utiliser pour la demande de résiliation d’abonnement |
| Corps d’abonnement |body |Corps de la demande HTTP pour s’abonner |
| En-têtes de l’abonnement |headers |En-têtes de la demande HTTP pour s’abonner |
| Authentification de l’abonnement |Authentification |Authentification HTTP à utiliser pour s’abonner. [Connecteur HTTP](connectors-native-http.md#authentication) pour plus d’informations |
| Corps de résiliation d’abonnement |body |Corps de la demande HTTP de résiliation d’abonnement |
| En-têtes de résiliation d’abonnement |headers |En-têtes de la demande HTTP de résiliation d’abonnement |
| Authentification de la résiliation d’abonnement |authentication |Authentification HTTP à utiliser pour la résiliation d’abonnement. Voir [Connecteur HTTP](connectors-native-http.md#authentication) pour plus d’informations |

**Détails des résultats**

Requête Webhook

| Nom de la propriété | Type de données | Description |
| --- | --- | --- |
| headers |objet |En-têtes de requête Webhook |
| body |objet |Objet de la requête Webhook |
| Code d’état |int |Code d’état de la requête Webhook |

## <a name="next-steps"></a>Étapes suivantes

* [Créer une application logique](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Rechercher d’autres connecteurs](apis-list.md)
