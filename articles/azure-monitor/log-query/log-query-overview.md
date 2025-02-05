---
title: Analyser les données de journal dans Azure Monitor | Microsoft Docs
description: Pour récupérer des données de journal à partir d'Azure Monitor, vous devez exécuter une requête de journal.  Cet article explique comment les nouvelles requêtes de journal sont utilisées dans Azure Monitor et présente les concepts avec lesquels vous devez vous familiariser avant de créer une requête.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: bwren
ms.openlocfilehash: 1cb3946a93cbeff6a9b95e0a21edbf0523b53d5e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65203614"
---
# <a name="analyze-log-data-in-azure-monitor"></a>Analyser les données de journal dans Azure Monitor

Les données de journal collectées par Azure Monitor sont stockées dans un espace de travail Log Analytics, qui est basé sur [Azure Data Explorer](/azure/data-explorer). La fonctionnalité collecte des données de télémétrie à partir de diverses sources et exploite le [langage de requête de Kusto](/azure/kusto/query) utilisé par Data Explorer pour récupérer et analyser les données.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="log-queries"></a>Requêtes dans les journaux

Pour récupérer des données de journal à partir d'Azure Monitor, vous devez exécuter une requête de journal.  Que vous [analysiez des données sur le portail](portals.md), [configuriez une règle d’alerte](../platform/alerts-metric.md) pour être averti d’une condition particulière ou récupériez des données à l’aide de l’[API Journaux d’activité Azure Monitor](https://dev.loganalytics.io/), vous devrez utiliser une requête afin de spécifier les données souhaitées.  Cet article explique comment les requêtes de journal sont utilisées dans Azure Monitor et présente les concepts avec lesquels vous devez vous familiariser avant de créer une requête.

## <a name="where-log-queries-are-used"></a>Lorsque les requêtes dans les journaux sont utilisées

Voici plusieurs façons d’utiliser les requêtes dans Azure Monitor :

- **Portail.** Vous pouvez effectuer une analyse interactive des données de journal dans le [portail Azure](portals.md).  Cela vous permet de modifier votre requête et d’analyser les résultats dans divers formats et visualisations.  
- **Règles d’alerte.** Les [règles d’alerte](../platform/alerts-overview.md) identifient de façon proactive les problèmes à partir des données dans votre espace de travail.  Chaque règle d’alerte est basée sur une recherche dans les journaux qui est exécutée automatiquement à intervalles réguliers.  Les résultats sont inspectés pour déterminer si une alerte doit être créée.
- **Tableaux de bord.** Vous pouvez épingler les résultats de n’importe quelle requête dans un [tableau de bord Azure](../learn/tutorial-logs-dashboards.md) afin de visualiser les données de journal et les métriques ensemble et de partager ces informations avec d’autres utilisateurs Azure si vous le souhaitez. 
- **Vues.**  Vous pouvez créer des visualisations de données à inclure dans les tableaux de bord utilisateur avec le [Concepteur de vues](../platform/view-designer.md).  Les requêtes dans les journaux fournissent les données utilisées par les [vignettes](../platform/view-designer-tiles.md) et les [composants de visualisation](../platform/view-designer-parts.md) dans chaque vue.  

- **Exportation.**  Lorsque vous importez des données de journal d'Azure Monitor vers Excel ou [Power BI](../platform/powerbi.md), vous créez une requête de journal pour définir les données à exporter.
- **PowerShell.** Vous pouvez exécuter un script PowerShell à partir d’une ligne de commande ou d’un runbook Azure Automation qui utilise [Get-AzOperationalInsightsSearchResults](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult) pour récupérer des données de journal à partir d’Azure Monitor.  Cette applet de commande nécessite une requête pour déterminer les données à récupérer.
- **API Journaux d’activité Azure Monitor.**  L’[API Journaux d’activité Azure Monitor](../platform/alerts-overview.md) permet à tout client d’API REST de récupérer des données de journal d’activité à partir de l’espace de travail.  La demande API comprend une requête qui est exécutée sur Azure Monitor pour déterminer les données à récupérer.

![Recherches dans les journaux](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Écrivez votre requête.
Azure Monitor utilise [une version du langage de requête de Kusto](get-started-queries.md) pour récupérer et analyser les données de journal de différentes façons.  Vous commencerez généralement par des requêtes de base, puis vous passerez à des fonctions plus avancées à mesure que vos exigences deviendront plus complexes.

La structure de base d’une requête est une table source suivie d’une série d’opérateurs séparés par une barre verticale `|`.  Vous pouvez chaîner plusieurs opérateurs pour affiner les données et effectuer des fonctions avancées.

Par exemple, supposez que vous souhaitez rechercher les dix ordinateurs ayant eu le plus d’événements d’erreur durant les dernières 24 heures,

```Kusto
Event
| where (EventLevelName == "Error")
| where (TimeGenerated > ago(1days))
| summarize ErrorCount = count() by Computer
| top 10 by ErrorCount desc
```

ou que vous souhaitez rechercher les ordinateurs qui n’ont pas eu de pulsation durant les dernières 24 heures.

```Kusto
Heartbeat
| where TimeGenerated > ago(7d)
| summarize max(TimeGenerated) by Computer
| where max_TimeGenerated < ago(1d)  
```

Et si vous génériez un graphique en courbes avec l’utilisation du processeur pour chaque ordinateur durant la semaine dernière ?

```Kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
| render timechart    
```

Grâce à ces exemples rapides, vous pouvez constater que, quel que soit le type de données avec lequel vous travaillez, la structure de la requête est similaire.  Vous pouvez la décomposer en étapes distinctes où les données obtenues à partir d’une commande sont envoyées à la commande suivante par l’intermédiaire du pipeline.

Vous pouvez également interroger les données dans les espaces de travail Log Analytics au sein de votre abonnement.

```Kusto
union Update, workspace("contoso-workspace").Update
| where TimeGenerated >= ago(1h)
| summarize dcount(Computer) by Classification 
```

## <a name="how-azure-monitor-log-data-is-organized"></a>Organisation des données de journal Azure Monitor
Quand vous générez une requête, commencez par déterminer les tables où figurent les données que vous recherchez. Les différents types de données sont séparés en tables dédiées dans chaque [espace de travail Log Analytics](../learn/quick-create-workspace.md).  La documentation des diverses sources de données inclut le nom du type de données créé et une description de chacune de ses propriétés.  De nombreuses requêtes nécessitent les données d’une seule table, mais d’autres peuvent utiliser diverses options pour inclure des données provenant de plusieurs tables.

Bien qu’[Application Insights](../app/app-insights-overview.md) stocke les données d’application telles que les requêtes, les exceptions, les traces et les données d’utilisation dans les journaux d’activité Azure Monitor, ces données sont stockées dans une partition différente de celle des autres données de journal d’activité. Le même langage de requête vous permet d’accéder à ces données, mais vous devez utiliser la [console Application Insights](../app/analytics.md) ou l’[API REST Application Insights](https://dev.applicationinsights.io/) pour y accéder. Vous pouvez utiliser des [requêtes interressources](../log-query/cross-workspace-query.md) pour combiner des données Application Insights avec d'autres données de journal dans Azure Monitor.


![Tables](media/log-query-overview/queries-tables.png)




## <a name="next-steps"></a>Étapes suivantes
- Apprenez à utiliser [l’analytique des journaux d’activité pour créer et modifier des recherches dans les journaux](../log-query/portals.md).
- Consultez un [didacticiel sur l’écriture de requêtes](../log-query/get-started-queries.md) à l’aide du nouveau langage de requête.
