---
title: Présentation de l’API Gremlin Azure Cosmos DB
description: Découvrez comment utiliser Azure Cosmos DB pour stocker, interroger et parcourir des graphes volumineux avec une faible latence grâce au langage de requête de graphe Gremlin d’Apache TinkerPop.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 05/20/2019
ms.author: lbosq
ms.openlocfilehash: 6f5d90f8b825b7076a1a5122dbef3c8b2990e216
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954263"
---
# <a name="introduction-to-azure-cosmos-db-gremlin-api"></a>Présentation d’Azure Cosmos DB : API Gremlin

[Azure Cosmos DB](introduction.md) est le service de base de données multimodèle globalement distribué de Microsoft pour les applications stratégiques. Il s'agit d'une base de données multimodèle qui prend en charge les modèles de données sous forme de documents, de valeurs de clés, de graphiques et de colonnes. L’API Azure Cosmos DB Gremlin permet de stocker, d’utiliser, de modéliser et de parcourir des données de graphes.

Cet article fournit une vue d’ensemble de l’API Gremlin Azure Cosmos DB, et explique comment l’utiliser pour stocker des graphiques volumineux comportant des milliards de sommets et de bords. Vous pouvez interroger les graphiques avec une latence de quelques millisecondes, et faire évoluer facilement leur structure et leur schéma. Pour interroger Azure Cosmos DB, vous pouvez utiliser le langage de parcours de graphe [Apache TinkerPop](https://tinkerpop.apache.org) ou [Gremlin](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps).

## <a name="what-is-a-graph-database"></a>Qu’est une base de données de graphes
Les données telles qu’elles apparaissent dans le monde réel sont naturellement connectées. La modélisation de données traditionnelle se concentre sur les entités. Pour bon nombre d’applications, il y a aussi un besoin de modélisation ou de modélisation des entités et des relations de façon naturelle.

Un [graphique](http://mathworld.wolfram.com/Graph.html) est une structure composée de [sommets](http://mathworld.wolfram.com/GraphVertex.html) et de [bords](http://mathworld.wolfram.com/GraphEdge.html). Les sommets et les bords peuvent avoir un nombre arbitraire de propriétés. 

* **Sommets** – Les sommets désignent des objets discrets, comme une personne, un lieu ou un événement. 

* **Arêtes** – Les arêtes désignent les relations entre les sommets. Par exemple, une personne peut en connaître une autre, être impliquée dans un événement et avoir récemment été dans un lieu. 

* **Properties** – Les propriétés expriment des informations sur les arêtes et les sommets, par exemple, le nom et l’âge d’un sommet ou la date, l’heure et le poids d’une arête. Plus formellement, ce modèle est appelé un [graphique de propriétés](https://tinkerpop.apache.org/docs/current/reference/#intro). Azure Cosmos DB prend en charge le modèle de graphique de propriétés.

Ainsi, l’exemple de graphe suivant met en évidence des relations entre des personnes, des appareils mobiles, des centres d’intérêt et des systèmes d’exploitation :

![Exemple de base de données montrant des personnes, des appareils et des centres d’intérêt](./media/graph-introduction/sample-graph.png)

Les bases de données de graphiques vous permettent de modéliser et de stocker des graphiques naturellement et efficacement, ce qui les rend utiles pour de nombreux scénarios. Les bases de données de graphiques sont généralement des bases de données NoSQL, car ces cas d’utilisation requièrent souvent aussi une flexibilité des schémas et une itération rapide.

Vous pouvez combiner les traversées rapides qu’offrent les bases de données de graphes avec des algorithmes de graphe tels que la recherche de profondeur, la recherche de largeur et l’algorithme de Dijkstra, pour résoudre des problèmes dans divers domaines tels que les réseaux sociaux, la gestion de contenu, les questions géospatiales et les recommandations.

## <a name="features-of-azure-cosmos-db-graph-database"></a>Fonctionnalités de la base de données de graphes Azure Cosmos DB
 
Azure Cosmos DB est une base de données de graphiques entièrement gérée, qui offre une distribution mondiale, une mise à l’échelle élastique du débit et du stockage, des fonctions d’indexation et d’interrogation automatiques, des niveaux de cohérence ajustables et la prise en charge de la norme TinkerPop.

![Architecture graphique Azure Cosmos DB](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB se démarque des autres bases de données de graphiques sur le marché en proposant les fonctionnalités suivantes :

* Stockage et débit extensibles de façon élastique

  Les graphiques dans le monde réel doivent pouvoir augmenter leur échelle au-delà de la capacité d’un serveur unique. Avec Azure Cosmos DB, vous pouvez mettre à l’échelle vos graphiques en toute transparence sur plusieurs serveurs. Vous pouvez également mettre à l’échelle le débit de votre graphique de manière indépendante, en fonction de vos modèles d’accès. Azure Cosmos DB prend en charge des bases de données de graphiques qui peuvent être mises à l’échelle pour atteindre des tailles de stockage quasi illimitées et un débit approvisionné.

* Réplication multirégion

  Azure Cosmos DB réplique en toute transparence vos données de graphique vers toutes les régions que vous avez associées votre compte. La réplication vous permet de développer des applications qui requièrent un accès global aux données. Il existe des compromis dans les domaines de la cohérence, de la disponibilité, des performances et des garanties correspondantes. Azure Cosmos DB fournit un basculement régional transparent avec des API d’hébergement multiple. Vous pouvez mettre à l’échelle de façon élastique le débit et le stockage dans le monde entier.

* Requêtes et parcours rapides avec une syntaxe Gremlin familière

  Stockez des sommets et des bords hétérogènes, et interrogez ces documents via une syntaxe Gremlin familière. Azure Cosmos DB utilise une technologie d’indexation parallèle, structurée par des journaux et sans verrouillage qui permet d’indexer automatiquement tout le contenu. Cette capacité autorise des requêtes et traversées enrichies en temps réel, sans qu’il soit nécessaire de spécifier des indicateurs de schéma, des index secondaires ou des vues. Pour en savoir plus, consultez [Interroger des graphes à l’aide de Gremlin](gremlin-support.md).

* Gestion intégrale

  Ne vous souciez plus de gérer les ressources de base de données et d’ordinateur. En tant que service Microsoft Azure entièrement géré, vous n’avez pas à gérer de machines virtuelles, à déployer et configurer des logiciels, à gérer la mise à l’échelle ni à vous préoccuper des mises à niveau complexes de la couche Données. Chaque graphique est automatiquement sauvegardé et protégé contre les défaillances régionales. Vous pouvez facilement ajouter un compte Azure Cosmos DB et approvisionner la capacité dont vous avez besoin afin de vous concentrer pleinement sur votre application plutôt que sur l’exploitation et la gestion de votre base de données.

* Indexation automatique

  Par défaut, Azure Cosmos DB indexe automatiquement toutes les propriétés des nœuds et des arêtes du graphe et n’attend ou ne nécessite aucun schéma ou création d’index secondaires.

* Compatibilité avec Apache TinkerPop

  Azure Cosmos DB prend nativement en charge la norme Open Source Apache TinkerPop, et peut être intégré à d’autres systèmes graphiques compatibles avec TinkerPop. Vous pouvez donc facilement migrer depuis une autre base de données de graphe, comme Titan ou Neo4j, ou utiliser Azure Cosmos DB avec des frameworks d’analytique de graphe, comme Apache Spark GraphX.

* Niveaux de cohérence ajustables

  Effectuez un choix parmi cinq niveaux de cohérence bien définis pour obtenir un équilibre optimal entre cohérence et performances. Pour les requêtes et les opérations de lecture, Azure Cosmos DB propose cinq niveaux de cohérence distincts : Fort, En fonction de l’obsolescence, Par session, Préfixe cohérent et Éventuel. Ces niveaux de cohérence bien définis et granulaires vous permettent de trouver un bon compromis entre cohérence, disponibilité et latence. Pour plus d’informations, consultez [Niveaux de cohérence des données paramétrables dans Azure Cosmos DB](consistency-levels.md).

Azure Cosmos DB peut également utiliser plusieurs modèles, tels que des documents et des graphiques, au sein des mêmes conteneurs/bases de données. Vous pouvez utiliser un conteneur de documents pour stocker des données de graphiques côte à côte avec des documents. Vous pouvez utiliser tant des requêtes SQL sur JSON que des requêtes Gremlin pour interroger les mêmes données en tant que graphique.

## <a name="get-started"></a>Prise en main

Pour créer des comptes d’API Gremlin Azure Cosmos DB, il est possible d’utiliser l’interface de ligne de commande (CLI) Azure, Azure PowerShell ou le Portail Azure. Une fois le compte créé, vous pouvez accéder à ses bases de données de graphes à l’aide d’un point de terminaison de service API Gremlin, `https://<youraccount>.gremlin.cosmosdb.azure.com`, qui fournit un serveur frontal WebSocket pour Gremlin. Vous pouvez configurer vos outils compatibles avec TinkerPop, tels que la [console Gremlin](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), pour vous connecter à ce point de terminaison et créer des applications en Java, Node.js, ou tout pilote de client Gremlin.

Le tableau suivant présente des pilotes Gremlin courants que vous pouvez utiliser sur Azure Cosmos DB :

| Téléchargement | Documentation | Mise en route | Version du connecteur prise en charge |
| --- | --- | --- | --- |
| [.NET](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-DotNet) | [Gremlin.NET sur GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-dotnet) | [Créer un graphe avec .NET](create-graph-dotnet.md) | 3.4.0-RC2 |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) | [Gremlin JavaDoc](https://tinkerpop.apache.org/javadocs/current/full/) | [Créer un graphe avec Java](create-graph-java.md) | 3.2.0+ |
| [Node.JS](https://www.npmjs.com/package/gremlin) | [Gremlin-JavaScript sur GitHub](https://github.com/jbmusso/gremlin-javascript) | [Créer un graphe avec Node.js](create-graph-nodejs.md) | 2.6.0|
| [Python](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-python) | [Gremlin-Python sur GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-python) | [Créer un graphe avec Python](create-graph-python.md) | 3.2.7 |
| [PHP](https://packagist.org/packages/brightzone/gremlin-php) | [Gremlin-PHP sur GitHub](https://github.com/PommeVerte/gremlin-php) | [Créer un graphe avec PHP](create-graph-php.md) | 3.1.0 |
| [Console Gremlin](https://tinkerpop.apache.org/downloads.html) | [Documents TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |  [Créer un graphe à l’aide de la console Gremlin](create-graph-gremlin-console.md) | 3.2.0 + |

## <a name="graph-database-design-considerations"></a>Considérations relatives à la conception de bases de données de graphes

Lors de la conception du graphe, le choix de modéliser une entité sous la forme d’un sommet à part entière, et non d’une propriété d’autres entités de sommet, a des conséquences sur les performances et les coûts. Le facteur déterminant de cette décision est la manière dont les données sont interrogées, ainsi que sur l’extensibilité du modèle lui-même.

Pour prévoir la modélisation de l’entité, posez-vous les questions suivantes :

* Quelles sont les entités à récupérer sous forme de sommets pour la plupart de mes requêtes ?

* Parmi les informations que j’intègre au graphe, lesquelles sont ajoutées à des fins de filtrage des données ?

* Quelles entités sont de simples connexions à d’autres entités, qui sont ensuite récupérées pour leur valeur ?

* Quelles sont les informations que ma requête doit récupérer ? À combien s’élèveront les frais de RU ainsi générés ?

Prenons par exemple le graphe suivant :

![Exemple de considérations relatives à la conception de graphes](./media/graph-introduction/graph-design-considerations-example.png)

* Selon les requêtes, il est possible que la relation District (« quartier ») -> Store (« magasin ») soit exclusivement utilisée pour filtrer les sommets Store, par exemple, si les requêtes sont sous la forme « Obtenir tous les magasins qui appartiennent à un certain quartier ». Il peut dans ce cas être intéressant de ramener l’entité District d’un sommet à part entière à une propriété du sommet Store. 

* Cette approche présente l’avantage de réduire les coûts : pour chaque sommet Store, il fallait récupérer trois objets de graphe à la fois (District, District -> Store, Store), contre un seul sommet Store maintenant. Les performances peuvent s’en trouver améliorées, de même que le coût par requête.

* Dans la mesure où le sommet Store est lié à deux entités différentes, Employee (« employé ») et Product (« produit »), il peut offrir des possibilités de parcours supplémentaires. C’est donc un sommet nécessaire.  



## <a name="scenarios-that-can-use-gremlin-api"></a>Scénarios susceptibles d’utiliser l’API Gremlin
Voici quelques scénarios où la prise en charge des graphiques par Azure Cosmos DB peut être utile :

* Réseaux sociaux

  En associant des données sur vos clients et leurs interactions avec d’autres personnes, vous pouvez développer des expériences personnalisées, prédire le comportement des clients ou connecter entre elles des personnes ayant les mêmes intérêts. Azure Cosmos DB peut servir à gérer des réseaux sociaux et à suivre les données et les préférences des clients.

* Moteurs de recommandation

  Ce scénario est couramment utilisé dans le secteur de la vente au détail. En associant des informations sur les produits, les utilisateurs et les interactions des utilisateurs (achats, navigation ou notation d’un article), vous pouvez générer des recommandations personnalisées. Azure Cosmos DB, avec sa faible latence, sa mise à l’échelle élastique et sa prise en charge native des graphiques, est idéal pour la modélisation de ces interactions.

* Questions géospatiales

  De nombreuses applications dans les secteurs des télécommunications, de la logistique et de la planification de voyages nécessitent de trouver un lieu intéressant dans une zone donnée, ou de rechercher l’itinéraire le plus court/optimal entre deux lieux. Azure Cosmos DB constitue une solution naturelle à ces problèmes.

* Internet des Objets

  Avec le réseau et les connexions entre les appareils IoT modélisés sous forme de graphe, vous pouvez obtenir un meilleur aperçu de l’état de vos appareils et ressources. Vous pouvez aussi découvrir comment les modifications apportées à une partie du réseau peuvent potentiellement en affecter une autre partie.

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur la prise en charge des graphiques dans Azure Cosmos DB, consultez :

* Prise en main avec le [tutoriel graphique Azure Cosmos DB](create-graph-dotnet.md).
* Découvrez comment [interroger les graphes dans Azure Cosmos DB à l’aide de Gremlin](gremlin-support.md).
