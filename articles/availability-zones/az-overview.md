---
title: Que sont les zones de disponibilité Azure ? | Microsoft Docs
description: Pour créer des applications hautement disponibles et résilientes dans Azure, les zones de disponibilité fournissent des emplacements physiquement distincts que vous pouvez utiliser pour exécuter vos ressources.
services: ''
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: eefb5f3ea10d72cdf355fc810147414fe1714d67
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417009"
---
# <a name="what-are-availability-zones-in-azure"></a>Que sont les zones de disponibilité dans Azure ?
Les Zones de disponibilité constituent une offre à haute disponibilité qui protège vos applications et données contre les pannes des centres de données. Les Zones de disponibilité sont des emplacements physiques uniques au sein d’une région Azure. Chaque zone de disponibilité est composée d’un ou de plusieurs centres de données équipés d’une alimentation, d’un système de refroidissement et d’un réseau indépendants. Pour garantir la résilience, il existe un minimum de trois zones distinctes dans toutes les régions activées. La séparation physique des Zones de disponibilité dans une région protège les applications et les données des défaillances dans le centre de données. Les services redondants interzone répliquent vos applications et données entre des Zones de disponibilité pour les protéger contre des points uniques de panne. Avec les Zones de disponibilité, Azure propose des contrats de niveau de service de durée de fonctionnement des machines virtuelles de pointe de 99,99 %. La version complète du [contrat SLA Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/) explique la disponibilité garantie d’Azure dans son ensemble.

Une zone de disponibilité dans une région Azure est une combinaison d’un domaine d’erreur et d’un domaine de mise à jour. Par exemple, si vous créez trois ou plusieurs machines virtuelles dans trois zones d’une région Azure, vos machines virtuelles sont efficacement réparties sur trois domaines d’erreur et trois domaines de mise à jour. La plateforme Azure reconnaît cette répartition entre les domaines de mise à jour pour vous assurer que les machines virtuelles des différentes zones ne sont pas mises à jour en même temps.

Générez la haute disponibilité dans votre architecture d’applications par la colocalisation de vos ressources de calcul, de stockage, de mise en réseau et de données dans une zone et une réplication dans d’autres zones. Les services Azure qui prennent en charge les Zones de disponibilité sont classés en deux catégories :

- **Services zonaux** : vous épinglez la ressource à une zone spécifique (par exemple, des machines virtuelles, des disques managés, des adresses IP) ou
- **Services redondants interzone** : la plateforme effectue automatiquement la réplication entre des zones (par exemple, stockage redondant interzone, SQL Database).

Pour obtenir la continuité complète des activités sur Azure, générez votre architecture d’applications à l’aide de la combinaison des Zones de disponibilité et des paires de régions Azure. Vous pouvez effectuer une réplication synchrone de vos applications et données à l’aide des Zones de la disponibilité d’une région Azure pour répliquer en haute disponibilité et de façon asynchrone entre les régions Azure pour la protection de la récupération d’urgence.
 
![affichage conceptuel d’une zone devenant indisponible dans une région](./media/az-overview/az-graphic-two.png)

## <a name="services-support-by-region"></a>Prise en charge des services par région

Les combinaisons des services Azure et les régions qui prennent en charge les Zones de disponibilité sont :


|                                 |Amérique |              |           |           | Europe |              |          |              | Asie-Pacifique |                 |
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|
|          |USA Centre|USA Est|USA Est 2|USA Ouest 2|France Centre|Europe Nord|Royaume-Uni Sud|Europe Ouest|Japon Est|Asie du Sud-Est|
| **Calcul**                         |            |              |           |           |                |              |          |             |            |                |
| Machines virtuelles Linux          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Machines virtuelles Windows        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Virtual Machine Scale Sets      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| **Stockage**   |            |              |           |           |                |              |          |             |            |                |
| Disques managés                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Stockage redondant          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| **Mise en réseau**                     |            |              |           |           |                |              |          |             |            |                |
| Adresse IP standard        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Standard Load Balancer     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Passerelle VPN                     | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| ExpressRoute                    | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| Application Gateway   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    | &#10003;       | &#10003;       |
| **Bases de données**                     |            |              |           |           |                |              |          |             |            |                |
| Base de données SQL                    | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |            | &#10003;       |
| Azure Cosmos DB                    |    |    |   |  |       |     | &#10003; |     |            | &#10003;       |
| **Analyse**                       |            |              |           |           |                |              |          |             |            |                |
| Event Hubs                      | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| **Intégration**                     |            |              |           |           |                |              |          |             |            |                |
| Service Bus (Niveau Premium uniquement) | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |



## <a name="services-resiliency"></a>Résilience des services
Tous les services de gestion Azure sont conçues pour être résilient à partir des défaillances au niveau de la région. Dans le spectre d’échecs, un ou plusieurs échecs de Zone de disponibilité au sein d’une région ont un plus petit rayon d’échec par rapport à un échec de la région entière. Azure peut récupérer à partir d’une défaillance au niveau de la zone de services de gestion de la région ou à partir d’une autre région Azure. Azure effectue une seule zone de maintenance critiques à la fois dans une région, afin d’éviter toute défaillance ayant un impact sur les ressources client déployées dans les Zones de disponibilité au sein d’une région.

## <a name="pricing"></a>Tarifs
Il n’existe aucun coût supplémentaire pour les machines virtuelles déployées dans une Zone de disponibilité. Un contrat de niveau de service des machines virtuelles de 99,99 % est offert lorsque deux machines virtuelles ou plus sont déployées sur deux Zones de disponibilité ou plus au sein d’une région Azure. Il y aura des frais supplémentaires de transfert des données de machine virtuelle à machine virtuelle entre les Zones de disponibilité. Pour plus d'informations, consultez la page [Tarification de la bande passante](https://azure.microsoft.com/pricing/details/bandwidth/).


## <a name="get-started-with-availability-zones"></a>Prise en main des Zones de disponibilité
- [Créer une machine virtuelle](../virtual-machines/windows/create-portal-availability-zone.md)
- [Ajouter un disque géré à l’aide de PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Créer un groupe de machines virtuelles identiques redondant interzone](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Machines virtuelles de l’équilibreur dans des zones à l’aide de Load Balancer Standard avec un serveur frontal redondant interzone](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Machines virtuelles de l’équilibreur de charge dans une zone à l’aide de Load Balancer Standard avec un serveur frontal zonal](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Stockage redondant interzone](../storage/common/storage-redundancy-zrs.md)
- [Base de données SQL](../sql-database/sql-database-high-availability.md#zone-redundant-configuration)
- [Géo-reprise d’activité après sinistre Event Hubs](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Géo-reprise d’activité après sinistre Service Bus](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Créer une passerelle de réseau virtuel redondante interzone](../vpn-gateway/create-zone-redundant-vnet-gateway.md)
- [Ajoutez la région redondant de zone pour Azure Cosmos DB](../cosmos-db/high-availability.md#availability-zone-support)


## <a name="next-steps"></a>Étapes suivantes
- [Modèles de démarrage rapide](https://aka.ms/azqs)
