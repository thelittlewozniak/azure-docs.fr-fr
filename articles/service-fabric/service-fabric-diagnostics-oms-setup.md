---
title: Azure Service Fabric - configurer la surveillance avec les journaux d’Azure Monitor | Microsoft Docs
description: Découvrez comment configurer les journaux d’Azure Monitor pour visualiser et analyser les événements à surveiller vos clusters Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/20/2019
ms.author: srrengar
ms.openlocfilehash: c8f7198b59a0fe7ed6775736f8b97f5b5a262640
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306860"
---
# <a name="set-up-azure-monitor-logs-for-a-cluster"></a>Configurer les journaux Azure Monitor pour un cluster

Nous vous recommandons d’utiliser les journaux Azure Monitor pour superviser les événements au niveau du cluster. Vous pouvez configurer un espace de travail Log Analytics à partir d’Azure Resource Manager, de PowerShell ou de la Place de marché Azure. Si vous gérez un modèle Resource Manager mis à jour de votre déploiement pour une utilisation ultérieure, utilisez le même modèle pour configurer votre environnement de journaux Azure Monitor. Le déploiement via la Place de marché est plus facile si vous avez déjà déployé un cluster et activé les diagnostics. Si vous ne disposez pas d’un accès de niveau abonnement pour le compte sur lequel vous effectuez le déploiement, déployez avec PowerShell ou le modèle Resource Manager.

> [!NOTE]
> Pour configurer les journaux Azure Monitor pour surveiller votre cluster, vous devez les diagnostics sont activés pour afficher les événements au niveau du cluster ou au niveau de la plateforme. Reportez-vous à [Agrégation et collecte d’événements à l’aide des diagnostics Windows Azure](service-fabric-diagnostics-event-aggregation-wad.md) et [Agrégation et collection d’événements à l’aide de Linux Azure Diagnostics](service-fabric-diagnostics-oms-syslog.md) pour en savoir plus

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Déployer un espace de travail Log Analytics à l’aide de la Place de marché Microsoft Azure

Si vous voulez ajouter un espace de travail Log Analytics après avoir déployé un cluster, accédez à la Place de marché Azure dans le portail, puis recherchez **Service Fabric Analytics**. Il s’agit d’une solution personnalisée pour les déploiements Service Fabric possédant des données spécifiques à Service Fabric. Dans ce processus, vous allez créer la solution (le tableau de bord pour afficher les informations) et l’espace de travail (l’agrégation des données de cluster sous-jacentes).

1. Sélectionnez **Nouveau** dans le menu de navigation gauche. 

2. Recherchez **Service Fabric Analytics**. Sélectionnez la ressource qui s’affiche.

3. Sélectionnez **Créer**.

    ![Service Fabric Analytics sur la Place de marché](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Dans la fenêtre de création Service Fabric Analytics, sélectionnez **Sélectionner un espace de travail** pour le champ **Espace de travail OMS**, puis **Créer un espace de travail**. Renseignez les entrées nécessaires. Ici, la seule exigence est que l’abonnement pour le cluster Service Fabric et celui pour l’espace de travail soient identiques. Quand vos entrées ont été validées, le déploiement de votre espace de travail commence. Ce déploiement ne prend que quelques minutes.

5. Une fois le déploiement terminé, sélectionnez une nouvelle fois **Créer** au bas de la fenêtre de création Service Fabric Analytics. Vérifiez que le nouvel espace de travail s’affiche sous **Espace de travail OMS**. Cette action ajoute la solution à l’espace de travail que vous avez créé.

Si vous utilisez Windows, poursuivez les étapes suivantes pour connecter des journaux Azure Monitor pour le compte de stockage où sont stockés vos événements de cluster. 

>[!NOTE]
>La solution Service Fabric Analytique est uniquement pris en charge pour les clusters Windows. Pour les clusters Linux, consultez notre article sur [comment configurer les journaux Azure Monitor pour les clusters Linux](service-fabric-diagnostics-oms-syslog.md).  

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Connecter l’espace de travail Log Analytics à votre cluster 

1. L’espace de travail a besoin d’être connecté aux données de diagnostic provenant de votre cluster. Accédez au groupe de ressources dans lequel vous avez créé la solution Service Fabric Analytics. Sélectionnez **ServiceFabric\<nomEspaceTravail\>** et accédez à sa page de présentation. C’est là que vous pourrez modifier les paramètres de la solution et de l’espace de travail, et accéder à l’espace de travail Log Analytics.

2. Dans le menu de navigation gauche, sous **Sources de données de l’espace de travail**, sélectionnez **Journaux d’activité de comptes de stockage**.

3. Dans la page **Journaux d’activité de comptes de stockage**, sélectionnez **Ajouter** en haut pour ajouter les journaux d’activité de votre cluster à l’espace de travail.

4. Sélectionnez **Compte de stockage** pour ajouter le compte approprié créé dans votre cluster. Si vous avez utilisé le nom par défaut, le compte de stockage est nommé **sfdg\<nomGroupeRessources\>** . Vous pouvez également le confirmer avec le modèle Azure Resource Manager utilisé pour déployer votre cluster, en vérifiant la valeur utilisée pour **applicationDiagnosticsStorageAccountName**. Si le nom n’apparaît pas, faites défiler vers le bas et sélectionnez **Charger plus**. Sélectionnez le nom du compte de stockage.

5. Spécifiez le type de données. Affectez-y la valeur **Événements de Service Fabric**.

6. Vérifiez que la source a automatiquement la valeur **WADServiceFabric\*EventTable**.

7. Sélectionnez **OK** pour connecter votre espace de travail aux journaux d’activité de votre cluster.

    ![Ajouter des journaux de compte de stockage dans les journaux d’Azure Monitor](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Le compte apparaît maintenant dans le cadre des journaux d’activité de votre compte de stockage dans les sources de données de votre espace de travail.

Vous avez ajouté la solution Service Fabric Analytics à un espace de travail Log Analytics qui est à présent connecté à la table du journal des applications et à la plateforme de votre cluster. Vous pouvez ajouter des sources supplémentaires à l’espace de travail de la même façon.


## <a name="deploy-azure-monitor-logs-with-azure-resource-manager"></a>Déployer des journaux Azure Monitor avec Azure Resource Manager

Lors du déploiement d’un cluster avec un modèle Resource Manager, celui-ci crée un espace de travail Log Analytics, y ajoute la solution Service Fabric et la configure pour lire des données provenant des tables de stockage appropriées.

Vous pouvez utiliser et modifier [cet exemple de modèle](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) pour répondre à vos besoins. Ce modèle effectue les actions suivantes :

* Crée un cluster Service Fabric à 5 nœuds
* Crée un espace de travail Log Analytics et une solution Service Fabric
* Configure l’agent Log Analytics pour collecter et envoyer 2 exemples de compteurs de performances à l’espace de travail
* Configure WAD pour collecter Service Fabric et les envoie vers les tables de stockage Azure (WADServiceFabric*EventTable)
* Configure l’espace de travail Log Analytics pour lire les événements à partir de ces tables


Vous pouvez déployer le modèle comme une mise à niveau Resource Manager à votre cluster à l’aide de la `New-AzResourceGroupDeployment` API dans le module Azure PowerShell. Voici un exemple de commande :

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "<resourceGroupName>" -TemplateFile "<templatefile>.json" 
``` 

Azure Resource Manager détecte que cette commande est une mise à jour d’une ressource existante. Il traite uniquement les modifications qui existent entre le modèle du déploiement existant et le nouveau modèle fourni.

## <a name="deploy-azure-monitor-logs-with-azure-powershell"></a>Déployer des journaux Azure Monitor avec Azure PowerShell

Vous pouvez également déployer votre ressource d’analytique de journal via PowerShell à l’aide de la `New-AzOperationalInsightsWorkspace` commande. Pour utiliser cette méthode, vérifiez que vous avez installé [Azure Powershell](https://docs.microsoft.com/powershell/azure/install-Az-ps). Utilisez ce script pour créer un espace de travail Log Analytics et lui ajouter la solution Service Fabric : 

```powershell

$SubID = "<subscription ID>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<Log Analytics workspace name>"
$solution = "ServiceFabric"

# Sign in to Azure and access the correct subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

Lorsque vous avez terminé, suivez les étapes décrites dans la section précédente pour connecter des journaux Azure Monitor pour le compte de stockage approprié.

Vous pouvez également ajouter d’autres solutions ou apporter d’autres modifications à votre espace de travail Log Analytics avec PowerShell. Pour plus d’informations, consultez [gérer Azure Monitor se connecte à l’aide de PowerShell](../azure-monitor/platform/powershell-workspace-configuration.md).

## <a name="next-steps"></a>Étapes suivantes
* [Déployez l’agent Log Analytics](service-fabric-diagnostics-oms-agent.md) sur vos nœuds pour collecter les compteurs de performances, ainsi que les statistiques et les journaux Docker de vos conteneurs
* Familiarisez-vous avec les [journal des requêtes et recherches](../log-analytics/log-analytics-log-searches.md) fonctionnalités offertes dans le cadre des journaux d’Azure Monitor
* [Utiliser le Concepteur de vue pour créer des vues personnalisées dans les journaux Azure Monitor](../azure-monitor/platform/view-designer.md)
