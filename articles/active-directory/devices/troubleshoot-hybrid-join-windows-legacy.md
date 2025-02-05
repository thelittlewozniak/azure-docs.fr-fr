---
title: Dépanner des appareils hybrides de bas niveau joints à Azure Active Directory | Microsoft Docs
description: Dépannez des appareils hybrides de bas niveau joints à Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: joflore
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce1d2aaed85f61e2e18f696d749219a1b951c862
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66474285"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Dépanner des appareils hybrides de bas niveau joints à Azure Active Directory 

Cet article s’applique uniquement aux appareils suivants : 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Pour Windows 10 ou Windows Server 2016, consultez la page [Dépanner des appareils hybrides Windows 10 et Windows Server 2016 joints à Azure Active Directory](troubleshoot-hybrid-join-windows-current.md).

Cet article suppose que vous avez [configuré des appareils hybrides joints à Azure Active Directory](hybrid-azuread-join-plan.md) de façon à prendre en charge les scénarios suivants :

- Accès conditionnel basé sur les appareils


Cet article vous fournit des conseils sur la façon de résoudre les problèmes potentiels.  

**Bon à savoir :** 

- Le fonctionnement de la jonction Azure AD Hybride pour appareils Windows de bas niveau est légèrement différent dans Windows 10. De nombreux clients ne se rendent pas compte qu’ils ont besoin d’AD FS (pour les domaines fédérés) ou de l’authentification unique transparente configurée (pour les domaines managés).

- Pour les clients disposant de domaines fédérés : si le point de connexion de service a été configuré de sorte qu’il pointe vers le nom de domaine géré (par exemple contoso.onmicrosoft.com au lieu de contoso.com), la jonction Azure AD Hybride pour appareils Windows de bas niveau ne fonctionne pas.

- Pour le moment, le nombre maximal d’appareils par utilisateur s’applique également aux appareils de bas niveau joints par Azure AD Hybride. 

- Le même appareil physique apparaît plusieurs fois dans Azure AD si plusieurs utilisateurs de domaine se connectent aux appareils de bas niveau joints par Azure AD Hybride.  Par exemple, si *jdoe* et *jharnett* se connectent à un appareil, une inscription (DeviceID) distincte est créée pour chacun d’eux dans l’onglet d’informations **UTILISATEUR**. 

- Vous pouvez également obtenir plusieurs entrées pour un appareil dans l’onglet Informations utilisateur en raison d’une réinstallation du système d’exploitation ou d’une réinscription manuelle.

- L’inscription / jointure d’appareils initiale est configurée pour effectuer une tentative à la connexion ou au verrouillage / déverrouillage. Un délai de cinq minutes peut être déclenché par une tâche du Planificateur de tâches. 

- Si le système d’exploitation est Windows 7 SP1 ou Windows Server 2008 R2 SP1, vérifiez que [KB4284842](https://support.microsoft.com/help/4284842) est installé. Cette mise à jour empêche les échecs d’authentification qui se produisent lorsque le client perd l’accès aux clés protégées après avoir modifié le mot de passe.

## <a name="step-1-retrieve-the-registration-status"></a>Étape 1 : Récupérer l’état de l’inscription 

**Pour vérifier l’état de l’inscription :**  

1. Connectez-vous avec le compte d’utilisateur qui a effectué une jointure à Azure AD hybride.

2. Ouvrez l’invite de commandes. 

3. Saisissez `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Cette commande affiche une boîte de dialogue qui vous donne des détails sur l’état de la jonction.

![Workplace Join pour Windows](./media/troubleshoot-hybrid-join-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Étape 2 : Évaluer l’état de la jointure Azure AD hybride 

Si l’appareil n’a pas été joint par Azure AD Hybride, vous pouvez tenter d’effectuer une jonction Azure AD Hybride en cliquant sur le bouton Joindre. Si cette tentative échoue, les détails de cet échec s’affichent.


**Les tâches les plus courantes sont :**

- Une mauvaise configuration d’AD FS ou d’Azure AD, ou des problèmes de réseau

    ![Workplace Join pour Windows](./media/troubleshoot-hybrid-join-windows-legacy/02.png)
    
  - Autoworkplace.exe ne peut pas s’authentifier sans assistance auprès d’Azure AD ou d’AD FS. Cela peut être dû à l’absence ou à une mauvaise configuration d’AD FS (pour les domaines fédérés) ou de l’authentification unique transparente d’Azure AD (pour les domaines managés), ou encore à des problèmes de réseau. 
    
    - Cela peut signifier que l’authentification multifacteur (MFA) est activé/configuré pour l’utilisateur et WIAORMULTIAUTHN n’est pas configuré sur le serveur AD FS. 
     
    - Ce problème peut également découler du fait que la page de découverte du domaine d’accueil (HRD) attend une intervention de la part de l’utilisateur, ce qui empêche **autoworkplace.exe** de demander un jeton sans assistance.
     
    - Ce problème peut être lié au fait que les URL d’AD FS et d’Azure AD sont manquantes dans la zone intranet d’Internet Explorer sur le client.
     
    - Des problèmes de connectivité réseau empêchent peut-être **autoworkplace.exe** d’atteindre les URL d’AD FS ou d’Azure AD. 
     
    - **Autoworkplace.exe** , le client doit avoir une visibilité directe à partir du client de l’entreprise en local contrôleur de domaine Active Directory, ce qui signifie que cette jonction hybride Azure AD réussit uniquement lorsque le client est connecté à l’intranet de l’organisation.
     
    - Votre organisation utilise l’authentification unique Azure AD sans interruption, `https://autologon.microsoftazuread-sso.com` ou `https://aadg.windows.net.nsatc.net` ne sont pas présents dans les paramètres intranet Internet Explorer de l’appareil et l’option **Autoriser les mises à jour de la barre d’état via le script** n’est pas activée pour la zone Intranet.

- Vous n’êtes pas connecté en tant qu’utilisateur du domaine

    ![Workplace Join pour Windows](./media/troubleshoot-hybrid-join-windows-legacy/03.png)
    
    Cette situation peut se produire pour différentes raisons :
    
    - L’utilisateur connecté n’est pas un utilisateur de domaine (par exemple, un utilisateur local). La jointure Azure AD hybride sur les appareils de bas niveau est uniquement prise en charge pour les utilisateurs de domaine.
    
    - Le client ne peut pas se connecter à un contrôleur de domaine.    

- Un quota a été atteint.

    ![Workplace Join pour Windows](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- Le service ne répond pas. 

    ![Workplace Join pour Windows](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

Vous pouvez aussi trouver ces informations dans le journal des événements, sous : **Applications and Services Log\Microsoft-Workplace Join**
  
**Voici les causes les plus courantes d’échec d’une jointure Azure AD hybride :** 

- Votre ordinateur n’est pas connecté au réseau interne de votre organisation, ni à un VPN avec une connexion à votre contrôleur de domaine AD local.

- Vous êtes connecté à votre ordinateur avec un compte d’ordinateur local. 

- Problèmes de configuration du service : 

  - Le serveur AD FS n’a pas été configuré pour prendre en charge **WIAORMULTIAUTHN**. 

  - La forêt de votre ordinateur n’a aucun objet de point de connexion de service qui pointe vers votre nom de domaine vérifié dans Azure AD. 
  
  - Si votre domaine est managé, l’authentification unique transparente n’a pas été configurée ou ne fonctionne pas.

  - Un utilisateur a atteint la limite d’appareils. 

## <a name="next-steps"></a>Étapes suivantes

Pour toute question, consultez [FAQ sur la gestion des appareils](faq.md)  
