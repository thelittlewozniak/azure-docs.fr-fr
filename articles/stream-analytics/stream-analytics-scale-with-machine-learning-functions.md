---
title: Mettre à l’échelle les fonctions Machine Learning dans Azure Stream Analytics
description: Cet article décrit comment mettre à l’échelle des travaux Stream Analytics qui utilisent des fonctions Machine Learning en configurant des unités de partitionnement et de diffusion.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: f11034a4970e3fb95333310af82a6b2a2551f1eb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61479145"
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Mettre à l’échelle votre travail Stream Analytics avec des fonctions Azure Machine Learning
Il est extrêmement facile de configurer un travail Stream Analytics et de l’utiliser pour exécuter des exemples de données. Que faire si nous avons besoin d’exécuter le même travail avec un volume de données plus important ? Nous devons comprendre comment configurer le travail Stream Analytics pour permettre sa mise à l’échelle. Dans ce document, nous nous concentrerons sur les aspects particuliers de la mise à l’échelle des travaux Stream Analytics avec des fonctions Machine Learning. Pour plus d’informations sur la procédure de mise à l’échelle des travaux Stream Analytics en général, voir l’article [Mise à l’échelle des travaux](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Qu’est-ce qu’une fonction Azure Machine Learning dans Stream Analytics ?
Une fonction Machine Learning dans Stream Analytics peut être utilisée comme un appel de fonction normal dans le langage de requête Stream Analytics. Toutefois, en arrière-plan, ces appels de fonction sont en fait des demandes de services web Microsoft Azure Machine Learning. Les services web Machine Learning prennent en charge le « traitement par lot » de plusieurs lignes, appelé mini-lot, dans un même appel d’API de service web pour améliorer le débit général. Pour plus d’informations, consultez [Services web Azure Machine Learning](../machine-learning/studio/consume-web-services.md).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Configurer un travail Stream Analytics avec des fonctions Azure Machine Learning
Lorsque vous configurez une fonction Machine Learning pour un travail Stream Analytics, vous devez prendre en compte deux paramètres : la taille de lot des appels de fonction Machine Learning et les unités de diffusion en continu approvisionnées pour le travail Stream Analytics. Pour déterminer les valeurs appropriées des unités de streaming, commencez par trouver le bon compromis entre latence et débit, autrement dit, entre la latence du travail Stream Analytics et le débit de chaque unité de streaming. Il est toujours possible d’ajouter des unités de diffusion en continu à un travail pour augmenter le débit d’une requête Stream Analytics correctement partitionnée. Toutefois, cet ajout augmente le coût d’exécution du travail.

Il est donc important de déterminer la *tolérance* de latence pour l’exécution d’un travail Stream Analytics. L’augmentation de la taille de lot entraîne naturellement une latence supplémentaire dans le cadre de l’exécution des demandes de services Azure Machine Learning, et donc une augmentation de la latence du travail Stream Analytics. En revanche, l’accroissement de la taille de lot permet au travail Stream Analytics de traiter *davantage d’événements avec le *même nombre* de demandes de services web Machine Learning. L’augmentation de la latence de service web Machine Learning est souvent consécutive à l’augmentation de la taille de lot ; il est donc important de prendre en compte la taille de lot la plus rentable pour un service web Machine Learning dans n’importe quelle situation donnée. La taille de lot par défaut pour les demandes de service web est de 1 000 et peut être modifiée au moyen de [l’API REST Stream Analytics](https://msdn.microsoft.com/library/mt653706.aspx "l’API REST Stream Analytics") ou du [client PowerShell pour Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "client PowerShell pour Stream Analytics").

Une fois qu’une taille de lot a été déterminée, la quantité d’unités de diffusion en continu peut être fixée en fonction du nombre d’événements que la fonction doit traiter par seconde. Pour plus d’informations sur les unités de diffusion en continu, voir [Mise à l’échelle des travaux Stream Analytics](stream-analytics-scale-jobs.md).

En général, il existe 20 connexions simultanées au service web Machine Learning pour chaque ensemble de 6 unités de streaming, mais les travaux à 1 unité de streaming et ceux à 3 unités de streaming obtiennent aussi 20 connexions simultanées.  Par exemple, si le débit de données d’entrée est de 200 000 événements par seconde, et que la taille de lot par défaut de 1000 est conservée, la latence de service web résultante avec un mini-lot de 1000 événements sera de 200 ms. Cela signifie que chaque connexion peut envoyer cinq demandes au service web Machine Learning en une seconde. Dans le cas de 20 connexions, le travail Stream Analytics peut traiter 20 000 événements en 200 ms, et donc 100 000 événements en une seconde. Par conséquent, pour traiter 200 000 événements par seconde, le travail Stream Analytics a besoin de 40 connexions simultanées, ce qui correspond à 12 unités de diffusion en continu. Le diagramme ci-dessous illustre les demandes émanant du travail Stream Analytics vers le point de terminaison de service web Machine Learning. Pour chaque ensemble de 6 unités de diffusion en continu, il existe jusqu’à 20 connexions simultanées au service web Machine Learning.

![Mettre à l’échelle Stream Analytics avec des fonctions Machine Learning - Exemple de deux travaux](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Mettre à l’échelle Stream Analytics avec des fonctions Machine Learning - Exemple de deux travaux")

Généralement, pour la taille de lot ***B*** et la latence de service web en millisecondes ***L*** pour la taille de lot B, le débit d’un travail Stream Analytics avec ***N*** unités de diffusion en continu est le suivant :

![Mettre à l’échelle Stream Analytics avec des fonctions Machine Learning - Formule](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Mettre à l’échelle Stream Analytics avec des fonctions Machine Learning - Formule")

Un autre aspect éventuel à prendre en compte est le nombre maximal d’appels simultanés du côté du service web Machine Learning ; il est recommandé de définir ce paramètre sur sa valeur maximale (200 à l’heure actuelle).

Pour plus d’informations sur ce paramètre, voir [l’article sur la mise à l’échelle pour les services web Machine Learning](../machine-learning/studio/scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Exemple – Analyse de sentiments
L’exemple suivant inclut un travail Stream Analytics avec la fonction Machine Learning d’analyse de sentiments, comme décrit dans le [didacticiel d’intégration de Machine Learning à Stream Analytics](stream-analytics-machine-learning-integration-tutorial.md).

La requête est une requête simple entièrement partitionnée suivie de la fonction **sentiment**, comme indiqué dans l’exemple suivant :

```SQL
    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery
```
Considérons le scénario suivant ; avec un débit de 10 000 tweets par seconde, un travail Stream Analytics doit être créé pour effectuer une analyse de sentiments relative aux tweets (événements). Avec 1 unité de diffusion en continu, ce travail Stream Analytics serait-il en mesure de gérer le trafic ? L’utilisation de la taille de lot par défaut de 1 000 devrait permettre de prendre en charge le trafic d’entrée. En outre, la fonction Machine Learning ajoutée ne devrait pas générer plus d’une seconde de latence, ce qui constitue la latence générale par défaut du service web Machine Learning d’analyse de sentiments (avec une taille de lot par défaut de 1 000). La latence **totale** ou de bout en bout du travail Stream Analytics serait généralement de l’ordre de quelques secondes. Examinons ce travail Stream Analytics plus en détail, *en particulier* les appels de fonction Machine Learning. La taille de lot étant de 1000, un débit de 10 000 événements nécessitera l’envoi de 10 demandes environ au service web. Même avec 1 unité de streaming, le nombre de connexions simultanées est suffisant pour prendre en charge ce trafic d’entrée.

Si le taux d’événements d’entrée est multiplié par 100, le travail Stream Analytics devra alors traiter 1 000 000 de tweets par seconde. Deux options permettent de gérer cette augmentation :

1. augmenter la taille de lot ;
2. partitionner le flux d’entrée pour traiter les événements en parallèle.

Si nous choisissons la première option, la **latence** du travail augmente.

La seconde option nécessitera l’approvisionnement d’un plus grand nombre d’unités de diffusion en continu, et générera donc davantage de demandes de service web Machine Learning simultanées. Cela se traduira par une augmentation du **coût** du travail.

Supposons que la latence du service web Machine Learning d’analyse de sentiments soit inférieure ou égale à 200 ms pour des lots de 1000 événements, égale à 250 ms pour des lots de 5000 événements, égale à 300 ms pour des lots de 10 000 événements ou égale à 500 ms pour des lots de 25 000 événements.

1. Utilisation de la première option (**ne pas** provisionner plus d’unités de stockage). Vous pourriez augmenter la taille du lot pour atteindre **25 000**. Cela permettrait alors au travail de traiter 1 000 000 d’événements avec 20 connexions simultanées au service web Machine Learning (avec une latence de 500 ms par appel). La latence supplémentaire du travail Stream Analytics découlant des demandes de la fonction sentiment par rapport aux demandes de service web Machine Learning passerait donc de **200 ms** à **500 ms**. Toutefois, la taille de lot **ne peut pas** être augmentée à l’infini, car les services web Machine Learning exigent que la taille utile d’une demande soit de 4 Mo et imposent un délai d’expiration de 100 secondes pour les demandes de service web de taille moindre.
2. Si nous utilisions la deuxième option, en conservant une taille de lot de 1000, avec une latence de service web de 200 ms, chaque ensemble de 20 connexions simultanées au service web est en mesure de traiter 20 * 1000 * 5 événements = 100 000 événements par seconde. Pour traiter 1 000 000 d’événements par seconde, le travail aurait donc besoin de 60 unités de diffusion en continu. Par rapport à la première option, le travail Stream Analytics effectuerait un plus grand nombre de demandes de service web par lot, ce qui entraînerait alors une augmentation des coûts.

Le tableau ci-après présente le débit du travail Stream Analytics pour différentes unités de diffusion en continu et tailles de lot (en nombre d’événements par seconde).

| Taille de lot (latence Machine Learning) | 500 (200 ms) | 1 000 (200 ms) | 5 000 (250 ms) | 10 000 (300 ms) | 25 000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 unité de diffusion en continu** |2 500 |5 000 |20 000 |30 000 |50 000 |
| **3 unités de diffusion en continu** |2 500 |5 000 |20 000 |30 000 |50 000 |
| **6 unités de diffusion en continu** |2 500 |5 000 |20 000 |30 000 |50 000 |
| **12 unités de diffusion en continu** |5 000 |10 000 |40 000 |60 000 |100 000 |
| **18 unités de diffusion en continu** |7 500 |15 000 |60 000 |90 000 |150 000 |
| **24 unités de diffusion en continu** |10 000 |20 000 |80 000 |120 000 |200 000 |
| **…** |… |… |… |… |… |
| **60 unités de diffusion en continu** |25 000 |50 000 |200 000 |300 000 |500 000 |

À ce stade, vous devriez déjà avoir une bonne compréhension du mode de fonctionnement des fonctions Machine Learning dans Stream Analytics. Vous devez également avoir compris que les travaux Stream Analytics tirent (pull) les données des sources de données, et que chaque tirage retourne un lot d’événements destinés à être traités par le travail Stream Analytics. Comment ce modèle de transmission de type pull affecte-t-il les demandes de service web Machine Learning ?

En règle générale, la taille de lot que nous définissons pour les fonctions Machine Learning n’est pas exactement divisible par le nombre d’événements retournés lors de chaque tirage (pull) de travail Stream Analytics. Si cela se produit, le service web Machine Learning est dit avec des lots « partiels ». Cette approche est destinée à éviter un surcroît de latence de travail induit par le regroupement des événements issus des différentes transmissions de type pull.

## <a name="new-function-related-monitoring-metrics"></a>Nouvelles métriques de surveillance associées aux fonctions
Dans la zone de surveillance d’un travail Stream Analytics, trois nouvelles métriques associées aux fonctions ont été ajoutées. Il s’agit des métriques DEMANDES DE FONCTION, ÉVÉNEMENTS DE FONCTION et ÉCHEC DES DEMANDES DE FONCTION, comme illustré dans le graphique ci-après.

![Mettre à l’échelle Stream Analytics avec des fonctions Machine Learning - Métriques](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Mettre à l’échelle Stream Analytics avec des fonctions Machine Learning - Métriques")

Les définitions de ces métriques sont les suivantes :

**DEMANDES DE FONCTION** : Nombre de demandes de fonction.

**ÉVÉNEMENTS DE FONCTION** : Nombre d’événements dans les demandes de fonction.

**DEMANDES DE FONCTION AYANT ÉCHOUÉ** : Nombre de demandes de fonction ayant échoué.

## <a name="key-takeaways"></a>Points clés
Pour récapituler les points principaux, la mise à l’échelle d’un travail Stream Analytics avec des fonctions Machine Learning nécessite la prise en compte des éléments suivants :

1. taux d’événements d’entrée ;
2. latence tolérée pour le travail Stream Analytics en cours d’exécution (et donc taille de lot des demandes de service web Machine Learning) ;
3. unités de diffusion en continu Stream Analytics approvisionnées et nombre de demandes de service web Machine Learning (coûts associés aux fonctions supplémentaires).

Notre exemple portait sur une requête Stream Analytics entièrement partitionnée. Si vous avez besoin d’une requête plus complexe, le [Forum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) est une précieuse ressource pour obtenir une aide supplémentaire auprès de l’équipe Stream Analytics.

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur Stream Analytics, consultez :

* [Prise en main d'Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Mise à l’échelle des travaux Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Références sur le langage des requêtes d'Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Références sur l’API REST de gestion d’Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
