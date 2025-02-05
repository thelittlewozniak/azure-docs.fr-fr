---
title: Qu’est-ce qu’un score BLEU ? - Custom Translator
titleSuffix: Azure Cognitive Services
description: BLEU est une mesure des différences entre une traduction automatique et une ou plusieurs traductions de référence créées par l’homme pour une même phrase source. L’algorithme BLEU compare les phrases consécutives de la traduction automatique avec les phrases consécutives qu’il trouve dans la traduction de référence, et il compte le nombre de correspondances de manière pondérée.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: swmachan
ms.openlocfilehash: a77fd1a84c1ffc18a1e0c74000c72db5cdbb00e1
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447376"
---
# <a name="what-is-a-bleu-score"></a>Qu’est-ce qu’un score BLEU ?

[BLEU (Bilingual Evaluation Understudy)](https://en.wikipedia.org/wiki/BLEU) est une mesure des différences entre une traduction automatique et une ou plusieurs traductions de référence créées par l’homme pour une même phrase source.

## <a name="scoring-process"></a>Processus de scoring

L’algorithme BLEU compare les phrases consécutives de la traduction automatique avec les phrases consécutives qu’il trouve dans la traduction de référence, et il compte le nombre de correspondances de manière pondérée. Ces correspondances sont indépendantes de la position. Un degré de concordance plus élevé indique un degré de similitude plus élevé avec la traduction de référence, et un score plus élevé. L’intelligibilité et l’exactitude grammaticale ne sont pas prises en compte.

## <a name="how-bleu-works"></a>Comment la technologie BLEU fonctionne-t-elle ?

La force de la technologie BLEU est qu’elle correspond bien au jugement humain en faisant la moyenne des erreurs de jugement des phrases individuelles sur un corpus d’essai, plutôt que de tenter d’établir un jugement humain exact pour chaque phrase.

Une discussion plus approfondie des scores BLEU est disponible [ici](https://youtu.be/-UqDljMymMg).

Les résultats de la technologie BLEU dépendent fortement de l’étendue de votre domaine, de la cohérence des données de test avec les données d’entraînement et de réglage, et de la quantité de données dont vous disposez pour l’entraînement. Si vos modèles ont été entraînés sur un domaine étroit et que vos données d’apprentissage correspondent à celles de vos tests, vous pouvez vous attendre à un score BLEU élevé.

>[!NOTE]
>Une comparaison entre les scores BLEU n’est justifiable que lorsque les résultats BLEU sont comparés au même ensemble de tests, à la même paire de langues et au même moteur de traduction machine. Un score BLEU d’une autre série de tests est forcément différent.
