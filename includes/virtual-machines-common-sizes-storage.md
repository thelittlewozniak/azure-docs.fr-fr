---
title: Fichier Include
description: Fichier Include
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/17/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: de998265188b1482be22225bba3c6ab3d58e562b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755609"
---
Les tailles de machines virtuelles à stockage optimisé offrent un débit de disque et d’E/S élevé. Elles sont idéales pour les bases de données Big Data, SQL, NoSQL ainsi que pour l’entreposage de données et les grandes bases de données transactionnelles.  Cassandra, MongoDB, Cloudera et Redis en sont des exemples. Cet article fournit des informations sur le nombre de processeurs virtuels, de disques de données et de cartes réseau ainsi que sur le débit de stockage local et la bande passante réseau pour chaque taille optimisée.

La série Lsv2 offre un stockage NVMe local directement mappé, à haut débit et faible latence. Elle est dotée du [processeur AMD EPYC&trade; 7551](https://www.amd.com/en/products/epyc-7000-series) avec une augmentation de la fréquence de tous les cœurs de 2,55 GHz et une augmentation maximale de 3 GHz. Les machines virtuelles de la série Lsv2 comprennent entre 8 et 80 processeurs virtuels dans une configuration de multithreading simultané.  Il existe 8 Gio de mémoire par processeur virtuel et un périphérique NVMe SSD M.2 de 1,92 To pour 8 processeurs virtuels, avec une limite maximale de 19,2 To (10 x 1,92 To) sur le L80s v2.

> [!NOTE]
> Les machines virtuelles de la série Lsv2 sont optimisées pour utiliser le disque local du nœud attaché directement à la machine virtuelle au lieu d’utiliser des disques de données durables. Cela permet d’augmenter les IOPS/le débit de vos charges de travail. Les séries Lsv2 et Ls ne prennent pas en charge la création d’un cache local pour augmenter le taux d’IOPS pouvant être atteint par les disques de données durables.
>
> Le débit élevé et les IOPS du disque local rendent les machines virtuelles des séries Lsv2 et Ls idéales pour les magasins NoSQL tels qu’Apache Cassandra et MongoDB, qui répliquent les données sur plusieurs machines virtuelles pour assurer une persistance en cas de défaillance d’une seule machine virtuelle.
>
> Pour en savoir plus, consultez [Optimiser les performances sur les machines virtuelles de la série Lsv2](../articles/virtual-machines/linux/storage-performance.md).  


## <a name="lsv2-series"></a>Série Lsv2

ACU : 150-175

Premium Storage : Pris en charge

Mise en cache du Stockage Premium : Non pris en charge

| Taille          | Processeurs virtuels | Mémoire (Gio) | Disque temporaire<sup>1</sup> (Gio) | Disques NVMe<sup>2</sup> | Débit de disque NVMe <sup>3</sup> (IOPS de lecture / Mbits/s) | Débit maximal de disque de données sans mise en cache (IOPS / Mbits/s)<sup>4</sup> | Nombre max de disques de données | Nombre max de cartes réseau / Bande passante réseau attendue (MBps) |
|---------------|-----------|-------------|--------------------------|----------------|---------------------------------------------------|-------------------------------------------|------------------------------|------------------------------| 
| Standard_L8s_v2   |  8 |  64 |  80 |  1 x 1,92 To  | 400 000 / 2 000 | 8 000/160 | 16 | 2 / 3 200  |
| Standard_L16s_v2  | 16 | 128 | 160 |  2 x 1,92 To  | 800 000 / 4 000 | 16 000/320 | 32 | 4 / 6 400  |
| Standard_L32s_v2  | 32 | 256 | 320 |  4 x 1,92 To  | 1,5M / 8 000    | 32 000/640 | 32 | 8 / 12 800 |
| Standard_L64s_v2  | 64 | 512 | 640 |  8 x 1,92 To  | 2,9 M / 16 000   | 64 000/1 280 | 32 | 8 / 16 000+ |
| Standard_L80s_v2<sup>5</sup> | 80 | 640 | 800 | 10 x 1,92 To   | 3,8 M / 20000   | 80000/1400 | 32 | 8 / 16 000+ |

<sup>1</sup> Les machines virtuelles de la série Lsv2 disposent d’un disque de ressources temporaire SCSI standard pour l’utilisation du fichier d’échange du système d’exploitation (D: sur Windows, /dev/sdb sur Linux). Ce disque offre un stockage de 80 Gio, 4 000 IOPS et un taux de transfert de 80 Mbits/s pour 8 processeurs virtuels (par exemple, Standard_L80s_v2 fournit 800 Gio à 40 000 IOPS et 800 Mbits/s). Ainsi, les lecteurs NVMe peuvent être entièrement dédiés à l’utilisation d’applications. Ce disque est éphémère, et toutes les données seront perdues lors de l'arrêt/la libération.

<sup>2</sup> Les disques NVMe locaux sont éphémères ; les données présentes sur ces disques seront perdues si vous arrêtez/libérez votre machine virtuelle.

<sup>3</sup> La technologie Hyper-V NVMe Direct fournit un accès illimité aux disques NVMe locaux mappés de manière sécurisée à l'espace de la machine virtuelle invitée.  Pour atteindre des performances maximales, vous devez utiliser la dernière build WS2019, ou Ubuntu 18.04 ou 16.04, de la Place de marché Azure.  Les performances en écriture varient en fonction de la taille des E/S, de la charge du lecteur et de l’utilisation des fonctionnalités.

<sup>4</sup> Les machines virtuelles de la série Lsv2 ne fournissent pas de cache hôte pour le disque de données, car cela ne profite pas aux charges de travail Lsv2.  Toutefois, les machines virtuelles Lsv2 peuvent prendre en charge l’option de disque de système d’exploitation de machine virtuelle Ephemeral d’Azure (jusqu’à 30 Gio).

<sup>5</sup> Les machines virtuelles avec plus de 64 processeurs virtuels nécessitent l’un de ces systèmes d’exploitation invités pris en charge :
- Windows Server 2016 ou version ultérieure
- Ubuntu 16.04 LTS or version ultérieure, avec noyau Azure adapté (noyau 4.15 ou version ultérieure)
- SLES 12 SP2 ou version ultérieure
- RHEL ou CentOS version 6.7 à 6.10, avec le package LIS fourni par Microsoft 4.3.1 (ou version ultérieure) installé
- RHEL ou CentOS version 7.3, avec le package LIS fourni par Microsoft 4.2.1 (ou version ultérieure) installé
- RHEL ou CentOS 7.6 ou version ultérieure
- Oracle Linux avec UEK4 ou version ultérieure
- Debian 9 avec le noyau backports, Debian 10 ou version ultérieure
- CoreOS avec un noyau 4.14 ou version ultérieure


## <a name="size-table-definitions"></a>Définitions des tailles de tables

- La capacité de stockage est indiquée en unités de Gio ou 1 024^3 octets. Lors de la comparaison de disques mesurés en Go (1 000^3 octets) à des disques mesurés en Gio (1 024^3) n’oubliez pas que les indications de capacité en Gio peuvent sembler plus petites. Par exemple, 1 023 Gio = 1 098,4 Go
- Le débit de disque est mesuré en opérations d’entrée/sortie par seconde (IOPS) et Mbit/s  où Mbit/s  = 10^6 octets par seconde.
- Si vous souhaitez obtenir des performances optimales pour vos machines virtuelles, vous devez limiter le nombre de disques de données à 2 disques par processeur virtuel.
- La **bande passante réseau attendue** est la [bande passante agrégée maximale qui est allouée par type de machine virtuelle](../articles/virtual-network/virtual-machine-network-throughput.md) entre toutes les cartes réseau, pour toutes les destinations. Les limites supérieures ne sont pas garanties, mais servent de points de repère pour sélectionner le type de machine virtuelle adapté à l’application prévue. Les performances réseau réelles dépendent de nombreux facteurs, notamment la congestion du réseau, les charges de l’application, ainsi que les paramètres réseau. Pour plus d’informations sur l’optimisation du débit du réseau, consultez [Optimisation du débit du réseau pour Windows et Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Pour atteindre la performance réseau attendue sous Linux ou Windows, il peut être nécessaire de sélectionner une version spécifique ou d’optimiser votre machine virtuelle. Pour plus d’informations, consultez [Tester de manière fiable le débit d’une machine virtuelle](../articles/virtual-network/virtual-network-bandwidth-testing.md).
