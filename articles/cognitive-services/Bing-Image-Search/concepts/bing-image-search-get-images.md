---
title: 'Obtention d’images à partir du web : API Recherche d’images Bing'
titleSuffix: Azure Cognitive Services
description: Utilisez l’API Recherche d’images Bing pour rechercher et télécharger des images pertinentes à partir du web.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: f169f969a1acf4cefc8cee27f74a99730491176a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389408"
---
# <a name="get-images-from-the-web-with-the-bing-image-search-api"></a>Obtenez des images à partir du web avec l’API Recherche d’images Bing

Lorsque vous utilisez l’API REST Recherche d’images Bing, vous pouvez obtenir des images web qui sont liées au terme de votre recherche en envoyant la requête GET suivante :

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

> [!IMPORTANT]
> * Toutes les requêtes doivent provenir d’un serveur, et non d’un client.
> * Si vous appelez l’une des API de recherche Bing pour la première fois, n’incluez pas l’en-tête ID client. Indiquez uniquement l’ID client si vous avez précédemment appelé une API Bing qui a renvoyé un ID client pour la combinaison utilisateur/appareil.
> * Les images doivent être affichées dans l’ordre dans lequel elles apparaissent dans la réponse.

## <a name="get-images-from-a-specific-web-domain"></a>Obtenir des images à partir d’un domaine web spécifique

Pour obtenir des images à partir d’un domaine spécifique, utilisez l’opérateur de requête [site:](https://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> Les réponses aux requêtes utilisant l’opérateur `site:` peuvent inclure du contenu pour adultes, quel que soit le paramètre [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch). N’utilisez `site:` que si vous connaissez le contenu du domaine.

L’exemple suivant montre comment obtenir les petites images découvertes par Bing la semaine dernière sur le site ContosoSailing.com.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="filter-images"></a>Filtrer les images

 Par défaut, l’API Recherche d’images renvoie toutes les images ayant un rapport avec la requête. Si vous souhaitez filtrer les images que Bing renvoie (par exemple, pour ne recevoir que des images avec un arrière-plan transparent ou d’une taille spécifique), utilisez les paramètres de requête suivants :

* [aspect](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#aspect) : filtre les images selon leurs proportions (par exemple, standard ou grand écran).
* [color](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#color) : filtre les images selon leur couleur (couleur dominante ou noir et blanc).
* [freshness](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#freshness) : filtre les images selon leur ancienneté (par exemple, celles découvertes par Bing la semaine dernière).
* [height](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#width) : filtre les images selon leur largeur et leur hauteur.
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagecontent) : filtre les images selon leur contenu (par exemple, celles qui montrent uniquement le visage d’une personne).
* [imageType](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype) : filtre les images selon leur type (images clipart, images GIF animées, arrière-plans transparents, etc.).
* [license](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#license) : filtre les images selon le type de contrat de licence associé au site.
* [size](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#size) : filtre les images selon leur taille (par exemple, petites images jusqu’à 200 x 200 pixels).

Pour obtenir des images à partir d’un domaine spécifique, utilisez l’opérateur de requête [site:](https://msdn.microsoft.com/library/ff795613.aspx).

 > [!NOTE]
 > Les réponses aux requêtes utilisant l’opérateur `site:` peuvent inclure du contenu pour adultes, quel que soit le paramètre [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch). N’utilisez `site:` que si vous connaissez le contenu du domaine.

L’exemple suivant montre comment obtenir les petites images découvertes par Bing la semaine dernière sur le site ContosoSailing.com.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Format de réponse de Recherche d’images Bing

Le message de réponse de Bing contient une réponse [Images](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) qui inclut une liste d’images que Cognitives Services a déterminé comme étant pertinentes pour la requête. Chaque objet [Image](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image) de la liste inclut les informations suivantes sur l’image : l’URL, sa taille, ses dimensions, son format d’encodage, une URL vers une miniature de l’image et les dimensions de la miniature.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Quand vous appelez l’API Recherche d’images Bing, Bing retourne une liste de résultats. La liste est un sous-ensemble du nombre total de résultats en rapport avec la requête. Le champ `totalEstimatedMatches` de la réponse contient une estimation du nombre d’images disponibles pour visualisation. Pour plus d’informations sur la manière de parcourir les images restantes, consultez [Parcourir les images](../paging-images.md).

## <a name="next-steps"></a>Étapes suivantes

Si vous n’avez pas déjà essayé l’API Recherche d’images Bing, essayez un [démarrage rapide](../quickstarts/csharp.md). Si vous recherchez quelque chose de plus complexe, suivez le didacticiel de création d’une [application web à une seule page](../tutorial-bing-image-search-single-page-app.md).
