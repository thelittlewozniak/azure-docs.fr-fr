---
title: Rapport de connexions risquées dans le portail Azure Active Directory | Microsoft Docs
description: En savoir plus sur le rapport des connexions risquées dans le portail Azure Active Directory
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40e125f8e1e7909c5866a03c0571f49ec42d690a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60287395"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Rapport de connexions à risque dans le portail Azure Active Directory

Azure Active Directory (Azure AD) détecte les actions suspectes liées aux comptes des utilisateurs. Pour chaque action détectée, un enregistrement appelé **événement à risque** est créé. Pour en savoir plus, consultez [Événements à risque dans Azure AD](concept-risk-events.md). 

Vous pouvez accéder aux rapports de sécurité depuis le [portail Azure](https://portal.azure.com) en sélectionnant le panneau **Azure Active Directory** et en accédant à la section **Sécurité**. 

Il existe deux rapports de sécurité distincts dont le calcul dépend des événements à risque :

- **Connexions risquées** : une connexion risquée est une tentative de connexion susceptible de provenir d’un utilisateur autre que le propriétaire légitime d’un compte d’utilisateur.

- **Utilisateurs avec indicateur de risque** : il s’agit d’un compte d’utilisateur susceptible d’être compromis. 

![les connexions risquées.](./media/concept-risky-sign-ins/10.png)

Pour savoir comment configurer les stratégies qui déclenchent ces événements à risque, consultez [Guide pratique pour configurer la stratégie sur les risques liés à l’utilisateur](../identity-protection/howto-user-risk-policy.md).  

## <a name="who-can-access-the-risky-sign-ins-report"></a>Qui peut accéder au rapport sur les connexions risquées ?

Les rapports sur les connexions risquées sont accessibles aux utilisateurs ayant les rôles suivants :

- Security Administrator
- Administrateur général
- Lecteur de sécurité

Pour découvrir comment attribuer des rôles d’administrateur à un utilisateur dans Azure Active Directory, consultez l’article [Afficher et attribuer des rôles d’administrateur dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>De quelle licence Azure AD avez-vous besoin pour accéder à un rapport de sécurité ?  

Toutes les éditions d’Azure AD vous indiquent les rapports de connexions risquées. Toutefois, le niveau de granularité d’un rapport varie entre les éditions : 

- Dans les **éditions Azure Active Directory Free et Basic**, vous obtenez la liste des connexions risquées. 

- L’édition **Azure Active Directory Premium 1** vous permet également d’examiner certains événements à risque sous-jacent, qui ont été détectés pour chaque rapport. 

- L’édition **Azure Active Directory Premium 2** vous fournit les informations les plus détaillées sur tous les événements à risque sous-jacent. Elle vous permet également de configurer des stratégies de sécurité répondant automatiquement aux niveaux de risque configurés.

## <a name="risky-sign-ins-report-for-azure-ad-free-and-basic-edition"></a>Rapport sur les connexions risquées pour les éditions Azure AD Free et Basic

Les éditions Azure AD Free et Basic vous fournissent la liste des connexions risquées qui ont été détectées pour vos utilisateurs. Chaque enregistrement contient les attributs suivants :

- **Utilisateur** : nom d’utilisateur qui a été utilisé pendant l’opération de connexion.
- **IP** : adresse IP du périphérique qui a été utilisé pour la connexion à Azure Active Directory.
- **Emplacement** : emplacement utilisé pour la connexion à Azure Active Directory. Il s’agit d'une approximation du meilleur effort basée sur les suivis, les données de registre, les recherches inversées et d’autres informations.
- **Heure de connexion** : heure à laquelle la connexion a été effectuée.
- **État** : état de la connexion.

![les connexions risquées.](./media/concept-risky-sign-ins/01.png)

Selon votre analyse de la connexion à risque, vous pouvez envoyer des commentaires à Azure AD en effectuant les actions suivantes :

- Résoudre
- Marquer comme faux positif
- Ignorer
- Réactiver

![les connexions risquées.](./media/concept-risky-sign-ins/21.png)

Ce rapport fournit aussi une option permettant de :

- Rechercher des ressources
- Télécharger les données du rapport

![les connexions risquées.](./media/concept-risky-sign-ins/93.png)

## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Rapport sur les connexions risquées pour les éditions Azure AD Premium

Le rapport sur les connexions risquées dans les éditions Azure AD Premium vous fournit les informations suivantes :

- Informations agrégées sur les [types d’événements à risque](concept-risk-events.md) qui ont été détectés. Avec l’édition **Azure AD Premium P1**, les détections qui ne sont pas couvertes par la licence s’affichent comme des événements à risque **Connexion avec un risque supplémentaire détecté**. Avec l’édition **Azure AD Premium P2**, vous obtenez des informations très détaillées sur toutes les détections sous-jacentes.

- Une option pour télécharger le rapport

![les connexions risquées.](./media/concept-risky-sign-ins/456.png)

Lorsque vous sélectionnez un événement à risque, vous obtenez une vue de rapport détaillé pour cet événement à risque qui vous permet d’effectuer les opérations suivantes :

- Une option pour configurer une [stratégie de résolution du risque utilisateur](../identity-protection/howto-user-risk-policy.md)  

- Passer en revue la chronologie de la détection de l’événement à risque  

- Passer en revue la liste des utilisateurs pour lesquels cet événement à risque a été détecté

- Fermer manuellement les événements à risque. 

![les connexions risquées.](./media/concept-risky-sign-ins/457.png)

> [!IMPORTANT]
> Vous pouvez trouver un événement à risque sans l’entrée correspondante dans le [rapport sur les connexions](concept-sign-ins.md). En effet, Identity Protection évalue le risque pour les connexions **interactives** et les connexions **non interactives**, alors que le rapport sur les connexions ne montre que les connexions interactives.

Lorsque vous sélectionnez un utilisateur, vous obtenez une vue de rapport détaillé pour cet utilisateur qui vous permet d’effectuer les opérations suivantes :

- Ouvrir la vue Toutes les connexions

- Réinitialisez le mot de passe de l'utilisateur.

- Ignorer tous les événements

- Analysez les événements à risque signalés pour l’utilisateur. 

![les connexions risquées.](./media/concept-risky-sign-ins/324.png)

Pour analyser un événement à risque, sélectionnez-en un dans la liste.  
Cette opération ouvre le panneau **Détails** pour cet événement à risque. Dans le panneau **Détails**, vous avez le choix entre fermer manuellement un événement à risque ou réactiver un événement à risque fermé manuellement. 

![les connexions risquées.](./media/concept-risky-sign-ins/325.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide pratique pour configurer la stratégie d'utilisateur à risque](../identity-protection/howto-user-risk-policy.md)
- [Guide pratique pour configurer la stratégie de remédiation des risques](../identity-protection/howto-user-risk-policy.md)
- [Type d’événement à risque](concept-risk-events.md)
