---
title: FAQ et problèmes connus en lien avec les identités managées pour ressources Azure
description: Problèmes connus en lien avec les identités managées pour ressources Azure.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a6797c7bd0c6bd8ce8d3f51b42cb4c2b1338fd6
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65950486"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>FAQ et problèmes connus en lien avec les identités managées pour ressources Azure

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Forum Aux Questions (FAQ)

> [!NOTE]
> Identités managées pour les ressources Azure est le nouveau nom du service anciennement nommé Managed Service Identity (MSI).

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>Les identités managées pour ressources Azure fonctionnent-elles avec Azure Cloud Services ?

Non, il n’existe aucun plan de prise en charge des identités managées pour ressources Azure dans Azure Cloud Services.

### <a name="does-managed-identities-for-azure-resources-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Les identités managées pour ressources Azure fonctionnent-elles avec la bibliothèque d’authentification Active Directory (ADAL) et la bibliothèque d’authentification Microsoft (MSAL) ?

Non, les identités managées pour ressources Azure ne sont pas encore intégrées avec les bibliothèques ADAL et MSAL. Pour plus d’informations sur l’acquisition d’un jeton pour les entités gérées pour les ressources Azure à l’aide du point de terminaison REST, consultez [l’utilisation des identités gérées pour les ressources Azure sur une machine virtuelle Azure pour acquérir un jeton d’accès](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>Quelle est la limite de sécurité des identités managées pour ressources Azure ?

La limite de sécurité de l’identité est la ressource à laquelle elle est attachée. Par exemple, la limite de sécurité activée pour une machine virtuelle avec des identités managées pour ressources Azure est la machine virtuelle. Tout code s’exécutant sur cette machine virtuelle est en mesure d’appeler les identités managées pour des jetons de point de terminaison et de demande de ressources Azure. L’expérience est similaire à celle d’autres ressources qui prennent en charge les identités managées pour ressources Azure.

### <a name="what-identity-will-imds-default-to-if-dont-specify-the-identity-in-the-request"></a>Quelle est l’identité utilisée par défaut par IMDS si vous ne spécifiez pas l’identité dans la demande ?

- Si l’identité managée affectée par le système est activée et qu’aucune identité n’est spécifiée dans la demande, IMDS utilise par défaut l’identité managée affectée par le système.
- Si l’identité managée affectée par le système n’est pas activée et qu’il n’existe qu’une seule identité managée affectée par l’utilisateur, IMDS utilise par défaut l’identité managée affectée par ce seul utilisateur. 
- Si l’identité managée affectée par le système n’est pas activée et qu’il existe plusieurs identités managées affectées par l’utilisateur, la spécification d’une identité managée dans la demande est obligatoire.

### <a name="should-i-use-the-managed-identities-for-azure-resources-imds-endpoint-or-the-vm-extension-endpoint"></a>Dois-je utiliser des identités gérées pour le point de terminaison IMDS ressources Azure ou le point de terminaison d’extension de machine virtuelle ?

Lorsque vous utilisez des identités gérées pour les ressources Azure avec des machines virtuelles, nous recommandons d’utiliser le point de terminaison IMDS. Azure Instance Metadata Service (IMDS) est un point de terminaison REST accessible à toutes les machines virtuelles IaaS créées par le biais d’Azure Resource Manager. 

Voici quelques avantages liés à l’utilisation d’identités managées pour ressources Azure sur IMDS :
- Tous les systèmes d’exploitation pris en charge par Azure IaaS peuvent utiliser des identités managées pour ressources Azure sur IMDS.
- N’est plus nécessaire d’installer une extension sur votre machine virtuelle pour activer les identités managées pour ressources Azure. 
- Les certificats utilisés par les identités managées pour ressources Azure ne sont plus présents dans la machine virtuelle.
- Le point de terminaison IMDS est une adresse IP non routable bien connue, uniquement accessible à partir de la machine virtuelle.
- 1000 affectée à l’utilisateur des identités gérées peuvent recevoir à une seule machine virtuelle. 

Les identités gérées pour l’extension de machine virtuelle de ressources Azure est toujours disponible. Toutefois, nous développons ne sont plus des nouvelles fonctionnalités dessus. Nous vous recommandons de passer pour utiliser le point de terminaison IMDS. 

Certaines des limitations de l’utilisation du point de terminaison d’extension de machine virtuelle sont :
- Prise en charge limitée pour les distributions de Linux : Stable de CoreOS, CentOS 7.1, 7.2 de Red Hat, Ubuntu 15.04, Ubuntu 16.04
- Seules 32 identités gérées affectée à l’utilisateur peuvent avoir à la machine virtuelle.


Remarque : Les identités gérées pour l’extension de machine virtuelle de ressources Azure ne seront pas prise en charge dans janvier 2019. 

Pour plus d’informations sur Azure Instance Metadata Service, voir la [documentation d’IMDS](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service).

### <a name="will-managed-identities-be-recreated-automatically-if-i-move-a-subscription-to-another-directory"></a>Les identités managées sont-elles recréées automatiquement si je déplace un abonnement vers un autre annuaire ?

Non. Si vous déplacez un abonnement vers un autre annuaire, vous devez les recréer manuellement et accorder à nouveau les affectations de rôle RBAC d’Azure.
- Pour les identités managées affectées par le système : désactivez et réactivez-les. 
- Pour les identités managées affectées par l’utilisateur : supprimez, recréez et rattachez-les aux ressources nécessaires (par exemple des machines virtuelles)

### <a name="can-i-use-a-managed-identity-to-access-a-resource-in-a-different-directorytenant"></a>Puis-je utiliser une identité managée pour accéder à une ressource dans un autre annuaire/locataire ?

Non. Les identités managées ne prennent actuellement pas en charge les scénarios entre annuaires. 

### <a name="what-azure-rbac-permissions-are-required-to-managed-identity-on-a-resource"></a>Quelles sont les autorisations RBAC Azure sont nécessaires pour une identité gérée sur une ressource ? 

- Identité gérée attribué par le système : Vous devez écrire des autorisations sur la ressource. Exemple : Microsoft.Compute/virtualMachines/write ou cette action est incluse dans les rôles intégrés spécifiques, tels que des ressources [collaborateur de Machine virtuelle](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#virtual-machine-contributor).
- Identité gérée affectée à l’utilisateur : Vous devez écrire des autorisations sur la ressource. Exemple : Microsoft.Compute/virtualMachines/write. En plus de [opérateur d’identité gérés](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-identity-operator) attribution de rôle sur l’identité gérée. 

### <a name="how-do-you-restart-the-managed-identities-for-azure-resources-extension"></a>Comment redémarrer l’extension des identités managées pour ressources Azure ?
Sur Windows et certaines versions de Linux, si l’extension s’arrête, la cmdlet suivante peut être utilisée pour redémarrer manuellement l’extension :

```powershell
Set-AzVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Où : 
- Type et nom de l’extension pour Windows : ManagedIdentityExtensionForWindows
- Type et nom de l’extension pour Linux : ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Problèmes connus

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>Le « Script d’automatisation » échoue lors de la tentative d’exportation de schéma pour l’extension des entités managées pour ressources Azure

Lorsque des identités managées pour ressources Azure sont activées sur une machine virtuelle, l’erreur suivante s’affiche en cas de tentative d’utiliser la fonctionnalité « Script d’automatisation » pour la machine virtuelle ou son groupe de ressources :

![Erreur d’exportation de script d’automatisation d’identités managées pour ressources Azure](./media/msi-known-issues/automation-script-export-error.png)

L’extension de machine virtuelle des identités managées pour ressources Azure (dont l’abandon est prévu en janvier 2019) ne prend pas en charge actuellement la possibilité d’exporter son schéma vers un modèle de groupe de ressources. Par conséquent, le modèle généré n’affiche pas les paramètres de configuration permettant d’activer des identités managées pour ressources Azure sur la ressource. Ces sections peuvent être ajoutées manuellement en suivant les exemples fournis dans [Configurer des identités managées pour ressources Azure sur une machine virtuelle Azure en utilisant un modèle](qs-configure-template-windows-vm.md).

Lorsque la fonctionnalité d’exportation de schéma sera disponible pour l’extension de machine virtuelle des identités managées pour ressources Azure (dont l’abandon est prévu en janvier 2019), elle sera répertoriée dans [Exportation de groupes de ressources contenant des extensions de machine virtuelle](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>La machine virtuelle ne démarre pas après avoir été déplacée d’un groupe de ressources ou d’un abonnement

Si vous déplacez une machine virtuelle en cours d’exécution, elle continue de s’exécuter pendant le déplacement. Toutefois, si la machine virtuelle est arrêtée et redémarrée après le déplacement, elle ne démarre plus. Ce problème se produit parce que la machine virtuelle ne met pas à jour la référence à l’identité des identités managées pour ressources Azure, mais continue de pointer dessus dans l’ancien groupe de ressources.

**Solution de contournement** 
 
Déclenchez une mise à jour sur la machine virtuelle afin qu’elle obtienne les valeurs correctes pour les identités managées pour ressources Azure. Vous pouvez modifier les propriétés d’une machine virtuelle pour mettre à jour la référence à l’identité des identités managées pour ressources Azure. Par exemple, vous pouvez définir une nouvelle valeur de balise sur la machine virtuelle avec la commande suivante :

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Cette commande définit une nouvelle balise « fixVM » avec une valeur de 1 sur la machine virtuelle. 
 
En définissant cette propriété, la machine virtuelle se met à jour avec l’URI de ressource appropriée des identités managées pour ressources Azure. Vous devriez ensuite être en mesure de démarrer la machine virtuelle. 
 
Une fois que la machine virtuelle est démarrée, la balise peut être supprimée en utilisant la commande suivante :

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

### <a name="vm-extension-provisioning-fails"></a>Échec de l’approvisionnement d’une extension de machine virtuelle

L’approvisionnement de l’extension de machine virtuelle peut échouer en raison d’échecs de recherche DNS. Redémarrez la machine virtuelle, puis réessayez.
 
> [!NOTE]
> L’extension de machine virtuelle doit être abandonnée d’ici à janvier 2019. Nous vous recommandons d’effectuer la transition à l’aide du point de terminaison IMDS.

### <a name="transferring-a-subscription-between-azure-ad-directories"></a>Transfert d’un abonnement entre des répertoires Azure AD

Les identités managées ne sont pas mises à jour lorsqu’un abonnement est déplacé/transféré vers un autre répertoire. Par conséquent, toutes les identités managées inexistantes attribuées par le système ou un utilisateur seront rompues. 

Solution de contournement pour les entités gérées dans un abonnement qui a été déplacé vers un autre répertoire :

 - Pour les identités managées affectées par le système : désactivez et réactivez-les. 
 - Pour les identités managées affectées par l’utilisateur : supprimez, recréez et rattachez-les aux ressources nécessaires (par exemple des machines virtuelles)

### <a name="moving-a-user-assigned-managed-identity-to-a-different-resource-groupsubscription"></a>Déplacement d’une identité gérée affectée à l’utilisateur à un groupe de ressources différent/abonnement

Le déplacement d’une identité managée affectée par l’utilisateur à un autre groupe de ressources entraînera l’arrêt de l’identité. Par conséquent, les ressources (par exemple, machine virtuelle) à l’aide de cette identité ne seront pas en mesure de demander des jetons pour celui-ci. 
