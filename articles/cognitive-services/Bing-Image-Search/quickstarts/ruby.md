---
title: 'Démarrage rapide : Rechercher des images - API REST Recherche d’images Bing et Ruby'
titleSuffix: Azure Cognitive Services
description: Utilisez ce guide de démarrage rapide pour envoyer des requêtes de recherche d’images à l’API REST Recherche d’images Bing à l’aide de Ruby et recevoir des réponses JSON.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 60a8c0009073a996aef7e7fd0bda8f88311b34e6
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66383595"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-ruby"></a>Démarrage rapide : Rechercher des images à l’aide de l’API REST Recherche d’images Bing et Ruby

Utilisez ce guide de démarrage rapide pour effectuer votre premier appel à l’API Recherche d’images Bing et recevoir une réponse JSON. Cette application Ruby simple envoie une requête de recherche à l’API et affiche les résultats bruts.

Alors que cette application est écrite en Ruby, l’API est un service web RESTful compatible avec la plupart des langages de programmation.

Le code source de cet exemple est disponible sur [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingImageSearchv7.rb).
## <a name="prerequisites"></a>Prérequis

* [La dernière version de Ruby](https://www.ruby-lang.org/en/downloads/).

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

Consultez également [Tarification Cognitive Services - API Recherche Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>Créer et initialiser l’application

1. Importez les packages suivants dans votre fichier de code.

    ```ruby
    require 'net/https'
    require 'uri'
    require 'json'
    ```

2. Créez des variables pour le point de terminaison d’API, le chemin de recherche d’API d’image, votre clé d’abonnement et le terme de recherche.

    ```ruby
    uri  = "https://api.cognitive.microsoft.com"
    path = "/bing/v7.0/images/search"
    term = "puppies"
    ```

## <a name="format-and-make-an-api-request"></a>Mettre en forme et effectuer une requête d’API

Utilisez les variables de la dernière étape pour mettre en forme une URL de recherche pour la requête d’API. Envoyez ensuite la requête.

```ruby
uri = URI(uri + path + "?q=" + URI.escape(term))


request = Net::HTTP::Get.new(uri)
request['Ocp-Apim-Subscription-Key'] = accessKey

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end
```

## <a name="process-and-print-the-json"></a>Traiter et imprimer du contenu JSON

Une fois la réponse reçue, vous pouvez analyser le contenu JSON et obtenir des valeurs à partir de celui-ci. Par exemple, l’URL de miniature vers le premier résultat et le nombre total d’images retournée.

```ruby
response.each_header do |key, value|
    # header names are lowercased
    if key.start_with?("bingapis-") or key.start_with?("x-msedge-") then
        puts key + ": " + value
    end
end

parsed_json = JSON.parse(response.body)
total_returned_images = parsed_json["totalEstimatedMatches"]
first_result = parsed_json["value"][0]["thumbnailUrl"]

puts "total number of returned matches: #{total_returned_images}"
puts "Url to the thumbnail of the first returned search result: #{first_result}"
```

## <a name="example-json-response"></a>Exemple de réponse JSON

Les réponses de l’API Recherche d’images Bing sont retournées au format JSON. Cet exemple de réponse a été tronqué pour afficher un résultat unique.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }]
}
```


## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Tutoriel d’une application monopage Recherche d’images Bing](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Voir aussi

* [Qu’est-ce que la Recherche d’images Bing ?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Essayez une démonstration interactive en ligne](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obtenir une clé d’accès Cognitive Services gratuite](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentation Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Informations de référence sur l’API Recherche d’images Bing](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
