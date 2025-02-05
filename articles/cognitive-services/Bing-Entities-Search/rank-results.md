---
title: Utilisation du classement pour afficher les réponses – Recherche d’entités Bing
titlesuffix: Azure Cognitive Services
description: Découvrez comment utiliser le classement pour afficher les réponses retournées par l’API Recherche d’entités Bing.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 9e2a4075436145a0cc185b7ab1b406fa8d27b8e3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60309332"
---
# <a name="using-ranking-to-display-entity-search-results"></a>Utilisation du classement pour afficher les résultats de la recherche d’entités  

Chaque réponse de recherche d’entité inclut une réponse [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) qui spécifie comment vous devez afficher les résultats de recherche renvoyés par l’API Recherche d’entités Bing. La réponse de classement regroupe les résultats par pôle, principal et encadré. Le résultat en pôle est le plus important ou connu et doit être affiché en premier. Si vous n’affichez pas les résultats restants dans un format principal ou en encadré, vous devez fournir au contenu principal une visibilité supérieure à celui en encadré. 
  
Au sein de chaque groupe, le tableau [Éléments](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) identifie l’ordre dans lequel doit apparaître le contenu. Chaque élément fournit deux façons d’identifier le résultat au sein d’une réponse.  
 

|Champ | Description  |
|---------|---------|
|`answerType` et `resultIndex` | `answerType` identifie la réponse (Entité ou Lieu) et `resultIndex` identifie un résultat au sein de la réponse (par exemple, une entité). L’index commence à 0.|
|`value`    | `value` contient un identifiant qui correspond à l’identifiant de la réponse ou d’un résultat au sein de la réponse. La réponse ou les résultats contiennent l’identifiant, mais pas les deux. |
  
L’utilisation de `answerType` et `resultIndex` est un processus en deux étapes. Tout d’abord, utilisez `answerType` pour identifier la réponse qui contient les résultats à afficher. Ensuite, utilisez `resultIndex` pour indexer dans les résultats de cette réponse afin d’en afficher le résultat. (La valeur `answerType` correspond au nom du champ dans l’objet [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse).) Si vous êtes censé afficher tous les résultats de la réponse ensemble, l’élément de réponse de classement n’inclut pas le champ `resultIndex`.

Vous devez faire correspondre l’ID du classement et l’ID d’une réponse (ou un de ses résultats) pour pouvoir utiliser cet ID. Si un objet de réponse inclut un champ `id`, affichez tous les résultats de réponses ensemble. Par exemple, si l’objet `Entities` inclut le champ `id`, affichez tous les articles d’entités ensemble. Si l’objet `Entities` n’inclut pas le champ `id`, alors chaque entité comprend un champ `id` et la réponse de classement mélange les résultats d’entités avec les résultats de lieux.  
  
## <a name="ranking-response-example"></a>Exemple de réponse de classement

Le code suivant vous fournit un exemple [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse).
  
```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Jimi Hendrix"
  },
  "entities": { ... },
  "rankingResponse": {
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        },
        {
          "answerType": "Entities",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.1"
          }
        }
      ]
    }
  }
}
```

Selon cette réponse de classement, l’encadré devrait afficher les deux résultats d’entité associés à Jimi Hendrix.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer une application web monopage](tutorial-bing-entities-search-single-page-app.md)
