---
title: Compétences cognitives déconseillées - Recherche Azure
description: Cette page contient la liste des compétences cognitives de recherche qui sont considérées comme déconseillées et qui ne seront pas prises en charge dans un avenir proche.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: a73c7e381cb6001b773251a1812466b3c82373f2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65541734"
---
# <a name="deprecated-cognitive-search-skills"></a>Compétences de recherche cognitive déconseillées

Ce document décrit les compétences cognitives qui sont considérées comme déconseillées. Utilisez le guide suivant pour le contenu :

* Nom de la compétence : nom de la compétence qui sera déconseillée, il est mappé sur l’attribut @odata.type.
* Dernière version de l’API disponible : dernière version de l’API publique de recherche Azure via laquelle les compétences contenant la compétence déconseillée correspondante peut être créée/mise à jour.
* Fin de la prise en charge : dernier jour après lequel la compétence correspondante est considérée non prise en charge. Les compétences créées précédemment doivent continuer à fonctionner, mais il est recommandé aux utilisateurs de migrer hors d’une compétence déconseillée.
* Recommandations : chemin de migration vers l’avant pour utiliser une compétence prise en charge. Il est conseillé aux utilisateurs de suivre ces suggestions pour continuer à bénéficier du support technique.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft.Skills.Text.NamedEntityRecognitionSkill

### <a name="last-available-api-version"></a>Dernière version d’API disponible

2019-05-06-Preview

### <a name="end-of-support"></a>Fin de la prise en charge

15 février 2019

### <a name="recommendations"></a>Recommandations 

Utilisez [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) à la place. Il fournit la plupart des fonctionnalités de NamedEntityRecognitionSkill à un meilleur niveau de qualité. Il détient également des informations plus riches dans ses champs de sortie complexes.

Pour migrer vers la [compétence de reconnaissance des entités](cognitive-search-skill-entity-recognition.md), vous devrez apporter une ou plusieurs des modifications suivantes à votre définition de compétence. Vous pouvez mettre à jour la définition de compétence à l’aide de l’[API de mise à jour de compétences](https://docs.microsoft.com/rest/api/searchservice/update-skillset).

> [!NOTE]
> actuellement, le score de confiance comme concept n’est pas pris en charge. Le paramètre `minimumPrecision` existe sur `EntityRecognitionSkill` pour une utilisation ultérieure et la compatibilité descendante.

1. *(Obligatoire)* Modifiez `@odata.type` en remplaçant `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` par `"#Microsoft.Skills.Text.EntityRecognitionSkill"`.

2. *(Facultatif)* Si vous utilisez la sortie `entities`, utilisez la sortie de collection complexe `namedEntities` issue de `EntityRecognitionSkill` à la place. Vous pouvez utiliser `targetName` dans la définition de compétence pour le mapper à une annotation appelée `entities`.

3. *(Facultatif)* Si vous ne spécifiez pas explicitement `categories`, `EntityRecognitionSkill` peut retourner un type différent de catégories en plus de celles qui étaient prises en charge par `NamedEntityRecognitionSkill`. Si ce comportement est indésirable, veillez à définir explicitement le paramètre `categories` sur `["Person", "Location", "Organization"]`.

    _Exemples de définitions de migration_

    * Migration simple

        _(Avant) Définition de compétence NamedEntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
        _(Après) Définition de compétence EntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
    
    * Migration légèrement compliquée

        _(Avant) Définition de compétence NamedEntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "entities"
            }
            ]
        }
        ```
        _(Après) Définition de compétence EntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person", "Location", "Organization" ],
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "namedEntities",
                "targetName": "entities"
            }
            ]
        }
        ```

## <a name="see-also"></a>Voir aussi

+ [Compétences prédéfinies](cognitive-search-predefined-skills.md)
+ [Guide pratique pour définir un ensemble de compétences](cognitive-search-defining-skillset.md)
+ [Compétence de reconnaissance des entités](cognitive-search-skill-entity-recognition.md)
