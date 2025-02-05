---
title: Présentation d’Azure Files | Microsoft Docs
description: Vue d’ensemble d’Azure Files, un service qui vous permet de créer et d’utiliser les partages de fichiers réseau dans le cloud à l’aide du protocole SMB standard du secteur.
services: storage
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 075a3cea426fd5f54ef142648754fa9a9e2810b4
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508317"
---
# <a name="what-is-azure-files"></a>Qu’est-ce qu’Azure Files ?
Azure Files offre des partages de fichiers managés dans le cloud qui sont accessibles via le protocole [SMB (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) standard. Les partages de fichiers Azure peuvent être montés simultanément sur des déploiements cloud ou locaux de Windows, Linux et macOS. En outre, les partages de fichiers Azure peuvent être mis en cache sur les serveurs Windows avec Azure File Sync pour un accès rapide à proximité de l’endroit où les données sont utilisées.

## <a name="videos"></a>Videos
| Présentation d’Azure File Sync (2 min) | Azure Files et Sync (Ignite 2017) (85 min)  |
|-|-|
| [![Capture vidéo de la présentation d’Azure File Sync – Cliquez pour lancer la vidéo !](./media/storage-files-introduction/azure-file-sync-video-snapshot.png)](https://www.youtube.com/watch?v=Zm2w8-TRn-o) | [![Capture vidéo de la présentation d’Azure Files et Sync – Cliquez pour lancer la vidéo !](./media/storage-files-introduction/ignite-2018-video.png)](https://www.youtube.com/watch?v=GMzh2M66E9o) |

## <a name="why-azure-files-is-useful"></a>Pourquoi Azure Files est-il utile ?
Les partages de fichiers Azure peuvent être utilisés pour :

* **Remplacer ou complémenter les serveurs de fichiers locaux** :  
    Azure Files peut être utilisé pour remplacer complètement ou compléter les serveurs de fichiers locaux traditionnels ou les périphériques NAS. Les systèmes d’exploitation courants tels que Windows, Mac OS et Linux peuvent monter directement des partages Azure Files, n’importe où dans le monde. Les partages de fichiers Azure peuvent également être répliqués avec Azure File Sync vers des serveurs Windows, localement ou dans le cloud, pour une mise en cache performante et distribuée des données là où elles sont utilisées.

* **Migration « lift-and-shift » des applications** :  
    Azure Files facilite le passage dans le cloud des applications qui exigent un partage de fichiers pour stocker les données utilisateur ou l’application de fichier. Azure Files permet une transition classique, où l’application et ses données sont déplacées vers Azure, ainsi qu’une transition hybride, où les données d’application sont déplacées vers Azure Files et l’application s’exécute toujours en local. 

* **Simplifier le développement cloud** :  
    Azure Files permet aussi de simplifier de plusieurs façons les nouveaux projets de développement cloud. Par exemple :
    * **Paramètres d’application partagés** :  
        Pour les applications distribuées, les fichiers de configuration sont souvent centralisés à un emplacement accessible par différentes instances d’application. Les instances de l’application peuvent charger leur configuration via l’API REST File et l’utilisateur peut y accéder en fonction des besoins en montant le partage SMB localement.

    * **Partage de diagnostic** :  
        Un partage de fichiers Azure est un emplacement pratique où les applications cloud peuvent écrire leurs journaux d’activité, mesures et images mémoire. Les journaux d’activité peuvent être écrits par les instances de l’application via l’API REST File, et les développeurs peuvent y accéder en montant le partage de fichiers sur leur ordinateur local. Cela permet une grande souplesse, car les développeurs peuvent adopter le développement cloud sans abandonner les outils existants qu’ils apprécient.

    * **Développement / test / débogage** :  
        Lorsque les développeurs ou les administrateurs travaillent sur des machines virtuelles situées dans le cloud, ils ont souvent besoin de différents outils ou utilitaires. La copie de ces outils et utilitaires vers chaque machine virtuelle peut prendre beaucoup de temps. En montant un partage de fichiers Azure localement sur les machines virtuelles, un développeur et un administrateur peuvent accéder rapidement à leurs outils et utilitaires, car aucune copie n’est requise.

## <a name="key-benefits"></a>Principaux avantages
* **Accès partagé**. Les partages de fichiers Azure prennent en charge les protocoles standard SMB. Cela signifie que vous pouvez facilement remplacer vos partages de fichiers locaux par des partages de fichiers Azure, sans vous soucier de la compatibilité des applications. Être en mesure de partager un système de fichiers sur plusieurs ordinateurs et applications/instances peut s’avérer très avantageux avec Azure Files pour les applications qui nécessitent des capacités de partage. 
* **Gestion intégrale**. Les partages de fichiers Azure peuvent être créés sans avoir à gérer le matériel ou un système d’exploitation. Cela signifie que vous n’êtes pas obligé de gérer les mises à jour correctives du système d’exploitation du serveur avec des mises à niveau de sécurité critiques ou de remplacer les disques durs défaillants.
* **Écriture de scripts et outils**. Les cmdlets PowerShell et l’interface CLI Azure peuvent être utilisées pour créer, monter et gérer les partages de fichiers Azure dans le cadre de l’administration des applications Azure. Vous pouvez créer et gérer les partages de fichiers Azure via le portail Azure et l’Explorateur Stockage Azure. 
* **Résilience**. Azure Files a été entièrement conçu de manière à être toujours disponible. L’utilisation d’Azure Files au lieu des partages de fichiers locaux signifie que vous n’avez plus à vous soucier des pannes de courant ou des problèmes de réseau. 
* **Programmabilité familière**. Les applications exécutées dans Azure peuvent accéder aux données dans le partage via les [API d’E/S du système](https://msdn.microsoft.com/library/system.io.file.aspx) de fichier. Les développeurs peuvent ainsi tirer profit de leur code et compétences actuels pour migrer les applications existantes. En plus des API d’E/S du système, vous pouvez utiliser les [bibliothèques de client de stockage Azure](https://msdn.microsoft.com/library/azure/dn261237.aspx) ou l’[API REST de stockage Azure](/rest/api/storageservices/file-service-rest-api).

## <a name="next-steps"></a>Étapes suivantes
* [Création d’un partage de fichiers Azure](storage-how-to-create-file-share.md)
* [Connexion et montage sous Windows](storage-how-to-use-files-windows.md)
* [Connexion et montage sous Linux](storage-how-to-use-files-linux.md)
* [Connexion et montage sous macOS](storage-how-to-use-files-mac.md)
