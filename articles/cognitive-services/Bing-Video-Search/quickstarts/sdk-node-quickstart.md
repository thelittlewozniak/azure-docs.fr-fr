---
title: 'Démarrage rapide : Rechercher des vidéos à l’aide du SDK Recherche de vidéos Bing pour Node.js'
titleSuffix: Azure Cognitive Services
description: Utilisez ce guide de démarrage rapide pour envoyer des requêtes de recherche de vidéos à l’aide du SDK Recherche de vidéos Bing pour Node.js.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: f00f4c90d529e95aa495f68802f4da9a097d3b2b
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798005"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-nodejs"></a>Démarrage rapide : Effectuer une recherche de vidéos avec le SDK Recherche de vidéos Bing pour Node.js

Utilisez ce guide de démarrage rapide pour démarrer une recherche de vidéos avec le SDK Recherche de vidéos Bing pour Node.js. Si l’outil Recherche de vidéos Bing dispose d’une API REST compatible avec la plupart des langages de programmation, le SDK offre quant à lui un moyen facile d’intégrer le service à vos applications. Le code source de cet exemple est disponible sur [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js). Il contient d’autres fonctionnalités et annotations.

## <a name="prerequisites"></a>Prérequis

- [Node.JS](https://www.nodejs.org/)

Pour configurer une application console à l’aide du SDK Recherche de vidéos Bing :
* Exécutez `npm install ms-rest-azure` dans votre environnement de développement.
* Exécutez `npm install azure-cognitiveservices-videosearch` dans votre environnement de développement.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Créer et initialiser l’application

1. Créez un fichier JavaScript dans votre éditeur ou IDE favori, puis ajoutez une instruction `require()` pour le SDK Recherche de vidéos Bing et le module `CognitiveServicesCredentials`. Créez une variable pour votre clé d’abonnement. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
    ```

2. Créez une instance de `CognitiveServicesCredentials` avec votre clé. Ensuite, utilisez-la pour créer une instance du client de recherche de vidéos.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Envoyer la requête de recherche

1. Utilisez `client.videosOperations.search()` pour envoyer une requête de recherche à l’API Recherche de vidéos Bing. Quand les résultats de la recherche sont retournés, utilisez `.then()` pour journaliser le résultat.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer une application web monopage](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Voir aussi 

* [Qu’est-ce que l’API Recherche de vidéos Bing ?](../overview.md)
* [Exemples du Kit de développement logiciel (SDK) .NET pour Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)