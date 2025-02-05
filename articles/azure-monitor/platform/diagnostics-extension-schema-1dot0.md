---
title: Schéma de configuration Diagnostics Azure 1.0
description: Applicable UNIQUEMENT si vous utilisez le Kit de développement logiciel (SDK) Azure 2.4 et les versions antérieures avec Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric ou Cloud Services.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: ac2b79d670b803573a359dfc9f8738f972f2d9b5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60237858"
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Schéma de configuration Diagnostics Azure 1.0
> [!NOTE]
> Diagnostics Azure est le composant utilisé pour collecter les compteurs de performances et d’autres statistiques de Machine virtuelles Azure, Virtual Machine Scale Sets, Service Fabric et Cloud Services.  Cette page vous concerne uniquement si vous utilisez l’un de ces services.
>

Diagnostics Azure est utilisé avec d'autres produits de diagnostic Microsoft tels qu'Azure Monitor, qui inclut Application Insights et Log Analytics.

Le fichier de configuration Diagnostics Azure définit les valeurs qui sont utilisées pour initialiser le moniteur de diagnostics. Ce fichier est utilisé pour initialiser les paramètres de configuration de diagnostic lorsque le moniteur de diagnostic démarre.  

 Par défaut, le fichier de schéma de configuration Diagnostics Azure est installé dans le répertoire `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas`. Remplacez `<version>` par la version installée du [Kit de développement logiciel (SDK) Azure](https://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  Le fichier de configuration de diagnostic est généralement utilisé avec les tâches de démarrage qui requièrent la collecte de données de diagnostic au début du processus de démarrage. Pour plus d’informations sur l’utilisation de Diagnostics Azure, consultez [Collecte de données de journalisation à l’aide de Diagnostics Azure](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Exemple du fichier de configuration des diagnostics  
 L’exemple suivant montre un fichier de configuration de diagnostic standard :  

```xml  
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"  
      configurationChangePollInterval="PT1M"  
      overallQuotaInMB="4096">  
   <DiagnosticInfrastructureLogs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  
   <Logs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  

   <Directories bufferQuotaInMB="1024"   
      scheduledTransferPeriod="PT1M">  

      <!-- These three elements specify the special directories   
           that are set up for the log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories the DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative to a local   
                 resource defined in the service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- The counter specifier is in the same format as the imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- The event log name is in the same format as the imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a>Espace de noms DiagnosticsConfiguration  
 L’espace de noms XML du fichier de configuration des diagnostics est :  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Éléments du schéma  
 Le fichier de configuration de diagnostic inclut les éléments suivants.


## <a name="diagnosticmonitorconfiguration-element"></a>Élément DiagnosticMonitorConfiguration  
Élément de niveau supérieur du fichier de configuration de diagnostic.  

Attributs :

|Attribut  |Type   |Obligatoire| Default | Description|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|duration|Facultatif | PT1M| Spécifie l’intervalle auquel le moniteur de diagnostic s’enquiert des modifications de configuration de diagnostic.|  
|**overallQuotaInMB**|unsignedInt|Facultatif| 4 000 Mo. La valeur indiquée ne doit pas dépasser ce montant |Quantité totale de stockage du système de fichiers allouée pour la journalisation de toutes les mémoires tampons.|  

## <a name="diagnosticinfrastructurelogs-element"></a>Élément DiagnosticInfrastructureLogs  
Définit la configuration de la mémoire tampon pour les journaux d’activité générés par l’infrastructure de diagnostic sous-jacente.

Élément parent : Élément DiagnosticMonitorConfiguration.  

Attributs :

|Attribut|Type|Description|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|facultatif. Définit la quantité maximale de stockage du système de fichiers disponible pour les données spécifiées.<br /><br /> La valeur par défaut est 0.|  
|**scheduledTransferLogLevelFilter**|chaîne|facultatif. Définit le niveau de gravité minimal des entrées de journal transférées. La valeur par défaut est **Non défini**. Les autres valeurs possibles sont **Détaillé**, **Informations**, **Avertissement**, **Erreur**, et **Critique**.|  
|**scheduledTransferPeriod**|duration|facultatif. Définit l’intervalle entre les transferts planifiés de données, arrondi à la minute la plus proche.<br /><br /> La valeur par défaut est PT0S.|  

## <a name="logs-element"></a>Élément Logs  
 Définit la configuration de la mémoire tampon des journaux d’activité Azure de base.

 Élément parent : Élément DiagnosticMonitorConfiguration.  

Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|facultatif. Définit la quantité maximale de stockage du système de fichiers disponible pour les données spécifiées.<br /><br /> La valeur par défaut est 0.|  
|**scheduledTransferLogLevelFilter**|chaîne|facultatif. Définit le niveau de gravité minimal des entrées de journal transférées. La valeur par défaut est **Non défini**. Les autres valeurs possibles sont **Détaillé**, **Informations**, **Avertissement**, **Erreur**, et **Critique**.|  
|**scheduledTransferPeriod**|duration|facultatif. Définit l’intervalle entre les transferts planifiés de données, arrondi à la minute la plus proche.<br /><br /> La valeur par défaut est PT0S.|  

## <a name="directories-element"></a>Élément Directories  
Définit la configuration de la mémoire tampon pour les journaux d’activité basés sur des fichiers que vous pouvez définir.

Élément parent : Élément DiagnosticMonitorConfiguration.  


Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|facultatif. Définit la quantité maximale de stockage du système de fichiers disponible pour les données spécifiées.<br /><br /> La valeur par défaut est 0.|  
|**scheduledTransferPeriod**|duration|facultatif. Définit l’intervalle entre les transferts planifiés de données, arrondi à la minute la plus proche.<br /><br /> La valeur par défaut est PT0S.|  

## <a name="crashdumps-element"></a>Élément CrashDumps  
 Définit le répertoire de vidages sur incident.

 Élément parent : Élément Directories.  

Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**container**|chaîne|Nom du conteneur dans lequel le contenu du répertoire doit être transféré.|  
|**directoryQuotaInMB**|unsignedInt|facultatif. Définit la taille maximale du répertoire en mégaoctets.<br /><br /> La valeur par défaut est 0.|  

## <a name="failedrequestlogs-element"></a>Élément FailedRequestLogs  
 Définit le répertoire de journaux des demandes ayant échoué.

 Élément parent : élément Directories.  

Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**container**|chaîne|Nom du conteneur dans lequel le contenu du répertoire doit être transféré.|  
|**directoryQuotaInMB**|unsignedInt|facultatif. Définit la taille maximale du répertoire en mégaoctets.<br /><br /> La valeur par défaut est 0.|  

##  <a name="iislogs-element"></a>Élément IISLogs  
 Définit le répertoire des journaux IIS.

 Élément parent : élément Directories.  

Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**container**|chaîne|Nom du conteneur dans lequel le contenu du répertoire doit être transféré.|  
|**directoryQuotaInMB**|unsignedInt|facultatif. Définit la taille maximale du répertoire en mégaoctets.<br /><br /> La valeur par défaut est 0.|  

## <a name="datasources-element"></a>Élément DataSources  
 Définit zéro ou plusieurs répertoires de journaux supplémentaires.

 Élément parent : Élément Directories.

## <a name="directoryconfiguration-element"></a>Élément DirectoryConfiguration  
 Définit le répertoire de fichiers journaux à surveiller.

 Élément parent : Élément DataSources.

Attributs :

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**container**|chaîne|Nom du conteneur dans lequel le contenu du répertoire doit être transféré.|  
|**directoryQuotaInMB**|unsignedInt|facultatif. Définit la taille maximale du répertoire en mégaoctets.<br /><br /> La valeur par défaut est 0.|  

## <a name="absolute-element"></a>Élément Absolute  
 Définit un chemin d’accès absolu du répertoire à surveiller avec une extension d’environnement facultative.

 Élément parent : Élément DirectoryConfiguration.  

Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**path**|chaîne|Requis. Chemin d’accès absolu au répertoire à surveiller.|  
|**expandEnvironment**|booléenne|Requis. Si la valeur **true** est attribuée, les variables d’environnement du chemin d’accès sont développées.|  

## <a name="localresource-element"></a>Élément LocalResource  
 Définit un chemin d’accès relatif à une ressource locale spécifiée dans la définition de service.

 Élément parent : Élément DirectoryConfiguration.  

Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**name**|chaîne|Requis. Nom de la ressource locale qui contient le répertoire à surveiller.|  
|**relativePath**|chaîne|Requis. Chemin d’accès relatif à la ressource locale à surveiller.|  

## <a name="performancecounters-element"></a>Élément PerformanceCounters  
 Définit le chemin d’accès au compteur de performance à collecter.

 Élément parent : Élément DiagnosticMonitorConfiguration.


 Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|facultatif. Définit la quantité maximale de stockage du système de fichiers disponible pour les données spécifiées.<br /><br /> La valeur par défaut est 0.|  
|**scheduledTransferPeriod**|duration|facultatif. Définit l’intervalle entre les transferts planifiés de données, arrondi à la minute la plus proche.<br /><br /> La valeur par défaut est PT0S.|  

## <a name="performancecounterconfiguration-element"></a>Élément PerformanceCounterConfiguration  
 Définit le compteur de performance à collecter.

 Élément parent : Élément PerformanceCounters.  

 Attributs :  

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**counterSpecifier**|chaîne|Requis. Chemin d’accès au compteur de performance à collecter.|  
|**sampleRate**|duration|Requis. Vitesse à laquelle le compteur de performance doit être collecté.|  

## <a name="windowseventlog-element"></a>Élément WindowsEventLog  
 Définit les journaux des événements à surveiller.

 Élément parent : Élément DiagnosticMonitorConfiguration.

  Attributs :

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|facultatif. Définit la quantité maximale de stockage du système de fichiers disponible pour les données spécifiées.<br /><br /> La valeur par défaut est 0.|  
|**scheduledTransferLogLevelFilter**|chaîne|facultatif. Définit le niveau de gravité minimal des entrées de journal transférées. La valeur par défaut est **Non défini**. Les autres valeurs possibles sont **Détaillé**, **Informations**, **Avertissement**, **Erreur**, et **Critique**.|  
|**scheduledTransferPeriod**|duration|facultatif. Définit l’intervalle entre les transferts planifiés de données, arrondi à la minute la plus proche.<br /><br /> La valeur par défaut est PT0S.|  

## <a name="datasource-element"></a>Élément DataSource  
 Définit les journaux des événements à surveiller.

 Élément parent : Élément WindowsEventLog.  

 Attributs :

|Attribut|Type|Description|  
|---------------|----------|-----------------|  
|**name**|chaîne|Requis. Expression XPath spécifiant le journal à collecter.|  

