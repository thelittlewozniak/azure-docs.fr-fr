---
title: Transformer des données avec Databricks Python - Azure | Microsoft Docs
description: Découvrez comment traiter ou transformer des données en exécutant une activité Databricks Python.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/15/2018
author: gauravmalhot
ms.author: gamal
ms.reviewer: maghan
manager: craigg
ms.openlocfilehash: 3ab3ec5380fbc90dffd4f258073ad8b477e2318a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002842"
---
# <a name="transform-data-by-running-a-python-activity-in-azure-databricks"></a>Transformer des données en exécutant une activité Python dans Azure Databricks

L’activité Azure Databricks Python dans un [pipeline Data Factory](concepts-pipelines-activities.md) exécute un fichier Python dans votre cluster Azure Databricks. Cet article s’appuie sur l’article  [Activités de transformation des données](transform-data.md) , qui présente une vue d’ensemble de la transformation des données et les activités de transformation prises en charge. Azure Databricks est une plateforme gérée pour exécuter Apache Spark.

Pour une présentation de onze minutes et la démonstration de cette fonctionnalité, regardez la vidéo suivante :

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-python-activity-definition"></a>Définition de l’activité Databricks Python

Voici l’exemple de définition JSON d’une activité Databricks Python :

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksSparkPython",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "pythonFile": "dbfs:/docs/pi.py",
            "parameters": [
                "10"
            ],
            "libraries": [
                {
                    "pypi": {
                        "package": "tensorflow"
                    }
                }
            ]
        }
    }
}
```

## <a name="databricks-python-activity-properties"></a>Propriétés de l’activité Databricks Python

Le tableau suivant décrit les propriétés JSON utilisées dans la définition JSON :

|Propriété|Description|Obligatoire|
|---|---|---|
|Nom|Nom de l'activité dans le pipeline.|OUI|
|description|Texte décrivant l’activité.|Non|
|Type|Pour l’activité Databricks Python, le type d’activité est DatabricksSparkPython.|OUI|
|linkedServiceName|Nom du service lié Databricks sur lequel s’exécute l’activité Python. Pour en savoir plus sur ce service lié, consultez l’article  [Services liés de calcul](compute-linked-services.md) .|OUI|
|pythonFile|L’URI du fichier Python à exécuter. Seuls les chemins DBFS sont pris en charge.|OUI|
|parameters|Les paramètres de ligne de commande qui sont passés au fichier Python. C’est un tableau de chaînes.|Non|
|libraries|Liste de bibliothèques à installer sur le cluster qui exécute la tâche. Il peut s’agir d’un tableau de < chaîne, objet >|Non|

## <a name="supported-libraries-for-databricks-activities"></a>Bibliothèques prises en charge pour les activités Databricks

Dans la définition d’activité Databricks ci-dessus, vous précisez ces types de bibliothèques : *jar*, *egg*, *maven*, *pypi*,  *cran*.

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "https://cran.us.r-project.org"
            }
        }
    ]
}

```

Pour plus d’informations, consultez la [documentation Databricks](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) pour les types de bibliothèques.

## <a name="how-to-upload-a-library-in-databricks"></a>Comment charger une bibliothèque dans Databricks

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Interface utilisateur de l’espace de travail Databricks](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Pour obtenir le chemin dbfs de la bibliothèque ajoutée par le biais de l’interface utilisateur, vous pouvez utiliser [CLI Databricks (installation)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

En général, les bibliothèques Jar sont stockées sous dbfs:/FileStore/jars lors de l’utilisation de l’interface utilisateur. Vous pouvez toutes les répertorier à l’aide de CLI : *databricks fs ls dbfs:/FileStore/jars* 



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Copier une bibliothèque avec CLI Databricks](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Exemple : *databricks fs cp SparkPi-assembly-0.1.jar dbfs:/FileStore/jars*