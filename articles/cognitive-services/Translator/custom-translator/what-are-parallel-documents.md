---
title: Que sont des documents parallèles ? - Custom Translator
titleSuffix: Azure Cognitive Services
description: Les documents parallèles sont des paires de documents dont un document est la traduction de l’autre. Un document de la paire contient des phrases dans la langue source et l’autre document contient ces phrases traduites dans la langue cible.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: conceptual
ms.openlocfilehash: e644a4df99669e7ad69e08090418c2a3cffc7e9d
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389820"
---
# <a name="what-are-parallel-documents"></a>Que sont des documents parallèles ?

Les documents parallèles sont des paires de documents dont un document est la traduction de l’autre. Un document de la paire contient des phrases dans la langue source et l’autre document contient ces phrases traduites dans la langue cible.
Peu importe quelle langue est marquée comme « source » et quelle langue est marquée comme « cible » : un document parallèle peut être utilisé pour entraîner un système de traduction dans les deux sens.

## <a name="requirements"></a>Configuration requise

Vous devez au minimum de 10 000 phrases parallèle uniques pour l’apprentissage d’un système. Vous pouvez ajouter en permanence plus de contenu parallèle et relancer l’apprentissage afin d’améliorer la qualité de votre système de traduction. Cette approche fait partie des meilleures pratiques.

Microsoft exige que les documents chargés dans le Custom Translator respectent les droits d’auteur et la propriété intellectuelle d’un tiers. Pour plus d’informations, veuillez consulter les [conditions d’utilisation](https://azure.microsoft.com/support/legal/cognitive-services-terms/).
Le chargement d’un document à l’aide du portail ne transfère pas les droits de propriété intellectuelle du document lui-même.

## <a name="use-of-parallel-documents"></a>Utilisation de documents parallèles

Des documents parallèles sont utilisés par le système :

1.  Pour savoir comment de mots, d’expressions et de phrases sont généralement mappés entre les deux langages.

2.  Pour savoir comment traiter le contexte approprié selon les phrases environnantes. Un mot peut ne pas toujours se traduire exactement par le même mot dans l’autre langue.

Il est vivement conseillé de s’assurer qu’il y a une correspondance 1:1 entre les versions linguistiques source et cible des documents.

Si votre projet est spécifique à un domaine (catégorie), vos documents doivent être cohérents dans la terminologie de cette catégorie. La qualité du système de traduction qui en résulte dépend du nombre de phrases de votre document et de la qualité de celles-ci. Plus vos documents contiennent d’exemples avec des usages divers pour un mot spécifique à votre catégorie, meilleur sera le travail que le système pourra faire pendant la traduction.

Les documents téléchargés sont privés dans chaque espace de travail et peuvent être utilisés dans autant de projets ou d’apprentissages que vous le souhaitez. Les phrases extraites de vos documents sont stockées séparément dans votre référentiel sous forme de fichiers texte Unicode simples et sont disponibles pour suppression. N’utilisez pas le Custom Translator comme référentiel de documents, car vous ne pourrez pas télécharger les documents que vous avez chargés dans le format de chargement choisi.



## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment utiliser un [dictionnaire](what-is-dictionary.md) dans Custom Translator.
