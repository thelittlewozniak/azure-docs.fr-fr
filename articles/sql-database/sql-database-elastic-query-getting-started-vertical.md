---
title: Prise en main des requêtes de bases de données croisées (partitionnement vertical) | Microsoft Docs
description: comment utiliser une requête de base de données élastique avec des bases de données partitionnées verticalement
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 116a465a0ddc913e342e0ffcc1fb29f5bf969419
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60307442"
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Prise en main des requêtes de bases de données croisées (partitionnement vertical) (version préliminaire)

Les requêtes de bases de données élastiques (en préversion) pour Azure SQL Database vous permettent d’exécuter des requêtes T-SQL qui s’étendent sur plusieurs bases de données via un seul point de connexion. Cet article s’applique aux [bases de données partitionnées verticalement](sql-database-elastic-query-vertical-partitioning.md).  

À l’issue du processus, vous serez capable de configurer et d’utiliser une base de données Azure SQL pour exécuter des requêtes qui s’étendent sur plusieurs bases de données associées.

Pour plus d’informations sur la fonctionnalité de requête de base de données élastique, consultez l’article [Vue d’ensemble de la requête de base de données élastique Azure SQL Database](sql-database-elastic-query-overview.md).

## <a name="prerequisites"></a>Prérequis

L’autorisation ALTER ANY EXTERNAL DATA SOURCE est requise. Cette autorisation est incluse dans l’autorisation ALTER DATABASE. Les autorisations ALTER ANY EXTERNAL DATA SOURCE sont nécessaires pour faire référence à la source de données sous-jacente.

## <a name="create-the-sample-databases"></a>Créer les exemples de bases de données

Pour commencer, créez deux bases de données, **Customers** et **Orders**, dans les mêmes serveurs SQL Database ou non.

Exécutez les requêtes suivantes sur la base de données **Orders** pour créer la table **OrderInformation** et ajouter les exemples de données.

    CREATE TABLE [dbo].[OrderInformation](
        [OrderID] [int] NOT NULL,
        [CustomerID] [int] NOT NULL
        )
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)

Exécutez maintenant la requête suivante sur la base de données **Customers** pour créer la table **CustomerInformation** et ajouter les exemples de données.

    CREATE TABLE [dbo].[CustomerInformation](
        [CustomerID] [int] NOT NULL,
        [CustomerName] [varchar](50) NULL,
        [Company] [varchar](50) NULL
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC)
    )
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC')
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ')
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO')

## <a name="create-database-objects"></a>Créez des objets de base de données

### <a name="database-scoped-master-key-and-credentials"></a>Clé principale et informations d’identification de la base de données

1. Ouvrez SQL Server Management Studio ou SQL Server Data Tools dans Visual Studio.
2. Connectez-vous à la base de données Orders et exécutez les commandes T-SQL suivantes :

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master_key_password>';
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';  

    Les valeurs « username » et « password » doivent être le nom d’utilisateur et le mot de passe de connexion à la base de données Customers.
    L’authentification à l’aide d’Azure Active Directory avec des requêtes élastiques n’est pas prise en charge actuellement.

### <a name="external-data-sources"></a>Sources de données externes

Pour créer une source de données externe, exécutez la commande suivante sur la base de données Orders :

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
        (TYPE = RDBMS,
        LOCATION = '<server_name>.database.windows.net',
        DATABASE_NAME = 'Customers',
        CREDENTIAL = ElasticDBQueryCred,
    ) ;

### <a name="external-tables"></a>Tables externes

Créez une table externe sur la base de données Orders qui corresponde à la définition de la table CustomerInformation :

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
    ( [CustomerID] [int] NOT NULL,
      [CustomerName] [varchar](50) NOT NULL,
      [Company] [varchar](50) NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc)

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Exécutez un exemple de requête T-SQL de base de données élastique

Après avoir défini vos tables externes et votre source de données externe, vous pouvez utiliser T-SQL pour exécuter des requêtes sur vos tables externes. Exécutez cette requête sur la base de données Orders :

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company
    FROM OrderInformation
    INNER JOIN CustomerInformation
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID

## <a name="cost"></a>Coût

Actuellement, la fonctionnalité de requête de base de données élastique est incluse dans le coût de votre base de données Azure SQL.  

Pour plus d’informations sur la tarification, voir [Tarification SQL Database](https://azure.microsoft.com/pricing/details/sql-database).

## <a name="next-steps"></a>Étapes suivantes

* Pour une présentation des requêtes élastiques, consultez [Présentation des requêtes élastiques](sql-database-elastic-query-overview.md).
* Pour la syntaxe et des exemples de requêtes pour des données partitionnées verticalement, consultez [Requêtes sur des données partitionnées verticalement](sql-database-elastic-query-vertical-partitioning.md)
* Pour commencer le didacticiel sur le partitionnement horizontal (sharding), consultez [Prise en main des requêtes élastiques pour le partitionnement horizontal (sharding)](sql-database-elastic-query-getting-started.md).
* Pour la syntaxe et des exemples de requêtes pour des données partitionnées horizontalement, consultez [Requêtes sur des données partitionnées horizontalement](sql-database-elastic-query-horizontal-partitioning.md)
* Consultez [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) pour une procédure stockée qui exécute une instruction Transact-SQL sur une seule base de données Azure SQL distante ou un ensemble de bases de données servant de partitions dans un schéma de partitionnement horizontal.
