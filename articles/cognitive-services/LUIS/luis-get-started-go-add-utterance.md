---
title: Modifier, effectuer l'apprentissage d'une application, Go
description: Dans ce guide de démarrage rapide sur le langage Go, vous allez ajouter des exemples d’énoncés à une application de domotique et effectuer l’entraînement de l’application.
titleSuffix: Language Understanding - Microsoft Cognitive Services
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: diberry
ms.openlocfilehash: 973323cd14d9472d9845f0709fc6d2924efc56f5
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357152"
---
# <a name="quickstart-change-model-using-go"></a>Démarrage rapide : Modifier un modèle à l'aide de Go

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Le langage de programmation [Go](https://golang.org/) doit être installé.
* [VSCode](https://code.visualstudio.com) 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Exemples d’énoncés de fichier JSON

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Créer un code de démarrage rapide 

1. Créez `add-utterances.go` avec VSCode. 

2. Ajoutez des dépendances. 

   [!code-go[Add dependencies.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=2-10 "Add dependencies.")]

3. Ajoutez une fonction de requête HTTP générique, qui inclut la clé de création de transmission dans l’en-tête. 

   [!code-go[Add HTTP request function which includes passing authoring key in header.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=12-36 "Add HTTP request function, which includes passing authoring key in header. ")]

4. Ajoutez des exemples d’énoncés à partir du fichier JSON.

   [!code-go[Add example utterances from JSON file.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=62-76 "Add example utterances from JSON file.")]

5. Demandez la formation. Utilise une fonction d’assistance pour définir le verbe pour le même itinéraire que l’état de la formation. 

   [!code-go[Request training.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=77-86 "Request training. ")]

6. Demandez l’état de la formation. Utilise une fonction d’assistance pour définir le verbe pour le même itinéraire que la demande de la formation. 

   [!code-go[Request training status.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=87-90 "Request training status. ")]

7. Ajoutez la fonction principale pour gérer l’analyse de ligne de commande.

   [!code-go[Add main function to handle command line parsing.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=38-60 "Add main function to handle command-line parsing.")]

## <a name="add-an-utterance-from-the-command-line-train-and-get-status"></a>Ajouter un énoncé à partir de la ligne de commande, effectuer l’apprentissage et obtenir l’état

1. À partir d’une invite de commandes dans le répertoire où vous avez créé le fichier Go, entrez `go build add-utterances.go` pour compiler le fichier Go. L’invite de commande ne retourne pas d’informations pour une génération réussie.

2. Exécutez l’application Go à partir de la ligne de commande en entrant le texte suivant dans l’invite de commandes : 

    ```console
    add-utterances -appID <your-app-id> -authoringKey <add-your-authoring-key> -version <your-version-id> -region westus -utteranceFile utterances.json

    ```

    Remplacez `<add-your-authoring-key>` par la valeur de votre clé de création (également connue sous le nom de clé de démarrage). Remplacez `<your-app-id>` par la valeur de l’ID de votre application. Remplacez `<your-version-id>` par la valeur de votre version. La version par défaut est `0.1`.

    Cette invite de commandes affiche les résultats :

    ```console
    add example utterances requested
    [
        {
            "text": "go lang 1",
            "intentName": "None",
            "entityLabels": []
        }
    ,
        {
            "text": "go lang 2",
            "intentName": "None",
            "entityLabels": []
        }
    ]
    201
    [
        {
            "value": {
                "ExampleId": 77783998,
                "UtteranceText": "go lang 1"
            },
            "hasError": false
        },
        {
            "value": {
                "ExampleId": 77783999,
                "UtteranceText": "go lang 2"
            },
            "hasError": false
        }
    ]
    training selected
    202
    {"statusId":9,"status":"Queued"}
    training status selected
    200
    [
        {
            "modelId": "c52d6509-9261-459e-90bc-b3c872ee4a4b",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "5119cbe8-97a1-4c1f-85e6-6449f3a38d77",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "01e6b6bc-9872-47f9-8a52-da510cddfafe",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "33b409b2-32b0-4b0c-9e91-31c6cfaf93fb",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "1fb210be-2a19-496d-bb72-e0c2dd35cbc1",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "3d098beb-a1aa-423f-a0ae-ce08ced216d6",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "cce854f8-8f8f-4ed9-a7df-44dfea562f62",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "4d97bf0d-5213-4502-9712-2d6e77c96045",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        }
    ]
    ```

    Cette réponse comprend le code d’état HTTP pour chacun des trois appels HTTP, ainsi que toutes les réponses JSON retournées dans le corps de la réponse. 

## <a name="clean-up-resources"></a>Supprimer des ressources
Une fois le démarrage rapide terminé, supprimez tous les fichiers créés pour ce démarrage rapide. 

## <a name="next-steps"></a>Étapes suivantes
> [!div class="nextstepaction"] 
> [Générer une application web avec un domaine personnalisé](luis-quickstart-intents-only.md) 
