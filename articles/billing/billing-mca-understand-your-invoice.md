---
title: Comprendre votre facture de Contrat client Microsoft | Microsoft Docs
description: Découvrez comment lire et comprendre les termes de votre facture MCA
services: ''
documentationcenter: ''
author: jureid
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: aee51793c66ae57f740300797b8fdc1799e685cd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65604041"
---
# <a name="understand-terms-on-your-microsoft-customer-agreement-invoice"></a>Comprendre les termes indiqués sur votre facture du Contrat client Microsoft

Cet article s’applique à un compte de facturation associé à un Contrat client Microsoft. [Vérifiez que vous avez accès à un Contrat client Microsoft](#check-access-to-a-microsoft-customer-agreement).

Votre facture fournit un résumé de vos frais et des instructions pour le paiement. Elle est disponible au téléchargement au format PDF (.pdf) à partir du [portail Azure](https://portal.azure.com/) ou peut être envoyée par courrier électronique. Pour plus d’informations, consultez [Afficher et télécharger votre facture Microsoft Azure](billing-download-azure-invoice.md).

<!-- ## When am I billed?

You are invoiced on a monthly basis. You can find out which day of the month you receive invoices by checking *invoice date* under billing profile properties in the [Azure portal](https://portal.azure.com/). Charges that occur between the end of the billing period and the invoice date are included in the next month's invoice, since they are in the next billing period. The billing period start and end dates for each invoice are listed in the invoice PDF above **Billing Summary**. -->

## <a name="invoice-terms-and-descriptions"></a>Termes et descriptions de la facture

Les sections suivantes répertorient les termes importants que vous voyez sur votre facture, ainsi qu’une description pour chacun d’entre eux.

### <a name="invoice-summary"></a>Récapitulatif de la facture

Le **récapitulatif de la facture** se trouve en haut de la première page et affiche des informations sur votre profil de facturation et votre mode de paiement.

![Section Récapitulatif de la facture](./media/billing-understand-your-invoice-mca/invoicesummary.png)

| Terme | Description |
| --- | --- |
| Vendu à |Adresse de votre entité juridique, qui se trouve dans les propriétés du compte de facturation.|
| Facturer à |Adresse associée au profil de facturation qui reçoit la facture, qui se trouve dans les propriétés de profil de facturation.|
| Profil de facturation |Nom du profil de facturation qui reçoit la facture. |
| Numéro de bon de commande number |Numéro de commande facultatif, attribué par vous pour le suivi |
| Numéro de facture |Numéro de facture unique généré par Microsoft, utilisé à des fins de suivi. |
| Date de facture |Date à laquelle la facture a été générée, généralement cinq à douze jours après la fin du cycle de facturation. Vous pouvez vérifier la date de votre facture dans les propriétés du profil de facturation.|
| Conditions de paiement |Mode de paiement de votre facture Microsoft. La mention *30 jours nets* signifie que le règlement doit être effectué dans les 30 jours suivant la date de facturation. |

### <a name="billing-summary"></a>Récapitulatif de facturation

Le **récapitulatif de facturation** affiche les frais par rapport au profil de facturation depuis la période de facturation précédente, ainsi que les crédits qui ont été appliqués, le cas échéant, les taxes et le montant total dû.

![Section Récapitulatif de facturation](./media/billing-understand-your-invoice-mca/billingsummary.png)

| Terme | Description |
| --- | --- |
| Charges|Montant total des frais appliqués par Microsoft à ce profil de facturation depuis la dernière période de facturation. |
| Crédits |Crédits que vous avez reçus suite à des retours. |
| Crédits Azure appliqués (Azure Credits Applied) | Crédits Azure appliqués automatiquement au montant facturé par Azure pour chaque période de facturation. |
| Sous-total |Le montant hors taxes dû. |
| Taxe |Type et montant des taxes à payer, selon le pays/la région de votre profil de facturation. Si vous n’êtes pas obligé de payer une taxe, cette zone ne s’affiche pas sur votre facture. |
| Économies totales estimées (Estimated Total Savings) |Estimation du montant total que vous avez économisé suite à des remises effectives. Le cas échéant, les taux de remise efficaces sont répertoriés sous les éléments de ligne d’achat dans la zone des détails par facture. |

### <a name="invoice-sections"></a>Sections de facture

Pour chaque section de la facture, sous votre profil de facturation, vous verrez apparaître les frais, le montant des crédits Azure appliqués, les taxes et le montant total dû.

`Total = Charges - Azure Credit + Tax`

### <a name="details-by-invoice-section"></a>Section des détails par facture

Ces détails indiquent le coût pour chaque section de facture, réparti par ordre de produit. Dans chaque commande du produit, le coût est réparti par type de service. Vous pouvez consulter les frais quotidiens associés à vos produits et services dans le Portail Microsoft Azure et à l’utilisation des fonctionnalités Azure, ainsi que les fichiers CSV associés. Pour en avoir plus, voir [Comprendre les frais de facture de votre contrat client de Microsoft](billing-mca-understand-your-bill.md).

Le montant total dû pour chaque famille de services est calculé via la soustraction du montant des *crédits Azure* des *crédits/frais*, puis l’ajout du montant de la *taxe* :

<!-- `Total = Charges/Credits - Azure Credit + Tax` -->

![Section des détails par facture](./media/billing-understand-your-invoice-mca/invoicesectiondetails.png)

| Terme |Description |
| --- | --- |
| Prix unitaire | Prix unitaire effectif du service (dans la devise de tarification) utilisé pour évaluer le taux d’utilisation. Il est unique pour un produit, une famille de services, un compteur et une offre. |
| Qté | Quantité de produits achetés ou consommés pendant la période de facturation. |
| Frais/crédits | Montant net des frais après application des crédits/remboursements. |
| Crédit Azure | Montant des crédits Azure appliqués aux frais/crédits.|
| Taux de la taxe | Taux d’imposition dépendant du pays/de la région. |
| Montant des taxes | Montant d’imposition appliqué pour l’achat basé sur les taux d’imposition. |
| Total | Montant total dû pour l’achat. |

### <a name="how-to-pay"></a>Comment acheter

En bas de la facture, vous trouverez des instructions de paiement. Vous pouvez payer par chèque, par virement ou en ligne. Si vous payez en ligne, vous pouvez utiliser une carte de crédit ou des crédits Azure, le cas échéant.

### <a name="publisher-information"></a>Informations sur l’éditeur

Si votre facture inclut des services tiers, le nom et l’adresse de chaque éditeur sont indiqués sur la partie inférieure.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Vérifier l’accès à un Contrat client Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Vous avez besoin d’aide ? Nous contacter

Si vous avez des questions ou besoin d’aide, créez une [demande de support](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Étapes suivantes

- [Comprendre les frais indiqués sur la facture de votre profil de facturation](billing-mca-understand-your-bill.md)
- [Obtention de votre facture Azure et de vos données d’utilisation quotidienne](billing-download-azure-invoice-daily-usage-date.md)
- [Afficher les tarifs Azure de votre organisation](billing-ea-pricing.md)
- [Afficher les documents de taxe de votre contrat de client de Microsoft](billing-mca-download-tax-document.md)
