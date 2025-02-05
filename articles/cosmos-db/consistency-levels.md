---
title: Niveaux de cohérence dans Azure Cosmos DB
description: Azure Cosmos DB offre cinq niveaux de cohérence qui permettent de faire des compromis avisés entre cohérence éventuelle, disponibilité et latence.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: dcd51756a9c5a5a24a082862bb911cc2d2605d61
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954362"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Niveaux de cohérence dans Azure Cosmos DB

Les bases de données distribuées qui reposent sur la réplication afin d’offrir une haute disponibilité, une faible latence ou les deux, constituent le compromis fondamental entre la cohérence de la lecture et la disponibilité, la latence et le débit. La plupart des bases de données distribuées commercialisés demandent aux développeurs de choisir entre les deux modèles de cohérence extrême : *fort* cohérence et *éventuelle* cohérence. Le  [linéarisabilité](https://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) ou le modèle de cohérence forte est la référence en matière de programmabilité des données. Mais il ajoute un prix d’une latence plus élevée (dans un état stable) et réduction de la disponibilité (lors de pannes). En revanche, la cohérence éventuelle offre une plus haute disponibilité et meilleures performances, mais rend difficile à programmer des applications. 

Azure Cosmos DB aborde la cohérence des données sous forme d’un éventail de choix, plutôt que de proposer deux extrêmes. Cohérence forte et la cohérence éventuelle sont aux extrémités du spectre, mais il existe de nombreux choix de cohérence dans le spectre. Les développeurs peuvent utiliser ces options pour rendre les choix précis et granulaires compromis en ce qui concerne la haute disponibilité et performances. 

Avec Azure Cosmos DB, les développeurs peuvent faire leur choix parmi cinq modèles de cohérence bien définis sur le spectre de la cohérence. De plus forte à la plus souple, les modèles incluent *fort*, *obsolescence*, *session*, *préfixe cohérent*et *éventuelle* cohérence. Les modèles sont bien définis et intuitives et peuvent être utilisées pour des scénarios réels spécifiques. Chaque modèle fournit [compromis entre disponibilité et performances](consistency-levels-tradeoffs.md) et repose sur les contrats SLA. L’illustration suivante montre les niveaux de cohérence différents comme un spectre.

![La cohérence en tant que spectre](./media/consistency-levels/five-consistency-levels.png)

Les niveaux de cohérence sont indépendants de la région et sont garanties pour toutes les opérations, quel que soit la région à partir de laquelle les lectures et écritures sont fournies, le nombre de régions associées à votre compte Azure Cosmos, ou si votre compte est configuré avec un seul ou plusieurs régions d’écriture.

## <a name="scope-of-the-read-consistency"></a>Étendue de la cohérence de lecture

La cohérence de lecture s’applique à une seule opération de lecture dans la plage de clé de partition ou partition logique. L’opération de lecture peut être émise par un client distant ou une procédure stockée.

## <a name="configure-the-default-consistency-level"></a>Configurer le niveau de cohérence par défaut

Vous pouvez configurer le niveau de cohérence par défaut sur votre compte Azure Cosmos DB à tout moment. Le niveau de cohérence par défaut configuré sur votre compte s’applique à toutes les bases de données Azure Cosmos et conteneurs sous ce compte. Toutes les lectures et requêtes émises vers un conteneur ou une base de données utilisent le niveau de cohérence par défaut spécifié. Pour en savoir plus, consultez [Configurer le niveau de cohérence par défaut](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garanties associées aux niveaux de cohérence

Les contrats de niveau de service complets fournis par Azure Cosmos DB garantissent que 100 % des requêtes de lecture respecteront la garantie de cohérence dans le cadre du niveau de cohérence choisi. Une requête de lecture respecte le contrat de niveau de service de cohérence si toutes les garanties de cohérence associées au niveau de cohérence sont satisfaites. Les définitions précises des cinq niveaux de cohérence dans Azure Cosmos DB à l’aide du [langage de spécification TLA +](https://lamport.azurewebsites.net/tla/tla.html) sont fournies dans le [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla) référentiel GitHub. 

La sémantique des cinq niveaux de cohérence est décrite ici :

- **Remarque**: une cohérence forte offre une garantie de [linéarisabilité](https://aphyr.com/posts/313-strong-consistency-models). Garantit que les lectures retournent la version validée la plus récente d’un élément. Un client ne voit jamais une écriture partielle ou non validée. Les utilisateurs sont toujours assurés de lire la toute dernière écriture validée.

- **Obsolescence limitée**: les lectures honoreront la garantie de préfixe cohérent. Les lectures peuvent rester derrière les écritures par, au maximum *« K »* versions (par exemple, « mises à jour ») d’un élément ou par *« T »* intervalle de temps. En d’autres termes, lorsque vous choisissez d’obsolescence limitée, le « obsolescence » peut être configurée de deux manières : 

  * Le nombre de versions (*K*) de l’élément
  * L’intervalle de temps (*T*) par lequel les lectures peuvent rester derrière les écritures 

  La cohérence de type obsolescence limitée fournit l’ordre global total, en dehors de la « fenêtre d’obsolescence ». Notez que des garanties de lecture unitone existent dans une région à l’intérieur et en dehors de la fenêtre d’obsolescence. Cohérence forte a la même sémantique que celui proposé par obsolescence. La fenêtre d’obsolescence est égale à zéro. L’obsolescence limitée est également appelée linéarisabilité retardée. Lorsqu’un client effectue des opérations de lecture dans une région qui accepte les écritures, les garanties fournies par une cohérence obsolescence limitée sont identiques à ces garanties par la cohérence forte.

- **Session**: les lectures honoreront les garanties de préfixe cohérent (en partant du principe qu’il s’agit d’une session à « writer » unique), de lectures unitones, d’écritures unitones, de lecture de vos écritures et d’écriture suivant les lectures. La cohérence de session est limitée à une session client.

- **Préfixe cohérent** : les mises à jour retournées contiennent un préfixe de toutes les mises à jour, sans interruption. Niveau de cohérence préfixe cohérent garantit que les lectures ne voient jamais les écritures de désordre.

- **Eventual (Éventuel)**: Il n’existe aucune garantie de classement pour les lectures. En l’absence d’autres écritures, les réplicas finissent par converger.

## <a name="consistency-levels-explained-through-baseball"></a>Niveaux de cohérence – Exemple du baseball

Prenons comme exemple un scénario de match de base-ball. Imaginez une séquence d’écritures représentant le score d’un match de base-ball. Le score par manche est décrit dans le document [Replicated data consistency through baseball](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) (Cohérence des données répliquées - Exemple du baseball). Ce match de baseball hypothétique est actuellement au milieu de sa septième manche. Il s’agit de la pause de la septième manche. Les visiteurs sont trouvent derrière avec un score de 2 à 5 comme indiqué ci-dessous :

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Points** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Visiteurs** | 0 | 0 | 1 | 0 | 1 | 0 | 0 |  |  | 2 |
| **Locaux** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5. |

Un conteneur Azure Cosmos totaux l’exécution pour les visiteurs et les équipes de base. Lorsque le match est en cours, il se peut que les clients lisent des scores différents à cause des différentes garanties de lecture. Le tableau suivant répertorie l’ensemble complet des scores qui pourraient être renvoyés par la lecture des scores des visiteurs et des locaux, en fonction des cinq garanties de cohérence. Le score des visiteurs est répertorié en premier. Les différentes valeurs de retour possibles sont séparées par des virgules.

| **Niveau de cohérence** | **Scores (visiteurs, accueil)** |
| - | - |
| **Fort** | 2-5 |
| **Obsolescence limitée** | Scores obsolètes (d’une manche tout au plus) : 2-3, 2-4, 2-5 |
| **Session** | <ul><li>Pour l’auteur : 2-5</li><li> Pour toute personne autre que l’auteur : 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5</li><li>Après lecture de 1-3 : 1-3, 1-4, 1-5, 2-3, 2-4, 2-5</li> |
| **Préfixe cohérent** | 0-0, 0-1, 1-1, 1-2, 1-3, 2-3, 2-4, 2-5 |
| **Éventuel** | 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5 |

## <a name="additional-reading"></a>Documentation supplémentaire

Pour en savoir plus sur les concepts de cohérence, lisez les articles suivants :

- [Spécifications TLA + de haut niveau pour les cinq niveaux de cohérence proposés par Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla)
- [La cohérence des données répliquées expliquée par le base-ball (vidéo) par Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I)
- [La cohérence des données répliquées expliquée par le base-ball (livre blanc) par Doug Terry](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Garanties de session pour des données répliquées peu cohérentes](https://dl.acm.org/citation.cfm?id=383631)
- [Compromis de cohérence en termes de conception de systèmes de base de données distribuée modernes : CAP n’est qu’une partie de l’histoire](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf) (Probabilités en fonction de l’obsolescence limitée (PBS) pour les quorums partiels pratiques)
- [Niveau de cohérence éventuel repensé](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les niveaux de cohérence dans Azure Cosmos DB, lisez les articles suivants :

* [Choisir le bon niveau de cohérence pour votre application](consistency-levels-choosing.md)
* [Niveaux de cohérence pour les API Azure Cosmos DB](consistency-levels-across-apis.md)
* [Compromis entre disponibilité et performance pour différents niveaux de cohérence](consistency-levels-tradeoffs.md)
* [Configurer le niveau de cohérence par défaut](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Remplacer le niveau de cohérence par défaut](how-to-manage-consistency.md#override-the-default-consistency-level)

