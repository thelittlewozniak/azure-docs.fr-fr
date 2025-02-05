---
title: Exemples de stockage Azure avec Java| Microsoft Docs
description: Affichez, téléchargez et exécutez des exemples de code et des applications pour Azure Storage. Découvrez des exemples de mise en route d’objets blob, de files d’attente, de tables et de fichiers à l’aide des bibliothèques clientes de stockage Java.
services: storage
author: mhopkins-msft
ms.service: storage
ms.devlang: java
ms.topic: article
ms.date: 05/03/2019
ms.author: mhopkins
ms.subservice: common
ms.openlocfilehash: 3d241f1905244d3a8039372262f84ba0fd25220d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209797"
---
# <a name="azure-storage-samples-using-java"></a>Exemples de stockage Azure avec Java

## <a name="java-sample-index"></a>Index des exemples Java

Le tableau suivant fournit une vue d’ensemble de notre référentiel d’exemples et des scénarios traités dans chaque exemple. Cliquez sur les liens pour afficher l’exemple de code correspondant dans GitHub.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Point de terminaison</th><th style="font-size:110%">Scénario</th><th style="font-size:110%">Exemple de code</th></tr></thead><tbody>
<tr>
<td rowspan="16"><b>Objet blob</b></td>
<td>Append Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Objet blob de blocs</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Chiffrement côté client</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Getting Started with Azure Client Side Encryption in Java (Prise en main du chiffrement côté client Azure dans Java)</a></td>
</tr>
<tr>
<td>Copie d'un objet blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Create Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Delete Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Delete Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Métadonnées/propriétés/statistiques des objets blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>ACL/métadonnées/propriétés du conteneur</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Get Page Ranges</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java#L399">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Objet blob/conteneur de bail</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Objet blob/conteneur de liste</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Objet blob de pages</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513">Exemple de tests SAS</a></td>
</tr>   
<tr>
<td>Propriétés du service</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td>Snapshot Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)</a></td>
</tr>
<tr>
<td rowspan="9"><b>File</b></td>
<td>Créer des partages/répertoires/fichiers</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td>Créer des partages/répertoires/fichiers</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td>Propriétés/métadonnées du répertoire</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td>Télécharger des fichiers</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td>Propriétés/métadonnées/mesures de fichier</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td>Propriétés du service de fichiers</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td>Liste des répertoires et des fichiers</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td>Partages de listes</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td>Propriétés/métadonnées/statistiques de partage</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)</a></td>
</tr>
<tr>
<td rowspan="8"><b>File d'attente</b></td>
<td>Ajouter un message</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java#L63">Getting Started with Azure Queue Service in Java (Prise en main du service de File d’attente Azure en Java)</a></td>
</tr>
<tr>
<td>Chiffrement côté client</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption/blob/master/src/gettingstarted/KeyVaultGettingStarted.java">Getting Started with Azure Client Side Encryption in Java (Prise en main du chiffrement côté client Azure dans Java)</a></td>
</tr>
<tr>
<td>Créer des files d’attente</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java (Prise en main du service de File d’attente Azure en Java)</a></td>
</tr>
<tr>
<td>Supprimer le message/la file d’attente</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java (Prise en main du service de File d’attente Azure en Java)</a></td>
</tr>
<tr>
<td>Lire furtivement un message</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java (Prise en main du service de File d’attente Azure en Java)</a></td>
</tr>
<tr>
<td>ACL/métadonnées/statistiques de la file d’attente</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java (Prise en main du service de File d’attente Azure en Java)</a></td>
</tr>
<tr>
<td>Propriétés du service de File d’attente</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java (Prise en main du service de File d’attente Azure en Java)</a></td>
</tr>
<tr>
<td>Mise à jour de message</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java (Prise en main du service de File d’attente Azure en Java)</a></td>
</tr>
<tr>
<td rowspan="7"><b>Table</b></td>
<td>Créer une table</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java (Prise en main du service de Table Azure en Java)</a></td>
</tr>
<tr>
<td>Supprimer l’entité/la table</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java (Prise en main du service de Table Azure en Java)</a></td>
</tr>
<tr>
<td>Insertion/fusion/remplacement d’entité</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java (Prise en main du service de Table Azure en Java)</a></td>
</tr>
<tr>
<td>Query Entities</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java (Prise en main du service de Table Azure en Java)</a></td>
</tr>
<tr>
<td>Tables de requête</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java (Prise en main du service de Table Azure en Java)</a></td>
</tr>
<tr>
<td>ACL/propriétés de la table</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java">Getting Started with Azure Table Service in Java (Prise en main du service de Table Azure en Java)</a></td>
</tr>
<tr>
<td>Update Entity</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java (Prise en main du service de Table Azure en Java)</a></td>
</tr>
</tbody>
</table>
<br/>

## <a name="azure-code-samples-library"></a>Bibliothèque Exemples de code Azure

Pour voir la bibliothèque complète des exemples, consultez la bibliothèque d’[exemples de code Azure](https://azure.microsoft.com/resources/samples/?service=storage) pour trouver des exemples de stockage Azure que vous pouvez télécharger et exécuter localement. Les exemples de code fournis par la bibliothèque sont au format .zip. Vous pouvez également parcourir et cloner le dépôt GitHub pour chaque exemple.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Guides de prise en main

Consultez les guides suivants si vous recherchez des instructions sur l’installation et la prise en main des bibliothèques clientes de stockage Azure.

* [Getting Started with Azure Blob Service in Java (Prise en main du service Azure Blob en Java)](../blobs/storage-quickstart-blobs-java.md)
* [Getting Started with Azure Queue Service in Java (Prise en main du service de File d’attente Azure en Java)](../queues/storage-java-how-to-use-queue-storage.md)
* [Getting Started with Azure Table Service in Java (Prise en main du service de Table Azure en Java)](../../cosmos-db/table-storage-how-to-use-java.md)
* [Getting Started with Azure File Service in Java (Prise en main du service de fichiers Azure en Java)](../files/storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les exemples pour d’autres langages :

* .NET : [Exemples relatifs au service Stockage Azure avec .NET](storage-samples-dotnet.md)
* Tous les autres langages : [Exemples relatifs à Stockage Azure](storage-samples.md)
