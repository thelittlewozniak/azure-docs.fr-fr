---
title: Utiliser PowerShell pour intégrer Azure Security Center et protéger votre réseau | Microsoft Docs
description: Ce document vous guide tout au long du processus d’intégration d’Azure Security Center à l’aide de cmdlets PowerShell.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/2/2018
ms.author: rkarlin
ms.openlocfilehash: 73043680ea7b8b63a329d0a457449b635b7b80f2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60703589"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>Automatiser l’intégration d’Azure Security Center à l’aide de PowerShell

Vous pouvez sécuriser vos charges de travail Azure par programme à l’aide du module PowerShell d’Azure Security Center.
PowerShell vous permet d’automatiser des tâches et d’éviter toute erreur humaine inhérente aux tâches manuelles. Cela est particulièrement utile dans le cadre de déploiements à grande échelle impliquant des dizaines d’abonnements avec des centaines de milliers de ressources qui doivent toutes être sécurisées dès le début.

L’intégration d’Azure Security Center à l’aide de PowerShell vous permet d’automatiser par programme l’intégration et la gestion de vos ressources Azure, ainsi que d’ajouter les contrôles de sécurité nécessaires.

Cet article fournit un exemple de script PowerShell que vous pouvez modifier et utiliser dans votre environnement pour déployer Azure Security Center vers vos abonnements. 

Dans cet exemple, nous allons activer Azure Security Center sur un abonnement dont l’ID est d07c0080-170c-4c24-861d-9c817742786c, puis appliquer les paramètres recommandés qui fournissent un niveau élevé de protection, en implémentant le niveau Standard d’Azure Security Center qui fournit des fonctionnalités avancées de détection des menaces et de protection contre celles-ci :

1. Définissez le [niveau de protection ASC Standard](https://azure.microsoft.com/pricing/details/security-center/). 
 
2. Définissez l’espace de travail Log Analytics auquel le Microsoft Monitoring Agent enverra les données collectées sur les machines virtuelles associées à l’abonnement. Dans cet exemple, il s’agit d’un espace de travail défini par l’utilisateur (myWorkspace).

3. Activez la fonctionnalité d’approvisionnement automatique d’agent d’Azure Security Center qui [déploie Microsoft Monitoring Agent](security-center-enable-data-collection.md#auto-provision-mma).

5. Définissez le [CISO de l’organisation en tant que contact de sécurité pour les événements notables et alertes ASC](security-center-provide-security-contact-details.md).

6. Assignez les [stratégies de sécurité par défaut](tutorial-security-policy.md) d’Azure Security Center.

## <a name="prerequisites"></a>Prérequis

Avant d’exécuter les cmdlets Azure Security Center, vous devez effectuer les étapes suivantes :

1.  Exécutez PowerShell en tant qu’administrateur.
2.  Exécutez les commandes suivantes dans PowerShell :
      
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Install-Module -Name Az.Security -Force

## <a name="onboard-security-center-using-powershell"></a>Intégrer Azure Security Center à l’aide de PowerShell

1.  Inscrivez vos abonnements auprès du fournisseur de ressources d’Azure Security Center :

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  Facultatif : définissez le niveau de couverture (niveau tarifaire) des abonnements (par défaut, le niveau tarifaire est défini sur Gratuit) :

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzSecurityPricing -Name "default" -PricingTier "Standard"

3.  Configurez un espace de travail Log Analytics auquel les agents rendront compte. Vous devez disposer d’un espace de travail Log Analytics que vous avez créé, auquel les machines virtuelles de l’abonnement rendront compte. Vous pouvez définir plusieurs abonnements rendant compte au même espace de travail. À défaut de définition, l’espace de travail par défaut est utilisé.

        Set-AzSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  Approvisionnez automatiquement l’installation du Microsoft Monitoring Agent sur vos machines virtuelles Azure :
    
        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > Nous vous recommandons d’activer l’approvisionnement automatique pour vous assurer que vos machines virtuelles Azure sont automatiquement protégées par Azure Security Center.
    >

5.  Facultatif : il est fortement recommandé de définir les contacts de sécurité pour les abonnements que vous intégrez, qui seront utilisés comme destinataires des alertes et notifications générées par Security Center :

        Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertAdmin -NotifyOnAlert 

6.  Assignez l’initiative de stratégie Azure Security Center par défaut :

        Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ '[Preview]: Enable Monitoring in Azure Security Center'}
        New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

Vous avez correctement intégré Azure Security Center avec PowerShell.

Vous pouvez désormais utiliser ces cmdlets PowerShell avec des scripts d’automatisation pour itérer par programme dans les abonnements et ressources. Cela permet de gagner du temps et réduit la probabilité d’erreur humaine. Vous pouvez utiliser cette [exemple de script](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) en tant que référence.






## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur la façon d’utiliser PowerShell pour automatiser l’intégration à Azure Security Center, voir l’article suivant :

* [Az.Security](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Security/Commands.Security/help/Az.Security.md).

Pour en savoir plus sur Azure Security Center, voir l’article suivant :

* [Définition des stratégies de sécurité dans Azure Security Center](tutorial-security-policy.md) : découvrez comment configurer des stratégies de sécurité pour vos groupes de ressources et abonnements Azure.
* [Gestion et résolution des alertes de sécurité dans Azure Security Center](security-center-managing-and-responding-alerts.md) : découvrez comment gérer et résoudre les alertes de sécurité.
* [FAQ Azure Security Center](security-center-faq.md) : forum aux questions concernant l’utilisation de ce service.
