---
title: Script Azure PowerShell - Créer un compte d’API Cassandra Azure Cosmos DB
description: Exemple de script Azure PowerShell - Créer un compte d’API Cassandra Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.devlang: PowerShell
ms.topic: sample
ms.date: 05/29/2019
ms.reviewer: sngun
ms.openlocfilehash: b3abe4e51f75996db88dcebb5e98baf5b5fc08dd
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479295"
---
# <a name="azure-cosmos-db-create-a-cassandra-api-account-using-powershell"></a>Azure Cosmos DB : Créer un compte d’API Cassandra avec PowerShell

Cet exemple de script PowerShell crée un compte d’API Azure Cosmos DB Cassandra. 

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Exemple de script

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/cosmosdb/create-and-configure-cassandra-database/create-and-configure-cassandra-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Create an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a>Nettoyer le déploiement

Une fois l’exemple de script exécuté, la commande suivante permet de supprimer le groupe de ressources et toutes les ressources associées.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Crée un serveur logique qui héberge une base de données ou un pool élastique. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Supprime un groupe de ressources, y compris toutes les ressources imbriquées. |
|||

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur Azure PowerShell, consultez la [documentation Azure PowerShell](https://docs.microsoft.com/powershell/).

Vous trouverez des exemples supplémentaires de scripts Azure Cosmos DB PowerShell sur la page [Scripts PowerShell Azure Cosmos DB](../../powershell-samples.md).
