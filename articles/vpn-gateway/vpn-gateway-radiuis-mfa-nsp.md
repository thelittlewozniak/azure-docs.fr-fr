---
title: Sécuriser l’authentification RADIUS de la passerelle VPN Azure avec un serveur NPS pour l’authentification multifacteur | Microsoft Docs
description: Décrit comment intégrer l’authentification RADIUS de la passerelle Azure avec un serveur NPS pour l’authentification multifacteur.
services: vpn-gateway
documentationcenter: na
author: ahmadnyasin
manager: willchen
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 4e11b1bc16f874f892288f9677a71023f483de7c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60458080"
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Intégrer l’authentification RADIUS de la passerelle VPN Azure avec un serveur NPS pour l’authentification multifacteur 

L’article décrit comment intégrer un serveur NPS (Network Policy Server) avec l’authentification RADIUS de passerelle VPN Azure pour fournir une authentification multifacteur (MFA) pour les connexions VPN point à site. 

## <a name="prerequisite"></a>Configuration requise

Pour activer l’authentification multifacteur, les utilisateurs doivent être dans Azure Active Directory (Azure AD), qui doit être synchronisé à partir de l’environnement local ou cloud. L’utilisateur doit également avoir terminé le processus d’inscription automatique pour l’authentification multifacteur.  Pour plus d’informations, voir [Configurer mon compte pour la vérification en deux étapes](../active-directory/user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="detailed-steps"></a>Procédure détaillée

### <a name="step-1-create-a-virtual-network-gateway"></a>Étape 1 : Créer une passerelle de réseau virtuel

1. Connectez-vous au [Portail Azure](https://portal.azure.com).
2. Dans le réseau virtuel qui hébergera la passerelle de réseau virtuel, sélectionnez **Sous-réseaux**, puis **Sous-réseau de passerelle** pour créer un sous-réseau. 

    ![Image relative à l’ajout d’une passerelle de sous-réseau](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Créez une passerelle de réseau virtuel en spécifiant les paramètres suivants :

    - **Type de passerelle** : Sélectionnez **VPN**.
    - **Type de VPN** : Sélectionnez **Route-based**.
    - **SKU** : Sélectionnez un type de référence SKU en fonction de vos exigences.
    - **Réseau virtuel** : Sélectionnez le réseau virtuel dans lequel vous avez créé le sous-réseau de passerelle.

        ![Image relative aux paramètres de passerelle de réseau virtuel](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>Étape 2 : configurer le serveur NPS pour Azure MFA

1. Sur le serveur NPS, [installez l’extension NPS pour Azure MFA](../active-directory/authentication/howto-mfa-nps-extension.md#install-the-nps-extension).
2. Ouvrez la console NSP, cliquez avec le bouton droit de la souris sur **Clients RADIUS**, puis sélectionnez **Nouveau**. Créez le client RADIUS en spécifiant les paramètres suivants :

    - **Nom convivial** : Saisissez un nom.
    - **Adresse (IP ou DNS)**  : Indiquez le sous-réseau de passerelle que vous avez créé à l’étape 1.
    - **Secret partagé** : saisissez une clé secrète et gardez-la en mémoire pour plus tard.

      ![Image relative aux paramètres du client RADIUS](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  Dans l’onglet **Avancé**, définissez le nom du fournisseur sur **RADIUS Standard** et assurez-vous que la case **Options supplémentaires** n’est pas cochée.

    ![Image relative aux paramètres avancés du client RADIUS](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. Accédez à **Stratégies** > **Stratégies réseau**, double-cliquez sur la stratégie relative aux **connexions au serveur de routage et d’accès à distance Microsoft**, sélectionnez  **Accorder l’accès**, puis cliquez sur **OK**.

### <a name="step-3-configure-the-virtual-network-gateway"></a>Étape 3 : configurer la passerelle de réseau virtuel

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Ouvrez la passerelle de réseau virtuel que vous avez créée. Assurez-vous que le type de passerelle est défini sur **VPN** et que le type VPN est **Route-based**.
3. Cliquez sur **Configuration point à site** > **Configurer maintenant**, puis spécifiez les paramètres suivants :

    - **Pool d’adresses** : Indiquez le sous-réseau de passerelle que vous avez créé à l’étape 1.
    - **Type d'authentification** : Sélectionnez **Authentification RADIUS**.
    - **Adresse IP du serveur** : Tapez l’adresse IP du serveur NPS.

      ![Image relative aux paramètres point à site](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Étapes suivantes

- [Azure Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md)
- [Intégration de votre infrastructure NPS existante à Azure Multi-Factor Authentication](../active-directory/authentication/howto-mfa-nps-extension.md)
