---
title: Création d’un groupe dynamique et vérification d’état - Azure Active Directory | Microsoft Docs
description: Comment créer une règle d’appartenance à un groupe dans le portail Azure et comment vérifier l’état.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f828ff83e6b9c60eb08edef7f47e88185fb5aef8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60472140"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Créer un groupe dynamique et vérifier l’état

Dans Azure Active Directory (Azure AD), vous pouvez utiliser des règles pour déterminer l’appartenance aux groupes à partir des propriétés de l’utilisateur ou de l’appareil. Cet article explique comment configurer une règle pour un groupe dynamique dans le portail Azure.
L’appartenance dynamique est prise en charge pour les groupes de sécurité ou les groupes Office 365. Lorsqu’une règle d’appartenance de groupe est appliquée, les correspondances avec la règle d’appartenance des attributs utilisateur et appareil sont évaluées. Lorsqu’un attribut d’utilisateur ou d’appareil change, toutes les règles de groupe dynamique au sein de l’organisation sont traitées pour tenir compte de toutes les modifications d’appartenance. Les utilisateurs et les appareils sont ajoutés ou supprimés s’ils remplissent les conditions pour un groupe.

Pour obtenir des exemples de syntaxe des règles, des propriétés prises en charge, des opérateurs et des valeurs pris en charge pour une règle d’appartenance, consultez [Règles d’appartenance dynamique pour les groupes dans Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Pour créer une règle d’appartenance à un groupe

1. Connectez-vous au [Centre d’administration Azure AD](https://aad.portal.azure.com) avec un compte du rôle Administrateur général, Administrateur Intune ou administrateur d’utilisateurs dans le locataire.
2. Sélectionnez **Groupes**.
3. Sélectionnez **Tous les groupes**, puis **Nouveau groupe**.

   ![Sélectionnez la commande pour ajouter le nouveau groupe](./media/groups-create-rule/new-group-creation.png)

4. Dans la page **Groupe**, entrez le nom et la description du nouveau groupe. Sélectionnez un **Type d’appartenance** pour les utilisateurs ou les appareils, puis sélectionnez **Ajouter une requête dynamique**. Vous pouvez utiliser le générateur de règle pour créer une règle simple, ou [écrire une règle d’appartenance vous-même](groups-dynamic-membership.md).

   ![Ajouter une règle d’appartenance au groupe dynamique](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Pour afficher les propriétés d’extension personnalisées disponibles pour votre requête d’appartenance
   1. Sélectionnez **Obtenir les propriétés d’extension personnalisée**
   2. Entrez l’ID de l’application, puis sélectionnez **Actualiser les propriétés**. 
6. Après avoir créé la règle, sélectionnez **Ajouter une requête** dans le bas du panneau.
7. Sélectionnez **Créer** on the **Groupe** panneau pour créer le groupe.

Si la règle que vous avez entrée n’est pas valide, une explication de la raison pour laquelle la règle n’a pas pu être traitée s’affiche en haut à droite du portail. Lisez-la avec attention pour savoir comment corriger la règle.

## <a name="turn-on-or-off-welcome-email"></a>Activer ou désactiver l’e-mail de bienvenue

Lorsqu’un groupe Office 365 est créé, une notification de bienvenue est envoyée aux utilisateurs qui sont ajoutés au groupe. Plus tard, si un attribut d’utilisateur ou d’appareil change, toutes les règles de groupe dynamique au sein de l’organisation sont traitées pour les modifications d’appartenance. Les utilisateurs qui sont alors ajoutés reçoivent également la notification de bienvenue. Vous pouvez désactiver ce comportement dans [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps). 

## <a name="check-processing-status-for-a-rule"></a>Vérifier l’état de traitement d’une règle

Vous pouvez voir l’état du traitement de l’appartenance et la date de la dernière mise à jour dans la page **Vue d’ensemble** du groupe.
  
  ![afficher l’état du groupe dynamique](./media/groups-create-rule/group-status.png)

Les messages d’état suivants peuvent être affichés pour l’état **Traitement de l’appartenance** :

* **Évaluation** :  le changement de groupe a été reçu et les mises à jour sont en cours d’évaluation.
* **Traitement** : les mises à jour sont en cours de traitement.
* **Mise à jour terminée** : le traitement est terminé et toutes les mises à jour applicables ont été effectuées.
* **Erreur de traitement** :  Impossible de terminer le traitement en raison d’une erreur d’évaluation de la règle d’appartenance.
* **Mise à jour suspendue** : les mises à jour de la règle d’appartenance dynamique ont été suspendues par l’administrateur. Le paramètre MembershipRuleProcessingState est défini sur « Suspendu ».

Les messages d’état suivants peuvent être affichés pour l’état **Dernière mise à jour de l’appartenance** :

* &lt;**Date et heure**&gt; : date et heure de la dernière mise à jour de l’appartenance.
* **En cours** : les mises à jour sont en cours d’exécution.
* **Inconnue** : impossible de récupérer l’heure de la dernière mise à jour. Le groupe est peut-être nouveau.

Si une erreur se produit lors du traitement de la règle d’appartenance pour un groupe spécifique, une alerte s’affiche en haut de la page **Vue d’ensemble** du groupe. Si aucune mise à jour d’appartenance dynamique en attente ne peut être traitée pour tous les groupes au sein du locataire pendant plus de 24 heures, une alerte s’affiche en haut de **Tous les groupes**.

![alertes de message d’erreur de traitement](./media/groups-create-rule/processing-error.png)

Ces articles fournissent des informations supplémentaires sur les groupes dans Azure Active Directory.

* [Consulter les groupes existants](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Création d’un nouveau groupe et ajout de membres](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Gérer les paramètres d’un groupe](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Gérer l’appartenance à un groupe](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Gérer les règles dynamiques pour les utilisateurs dans un groupe](groups-dynamic-membership.md)
