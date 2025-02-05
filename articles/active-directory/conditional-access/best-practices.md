---
title: Meilleures pratiques l’accès conditionnel dans Azure Active Directory | Microsoft Docs
description: Découvrez-en davantage sur les éléments à connaître ce que vous devez évite lors de la configuration des stratégies d’accès conditionnel.
services: active-directory
keywords: accès conditionnel aux applications, accès conditionnel à Azure AD, accès sécurisé aux ressources d’entreprise, stratégies d’accès conditionnel
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/25/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67811e03bfa87a991b9eeb6f80ddddd87f781335
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66305732"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Meilleures pratiques l’accès conditionnel dans Azure Active Directory

Avec l’[accès conditionnel Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), vous pouvez contrôler la façon dont les utilisateurs autorisés accèdent à vos applications cloud. Cet article vous fournit des informations sur :

- ce que vous devez savoir ; 
- ce que vous devez éviter lors de la configuration des stratégies d’accès conditionnel. 

Cet article part du principe que vous connaissez les concepts et la terminologie décrits dans [Présentation de l’accès conditionnel dans Azure Active Directory](../active-directory-conditional-access-azure-portal.md).



## <a name="whats-required-to-make-a-policy-work"></a>Qu’est-ce qui est nécessaire pour faire fonctionner une stratégie ?

Lorsque vous créez une stratégie, aucun utilisateur, groupe, application ou contrôle d’accès n’est sélectionné.

![Applications cloud](./media/best-practices/02.png)


Pour que votre stratégie fonctionne, vous devez configurer les éléments suivants :


| Quoi           | Comment                                  | Pourquoi |
| :--            | :--                                  | :-- |
| **Applications cloud** |Sélection d’une ou de plusieurs applications.  | Une stratégie d’accès conditionnel a pour fonction de vous permettre de contrôler la manière dont les utilisateurs autorisés peuvent accéder à vos applications cloud.|
| **Utilisateurs et groupes** | Sélection d’au moins un utilisateur ou un groupe autorisé à accéder à vos applications cloud sélectionnées. | Une stratégie d’accès conditionnel, à laquelle aucun utilisateur ou groupe n’est affecté, n’est jamais déclenchée. |
| **Contrôles d’accès** | Sélection d’au moins un contrôle d’accès. | Si vos conditions sont remplies, votre processeur de stratégies doit connaître la procédure à suivre. |




## <a name="what-you-should-know"></a>Ce que vous devez savoir



### <a name="how-are-conditional-access-policies-applied"></a>Comment les stratégies d’accès conditionnel sont-elles appliquées ?

Plusieurs stratégies d’accès conditionnel peuvent s’appliquer lorsque vous accédez à une application cloud. Dans ce cas, toutes les stratégies qui s’appliquent doivent être satisfaites. Par exemple, si une première stratégie demande une authentification MFA et que la seconde demande un appareil conforme, vous devez passer par l’authentification multifacteur et utiliser un appareil compatible. 

Toutes les stratégies sont appliquées en deux phases :

- Dans la **première** phase, toutes les stratégies sont évaluées et tous les contrôles d’accès qui ne sont pas satisfaits sont collectés. 

- Dans la **deuxième** phase, vous êtes invité à vous conformer aux exigences que vous n’avez pas satisfaites. Si une des stratégies verrouille l’accès, vous êtes bloqué et n’êtes pas invité à répondre à d’autres contrôles de stratégie. Si aucune de ces stratégie ne vous bloque, vous êtes invité à répondre à d’autres contrôles de stratégie dans l’ordre suivant :

    ![Ordre](./media/best-practices/06.png)
    
    Fournisseurs d’authentification Multifacteur et les conditions d’utilisation externes venir.



### <a name="how-are-assignments-evaluated"></a>Comment les affectations sont-elles évaluées ?

Toutes les attributions sont reliées par l’opérateur logique **AND**. Si vous configurez plusieurs affectations, ces dernières doivent toutes être satisfaites pour qu’une stratégie soit déclenchée.  

Si vous souhaitez configurer une condition d’emplacement applicable à toutes les connexions non établies à partir du réseau de votre organisation, vous devez :

- inclure **Tous les emplacements** ;
- exclure **Toutes les adresses IP approuvées**.


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Que faire si votre accès au portail d’administration Azure AD est verrouillé ?

Si vous ne pouvez pas accéder au portail Azure AD en raison d’un paramètre incorrect dans une stratégie d’accès conditionnel :

- Vérifiez s’il existe d’autres administrateurs dans votre organisation dont l’accès n’a pas encore été verrouillé. Un administrateur ayant accès au portail Azure peut désactiver la stratégie qui affecte votre connexion. 

- Si aucun des administrateurs de votre organisation ne peut mettre à jour la stratégie, vous devez soumettre une demande de support. Le Support Microsoft peut consulter et mettre à jour les stratégies d’accès conditionnel qui empêchent l’accès.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Que se passe-t-il si vous avez configuré des stratégies dans le portail Azure Classic et le portail Azure ?  

Azure Active Directory applique les deux stratégies et l’utilisateur n’obtient l’accès que si toutes les conditions requises sont remplies.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Que se passe-t-il si vous disposez de stratégies dans le portail Intune Silverlight et dans le portail Azure ?

Azure Active Directory applique les deux stratégies et l’utilisateur n’obtient l’accès que si toutes les conditions requises sont remplies.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Que se passe-t-il si j’ai configuré plusieurs stratégies pour le même utilisateur ?  

À chaque connexion, Azure Active Directory évalue toutes les stratégies et vérifie que toutes les conditions requises sont remplies avant d’accorder l’accès à l’utilisateur. Le blocage de l’accès prévaut sur tous les autres paramètres de configuration. 


### <a name="does-conditional-access-work-with-exchange-activesync"></a>L’accès conditionnel fonctionne-t-il avec Exchange ActiveSync ?

Oui, vous pouvez utiliser Exchange ActiveSync dans une stratégie d’accès conditionnel avec certains [limitations](https://docs.microsoft.com/azure/active-directory/conditional-access/conditional-access-for-exo-and-spo#exchange-activesync). 

### <a name="how-should-you-configure-conditional-access-with-office-365-apps"></a>Comment devez-vous configurer l’accès conditionnel avec les applications Office 365 ?

Étant donné que les applications Office 365 sont interconnectées, nous vous recommandons d’affectation couramment applications utilisées ensemble lors de la création de stratégies.

Applications interconnectées courantes incluent Microsoft Flow, Microsoft Planner, Microsoft Teams, Office 365 Exchange Online, Office 365 SharePoint Online et Office 365 Yammer.

Il est important pour les stratégies qui requièrent des interactions de l’utilisateur, comme l’authentification multifacteur, lorsque l’accès est contrôlé au début d’une session ou une tâche. Si vous ne le faites, les utilisateurs ne pourront pas effectuer certaines tâches dans une application. Par exemple, si vous avez besoin de l’authentification multifacteur sur des appareils non gérés pour accéder à SharePoint, mais pas à la messagerie, les utilisateurs qui travaillent dans leur courrier électronique ne sera en mesure de joindre des fichiers de SharePoint à un message. Vous trouverez plus d’informations dans l’article [quelles sont les dépendances de service dans l’accès conditionnel Azure Active Directory ?](service-dependencies.md).





## <a name="what-you-should-avoid-doing"></a>Ce que vous devez éviter

L’infrastructure d’accès conditionnel vous offre une souplesse de configuration exceptionnelle. Toutefois, une grande souplesse signifie également que vous devez examiner soigneusement chaque stratégie de configuration avant de la mettre en œuvre afin d’éviter des résultats indésirables. Dans ce contexte, prêtez une attention particulière à l’affectation de jeux complets comme par exemple **tous les utilisateurs / groupes / applications cloud**.

Dans votre environnement, vous devez éviter les configurations suivantes :


**Pour tous les utilisateurs, toutes les applications cloud :**

- **Bloquer l’accès** : cette configuration bloque toute votre organisation, ce qui n’est pas une bonne idée.

- **Exiger un appareil conforme** : pour les utilisateurs qui n’ont pas encore inscrit leurs appareils, cette stratégie bloque tout accès, notamment l’accès au portail Intune. Si vous êtes un administrateur sans appareil inscrit, cette stratégie vous bloque et vous ne pouvez pas retourner dans le portail Azure pour modifier la stratégie.

- **Exiger la jonction de domaine** : ce blocage d’accès par stratégie peut également bloquer l’accès pour tous les utilisateurs de votre organisation si vous n’avez pas encore d’appareil joint à un domaine.

- **Exiger la stratégie de protection des applications** - ce bloc de stratégie accès est également susceptible de bloquer l’accès pour tous les utilisateurs de votre organisation si vous n’avez pas une stratégie Intune. Si vous êtes un administrateur sans une application cliente qui a une stratégie de protection d’application Intune, cette stratégie bloque le retour dans des portails tels que Intune et Azure.

**Pour tous les utilisateurs, toutes les applications cloud, toutes les plates-formes d’appareils :**

- **Bloquer l’accès** : cette configuration bloque toute votre organisation, ce qui n’est pas une bonne idée.


## <a name="how-should-you-deploy-a-new-policy"></a>Comment déployer une nouvelle stratégie ?

Pour commencer, vous devez évaluer votre stratégie à l’aide de [l’outil de simulation](what-if-tool.md).

Lorsque de nouvelles stratégies sont prêtes pour votre environnement, déployez-les en phases :

1. Appliquez une stratégie à un ensemble d’utilisateurs restreint et vérifiez qu’elle fonctionne comme prévu. 

2.  Lorsque vous développez une stratégie pour inclure davantage d’utilisateurs. Continuez à exclure tous les administrateurs de la stratégie pour vous assurer qu’ils ont toujours accès et qu’ils peuvent mettre à jour une stratégie si une modification est nécessaire.

3. Appliquez une stratégie à tous les utilisateurs uniquement si c’est nécessaire. 

Une bonne pratique consiste à créer un compte d’utilisateur :

- dédié à l’administration des stratégies ; 
- exclu de toutes vos stratégies.


## <a name="policy-migration"></a>Migration des stratégies

Envisagez d’effectuer la migration des stratégies que vous n’avez pas créées dans le portail Azure, car :

- Vous pouvez maintenant résoudre des scénarios que vous ne pouviez pas gérer auparavant.

- Vous pouvez réduire le nombre de stratégies que vous devez gérer en les consolidant.   

- Vous pouvez gérer toutes vos stratégies d’accès conditionnel dans un emplacement central.

- Le Portail Azure Classic a été mis hors service.   


Pour plus d’informations, consultez [Migrer les stratégies classiques dans le portail Azure](policy-migration.md).


## <a name="next-steps"></a>Étapes suivantes

Si vous voulez savoir :

- Comment configurer une stratégie d’accès conditionnel, consultez [Exiger une authentification multifacteur pour des applications spécifiques disposant d’un accès conditionnel Azure Active Directory](app-based-mfa.md).
- Comment planifier vos stratégies d’accès conditionnel, consultez [Guide pratique pour planifier votre déploiement de l’accès conditionnel dans Azure Active Directory](plan-conditional-access.md).
