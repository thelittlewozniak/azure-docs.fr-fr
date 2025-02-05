---
title: Créer des sections sur votre facture pour organiser vos coûts - Azure | Microsoft Docs
description: Découvrez comment organiser les coûts avec des sections de facture.
services: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.topic: conceptual
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 21d6c1671c57341d785c002f360c05cc5c610657
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371611"
---
# <a name="create-sections-on-your-invoice-to-organize-your-costs"></a>Créer des sections sur votre facture pour organiser vos coûts

Créez des sections dans votre facture pour organiser les coûts par département, par environnement de développement ou en fonction des besoins de votre organisation. Autorisez ensuite d’autres utilisateurs à créer des abonnements Azure pour la section. Tous les frais d’utilisation ou les achats liés aux abonnements sont ensuite repris dans la section de la facture. Consultez le total des frais pour la section sur votre facture dans le portail Azure ou dans l’analyse des coûts Azure. Pour plus d’informations, consultez [Voir les transactions par section de facture](billing-mca-understand-your-bill.md#view-transactions-by-invoice-sections).

Cet article s’applique à un compte de facturation associé à un Contrat client Microsoft. [Vérifiez que vous avez accès à un Contrat client Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="create-an-invoice-section-in-the-azure-portal"></a>Créer une section de facture dans le portail Azure

Pour créer une section de facture, vous devez être **propriétaire de profil de facturation** ou **contributeur de profil de facturation**. Pour plus d’informations, consultez [Gérer les sections de facture associées au profil de facturation](billing-understand-mca-roles.md#manage-invoice-sections-for-billing-profile).

1. Connectez-vous au [Portail Azure](https://portal.azure.com).

2. Effectuez une recherche sur **Gestion des coûts + facturation**.

   ![Capture d’écran montrant la recherche dans le portail Azure](./media/billing-mca-section-invoice/billing-search-cost-management-billing.png)

3. Sélectionnez **Sections de facture** dans le volet de gauche. Selon votre accès, vous devrez peut-être sélectionner un compte ou profil de facturation, puis sélectionner **Sections de facture**.

   ![Capture d’écran montrant la recherche dans le portail Azure](./media/billing-mca-section-invoice/billing-mca-list-invoice-sections.png)

4. En haut de la page, sélectionnez **Ajouter**.

5. Entrez le nom de la section de facture.

   ![Capture d’écran montrant la page de création de la section de facture](./media/billing-mca-section-invoice/mca-create-invoice-section.png)

6. Sélectionnez un profil de facturation. Tous les frais d’utilisation ou les achats liés à la section de facture seront repris dans la facture du profil de facturation.

7. Sélectionnez **Créer**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Vérifier l’accès à un Contrat client Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="next-steps"></a>Étapes suivantes

- [Créer un abonnement Azure associé dans le cadre de votre Contrat client Microsoft](billing-mca-create-subscription.md)
- [Autoriser d’autres utilisateurs à créer des abonnements Azure](billing-mca-create-subscription.md#give-others-permission-to-create-azure-subscriptions)
- [Obtenir la propriété de facturation des abonnements des utilisateurs d’autres comptes de facturation](billing-mca-request-billing-ownership.md)

## <a name="need-help-contact-support"></a>Vous avez besoin d’aide ? Contacter le support technique

Si vous avez toujours besoin d’aide, [contactez le support technique](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pour obtenir une prise en charge rapide de votre problème.
