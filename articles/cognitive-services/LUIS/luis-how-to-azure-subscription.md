---
title: Clés d’abonnement
titleSuffix: Language Understanding - Azure Cognitive Services
description: Vous n’avez pas besoin de créer de clés d’abonnement pour utiliser vos 1 000 premières requêtes de point de terminaison gratuites. Si vous recevez une erreur _hors quota_ sous la forme d’une erreur HTTP 403 ou 429, vous devez créer une clé et l’affecter à votre application.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/01/2019
ms.author: diberry
ms.openlocfilehash: 7315c80ad74eae07e41577fb2ac13742002e729e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60198578"
---
# <a name="using-subscription-keys-with-your-luis-app"></a>Utilisation des clés d’abonnement avec votre application LUIS

Vous n’avez pas besoin de créer de clés d’abonnement pour utiliser vos 1 000 premières requêtes de point de terminaison gratuites. Une fois que ces requêtes de point de terminaison sont utilisées, créez une ressource Azure dans le [portail Azure](https://portal.azure.com), puis affectez cette ressource à une application LUIS dans le [portail LUIS](https://www.luis.ai).

Si vous recevez une erreur _hors quota_ sous la forme d’une erreur HTTP 403 ou 429, vous devez créer une clé et l’affecter à votre application. 

Pour des tests et des prototypes uniquement, utilisez le niveau gratuit (F0). Pour les systèmes de production, utilisez un niveau [payant](https://aka.ms/luis-price-tier). N’utilisez pas la [clé de création](luis-concept-keys.md#authoring-key) pour les requêtes de point de terminaison en production.

<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"/>

## <a name="create-prediction-endpoint-runtime-resource-in-the-azure-portal"></a>Créer la ressource de runtime de point de terminaison de prédiction dans le portail Azure

En savoir plus avec le guide de démarrage rapide [Générer une application](get-started-portal-build-app.md).

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
<a name="key-concepts"></a>
<a name="authoring-key"></a>
<a name="create-and-use-an-endpoint-key"></a>
<a name="assign-endpoint-key"></a>
<a name="assign-resource"></a>


## <a name="assign-resource-key-to-luis-app-in-luis-portal"></a>Affecter une clé de ressource à l’application LUIS dans le portail LUIS

En savoir plus avec le guide de démarrage rapide [Déploiement](get-started-portal-deploy-app.md).

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

### <a name="unassign-resource"></a>Désaffecter la ressource
Quand vous désaffectez la clé de point de terminaison, elle n’est pas supprimée d’Azure. Elle n’est simplement plus liée à LUIS. 

Quand une clé de point de terminaison est désaffectée, ou non affectée à l’application, toute requête à l’URL de point de terminaison retourne une erreur : `401 This application cannot be accessed with the current subscription`. 

### <a name="include-all-predicted-intent-scores"></a>Inclure tous les scores d’intention prédits
La case à cocher **Include all predicted intent scores** (Inclure tous les scores d’intention prédits) permet de choisir que la réponse à la requête de point de terminaison inclue le score de prédiction pour chaque intention. 

Ce paramètre permet à votre chatbot ou à application appelante de LUIS de prendre une décision par programmation basée sur les scores des intentions renvoyées. En règle générale, les deux premières intentions sont les plus intéressantes. Si le score supérieur est l’intention None, votre chatbot peut choisir de poser une question de suivi qui effectue un choix définitif entre l’intention None et l’autre intention de score élevé. 

Les intentions et leurs scores sont également inclus dans les journaux d’activité de point de terminaison. Vous pouvez [exporter](luis-how-to-start-new-app.md#export-app) ces journaux d’activité et analyser les scores. 

```JSON
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

### <a name="enable-bing-spell-checker"></a>Activer le vérificateur orthographique Bing 
Dans les paramètres d’URL de point de terminaison (**Endpoint url settings**), le bouton bascule **Bing spell checker** (Vérificateur orthographique Bing) permet à LUIS de corriger les mots mal orthographiés avant la prédiction. Créez une **[clé de Vérification orthographique Bing](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)** . 

Ajoutez le paramètre de chaîne de requête **spellCheck=true** et **bing-spell-check-subscription-key={VOTRE_CLÉ_BING}** . Remplacez `{YOUR_BING_KEY_HERE}` par le clé de votre vérificateur orthographique Bing.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

### <a name="publishing-regions"></a>Régions de publication

En savoir plus sur les[régions](luis-reference-regions.md) de publication, notamment la publication en [Europe](luis-reference-regions.md#publishing-to-europe) et en [Australie](luis-reference-regions.md#publishing-to-australia). Les régions de publication sont différentes des régions de création. Créez une application dans la région de création qui correspond à la région de publication souhaitée pour le point de terminaison de requête.

## <a name="assign-resource-without-luis-portal"></a>Affecter des ressources sans portail LUIS

À des fins d’automation (par exemple pour le pipeline CI/CD), vous souhaiterez peut-être automatiser l’affectation d’une ressource LUIS à une application LUIS. Pour ce faire, vous devez suivre les étapes ci-dessous :

1. Obtenir un jeton Azure Resource Manager à partir de ce [site web](https://resources.azure.com/api/token?plaintext=true). Ce jeton expire ; veillez donc à l’utiliser immédiatement. La requête retourne un jeton Azure Resource Manager.

    ![Demander un jeton Azure Resource Manager et recevoir un jeton Azure Resource Manager](./media/luis-manage-keys/get-arm-token.png)

1. Utiliser le jeton pour demander les ressources LUIS dans les abonnements, à partir de l’[API LUIS d’obtention de comptes Azure](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), auxquels votre compte d’utilisateur a accès. 

    Cette API POST nécessite les paramètres suivants :

    |En-tête|Valeur|
    |--|--|
    |`Authorization`|La valeur de `Authorization` est `Bearer {token}`. Notez que la valeur du jeton doit être précédée du mot `Bearer` et d’un espace.| 
    |`Ocp-Apim-Subscription-Key`|Votre [clé de création](luis-how-to-account-settings.md).|

    Cette API retourne un tableau d’objets JSON de vos abonnements LUIS, notamment l’ID d’abonnement, le groupe de ressources et le nom de la ressource, retourné en tant que nom de compte. Recherchez l’élément dans le tableau qui est la ressource LUIS à affecter à l’application LUIS. 

1. Affectez le jeton à la ressource LUIS avec l’API d’[affectation de compte LUIS Azure à une application](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515). 

    Cette API POST nécessite les paramètres suivants :

    |Type|Paramètre|Valeur|
    |--|--|--|
    |En-tête|`Authorization`|La valeur de `Authorization` est `Bearer {token}`. Notez que la valeur du jeton doit être précédée du mot `Bearer` et d’un espace.|
    |En-tête|`Ocp-Apim-Subscription-Key`|Votre [clé de création](luis-how-to-account-settings.md).|
    |En-tête|`Content-type`|`application/json`|
    |Querystring|`appid`|L’ID d’application LUIS. 
    |body||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Quand cette API réussit, elle retourne l’état 201 - créé. 

## <a name="change-pricing-tier"></a>Changer le niveau tarifaire

1.  Dans [Azure](https://portal.azure.com), recherchez votre abonnement LUIS. Sélectionnez l’abonnement LUIS.
    ![Rechercher votre abonnement LUIS](./media/luis-usage-tiers/find.png)
1.  Sélectionnez **Niveau tarifaire** pour afficher les niveaux tarifaires disponibles. 
    ![Afficher les niveaux tarifaires](./media/luis-usage-tiers/subscription.png)
1.  Sélectionnez le niveau tarifaire, puis choisissez **Sélectionner** pour enregistrer le changement apporté. 
    ![Modifier votre niveau de paiement LUIS](./media/luis-usage-tiers/plans.png)
1.  Lorsque la modification de tarification est terminée, une fenêtre contextuelle vérifie le nouveau niveau tarifaire. 
    ![Vérifier votre niveau de paiement LUIS](./media/luis-usage-tiers/updated.png)
1. N’oubliez pas d’[affecter cette clé de point de terminaison](#assign-endpoint-key) sur la page **Publier** et de l’utiliser dans toutes les requêtes de point de terminaison. 

## <a name="how-to-fix-out-of-quota-errors-when-the-key-exceeds-pricing-tier-usage"></a>Comment corriger les erreurs de dépassement de quota quand la clé excède l’utilisation du niveau tarifaire
Chaque niveau autorise des requêtes de point de terminaison pour votre compte LUIS à un débit spécifique. Si le débit de requêtes est supérieur à celui autorisé pour votre compte facturé à l’usage par minute ou par mois, les requêtes reçoivent une erreur HTTP « 429 : 429 Trop de requêtes. »

Chaque niveau permet un nombre de requêtes cumulées par mois. Si le nombre total de requêtes dépasse le débit autorisé, les requêtes reçoivent une erreur HTTP « 403 : interdit ».  

## <a name="viewing-summary-usage"></a>Affichage résumé de l’utilisation
Vous pouvez afficher des informations sur l’utilisation de LUIS dans Azure. La page **Vue d’ensemble** affiche des informations récapitulatives récentes, y compris les appels et les erreurs. Si vous effectuez une requête de point de terminaison LUIS, puis consultez immédiatement la **page Vue d’ensemble**, attendez cinq minutes, le temps que l’utilisation s’affiche.

![Affichage résumé de l’utilisation](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Personnalisation des graphiques d’utilisation
Les métriques fournissent une vue plus détaillée des données.

![Mesures par défaut](./media/luis-usage-tiers/metrics-default.png)

Vous pouvez configurer vos graphiques de métriques pour la période et le type de métrique. 

![Mesures personnalisées](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Alerte de seuil de nombre total de transactions
Si vous souhaitez connaître le moment où vous avez atteint un certain seuil de transactions (10 000 transactions, par exemple), vous pouvez créer une alerte. 

![Alertes par défaut](./media/luis-usage-tiers/alert-default.png)

Ajout d’une alerte de métrique pour le **nombre total d’appels** pendant une certaine période. Ajouter les adresses de messagerie de toutes les personnes qui doivent recevoir l’alerte. Ajoutez des webhooks pour tous les systèmes qui doivent recevoir l’alerte. Vous pouvez également exécuter une application logique lorsque l’alerte est déclenchée. 

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment utiliser des [versions](luis-how-to-manage-versions.md) pour gérer les modifications apportées à votre application LUIS.
