---
title: Créer des révisions de modération avec la console d’API REST - Content Moderator
titlesuffix: Azure Cognitive Services
description: Utilisez les API de révision Azure Content Moderator afin de créer des révisions de texte ou d'image pour la modération humaine.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/18/2019
ms.author: sajagtap
ms.openlocfilehash: 254269ccedc92b9dfc164cc4665a8a8513682773
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60607555"
---
# <a name="create-human-reviews-rest"></a>Créer des révisions humaines (REST)

Les [révisions](./review-api.md#reviews) stockent et affichent le contenu que les modérateurs humains vont évaluer. Lorsqu’un utilisateur termine une révision, les résultats sont envoyés à un point de terminaison de rappel spécifié. Dans ce guide, vous apprendrez à configurer des révisions à l’aide des API REST de révision via la console d’API. Une fois que vous comprenez la structure des API, vous pouvez facilement déplacer ces appels vers n’importe quelle plateforme compatible avec REST.

## <a name="prerequisites"></a>Prérequis

- Connectez-vous ou créez un compte sur le site de l’[outil de révision](https://contentmoderator.cognitive.microsoft.com/) de Content Moderator.

## <a name="create-a-review"></a>Créer une révision

Pour créer une révision, accédez à la page de référence de l’API **[Révision - Créer](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4)** et sélectionnez le bouton de votre région clé (que vous pouvez trouver dans l’URL de point de terminaison à la page **Informations d’identification** de l’[Outil de révision](https://contentmoderator.cognitive.microsoft.com/)). Ceci fait démarrer la console d’API, où vous pouvez facilement construire et exécuter des appels d’API REST.

![Sélection d’une région Révision - Get](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>Entrer les paramètres d’appel REST

Entrez les valeurs de **teamName** et **Ocp-Apim-Subscription-Key** :

- **teamName** : ID d’équipe que vous avez créé lors de la configuration de votre compte [Outil de révision](https://contentmoderator.cognitive.microsoft.com/) compte (il se trouve dans le champ **Id** sur l’écran des informations d’identification de votre outil de révision).
- **Ocp-Apim-Subscription-Key** : votre clé Content Moderator. Vous pouvez la trouver sous l’onglet **Paramètres** de l’[Outil de révision](https://contentmoderator.cognitive.microsoft.com).

### <a name="enter-a-review-definition"></a>Entrez une définition de révision

Modifiez la zone **Corps de la demande** pour entrer la requête JSON avec les champs suivants :

- **Métadonnées** : paires clé-valeur personnalisées à retourner à votre point de terminaison de rappel. Si la clé est un code court qui est défini dans l’[outil de révision](https://contentmoderator.cognitive.microsoft.com), elle s’affiche sous forme d’une balise.
- **Contenu** : Dans le cas de contenu de type Image et Vidéo, il s’agit d’une chaîne URL pointant vers le contenu. Pour le contenu de type Texte, il s’agit de la chaîne de texte réelle.
- **ContentId** : chaîne d’identificateur personnalisée. cette chaîne est transmise à l'API et retournée via le rappel. Elle est utile pour associer des métadonnées ou des identificateurs internes aux résultats d’un travail de modération.
- **CallbackEndpoint** : (Facultatif) L’URL permettant de recevoir des informations de rappel lorsque la révision est terminée.

Le corps de la demande par défaut présente des exemples des différents types de révisions que vous pouvez créer :

```json
[Image]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Image",
    "Content": "<Content Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Text]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Text",
    "Content": "<Your Text Content>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Video]
[
  {
    "VideoFrames":[
      {
          "Id": "<Frame Id>",
          "Timestamp": "<Frame Timestamp",
          "FrameImage":"<Frame Image URL",
          "Metadata": [
            {
              "Key": "<Key>",
              "Value": "<Value"
            }
          ],
          "ReviewerResultTags": [
          ]
    ], 
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      },
      //For encrypted Videos
        {
          "Key": "protectedType",
          "Value": "AES or FairPlay or Widevine or Playready"
        },
        {
          "Key": "authenticationToken",
          "Value": "your viewtoken(In case of Video Indexer AES encryption type, this value is viewtoken from breakdown json)"
        },
      //For FairPlay encrypted type video include certificateUrl as well
        {
          "Key": "certificateUrl",
          "Value": "your certificate url"
        }
    ],
    "Type": "Video",
    "Content": "<Stream Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>",
    [Optional]
    "Timescale": "<Timescale of the video>
  }
]
```

### <a name="submit-your-request"></a>Soumettre votre requête
  
Sélectionnez **Envoyer**. Si l’opération réussit, l’**État de la réponse** est `200 OK` et la zone **Contenu de la réponse** affiche l’ID de la révision. Copiez cet ID pour l’utiliser dans les étapes suivantes.

![La zone Contenu de la réponse de la console Review - Create (Révision - Créer) affiche l’ID de révision](images/test-drive-review-2.PNG)

### <a name="examine-the-new-review"></a>Examiner la nouvelle révision

Dans l’[outil de révision](https://contentmoderator.cognitive.microsoft.com), sélectionnez **Révision** > **Image**/**Texte**/**Vidéo** (selon le contenu que vous avez utilisé). Le contenu que vous avez chargé doit apparaître, prêt pour la révision humaine.

![Image de l’outil de révision présentant un ballon de football](images/test-drive-review-5.PNG)

## <a name="get-review-details"></a>Obtenir les informations d’une révision

Pour récupérer des informations sur une révision existante, accédez à la page de référence d’API [Révision - Get](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) et sélectionnez le bouton de votre région (la région dans laquelle votre clé est administrée).

![Flux de travail - Sélection de région](images/test-drive-region.png)

Entrez les paramètres d’appel REST, comme dans la section ci-dessus. Pour cette étape, **reviewId** représente la chaîne d’ID unique que vous avez reçue lorsque vous avez créé la révision.

![Obtention des résultats dans la console Review - Create (Révision - Créer)](images/test-drive-review-3.PNG)
  
Sélectionnez **Envoyer**. Si l’opération réussit, l’**État de la réponse** est `200 OK` et la zone **Contenu de la réponse** affiche les informations de la révision au format JSON, comme suit :

```json
{  
  "reviewId":"201712i46950138c61a4740b118a43cac33f434",
  "subTeam":"public",
  "status":"Complete",
  "reviewerResultTags":[  
    {  
      "key":"a",
      "value":"False"
    },
    {  
      "key":"r",
      "value":"True"
    },
    {  
      "key":"sc",
      "value":"True"
    }
  ],
  "createdBy":"<teamname>",
  "metadata":[  
    {  
      "key":"sc",
      "value":"true"
    }
  ],
  "type":"Image",
  "content":"https://reviewcontentprod.blob.core.windows.net/<teamname>/IMG_201712i46950138c61a4740b118a43cac33f434",
  "contentId":"0",
  "callbackEndpoint":"<callbackUrl>"
}
```

Tenez compte des champs suivants dans la réponse :

- **statut**
- **reviewerResultTags** : s’affiche si toutes les balises ont été ajoutées manuellement par l’équipe de révision humaine (affichage du champ **createdBy**).
- **metadata** : montre les balises initialement ajoutées lors de la révision, avant les modifications apportées par l’équipe de révision manuelle.

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide, vous avez appris à créer des révisions de modération du contenu à l’aide de l’API REST. Nous allons maintenant intégrer des révisions à un scénario de modération de bout en bout en utilisant le tutoriel sur la [modération dans l’e-commerce](./ecommerce-retail-catalog-moderation.md).