---
title: Montée en charge verticale des machines virtuelles Azure avec Azure Automation | Microsoft Docs
description: Mettre à l’échelle une machine virtuelle Linux en réponse aux alertes avec Azure Automation
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: dcee199e-fa25-44d5-9b25-df564cee9b45
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/18/2019
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d9bf155f24c947f8a27a38af01aedcf0b041b94
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966053"
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a>Évolution verticale des machines virtuelles Linux Azure avec Azure Automation
La mise à l’échelle verticale consiste à augmenter ou à réduire les ressources d’un ordinateur en réponse à la charge de travail. Dans Azure, cette opération est possible en modifiant la taille de la machine virtuelle. Cela peut vous aider dans les scénarios suivants

* Si la machine virtuelle n’est pas souvent utilisée, vous pouvez la réduire pour réduire les coûts mensuels
* Si la machine virtuelle présente un pic d’utilisation, sa taille peut être augmentée pour augmenter sa capacité maximale

Les grandes lignes des étapes sont présentées ci-dessous.

1. Configuration d’Azure Automation pour accéder à vos machines virtuelles
2. Importer les runbooks de mise à l’échelle verticale Azure Automation dans votre abonnement
3. Ajouter un webhook à votre runbook
4. Ajouter une alerte à votre machine virtuelle

> [!NOTE]
> En raison de la taille de la première machine virtuelle, les différentes tailles de mise à l’échelle peuvent être limitées en fonction de la disponibilité des autres tailles dans le cluster dans lequel la machine virtuelle actuelle est déployée. Dans les runbooks publiés et utilisés dans cet article, nous nous chargeons de ce cas et appliquons uniquement la mise à l’échelle dans les paires de machines virtuelles suivantes. Cela signifie qu’une machine virtuelle Standard_D1v2 ne peut pas de suite être convertie en Standard_G5 ou Basic_A0. Également la Machine virtuelle tailles échelle / n’est pas pris en charge. Vous pouvez choisir d’effectuer une mise à l’échelle entre les paires de tailles suivantes :
> 
> | Paires de mise à l’échelle des machines virtuelles |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_A1_v2 |Standard_A8_v2 |
> | Standard_A2m_v2 |Standard_A8m_v2  |
> | Standard_B1s |Standard_B2s |
> | Standard_B1ms |Standard_B8ms |
> | D1 standard |D4 standard |
> | D11 standard |D14 standard |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1_v2 |Standard_D5_v2 |
> | Standard_D11_v2 |Standard_D14_v2 |
> | Standard_DS1_v2 |Standard_DS5_v2 |
> | Standard_DS11_v2 |Standard_DS14_v2 |
> | Standard_D2_v3 |Standard_D64_v3 |
> | Standard_D2s_v3 |Standard_D64s_v3 |
> | Standard_DC2s |Standard_DC4s |
> | Standard_E2v3 |Standard_E64v3 |
> | Standard_E2sv3 |Standard_E64sv3 |
> | Standard_F1 |Standard_F16 |
> | Standard_F1s |Standard_F16s |
> | Standard_F2sv2 |Standard_F72sv2 |
> | G1_standard |G5_standard |
> | Standard_GS1 |Standard_GS5 |
> | Standard_H8 |Standard_H16 |
> | Standard_H8m |Standard_H16m |
> | Standard_L4s |Standard_L32s |
> | Standard_L8s_v2 |Standard_L80s_v2 |
> | Standard_M8ms  |Standard_M128ms |
> | Standard_M32ls  |Standard_M64ls |
> | Standard_M64s  |Standard_M128s |
> | Standard_M64  |Standard_M128 |
> | Standard_M64m  |Standard_M128m |
> | Standard_NC6 |Standard_NC24 |
> | Standard_NC6s_v2 |Standard_NC24s_v2 |
> | Standard_NC6s_v3 |Standard_NC24s_v3 |
> | Standard_ND6s |Standard_ND24s |
> | Standard_NV6 |Standard_NV24 |
> | Standard_NV6s_v2 |Standard_NV24s_v2 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Configuration d’Azure Automation pour accéder à vos machines virtuelles
La première chose à faire est de créer un compte Azure Automation qui hébergera les runbooks utilisés pour mettre à l’échelle les instances de jeu de mise à l’échelle de machines virtuelles. Depuis peu, le service Automation dispose de la fonctionnalité « Compte d'identification » qui facilite la configuration du Principal du service permettant d'exécuter automatiquement les runbooks au nom de l’utilisateur de façon très simple. Pour en savoir plus à ce sujet, consultez l’article ci-dessous :

* [Authentifier des Runbooks avec un compte d’identification Azure](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importer les runbooks de mise à l’échelle verticale Azure Automation dans votre abonnement
Les runbooks nécessaires à la mise à l’échelle verticale de votre machine virtuelle sont déjà publiés dans la galerie de runbooks Azure Automation. Vous devez les importer dans votre abonnement. Vous pouvez apprendre à importer des runbooks dans l’article suivant.

* [Galeries de runbooks et de modules pour Azure Automation](../../automation/automation-runbook-gallery.md)

Les runbooks qui doivent être importés sont affichés dans l’image ci-dessous

![Importer des runbooks](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Ajouter un webhook à votre runbook
Une fois que vous avez importé les runbooks, vous devrez ajouter un webhook au runbook afin qu’il puisse être déclenché par une alerte émanant d’une machine virtuelle. Les détails de la création d’un webhook pour votre runbook se trouvent ici

* [Webhooks Azure Automation](../../automation/automation-webhooks.md)

Veillez à bien copier le webhook avant de fermer la boîte de dialogue, car vous en aurez besoin dans la section suivante.

## <a name="add-an-alert-to-your-virtual-machine"></a>Ajouter une alerte à votre machine virtuelle
1. Sélectionnez les paramètres des machines virtuelles
2. Sélectionnez Règles d’alerte
3. Sélectionnez Ajouter une alerte
4. Sélectionnez une valeur pour déclencher l’alerte
5. Sélectionnez une condition, qui une fois remplie, va déclencher l’alerte
6. Sélectionnez un seuil pour la condition de l’étape 5. à remplir
7. Sélectionnez une période sur laquelle le service d’analyse vérifie l’état de la condition et du seuil dans les étapes 5 et 6
8. Collez le webhook que vous avez copié dans la section précédente.

![Ajouter une alerte à la machine virtuelle 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Ajouter une alerte à la machine virtuelle 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

