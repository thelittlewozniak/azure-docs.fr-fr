---
title: Lier un compte Azure à un ID partenaire | Microsoft Docs
description: Suivez les engagements avec les clients Azure en liant les ID partenaires au compte utilisateur que vous utilisez pour gérer les ressources du client.
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: banders
ms.date: 03/12/2019
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 6bf61e2afd96e3923938ac4f815d34ae08f7c618
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64694221"
---
# <a name="link-a-partner-id-to-your-azure-accounts"></a>Lier un ID partenaire à vos comptes Azure

En tant que partenaire, vous pouvez suivre votre impact dans tous vos engagements avec les clients. Vous pouvez lier votre ID partenaire aux comptes utilisés pour gérer les ressources d’un client.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-access-from-your-customer"></a>Accéder à votre client

Avant de lier votre ID partenaire, votre client doit vous donner accès à ses ressources Azure en utilisant l’une des options suivantes :

- **Utilisateur invité** : votre client peut vous ajouter en tant qu’utilisateur invité et vous attribuer des rôles de contrôle d’accès en fonction du rôle (RBAC). Pour plus d’informations, consultez [Ajouter des utilisateurs invités à partir d’un autre répertoire](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Compte Active Directory** : votre client peut créer un compte d’utilisateur pour vous dans son propre répertoire et affecter un rôle RBAC.

- **Principal du service** : votre client peut ajouter une application ou un script provenant de votre organisation dans son répertoire et affecter un rôle RBAC. L’identité de l’application ou du script correspond à un principal du service.

## <a name="link-to-a-partner-id"></a>Lien vers un ID partenaire

Lorsque vous avez accès aux ressources du client, utilisez le portail Azure, PowerShell ou Azure CLI pour lier votre ID Microsoft Partner Network (ID MPN) à votre ID d’utilisateur ou principal du service. Liez l’ID partenaire dans chaque locataire client.

### <a name="use-the-azure-portal-to-link-to-a-new-partner-id"></a>Utiliser le portail Azure pour définir un lien vers un nouvel ID partenaire

1. Accédez à [Lien vers un ID partenaire](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) dans le portail Azure.

2. Connectez-vous au portail Azure.

3. Entrez l’ID partenaire Microsoft. L’ID partenaire est l’ID [Microsoft Partner Network](https://partner.microsoft.com/) de votre organisation.

   ![Capture d’écran qui montre l’option Lien vers un ID partenaire](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Pour lier un ID partenaire d’un autre client, changez de répertoire. Sous **Changer de répertoire**, sélectionnez votre répertoire.

   ![Capture d’écran montrant Changer de répertoire](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-to-a-new-partner-id"></a>Utiliser PowerShell pour lier vers un nouvel ID partenaire

1. Installez le module PowerShell [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner).

2. Connectez-vous au locataire du client avec le compte d’utilisateur ou le principal du service. Pour plus d’informations, consultez [Se connecter avec PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

   ```azurepowershell-interactive
    C:\> Connect-AzAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
   ```

3. Définissez le lien vers le nouvel ID partenaire. L’ID partenaire est l’ID [Microsoft Partner Network](https://partner.microsoft.com/) de votre organisation.

    ```azurepowershell-interactive
    C:\> new-AzManagementPartner -PartnerId 12345
    ```

#### <a name="get-the-linked-partner-id"></a>Obtenir l’ID partenaire lié
```azurepowershell-interactive
C:\> get-AzManagementPartner
```

#### <a name="update-the-linked-partner-id"></a>Mettre à jour l’ID partenaire lié
```azurepowershell-interactive
C:\> Update-AzManagementPartner -PartnerId 12345
```
#### <a name="delete-the-linked-partner-id"></a>Supprimer l’ID partenaire lié
```azurepowershell-interactive
C:\> remove-AzManagementPartner -PartnerId 12345
```

### <a name="use-the-azure-cli-to-link-to-a-new-partner-id"></a>Utiliser Azure CLI pour définir un lien vers un nouvel ID partenaire
1. Installez l’extension Azure CLI.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ```

2. Connectez-vous au locataire du client avec le compte d’utilisateur ou le principal du service. Pour plus d’informations, consultez [Se connecter avec Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ```

3. Définissez le lien vers le nouvel ID partenaire. L’ID partenaire est l’ID [Microsoft Partner Network](https://partner.microsoft.com/) de votre organisation.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Obtenir l’ID partenaire lié
```azurecli-interactive
C:\ az managementpartner show
```

#### <a name="update-the-linked-partner-id"></a>Mettre à jour l’ID partenaire lié
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
```

#### <a name="delete-the-linked-partner-id"></a>Supprimer l’ID partenaire lié
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
```

## <a name="next-steps"></a>Étapes suivantes

Participez à la discussion dans la [Communauté de partenaires Microsoft](https://aka.ms/PALdiscussion) pour recevoir des mises à jour ou envoyer des commentaires.

## <a name="frequently-asked-questions"></a>Questions fréquentes (FAQ)

**Qui peut lier l’ID partenaire ?**

Tout utilisateur de l’organisation partenaire qui gère les ressources Azure du client peut lier l’ID partenaire au compte.

**Un ID partenaire peut-il être modifié une fois qu’il est lié ?**

Oui. Un ID partenaire lié peut être modifié, ajouté ou supprimé.

**Que se passe-t-il si un utilisateur dispose d’un compte dans plusieurs locataires clients ?**

Le lien entre l’ID partenaire et le compte est établi pour chaque locataire client. Liez l’ID partenaire dans chaque locataire client.

**D’autres partenaires ou clients peuvent-ils modifier ou supprimer le lien vers l’ID partenaire ?**

Le lien est associé au niveau du compte d'utilisateur. Vous seul pouvez modifier ou supprimer le lien vers l’ID partenaire. Le client et les autres partenaires ne peuvent pas modifier le lien vers l’ID partenaire.


**Quel ID MPN utiliser si ma société en possède plusieurs ?**

Vous pouvez utiliser n’importe quel ID MPN valide à l’exception d’un ID MPN d’organisation virtuelle (v-org). La plupart des partenaires choisissent d’utiliser l’ID MPN de la zone géographique dans laquelle le client est basé ou les services remis.

**Où trouver un rapport sur le chiffre d’affaires influencé pour l’ID de partenaire lié ?**

Vous pouvez trouver un rapport sur le chiffre d’affaires influencé dans le [tableau de bord Insights Mes insights](https://partner.microsoft.com/membership/reports/myinsights). Vous devez sélectionner le lien d’administrateur partenaire en tant que le type d’association partenaire.

**Pourquoi ne puis-je pas voir mon client dans les rapports ?**

Vous ne voyez pas le client dans les rapports pour les raisons suivantes

1. Le compte d’utilisateur lié n’a pas d’[accès en fonction du rôle](https://docs.microsoft.com/azure/role-based-access-control/overview) à un abonnement ou à une ressource Azure du client.

2. L’abonnement Azure auquel l’utilisateur a [accès en fonction du rôle](https://docs.microsoft.com/azure/role-based-access-control/overview) n’est d’aucune utilité.

**L’option Lier un ID partenaire fonctionne-t-elle avec Azure Stack ?**

Oui, vous pouvez lier votre ID partenaire pour Azure Stack.
