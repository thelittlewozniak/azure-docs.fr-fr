---
title: Approuver ou rejeter des demandes de rôles Azure AD dans PIM – Azure Active Directory | Microsoft Docs
description: Découvrez comment approuver ou rejeter des demandes de rôles Azure AD dans Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 47e9033b33c70a72a1685696f75a442e88eed033
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60289722"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-pim"></a>Approuver ou rejeter des demandes de rôles Azure AD dans PIM

Avec Azure Active Directory (Azure AD) Privileged Identity Management (PIM), vous pouvez configurer des rôles afin d’exiger une approbation pour les activations. Vous pouvez également choisir un ou plusieurs utilisateurs ou groupes en tant qu’approbateurs délégués. Les approbateurs délégués ont 24 heures pour approuver les demandes. Si une demande n’est pas approuvée sous 24 heures, l’utilisateur éligible doit soumettre une nouvelle demande. Le délai d'approbation de 24 heures n’est pas configurable.

Suivez les étapes décrites dans cet article pour approuver ou refuser des demandes de rôles requête HTTP.

## <a name="view-pending-requests"></a>Afficher les demandes en attente

En tant qu’approbateur délégué, vous recevez une notification par e-mail quand une demande de rôle Azure AD est en attente d’approbation. Vous pouvez afficher les demandes en attente dans PIM.

1. Connectez-vous au [Portail Azure](https://portal.azure.com/).

1. Ouvrez **Azure AD Privileged Identity Management**.

1. Cliquez sur **Rôles Azure AD**.

1. Cliquez sur **Approuver les demandes**.

    ![Rôles PIM – Rôles](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    La liste des demandes en attente de votre approbation s’affiche.

## <a name="approve-requests"></a>Approuver les demandes

1. Sélectionnez les demandes que vous souhaitez approuver, puis cliquez sur **Approuver** pour ouvrir le volet Approuver des demandes sélectionnées.

    ![Liste des demandes d’approbation PIM](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. Dans la zone **Approuver le motif**, tapez une raison.

    ![PIM – Approuver des demandes sélectionnées](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Cliquez sur **Approuver**.

    Le symbole d’état sera actualisé avec votre approbation.

    ![PIM – Approuver des demandes sélectionnées](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Refuser des demandes

1. Sélectionnez les demandes que vous souhaitez refuser, puis cliquez sur **Refuser** pour ouvrir le volet Refuser des demandes sélectionnées.

    ![Liste des demandes d’approbation PIM](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. Dans la zone **Refuser le motif**, tapez une raison.

    ![PIM – Refuser des demandes sélectionnées](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Cliquez sur **Refuser**.

    Le symbole d’état sera actualisé avec votre refus.

## <a name="next-steps"></a>Étapes suivantes

- [Notifications par e-mail dans PIM](pim-email-notifications.md)
- [Approuver ou rejeter les demandes de rôles de ressources Azure dans PIM](pim-resource-roles-approval-workflow.md)
