---
title: Ajouter la recherche en texte intégral au Stockage Blob Azure - Recherche Azure
description: Analyser le contenu textuel dans le Stockage Blob Azure pour l’indexation de Recherche Azure dans du code à l’aide de l’API REST de HTTP.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/01/2019
author: mgottein
manager: cgronlun
ms.author: magottei
ms.custom: seodec2018
ms.openlocfilehash: b6bb70e4c56adb162006d2597d301c73b12d2a8a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540885"
---
# <a name="searching-blob-storage-with-azure-search"></a>Recherche dans le Stockage Blob avec la Recherche Azure

La recherche dans les différents types de contenu enregistrés dans le Stockage Blob Azure peut constituer un problème difficile à résoudre. Toutefois, vous pouvez indexer et rechercher le contenu de vos objets blob en quelques clics à l’aide de la Recherche Azure. La recherche dans le Stockage Blob nécessite d’approvisionner un service Recherche Azure. Les différentes limites de service et les niveaux de tarification de Recherche Azure sont indiqués sur la [page de tarification](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Présentation de Recherche Azure
[Recherche Azure](https://aka.ms/whatisazsearch) est un service de recherche qui permet facilement aux développeurs d’ajouter des expériences de recherche de texte intégral fiables aux applications web et mobiles. En tant que service, Recherche Azure élimine la nécessité de gérer une infrastructure de recherche, tout en offrant un [SLA garantissant un temps d’activité de 99,9 %](https://aka.ms/azuresearchsla).

## <a name="index-and-search-enterprise-document-formats"></a>Indexation et recherche : formats de documents d’entreprise pris en charge
Du fait de la prise en charge de l’[extraction de documents](https://aka.ms/azsblobindexer) dans le Stockage Blob Azure, vous pouvez indexer le contenu suivant :

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

En extrayant le texte et les métadonnées de ces types de fichiers, vous pouvez effectuer une recherche dans plusieurs formats de fichiers avec une même requête. 

## <a name="search-through-your-blob-metadata"></a>Effectuer des recherches dans vos métadonnées d’objets blob
Un scénario courant qui facilite le tri dans les objets blob comportant tout type de contenu est celui qui consiste à indexer les métadonnées personnalisées et les propriétés système pour chaque objet blob. De cette façon, les informations de tous les objets blob sont indexées indépendamment du type de document. Vous pouvez alors effectuer un tri, un filtrage et une facette dans l’ensemble du contenu du stockage Blob.

[En savoir que plus sur l’indexation des métadonnées d’objets blob.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Recherche d’images
La recherche de texte intégral de Recherche Azure, la navigation à facettes et les capacités de tri peuvent désormais être appliquées aux métadonnées des images stockées dans les objets blob.

La recherche cognitive inclut des compétences pour le traitement d’image comme la [reconnaissance optique des caractères (OCR)](cognitive-search-skill-ocr.md) et d’identification des [composants visuels](cognitive-search-skill-image-analysis.md) qui permettent d’indexer le contenu visuel identifié dans chaque image.

## <a name="index-and-search-through-json-blobs"></a>Indexation et recherche à l’aide des objets blob JSON
Recherche Azure peut être configuré pour extraire le contenu structuré des objets blob qui contiennent des objets JSON. Recherche Azure peut lire les objets blob JSON et analyser le contenu structuré dans les champs adaptés du document Recherche Azure. Recherche Azure peut également extraire les objets blob contenant des objets JSON et mapper chaque élément avec un document Recherche Azure différent.

L’analyse JSON n’est actuellement pas configurable via le portail. [En savoir plus sur l’analyse des objets JSON dans Recherche Azure.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Démarrage rapide
Il est possible d’ajouter directement Recherche Azure à des objets Blob, à partir de la page Stockage Blob du portail.

![](./media/search-blob-storage-integration/blob-blade.png)

Cliquez sur **Ajouter Recherche Azure** pour lancer un flux dans lequel vous pouvez sélectionner un service Recherche Azure existant ou créer un nouveau service. Si vous créez un nouveau service, vous quittez l’expérience portail de votre compte Stockage. Vous pouvez revenir à la page Stockage du portail, sélectionner à nouveau l’option **Ajouter Recherche Azure**, puis sélectionner le service existant.

## <a name="next-steps"></a>Étapes suivantes
Consultez la [documentation](https://aka.ms/azsblobindexer) pour en savoir plus sur l’indexeur d’objets blob Recherche Azure.
