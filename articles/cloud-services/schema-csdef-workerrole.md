---
title: Schéma WorkerRole de définition d’Azure Cloud Services | Microsoft Docs
services: cloud-services
ms.custom: ''
ms.date: 04/14/2015
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 41cd46bc-c479-43fa-96e5-d6c83e4e6d89
caps.latest.revision: 55
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 90a11c5bb81a0d29f5f8a1c1696732453aa4b1ab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62095402"
---
# <a name="azure-cloud-services-definition-workerrole-schema"></a>Schéma WorkerRole de définition d’Azure Cloud Services
Le rôle de travail Azure est un rôle utile pour le développement généralisé. Il peut également effectuer le traitement en arrière-plan pour un rôle Web.

L’extension par défaut du fichier de définition de service est .csdef.

## <a name="basic-service-definition-schema-for-a-worker-role"></a>Schéma de définition de service de base pour un rôle de travail.
Le format de base du fichier de définition de service contenant un rôle de travail est le suivant.

```xml
<ServiceDefinition …>
  <WorkerRole name="<worker-role-name>" vmsize="<worker-role-size>" enableNativeCodeExecution="[true|false]">
    <Certificates>
      <Certificate name="<certificate-name>" storeLocation="[CurrentUser|LocalMachine]" storeName="[My|Root|CA|Trust|Disallow|TrustedPeople|TrustedPublisher|AuthRoot|AddressBook|<custom-store>" />
    </Certificates>
    <ConfigurationSettings>
      <Setting name="<setting-name>" />
    </ConfigurationSettings>
    <Endpoints>
      <InputEndpoint name="<input-endpoint-name>" protocol="[http|https|tcp|udp]" localPort="<local-port-number>" port="<port-number>" certificate="<certificate-name>" loadBalancerProbe="<load-balancer-probe-name>" />
      <InternalEndpoint name="<internal-endpoint-name" protocol="[http|tcp|udp|any]" port="<port-number>">
         <FixedPort port="<port-number>"/>
         <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>
      </InternalEndpoint>
     <InstanceInputEndpoint name="<instance-input-endpoint-name>" localPort="<port-number>" protocol="[udp|tcp]">
         <AllocatePublicPortFrom>
            <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>
         </AllocatePublicPortFrom>
      </InstanceInputEndpoint>
    </Endpoints>
    <Imports>
      <Import moduleName="[RemoteAccess|RemoteForwarder|Diagnostics]"/>
    </Imports>
    <LocalResources>
      <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />
    </LocalResources>
    <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />
    <Runtime executionContext="[limited|elevated]">
      <Environment>
         <Variable name="<variable-name>" value="<variable-value>">
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>
          </Variable>
      </Environment>
      <EntryPoint>
         <NetFxEntryPoint assemblyName="<name-of-assembly-containing-entrypoint>" targetFrameworkVersion="<.net-framework-version>"/>
         <ProgramEntryPoint commandLine="<application>" setReadyOnProcessStart="[true|false]" "/>
      </EntryPoint>
    </Runtime>
    <Startup priority="<for-internal-use-only>”>
      <Task commandLine="" executionContext="[limited|elevated]" taskType="[simple|foreground|background]">
        <Environment>
         <Variable name="<variable-name>" value="<variable-value>">
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>
          </Variable>
        </Environment>
      </Task>
    </Startup>
    <Contents>
      <Content destination="<destination-folder-name>" >
        <SourceDirectory path="<local-source-directory>" />
      </Content>
    </Contents>
  </WorkerRole>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Éléments du schéma
Le fichier de définition de service inclut ces éléments, ils sont détaillés dans les sections suivantes de cette rubrique :

[WorkerRole](#WorkerRole)

[ConfigurationSettings](#ConfigurationSettings)

[Paramètre](#Setting)

[LocalResources](#LocalResources)

[LocalStorage](#LocalStorage)

[Points de terminaison](#Endpoints)

[InputEndpoint](#InputEndpoint)

[InternalEndpoint](#InternalEndpoint)

[InstanceInputEndpoint](#InstanceInputEndpoint)

[AllocatePublicPortFrom](#AllocatePublicPortFrom)

[FixedPort](#FixedPort)

[FixedPortRange](#FixedPortRange)

[Certificates](#Certificates)

[Certificate](#Certificate)

[Imports](#Imports)

[Importationation](#Import)

[Runtime](#Runtime)

[Environment](#Environment)

[EntryPoint](#EntryPoint)

[NetFxEntryPoint](#NetFxEntryPoint)

[ProgramEntryPoint](#ProgramEntryPoint)

[Variable](#Variable)

[RoleInstanceValue](#RoleInstanceValue)

[Startup](#Startup)

[Tâche](#Task)

[Contents](#Contents)

[Contenu](#Content)

[SourceDirectory](#SourceDirectory)

##  <a name="WorkerRole"></a> WorkerRole
L’élément `WorkerRole` décrit un rôle utile pour le développement généralisé. Il peut également effectuer le traitement en arrière-plan pour un rôle Web. Un service peut contenir zéro ou plusieurs rôles de travail.

Le tableau suivant décrit les attributs d’un de l’élément `WorkerRole`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|Nom|chaîne|Requis. Nom du rôle de travail. Ce nom doit être unique.|
|enableNativeCodeExecution|booléenne|facultatif. La valeur par défaut est `true` ; les fonctions d’exécution du code natif et de confiance totale sont activées par défaut. Définissez cet attribut sur `false` pour désactiver l’exécution du code natif pour le rôle de travail et utilisez à la place la fonction de confiance totale Azure.|
|vmSize|chaîne|facultatif. Définissez cette valeur pour modifier la taille de la machine virtuelle allouée à ce rôle. La valeur par défaut est `Small`. Pour obtenir la liste des tailles de machine virtuelle possibles et leurs attributs, consultez [Tailles de machine virtuelle pour les services cloud](cloud-services-sizes-specs.md).|

##  <a name="ConfigurationSettings"></a> ConfigurationSettings
L’élément `ConfigurationSettings` décrit la collection de paramètres de configuration pour un rôle de travail. Cet élément est le parent de l’élément `Setting`.

##  <a name="Setting"></a> Setting
L’élément `Setting` décrit une paire nom-valeur qui spécifie un paramètre de configuration pour une instance d’un rôle.

Le tableau suivant décrit les attributs d’un de l’élément `Setting`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|Nom|chaîne|Requis. Nom unique du paramètre de configuration.|

Les paramètres de configuration d’un rôle sont des paires nom-valeur déclarées dans le fichier de définition de service et définies dans le fichier de configuration de service.

##  <a name="LocalResources"></a> LocalResources
L’élément `LocalResources` décrit la collection de ressources de stockage local pour un rôle de travail. Cet élément est le parent de l’élément `LocalStorage`.

##  <a name="LocalStorage"></a> LocalStorage
L’élément `LocalStorage` identifie une ressource de stockage local qui fournit l’espace de système de fichiers pour le service lors de l’exécution. Un rôle peut définir zéro ou plusieurs ressources de stockage local.

> [!NOTE]
>  L’élément `LocalStorage` peut apparaître en tant qu’enfant de l’élément `WorkerRole` pour prendre en charge la compatibilité avec les versions antérieures du kit SDK Azure.

Le tableau suivant décrit les attributs d’un de l’élément `LocalStorage`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|Nom|chaîne|Requis. Nom unique du magasin local.|
|cleanOnRoleRecycle|booléenne|facultatif. Indique si le magasin local doit être nettoyé lorsque le rôle est redémarré. La valeur par défaut est `true`.|
|sizeInMb|int|facultatif. Quantité souhaitée d’espace de stockage à allouer pour le magasin local, en Mo. S’il n’est pas spécifié, l’espace de stockage alloué par défaut est de 100 Mo. La quantité d’espace de stockage minimale pouvant être allouée est de 1 Mo.<br /><br /> La taille maximale des ressources locales dépend de la taille de la machine virtuelle. Pour plus d’informations, consultez [Tailles de machine virtuelle pour les services cloud](cloud-services-sizes-specs.md).|

Le nom du répertoire alloué à la ressource de stockage local correspond à la valeur fournie pour l’attribut name.

##  <a name="Endpoints"></a> Endpoints
L’élément `Endpoints` décrit la collection de points de terminaison d’entrée (externes), internes et d’entrée d’instance pour un rôle. Cet élément est le parent des éléments `InputEndpoint`, `InternalEndpoint` et `InstanceInputEndpoint`.

Les points de terminaison d’entrée et internes sont alloués séparément. Un service peut cumuler un total de 25 points de terminaison d’entrée, d’entrée d’instance et internes qui peuvent être alloués aux 25 rôles autorisés dans un service. Par exemple, avec 5 rôles vous pouvez au choix allouer 5 points de terminaison d’entrée par rôle, 25 points de terminaison d’entrée à un même rôle ou 1 point de terminaison d’entrée à chacun des 25 rôles.

> [!NOTE]
>  Chaque rôle déployé nécessite une instance par rôle. L’approvisionnement par défaut d’un abonnement est limité à 20 cœurs, et donc à 20 instances d’un rôle. Si votre application nécessite plus d’instances que ce qui est fourni par l’approvisionnement par défaut, consultez [Support pour la facturation, la gestion des abonnements et les quotas](https://azure.microsoft.com/support/options/) pour obtenir plus d’informations sur l’augmentation de votre quota.

##  <a name="InputEndpoint"></a> InputEndpoint
L’élément `InputEndpoint` décrit un point de terminaison externe à un rôle de travail.

Vous pouvez définir plusieurs points de terminaison combinant à la fois des points de terminaison HTTP, HTTPS, UDP et TCP. Vous pouvez spécifier le numéro de port souhaité pour un point de terminaison d’entrée, mais les numéros de port spécifiés pour chaque rôle dans le service doivent être uniques. Par exemple, si vous spécifiez qu’un rôle utilise le port 80 pour HTTP et le port 443 pour HTTPS, vous pouvez ensuite spécifier qu’un deuxième rôle utilise le port 8080 pour HTTP et le port 8043 pour HTTPS.

Le tableau suivant décrit les attributs d’un de l’élément `InputEndpoint`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|Nom|chaîne|Requis. Nom unique du point de terminaison externe.|
|protocol|chaîne|Requis. Protocole de transport du point de terminaison externe. Pour un rôle de travail, les valeurs possibles sont `HTTP`, `HTTPS`, `UDP` ou `TCP`.|
|port|int|Requis. Port du point de terminaison externe. Vous pouvez spécifier le numéro de port de votre choix, mais les numéros de port précisés pour chaque rôle dans le service doivent être uniques.<br /><br /> Les valeurs possibles sont comprises entre 1 et 65535 inclus (Kit SDK Azure version 1.7 ou supérieure).|
|certificat|chaîne|Obligatoire pour un point de terminaison HTTPS. Nom d’un certificat défini par un élément `Certificate`.|
|localPort|int|facultatif. Spécifie le port utilisé pour les connexions internes sur le point de terminaison. L’attribut `localPort` mappe le port externe sur le point de terminaison à un port interne sur un rôle. Cela est utile dans les scénarios où un rôle doit communiquer avec un composant interne sur un port différent de celui qui est exposé en externe.<br /><br /> Si elle n’est pas spécifiée, la valeur de `localPort` est la même que celle de l’attribut `port`. Définissez la valeur de `localPort` sur « * » pour attribuer automatiquement un port non alloué, détectable à l’aide de l’API d’exécution.<br /><br /> Les valeurs possibles sont comprises entre 1 et 65535 inclus (Kit SDK Azure version 1.7 ou supérieure).<br /><br /> L’attribut `localPort` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.|
|ignoreRoleInstanceStatus|booléenne|facultatif. Lorsque la valeur de cet attribut est définie sur `true`, l’état d’un service est ignoré et le point de terminaison n’est pas supprimé par l’équilibreur de charge. Définir cette valeur sur `true` est utile pour déboguer les instances occupées d’un service. La valeur par défaut est `false`. **Remarque :** un point de terminaison peut toujours recevoir du trafic, même si le rôle n'est pas à l'état Prêt.|
|loadBalancerProbe|chaîne|facultatif. Nom de la sonde de l’équilibreur de charge associée au point de terminaison d’entrée. Pour plus d’informations, consultez [Schéma LoadBalancerProbe](schema-csdef-loadbalancerprobe.md).|

##  <a name="InternalEndpoint"></a> InternalEndpoint
L’élément `InternalEndpoint` décrit un point de terminaison interne à un rôle de travail. Un point de terminaison interne est uniquement disponible pour les autres instances de rôle exécutées dans le service ; il n’est en revanche pas disponible pour les clients en dehors du service. Un rôle de travail peut avoir jusqu'à cinq points de terminaison internes HTTP, UDP ou TCP.

Le tableau suivant décrit les attributs d’un de l’élément `InternalEndpoint`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|Nom|chaîne|Requis. Nom unique du point de terminaison interne.|
|protocol|chaîne|Requis. Protocole de transport du point de terminaison interne. Les valeurs possibles sont `HTTP`, `TCP`, `UDP` ou `ANY`.<br /><br /> La valeur `ANY` spécifie que tous les protocoles et tous les ports sont autorisés.|
|port|int|facultatif. Port utilisé pour les connexions internes à charge équilibrée sur le point de terminaison. Un point de terminaison à charge équilibrée utilise deux ports : le port utilisé pour l’adresse IP publique, et le port utilisé sur l’adresse IP privée. Ces adresses sont généralement définies sur le même port, mais vous pouvez utiliser des ports différents.<br /><br /> Les valeurs possibles sont comprises entre 1 et 65535 inclus (Kit SDK Azure version 1.7 ou supérieure).<br /><br /> L’attribut `Port` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.|

##  <a name="InstanceInputEndpoint"></a> InstanceInputEndpoint
L’élément `InstanceInputEndpoint` décrit un point de terminaison d’entrée d’instance à un rôle de travail. Un point de terminaison d’entrée d’instance est associé à une instance de rôle spécifique à l’aide d’un réacheminement de port dans l’équilibreur de charge. Chaque point de terminaison d’entrée d’instance est mappé à un port spécifique à partir d’une plage de ports possibles. Cet élément est le parent de l’élément `AllocatePublicPortFrom`.

L’élément `InstanceInputEndpoint` n’est disponible que par le biais du kit SDK Azure version 1.7 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `InstanceInputEndpoint`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|Nom|chaîne|Requis. Nom unique du point de terminaison.|
|localPort|int|Requis. Spécifie le port interne que toutes les instances de rôle doivent écouter pour recevoir le trafic entrant transféré à partir de l’équilibreur de charge. Les valeurs possibles sont comprises entre 1 et 65535 inclus.|
|protocol|chaîne|Requis. Protocole de transport du point de terminaison interne. Les valeurs possibles sont `udp` ou `tcp`. Utilisez `tcp` pour le trafic http/https.|

##  <a name="AllocatePublicPortFrom"></a> AllocatePublicPortFrom
L’élément `AllocatePublicPortFrom` décrit la plage de ports publics qui peut être utilisée par les clients externes pour accéder à chaque point de terminaison d’entrée d’instance. Le numéro de port public (adresse IP virtuelle) est alloué à partir de cette plage, et affecté à chaque point de terminaison d’instance de rôle individuel pendant le déploiement et la mise à jour du locataire. Cet élément est le parent de l’élément `FixedPortRange`.

L’élément `AllocatePublicPortFrom` n’est disponible que par le biais du kit SDK Azure version 1.7 ou supérieure.

##  <a name="FixedPort"></a> FixedPort
L’élément `FixedPort` spécifie le port du point de terminaison interne, ce qui permet les connexions à charge équilibrée sur le point de terminaison.

L’élément `FixedPort` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `FixedPort`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|port|int|Requis. Port du point de terminaison interne. Cela a le même effet que de définir les attributs min et max de l’élément `FixedPortRange` sur le même port.<br /><br /> Les valeurs possibles sont comprises entre 1 et 65535 inclus (Kit SDK Azure version 1.7 ou supérieure).|

##  <a name="FixedPortRange"></a> FixedPortRange
L’élément `FixedPortRange` spécifie la plage de ports affectés au point de terminaison interne ou au point de terminaison d’entrée d’instance, et définit le port utilisé pour les connexions à charge équilibrée sur le point de terminaison.

> [!NOTE]
>  L’élément `FixedPortRange` fonctionne différemment selon l’élément dans lequel il se trouve. Lorsque l’élément `FixedPortRange` se trouve dans l’élément `InternalEndpoint`, il ouvre tous les ports sur l’équilibreur de charge dans la plage des attributs min et max pour toutes les machines virtuelles sur lesquelles le rôle s’exécute. Lorsque l’élément `FixedPortRange` se trouve dans l’élément `InstanceInputEndpoint`, il ouvre seulement un port dans la plage des attributs min et max sur chaque machine virtuelle exécutant le rôle.

L’élément `FixedPortRange` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `FixedPortRange`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|min|int|Requis. Port minimal dans la plage. Les valeurs possibles sont comprises entre 1 et 65535 inclus (Kit SDK Azure version 1.7 ou supérieure).|
|max|chaîne|Requis. Port maximal dans la plage. Les valeurs possibles sont comprises entre 1 et 65535 inclus (Kit SDK Azure version 1.7 ou supérieure).|

##  <a name="Certificates"></a> Certificates
L’élément `Certificates` décrit la collection de certificats pour un rôle de travail. Cet élément est le parent de l’élément `Certificate`. Un rôle peut avoir n’importe quel nombre de certificats associés. Pour plus d’informations sur l’utilisation de l’élément certificates, consultez [Modifier le fichier de définition de service avec un certificat](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files).

##  <a name="Certificate"></a> Certificate
L’élément `Certificate` décrit un certificat associé à un rôle de travail.

Le tableau suivant décrit les attributs d’un de l’élément `Certificate`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|Nom|chaîne|Requis. Nom de ce certificat, utilisé pour y faire référence quand il est associé à un élément `InputEndpoint` HTTPS.|
|storeLocation|chaîne|Requis. Emplacement du magasin de certificats dans lequel se trouve ce certificat sur la machine locale. Les valeurs possibles sont `CurrentUser` et `LocalMachine`.|
|storeName|chaîne|Requis. Nom du magasin de certificats dans lequel se trouve ce certificat sur la machine locale. Les valeurs possibles comprennent les noms de magasin intégrés `My`, `Root`, `CA`, `Trust`, `Disallowed`, `TrustedPeople`, `TrustedPublisher`, `AuthRoot`, `AddressBook`, ou un nom de magasin personnalisé. Si un nom de magasin personnalisé est indiqué, le magasin est automatiquement créé.|
|permissionLevel|chaîne|facultatif. Spécifie les autorisations d’accès données aux processus de rôle. Si vous souhaitez que seuls les processus élevés puissent accéder à la clé privée, spécifiez l’autorisation `elevated`. Une autorisation `limitedOrElevated` permet à tous les processus de rôle d’accéder à la clé privée. Les valeurs possibles sont `limitedOrElevated` ou `elevated`. La valeur par défaut est `limitedOrElevated`.|

##  <a name="Imports"></a> Imports
L’élément `Imports` décrit une collection de modules d’importation pour un rôle de travail qui ajoutent des composants au système d’exploitation invité. Cet élément est le parent de l’élément `Import`. Cet élément est facultatif, et un rôle ne peut avoir qu’un seul bloc runtime.

L’élément `Imports` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.

##  <a name="Import"></a> Import
L’élément `Import` spécifie un module à ajouter au système d’exploitation invité.

L’élément `Import` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `Import`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|moduleName|chaîne|Requis. Nom du module à importer. Les modules d’importation valides sont les suivants :<br /><br /> -   RemoteAccess<br />-   RemoteForwarder<br />-   Diagnostics<br /><br /> Les modules RemoteAccess et RemoteForwarder vous permettent de configurer votre instance de rôle pour les connexions Bureau à distance. Pour plus d’informations, consultez [Activer une connexion Bureau à distance](cloud-services-role-enable-remote-desktop-new-portal.md).<br /><br /> Le module Diagnostics vous permet de collecter des données de diagnostic pour une instance de rôle.|

##  <a name="Runtime"></a> Runtime
L’élément `Runtime` décrit une collection de paramètres de variables d’environnement pour un rôle de travail qui contrôlent l’environnement d’exécution du processus hôte Azure. Cet élément est le parent de l’élément `Environment`. Cet élément est facultatif, et un rôle ne peut avoir qu’un seul bloc runtime.

L’élément `Runtime` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.

Le tableau suivant décrit les attributs de l’élément `Runtime` :

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|executionContext|chaîne|facultatif. Spécifie le contexte dans lequel le processus de rôle est lancé. Le contexte par défaut est `limited`.<br /><br /> -   `limited` – Le processus est lancé sans privilèges d’administrateur.<br />-   `elevated` – Le processus est lancé avec des privilèges d’administrateur.|

##  <a name="Environment"></a> Environment
L’élément `Environment` décrit une collection de paramètres de variables d'environnement pour un rôle de travail. Cet élément est le parent de l’élément `Variable`. Un rôle peut avoir n’importe quel nombre de variables d’environnement définies.

##  <a name="Variable"></a> Variable
L’élément `Variable` spécifie une variable d’environnement à définir dans le système d’exploitation invité.

L’élément `Variable` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.

Le tableau suivant décrit les attributs de l’élément `Variable` :

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|Nom|chaîne|Requis. Nom de la variable d'environnement à définir.|
|value|chaîne|facultatif. Valeur à définir pour la variable d’environnement. Vous devez inclure un attribut value ou un élément `RoleInstanceValue`.|

##  <a name="RoleInstanceValue"></a> RoleInstanceValue
L’élément `RoleInstanceValue` spécifie l’attribut xPath à partir duquel récupérer la valeur de la variable.

Le tableau suivant décrit les attributs d’un de l’élément `RoleInstanceValue`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|xpath|chaîne|facultatif. Chemin de l’emplacement des paramètres de déploiement pour l’instance. Pour plus d’informations, consultez [Variables de configuration avec XPath](cloud-services-role-config-xpath.md).<br /><br /> Vous devez inclure un attribut value ou un élément `RoleInstanceValue`.|

##  <a name="EntryPoint"></a> EntryPoint
L’élément `EntryPoint` spécifie le point d’entrée d’un rôle. Cet élément est le parent des éléments `NetFxEntryPoint`. Ces éléments vous permettent de spécifier une autre application que l’application par défaut WaWorkerHost.exe pour agir comme point d’entrée du rôle.

L’élément `EntryPoint` n’est disponible que par le biais du kit SDK Azure version 1.5 ou supérieure.

##  <a name="NetFxEntryPoint"></a> NetFxEntryPoint
L’élément `NetFxEntryPoint` spécifie le programme à exécuter pour un rôle.

> [!NOTE]
>  L’élément `NetFxEntryPoint` n’est disponible que par le biais du kit SDK Azure version 1.5 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `NetFxEntryPoint`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|assemblyName|chaîne|Requis. Chemin et nom de fichier de l’assembly contenant le point d’entrée. Le chemin est relatif au dossier **\\%ROLEROOT%\Approot** (ne spécifiez pas **\\%ROLEROOT%\Approot** dans `commandLine`, car il est supposé). **%ROLEROOT%** est une variable d’environnement gérée par Azure, qui représente l’emplacement du dossier racine de votre rôle. Le dossier **\\%ROLEROOT%\Approot** désigne le dossier d’application de votre rôle.|
|targetFrameworkVersion|chaîne|Requis. La version de .NET framework sur laquelle l’assembly a été créé. Par exemple : `targetFrameworkVersion="v4.0"`.|

##  <a name="ProgramEntryPoint"></a> ProgramEntryPoint
L’élément `ProgramEntryPoint` spécifie le programme à exécuter pour un rôle. L’élément `ProgramEntryPoint` permet de spécifier un point d’entrée de programme non basé sur un assembly .NET.

> [!NOTE]
>  L’élément `ProgramEntryPoint` n’est disponible que par le biais du Kit SDK Azure version 1.5 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `ProgramEntryPoint`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|commandLine|chaîne|Requis. Chemin d’accès, nom de fichier et arguments de ligne de commande du programme à exécuter. Le chemin d’accès est lié au dossier **%ROLEROOT%\Approot** (ne spécifiez pas **%ROLEROOT%\Approot** dans la ligne de commande, car cela est implicite). **%ROLEROOT%** est une variable d’environnement gérée par Azure, qui représente l’emplacement du dossier racine de votre rôle. Le dossier **%ROLEROOT%\Approot** désigne le dossier d’application de votre rôle.<br /><br /> Si le programme se termine, le rôle est recyclé. Par conséquent, en règle générale, définissez le programme pour qu’il continue à s’exécuter au lieu qu’il démarre et exécute simplement une tâche déterminée.|
|setReadyOnProcessStart|booléenne|Requis. Spécifie si l’instance de rôle doit attendre que le programme de ligne de commande signale son démarrage. Cette valeur doit être définie sur `true` à ce stade. La valeur `false` est réservée à un usage ultérieur.|

##  <a name="Startup"></a> Startup
L’élément `Startup` décrit une collection de tâches qui s’exécutent au démarrage du rôle. Cet élément peut être le parent de l’élément `Variable`. Pour plus d’informations sur l’utilisation des tâches de démarrage de rôle, consultez [Guide pratique pour configurer des tâches de démarrage](cloud-services-startup-tasks.md). Cet élément est facultatif et un rôle peut n’avoir qu’un seul bloc startup.

Le tableau suivant décrit l’attribut de l’élément `Startup`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|priority|int|À usage interne uniquement.|

##  <a name="Task"></a> Task
L’élément `Task` spécifie la tâche de démarrage qui s’exécute au démarrage du rôle. Les tâches de démarrage peuvent être utilisées pour réaliser des tâches en préparation de l’exécution du rôle, telles que l’installation de composants logiciels ou l’exécution d’autres applications. Les tâches s’exécutent dans l’ordre selon lequel elles apparaissent dans le bloc de l’élément `Startup`.

L’élément `Task` n’est disponible que par le biais du kit SDK Azure version 1.3 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `Task`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|commandLine|chaîne|Requis. Script, comme un fichier CMD, contenant les commandes à exécuter. La commande de démarrage et les fichiers de commandes doivent être enregistrés au format ANSI. Les formats de fichier qui définissent un indicateur d’ordre des octets au début du fichier ne sont pas traités correctement.|
|executionContext|chaîne|Spécifie le contexte dans lequel le script est exécuté.<br /><br /> -   `limited` [Par défaut] – Exécution avec les mêmes privilèges que le rôle qui héberge le processus.<br />-   `elevated` – Exécution avec des privilèges d’administrateur.|
|taskType|chaîne|Spécifie le comportement d’exécution de la commande.<br /><br /> -   `simple` [Par défaut] – Le système attend que la tâche se termine avant de lancer d’autres tâches.<br />-   `background` – Le système n’attend pas que la tâche se termine.<br />-   `foreground` – Similaire à background, à la différence près que le rôle n’est redémarré que quand toutes les tâches foreground sont terminées.|

##  <a name="Contents"></a> Contents
L’élément `Contents` décrit la collection du contenu d’un rôle de travail. Cet élément est le parent de l’élément `Content`.

L’élément `Contents` n’est disponible que par le biais du kit SDK Azure version 1.5 ou supérieure.

##  <a name="Content"></a> Content
L’élément `Content` définit l’emplacement source du contenu à copier sur la machine virtuelle Azure, et le chemin de destination dans lequel il est copié.

L’élément `Content` n’est disponible que par le biais du kit SDK Azure version 1.5 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `Content`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|destination|chaîne|Requis. Emplacement sur la machine virtuelle Azure dans lequel le contenu est placé. Cet emplacement est relatif au dossier **%ROLEROOT%\Approot**.|

Cet élément est le parent de l’élément `SourceDirectory`.

##  <a name="SourceDirectory"></a> SourceDirectory
L’élément `SourceDirectory` définit le répertoire local à partir duquel le contenu est copié. Utilisez cet élément pour spécifier le contenu local à copier sur la machine virtuelle Azure.

L’élément `SourceDirectory` n’est disponible que par le biais du kit SDK Azure version 1.5 ou supérieure.

Le tableau suivant décrit les attributs d’un de l’élément `SourceDirectory`.

| Attribut | Type | Description |
| --------- | ---- | ----------- |
|chemin d’accès|chaîne|Requis. Chemin relatif ou absolu d’un répertoire local dont le contenu doit être copié sur la machine virtuelle Azure. L’extension des variables d’environnement dans le chemin du répertoire est prise en charge.|

## <a name="see-also"></a>Voir aussi
[Schéma de définition de services cloud (classique)](schema-csdef-file.md)