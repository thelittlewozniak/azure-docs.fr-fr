---
title: Vue d’ensemble de la sécurité Azure Service Fabric | Microsoft Docs
description: Cet article fournit une vue d’ensemble de la sécurité Azure Service Fabric.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: c5b5f80a43530fe6d0b90e65c3aef89a815157e4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60610885"
---
# <a name="azure-service-fabric-security-overview"></a>Vue d’ensemble de la sécurité Azure Service Fabric
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) est une plateforme de systèmes distribués qui facilite le packaging, le déploiement et la gestion de microservices évolutifs et fiables. Service Fabric gère les difficultés du développement et de la gestion des applications cloud. C’est pourquoi les développeurs et administrateurs peuvent éviter les problèmes d’infrastructure complexes et se concentrer sur l’implémentation de charges de travail stratégiques et astreignantes qui sont scalables et fiables.

Cet article est une vue d’ensemble des aspects de la sécurité d’un déploiement de Service Fabric.

## <a name="secure-your-cluster"></a>Sécuriser votre cluster
Azure Service Fabric est un orchestrateur de services sur un cluster de machines. Les clusters doivent être sécurisés pour empêcher les utilisateurs non autorisés de s’y connecter, surtout quand des charges de travail sont en cours d’exécution. Il est possible de créer un cluster non sécurisé, mais tous les utilisateurs anonymes risquent de s’y connecter (si les points de terminaison de gestion sont exposés sur l’Internet public).

Pour les clusters qui s’exécutent de façon autonome ou sur Azure, deux scénarios à prendre en compte sont la sécurité nœud à nœud et la sécurité client à nœud. Vous pouvez utiliser différentes technologies pour implémenter ces scénarios.

### <a name="node-to-node-security"></a>Sécurité nœud à nœud
La sécurité nœud à nœud s’applique à la communication entre les machines virtuelles ou les ordinateurs d’un cluster. Cette sécurité nœud à nœud garantit que seuls les ordinateurs autorisés à rejoindre le cluster peuvent participer à l’hébergement des applications et des services dans le cluster.

Les clusters qui s’exécutent sur Azure ou les clusters autonomes sur Windows peuvent utiliser la [Sécurité par certificat](https://msdn.microsoft.com/library/ff649801.aspx) ou la [Sécurité Windows](https://msdn.microsoft.com/library/ff649396.aspx) pour les ordinateurs Windows Server.

#### <a name="node-to-node-certificate-security"></a>Sécurité de certificat de nœud à nœud

Service Fabric utilise des certificats de serveur X.509 que vous spécifiez lorsque vous créez un cluster. Pour obtenir un rapide aperçu de ce que sont ces certificats et de la façon dont vous pouvez les acquérir ou les créer, consultez [Utilisation des certificats](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Configurez la sécurité par certificat quand vous créez le cluster en utilisant le portail Azure, des modèles Azure Resource Manager ou un modèle JSON autonome. Vous pouvez spécifier un certificat primaire et un certificat secondaire facultatif utilisé pour les renouvellements de certificats. Les certificats primaires et secondaires que vous spécifiez doivent être différents de ceux du client d’administration et du client en lecture seule que vous spécifiez pour la [sécurité client à nœud](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Sécurité client à nœud
Configurez la sécurité client à nœud à l’aide d’identités client. Pour établir une approbation entre un client et un cluster, vous devez configurer le cluster de façon à ce qu’il sache quelles identités client sont fiables.

Service Fabric prend en charge deux types de contrôle d’accès pour les clients qui sont connectés à un cluster Service Fabric :

-   **Administrateur** : Accès total aux fonctions de gestion, notamment les fonctionnalités de lecture/écriture.
-   **Utilisateur** : Accès uniquement en lecture aux fonctionnalités de gestion (par exemple, aux fonctionnalités de requête) et capacité à résoudre les applications et les services.

En utilisant le contrôle d’accès, les administrateurs de cluster peuvent limiter l’accès à certains types d’opérations de cluster. Cela rend le cluster plus sécurisé.

#### <a name="client-to-node-certificate-security"></a>Sécurité par certificat de client à nœud

Configurez la sécurité par certificat client à nœud quand vous créez le cluster en utilisant le portail Azure, des modèles Resource Manager ou un modèle JSON autonome. Vous devez spécifier un certificat client d’administration et/ou un certificat client utilisateur. Assurez-vous que ces certificats sont différents des certificats primaires et secondaires que vous spécifiez pour la sécurité nœud à nœud.

Les clients se connectant au cluster avec le certificat d’administration ont un accès complet aux fonctions de gestion. Les clients se connectant au cluster avec le certificat client utilisateur en lecture seule ont uniquement un accès en lecture aux fonctions de gestion. En d’autres termes, ces certificats sont utilisés pour le contrôle d’accès en fonction du rôle (RBAC).

Pour en savoir plus sur la configuration de la sécurité de certificat dans un cluster, consultez [Configurer un cluster à l’aide d’un modèle Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

#### <a name="client-to-node-azure-active-directory-security"></a>Sécurité Azure Active Directory de client à nœud

Les clusters s’exécutant sur Azure peuvent également sécuriser l’accès aux points de terminaison de gestion à l’aide d’Azure Active Directory (Azure AD). Pour plus d’informations sur la façon de créer les artefacts Azure Active Directory nécessaires, de les remplir pendant la création du cluster et de se connecter à ces clusters par la suite, consultez [Configurer un cluster en utilisant un modèle Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Azure AD permet aux organisation (appelées locataires) de gérer l’accès utilisateur aux applications. Il existe des applications avec une interface utilisateur de connexion Web et des applications avec une expérience client natif.

Un cluster Service Fabric offre différents points d’entrée pour leurs fonctionnalités de gestion, notamment les outils Service Fabric Explorer et Visual Studio. Par conséquent, vous allez créer deux applications Azure AD pour contrôler l’accès au cluster : une application web et une application native.

Pour les clusters Azure, nous vous recommandons d’utiliser la sécurité Azure AD pour authentifier les clients et les certificats pour la sécurité nœud à nœud.

Pour les clusters Windows Server autonomes avec Windows Server 2012 R2 et Active Directory, nous vous recommandons d’utiliser la sécurité Windows avec des comptes gMSA (group Managed Service Account). Sinon, continuez à utiliser la sécurité Windows avec les comptes Windows.

## <a name="understand-monitoring-and-diagnostics-in-service-fabric"></a>Comprendre la surveillance et les diagnostics dans Service Fabric
La [surveillance et les diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) sont essentiels au développement, au test et au déploiement d’applications et de services dans tout environnement. Les solutions Service Fabric conviennent mieux pour planifier et mettre en œuvre une surveillance et des diagnostics afin de s’assurer que les applications et les services fonctionnent comme prévu dans un environnement de développement local ou de production.

Du point de vue de la sécurité, les principaux objectifs de la surveillance et des diagnostics sont les suivants :

-   Détecter et diagnostiquer les problèmes de matériel et d’infrastructure pouvant être dus à un événement de sécurité.
-   Détecter les problèmes de logiciel et d’applications pouvant indiquer une compromission.
-   Comprendre la consommation des ressources afin d’empêcher les dénis de service faits par inadvertance.

Le flux de travail de la surveillance et des diagnostics se compose de trois étapes :

1.  **Génération d’événements** : La génération d’événements inclut les événements (journaux d’activité, traces, événements personnalisés) au niveau de l’infrastructure (cluster) et au niveau de l’application/du service. Pour comprendre ce qui est fourni et comment ajouter une instrumentation, vous pouvez poursuivre votre lecture sur les [événements de niveau infrastructure](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) et les [événements de niveau application](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app).

2.  **Agrégation d’événements** : Les événements générés doivent être collectés et agrégés pour pouvoir être affichés. En général, nous recommandons d’utiliser [Diagnostics Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (similaire à la collecte de journaux basée sur un agent) ou [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (collecte de journaux in-process).

3.  **Analyse** : Les événements doivent être visualisés et accessibles dans un certain format pour pouvoir être analysés et affichés. Il existe plusieurs plateformes pour l’analyse et la visualisation des données de surveillance et de diagnostics. Nous recommandons [journaux Azure Monitor](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) et [Azure Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) car ils s’intègrent bien avec Service Fabric.

Vous pouvez également utiliser [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) pour surveiller un grand nombre des ressources Azure sur lesquelles repose un cluster Service Fabric.

Un agent de surveillance est un service distinct qui peut surveiller l’intégrité et la charge des services, et créer des rapports d’intégrité pour n’importe quel élément de la hiérarchie du modèle d’intégrité. L’utilisation de l’agent de surveillance permet d’éviter des erreurs qui ne seraient pas détectées à partir de l’affichage d’un service unique. 

Les agents de surveillance sont également un bon emplacement pour héberger le code qui exécute des actions correctives sans intervention de l’utilisateur. Par exemple, le nettoyage de fichiers journaux dans un stockage à intervalles réguliers. Vous pouvez trouver un exemple de mise en œuvre de service de surveillance sur [l’échantillon d’agent de surveillance Azure Service Fabric](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-communication-by-using-certificates"></a>Sécuriser la communication à l’aide de certificats
Les certificats vous aident à sécuriser la communication entre les différents nœuds de votre cluster Windows autonome. À l’aide de certificats X.509, vous pouvez également authentifier les clients qui sont connectés à ce cluster. Cela garantit que seuls les utilisateurs autorisés peuvent accéder au cluster. Nous vous recommandons d’activer un certificat sur le cluster lors de sa création.

Les certificats numériques X.509 sont couramment utilisés pour authentifier les clients et serveurs. Ils sont également utilisés pour chiffrer et signer numériquement les messages.

Le tableau suivant répertorie les certificats dont vous aurez besoin pour la configuration de votre cluster :

|Définition des informations sur le certificat |Description|
|-------------------------------|-----------|
|ClusterCertificate|    Ce certificat est requis pour sécuriser les communications entre les nœuds sur un cluster. Vous pouvez utiliser deux certificats de cluster : un certificat principal et un certificat secondaire pour la mise à niveau.|
|ServerCertificate| Ce certificat est présenté au client lorsqu’il tente de se connecter à ce cluster. Vous pouvez utiliser deux certificats de serveur : un certificat principal et un certificat secondaire pour la mise à niveau.|
|ClientCertificateThumbprints|  Il s’agit d’un jeu de certificats à installer sur les clients authentifiés.|
|ClientCertificateCommonNames|  Il s’agit du nom commun du premier certificat client pour CertificateCommonName. CertificateIssuerThumbprint est l’empreinte numérique relative à l’émetteur de ce certificat.|
|ReverseProxyCertificate|   Il s’agit d’un certificat facultatif que vous pouvez spécifier pour sécuriser votre [Proxy inverse](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Pour plus d’informations sur la sécurisation des certificats, consultez [Sécuriser un cluster autonome sur Windows à l’aide de certificats X.509](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="understand-role-based-access-control"></a>Comprendre le contrôle d’accès en fonction du rôle
Spécifiez les rôles client utilisateur et administrateur au moment de la création du cluster en fournissant des identités distinctes (y compris des certificats) pour chacun. Pour plus d’informations sur les paramètres de contrôle d’accès par défaut et sur le changement des paramètres par défaut, consultez [Contrôle d’accès en fonction du rôle pour les clients Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-clusters-by-using-windows-security"></a>Sécuriser des clusters autonomes à l’aide de la sécurité Windows
Pour empêcher tout accès non autorisé à un cluster Service Fabric, vous devez sécuriser le cluster. La sécurité est particulièrement importante lorsque le cluster exécute des charges de travail de production. Configurez la sécurité de nœud à nœud et de client à nœud avec la sécurité Windows dans le fichier ClusterConfig.JSON.

Configurez la sécurité nœud à nœud en définissant [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) quand Service Fabric doit s’exécuter sous un compte gMSA. Pour créer des relations d’approbation entre les nœuds,vous devez faire en sorte qu’ils se connaissent mutuellement.

Configurez la sécurité nœud à nœud en définissant ClusterIdentity si vous souhaitez utiliser un groupe de machines dans un domaine Active Directory. Pour en savoir plus, consultez [Créer un groupe de machines dans Active Directory](https://msdn.microsoft.com/library/aa545347).

Configurez la sécurité client à nœud à l’aide de ClientIdentities. Vous devez configurer le cluster de façon à ce qu’il sache quelles identités clientes sont fiables. Vous pouvez établir cette approbation de deux manières :

-   Spécifiez les utilisateurs du groupe de domaines qui peuvent se connecter.
-   Spécifiez les utilisateurs du nœud de domaine qui peuvent se connecter.

## <a name="configure-application-security-in-service-fabric"></a>Configuration de la sécurité de l’application dans Service Fabric
### <a name="manage-secrets-in-service-fabric-applications"></a>Gérer les secrets dans les applications Service Fabric
Les secrets peuvent être des informations sensibles quelconques, notamment des chaînes de connexion de stockage, des mots de passe ou d’autres valeurs qui ne doivent pas être traitées en texte brut.

Vous pouvez utiliser [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) pour gérer les clés et les secrets. Toutefois, l’utilisation de secrets dans une application ne repose pas sur une plateforme cloud spécifique. Vous pouvez déployer des applications sur un cluster qui est hébergé n’importe où. Ce flux comprend quatre étapes principales :

1.  Obtenez un certificat de chiffrement de données.
2.  Installez le certificat dans votre cluster.
3.  Chiffrez les valeurs secrètes lors du déploiement d’une application avec le certificat et injectez-les dans le fichier de configuration Settings.xml d’un service.
4.  Lisez les valeurs chiffrées dans Settings.xml en les déchiffrant avec le même certificat de chiffrement.

Pour en savoir plus, consultez [Gérer les secrets dans des applications Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-an-application"></a>Configurer les stratégies de sécurité pour une application
Grâce à la sécurité Azure Service Fabric, vous pouvez sécuriser les applications en cours d’exécution dans le cluster sous différents comptes utilisateurs. La sécurité Service Fabric permet également de sécuriser les ressources que les applications utilisent au moment du déploiement sous les comptes d’utilisateur, par exemple les fichiers, les annuaires et les certificats. Ainsi, les applications en cours d’exécution sont mieux sécurisées, même dans un environnement hébergé partagé.

Les tâches de configuration des stratégies de sécurité sont les suivantes :

-   Configuration de la stratégie pour un point d’entrée de configuration de service
-   Lancement des commandes PowerShell à partir d’un point d’entrée de configuration
-   À l’aide de la console de redirection pour le débogage local
-   Configuration d’une stratégie pour les packages de code de service
-   Attribution d’une stratégie d’accès de sécurité pour les points de terminaison HTTP et HTTPS

## <a name="secure-communication-for-services"></a>Sécuriser la communication des services
La sécurité est un des aspects les plus importants de la communication. L’infrastructure d’application Reliable Services fournit quelques piles et outils de communication prédéfinis afin d’améliorer la sécurité. Pour plus d’informations, consultez [Sécuriser des communications de service à distance pour un service](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).

## <a name="next-steps"></a>Étapes suivantes
- Pour obtenir des informations conceptuelles sur la sécurité des clusters, consultez [Créer un cluster Service Fabric dans Azure à l’aide d’Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) et [Créer un cluster Service Fabric en utilisant le portail Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Pour en savoir plus sur la sécurité des clusters dans Service Fabric, consultez [Scénarios de sécurité du cluster Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
