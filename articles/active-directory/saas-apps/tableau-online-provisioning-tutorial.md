---
title: 'Didacticiel : Configurer Tableau Online pour l’approvisionnement automatique d’utilisateurs avec Azure Active Directory | Microsoft Docs'
description: Découvrez comment configurer Azure Active Directory pour approvisionner et déprovisionner automatiquement des comptes d’utilisateur sur Tableau Online.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0be9c435-f9a1-484d-8059-e578d5797d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2dbebfa5fa7d9b255cc685696bfe8b3f61d5cf6b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64708928"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Didacticiel : Configurer Tableau Online pour l’approvisionnement automatique d’utilisateurs

Ce tutoriel présente les étapes à effectuer dans Tableau Online et Azure Active Directory (Azure AD) afin de configurer Azure AD pour provisionner et déprovisionner automatiquement des utilisateurs et des groupes sur Tableau Online.

> [!NOTE]
> Ce tutoriel décrit un connecteur reposant sur le service de provisionnement d’utilisateurs Azure AD. Pour plus d’informations sur l’objet et le fonctionnement de ce service et pour accéder au forum aux questions, consultez [Automatisation de l’approvisionnement et de l’annulation de l’approvisionnement des utilisateurs pour les applications SaaS avec Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Prérequis

Le scénario décrit dans ce tutoriel part du principe que vous disposez des éléments suivants :

*   Un locataire Azure AD.
*   Un [locataire Tableau Online](https://www.tableau.com/)
*   Un compte d’utilisateur dans Tableau Online doté d’autorisations d’administrateur

> [!NOTE]
> L’intégration du provisionnement Azure AD repose sur [l’API REST Tableau Online](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm). Cette API est disponible pour les développeurs Tableau Online.

## <a name="add-tableau-online-from-the-azure-marketplace"></a>Ajouter Tableau Online à partir de la Place de marché Azure
Avant de configurer Tableau Online pour l’approvisionnement automatique d’utilisateurs avec Azure AD, ajoutez Tableau Online à partir de la Place de marché Azure à votre liste d’applications SaaS managées.

Pour ajouter Tableau Online à partir de la Place de marché Azure, procédez comme suit.

1. Dans le volet de navigation de gauche du [Portail Azure](https://portal.azure.com), sélectionnez **Azure Active Directory**.

    ![Icône Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter une application, sélectionnez **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, entrez **Tableau Online**, puis sélectionnez **Tableau Online** dans le volet de résultats. Pour ajouter l’application, sélectionnez **Ajouter**.

    ![Tableau Online dans la liste des résultats](common/search-new-app.png)

## <a name="assign-users-to-tableau-online"></a>Affectation d’utilisateurs à Tableau Online

Azure Active Directory utilise un concept appelé *affectations* pour déterminer les utilisateurs devant recevoir l’accès aux applications sélectionnées. Dans le cadre du provisionnement automatique d’utilisateurs, seuls les utilisateurs ou groupes affectés à une application dans Azure AD sont synchronisés.

Avant de configurer et d’activer le provisionnement automatique d’utilisateurs, identifiez les utilisateurs ou groupes dans Azure AD qui doivent accéder à Tableau Online. Pour affecter ces utilisateurs ou groupes à Tableau Online, suivez les instructions dans [Affecter un utilisateur ou un groupe à une application d’entreprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Conseils importants concernant l’affectation d’utilisateurs à Tableau Online

*   Nous vous recommandons d’affecter un seul utilisateur Azure AD à Tableau Online afin de tester la configuration du provisionnement automatique d’utilisateurs. Vous pouvez affecter des groupes ou des utilisateurs supplémentaires ultérieurement.

*   Lorsque vous affectez un utilisateur à Tableau Online, sélectionnez un rôle valide propre à l’application (si disponible) dans la boîte de dialogue d’affectation. Les utilisateurs dont le rôle est **Accès par défaut** sont exclus de l’approvisionnement.

## <a name="configure-automatic-user-provisioning-to-tableau-online"></a>Configurer l’approvisionnement automatique d’utilisateurs dans Tableau Online

Cette section vous guide tout au long des étapes de configuration du service d’approvisionnement Azure AD. Utilisez-le pour créer, mettre à jour et désactiver des utilisateurs ou des groupes dans Tableau Online en fonction des affectations d’utilisateurs ou de groupes dans Azure AD.

> [!TIP]
> Vous pouvez également activer l’authentification unique SAML pour Tableau Online. Suivez les instructions fournies dans le [tutoriel sur l’authentification unique pour Tableau Online](tableauonline-tutorial.md). L’authentification unique peut être configurée indépendamment de l’approvisionnement automatique d’utilisateurs, bien que ces deux fonctionnalités se complètent.

### <a name="configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Configurer l’approvisionnement automatique d’utilisateurs pour Tableau Online dans Azure AD

1. Connectez-vous au [Portail Azure](https://portal.azure.com). Sélectionnez **Applications d’entreprise** > **Toutes les applications** > **Tableau Online**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Tableau Online**.

    ![Lien Tableau Online dans la liste des applications](common/all-applications.png)

3. Sélectionnez l’onglet **Approvisionnement**.

    ![Approvisionnement de Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Définissez le **Mode d’approvisionnement** sur **Automatique**.

    ![Tableau Online : mode d’approvisionnement](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. Dans la section **Informations d’identification de l’administrateur**, entrez le domaine, le nom d’utilisateur de l’administrateur, le mot de passe d’administrateur et l’URL du contenu de votre compte Tableau Online :

   * Dans la zone **Domaine**, indiquez le sous-domaine en fonction de l’étape 6.

   * Dans la zone **Nom d’utilisateur de l’administrateur**, indiquez le nom de l’utilisateur du compte administrateur sur votre locataire Clarizen. Par exemple admin@contoso.com.

   * Dans la zone **Mot de passe d’administrateur**, indiquez le mot de passe du compte administrateur correspondant au nom d’utilisateur de l’administrateur.

   * Dans la zone **URL du contenu**, indiquez le sous-domaine en fonction de l’étape 6.

6. Lorsque vous êtes connecté à votre compte d’administration de Tableau Online, vous pouvez obtenir les valeurs **Domaine** et **URL du contenu** à partir de l’URL de la page d’administration.

    * Le **domaine** de votre compte Tableau Online peut être copié à partir de cette partie de l’URL :

        ![Tableau Online : domaine](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * **L’URL du contenu** de votre compte Tableau Online peut être copiée à partir de cette section. Cette valeur est définie lors de la configuration du compte. Dans cet exemple, la valeur est « contoso » :

        ![Tableau Online : URL du contenu](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > Votre **domaine** peut être différente de celui illustré ici.

7. Une fois que vous avez renseigné les zones présentées à l’étape 5, sélectionnez **Tester la connexion** pour vérifier qu’Azure AD peut se connecter à Tableau Online. Si la connexion échoue, vérifiez que votre compte Tableau Online dispose d’autorisations d’administrateur et réessayez.

    ![Tableau Online : test de connexion](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. Dans la zone **E-mail de notification**, entrez l’adresse e-mail de la personne ou du groupe qui doit recevoir les notifications d’erreur d’approvisionnement. Cochez la case **Envoyer une notification par e-mail en cas de défaillance**.

    ![Tableau Online : e-mail de notification](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. Sélectionnez **Enregistrer**.

10. Dans la section **Mappages**, sélectionnez **Synchroniser les utilisateurs Azure Active Directory sur Tableau Online**.

    ![Tableau Online : synchronisation des utilisateurs](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. Dans la section **Mappages des attributs**, passez en revue les attributs d’utilisateurs qui sont synchronisés entre Azure AD et Tableau Online. Les attributs sélectionnés en tant que propriétés de **Correspondance** sont utilisés pour faire correspondre les comptes utilisateur dans Tableau Online pour les opérations de mise à jour. Pour enregistrer les modifications, sélectionnez **Enregistrer**.

    ![Tableau Online : mise en correspondance des attributs utilisateur](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. Dans la section **Mappages**, sélectionnez **Synchroniser les groupes Azure Active Directory avec Tableau Online**.

    ![Tableau Online : synchronisation des groupes](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. Dans la section **Mappages des attributs**, passez en revue les attributs de groupes qui sont synchronisés entre Azure AD et Tableau Online. Les attributs sélectionnés en tant que propriétés de **Correspondance** sont utilisés pour faire correspondre les comptes utilisateur dans Tableau Online pour les opérations de mise à jour. Pour enregistrer les modifications, sélectionnez **Enregistrer**.

    ![Tableau Online : mise en correspondance des attributs de groupes](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Pour configurer des filtres d’étendue, suivez les instructions fournies dans le [tutoriel sur les filtres d’étendue](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Pour activer le service d’approvisionnement Azure AD pour Tableau Online, définissez le paramètre **État d’approvisionnement** sur **Activé** dans la section **Paramètres**.

    ![Tableau Online : état de l’approvisionnement](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. Définissez les utilisateurs ou groupes que vous voulez approvisionner dans Tableau Online. Dans la section **Paramètres**, sélectionnez les valeurs souhaitées sous **Étendue**.

    ![Tableau Online : étendue](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Lorsque vous êtes prêt à effectuer le provisionnement, sélectionnez **Enregistrer**.

    ![Tableau Online : enregistrement](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Cette opération démarre la synchronisation initiale de tous les utilisateurs ou groupes définis sous **Étendue** dans la section **Paramètres**. La synchronisation initiale prend plus de temps que les synchronisations ultérieures. Elles se produisent toutes les 40 minutes environ, tant que le service d’approvisionnement AD Azure s’exécute. 

Vous pouvez utiliser la section **Détails de la synchronisation** pour surveiller la progression et suivre les liens vers le rapport d’activité d’approvisionnement. Ce rapport décrit toutes les actions effectuées par le service d’approvisionnement Azure AD sur Tableau Online.

Pour avoir des informations sur la lecture des journaux d’activité d’approvisionnement Azure AD, consultez [Création de rapports sur l’approvisionnement automatique de comptes d’utilisateur](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Gestion du provisionnement de comptes d’utilisateur pour les applications d’entreprise](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Étapes suivantes

* [Découvrez comment consulter les journaux d’activité et obtenir des rapports sur l’activité d’approvisionnement](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
