---
title: Approvisionner le débit d’une base de données dans Azure Cosmos DB
description: Apprenez à approvisionner le débit au niveau de la base de données dans Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: rimman
ms.openlocfilehash: d73dd5ffe8cc8ed00288209b628d7175b795b335
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243772"
---
# <a name="provision-throughput-on-a-database-in-azure-cosmos-db"></a>Provisionner le débit sur une base de données dans Azure Cosmos DB

Cet article explique comment provisionner le débit sur une base de données dans Azure Cosmos DB. Vous pouvez approvisionner le débit d’un seul [conteneur](how-to-provision-container-throughput.md) ou d’une base de données et le partager entre les conteneurs au sein de la base de données. Pour savoir quand utiliser le débit au niveau du conteneur et au niveau de la base de données, consultez l’article [Cas d’usage du provisionnement du débit des conteneurs et des bases de données](set-throughput.md). Vous pouvez provisionner le débit au niveau d’une base de données via le portail Azure ou à l’aide des kits SDK Azure Cosmos DB.

## <a name="provision-throughput-using-azure-portal"></a>Approvisionner le débit à l’aide du portail Azure

### <a id="portal-sql"></a>API (Core) SQL

1. Connectez-vous au [Portail Azure](https://portal.azure.com/).

1. [Créez un compte Azure Cosmos](create-sql-api-dotnet.md#create-account) ou sélectionnez un compte Azure Cosmos existant.

1. Ouvrez le volet **Explorateur de données**, puis sélectionnez **Nouvelle base de données**. Fournissez les détails suivants :

   * Entrez un ID de base de données. 
   * Sélectionnez **Provisionner le débit**.
   * Entrez un débit, par exemple 1 000 RU.
   * Sélectionnez **OK**.

![Capture d’écran de la boîte de dialogue Nouvelle base de données](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-net-sdk"></a>Approvisionner le débit à l’aide du Kit de développement logiciel (SDK) .NET

> [!Note]
> Vous pouvez utiliser les SDK Cosmos pour l’API SQL afin de provisionner le débit de toutes les API. Vous pouvez éventuellement utiliser l’exemple suivant pour l’API Cassandra.

### <a id="dotnet-all"></a>Toutes les API

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>API Cassandra

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Étapes suivantes

Consultez les articles suivants pour en savoir plus sur le débit provisionné dans Azure Cosmos DB :

* [Mettre à l’échelle le débit provisionné au niveau global](scaling-throughput.md)
* [Provisionner le débit sur les conteneurs et les bases de données](set-throughput.md)
* [Approvisionner le débit d’un conteneur](how-to-provision-container-throughput.md)
* [Unités de requête et débit dans Azure Cosmos DB](request-units.md)