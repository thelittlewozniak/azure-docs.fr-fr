---
title: 'Didacticiel : Configurer Zscaler Two pour le provisionnement automatique d’utilisateurs avec Azure Active Directory | Microsoft Docs'
description: Ce tutoriel montre comment configurer Azure Active Directory pour provisionner et déprovisionner automatiquement des comptes d’utilisateur dans Zscaler Two.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0a250fcd-6ca1-47c2-a780-7a6278186a69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: 837014fde6962f64d7da023a001a4c41089a0097
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "67049432"
---
# <a name="tutorial-configure-zscaler-two-for-automatic-user-provisioning"></a>Didacticiel : Configurer Zscaler Two pour le provisionnement automatique d’utilisateurs

Ce tutoriel montre comment configurer Azure Active Directory (Azure AD) pour provisionner et déprovisionner automatiquement des utilisateurs et/ou des groupes dans Zscaler Two.

> [!NOTE]
> Ce tutoriel décrit un connecteur reposant sur le service de provisionnement d’utilisateurs Azure AD. Pour obtenir des informations importantes sur l’utilisation et le fonctionnement de ce service, ainsi que des réponses aux questions fréquentes, consultez [Automatiser le provisionnement et le déprovisionnement d’utilisateurs dans les applications SaaS avec Azure Active Directory](../active-directory-saas-app-provisioning.md).
>
> Ce connecteur est actuellement en préversion publique. Pour plus d’informations sur les conditions d’utilisation Azure générales relatives aux fonctionnalités en préversion, consultez [Conditions d’utilisation supplémentaires des préversions Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prérequis

Pour effectuer les étapes décrites dans ce tutoriel, vous avez besoin des éléments suivants :

* Un locataire Azure AD.
* Un locataire Zscaler Two.
* Un compte d’utilisateur Zscaler Two ayant des autorisations d’administrateur.

> [!NOTE]
> L’intégration du provisionnement Azure AD repose sur l’API SCIM Zscaler Two, qui est disponible pour les comptes entreprise.

## <a name="add-zscaler-two-from-the-gallery"></a>Ajouter Zscaler Two à partir de la galerie

Avant de configurer Zscaler Two pour le provisionnement automatique d’utilisateurs avec Azure AD, vous devez ajouter Zscaler Two à partir de la galerie d’applications Azure AD à votre liste d’applications SaaS managées.

Dans le [portail Azure](https://portal.azure.com), dans le volet de gauche, sélectionnez **Azure Active Directory** :

![Sélectionnez Azure Active Directory](common/select-azuread.png)

Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications** :

![Applications d’entreprise](common/enterprise-applications.png)

Pour ajouter une application, sélectionnez **Nouvelle application** en haut de la fenêtre :

![Sélectionner la nouvelle application](common/add-new-app.png)

Dans la zone de recherche, entrez **Zscaler Two**. Sélectionnez **Zscaler Two** dans les résultats, puis sélectionnez **Ajouter**.

![Liste des résultats](common/search-new-app.png)

## <a name="assign-users-to-zscaler-two"></a>Attribuer des utilisateurs à Zscaler Two

Les utilisateurs Azure AD doivent être autorisés à accéder aux applications sélectionnées pour pouvoir les utiliser. Dans le cadre du provisionnement automatique d’utilisateurs, seuls les utilisateurs ou groupes attribués à une application dans Azure AD sont synchronisés.

Avant de configurer et d’activer le provisionnement automatique d’utilisateurs, vous devez déterminer quels utilisateurs et/ou groupes dans Azure AD auront besoin d’accéder à Zscaler Two. Après cela, vous pouvez attribuer ces utilisateurs et groupes à l’application Zscaler Two en suivant les instructions fournies dans [Attribuer un utilisateur ou un groupe à une application d’entreprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-two"></a>Conseils importants pour l’attribution d’utilisateurs à Zscaler Two

* Nous vous recommandons d’attribuer au début un seul utilisateur Azure AD à Zscaler Two afin de tester la configuration du provisionnement automatique d’utilisateurs. Vous pourrez attribuer des utilisateurs et groupes supplémentaires ultérieurement.

* Quand vous attribuez un utilisateur à Zscaler Two, vous devez sélectionner un rôle d’application valide (si disponible) dans la boîte de dialogue d’attribution. Les utilisateurs dont le rôle est **Accès par défaut** sont exclus de l’approvisionnement.

## <a name="set-up-automatic-user-provisioning"></a>Configurer le provisionnement d’utilisateurs automatique

Cette section vous guide tout au long des étapes de configuration du service de provisionnement d’Azure AD pour créer, mettre à jour et désactiver des utilisateurs et des groupes dans Zscaler Two en fonction des attributions d’utilisateurs et de groupes définies dans Azure AD.

> [!TIP]
> Vous pouvez également choisir d’activer l’authentification unique basée sur SAML pour Zscaler Two. Le cas échéant, suivez les instructions fournies dans le [tutoriel sur la l’authentification unique pour Zscaler Two](zscaler-two-tutorial.md). L’authentification unique peut être configurée indépendamment du provisionnement automatique d’utilisateurs, mais ces deux fonctionnalités sont complémentaires.

1. Connectez-vous au [portail Azure](https://portal.azure.com) et sélectionnez **Applications d’entreprise** > **Toutes les applications** > **Zscaler Two** :

    ![Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Zscaler Two** :

    ![Liste des applications](common/all-applications.png)

3. Sélectionnez l’onglet **Provisioning** (Provisionnement) :

    ![Provisionnement de Zscaler Two](./media/zscaler-two-provisioning-tutorial/provisioning-tab.png)

4. Définissez **Provisioning Mode** (Mode de provisionnement) sur **Automatic** (Automatique) :

    ![Définir le mode de provisionnement](./media/zscaler-two-provisioning-tutorial/provisioning-credentials.png)

5. Dans la section **Admin Credentials** (Informations d’identification d’administrateur), entrez les valeurs **Tenant URL** (URL de locataire) et **Secret Token** (Jeton secret) de votre compte Zscaler Two, comme cela est décrit dans l’étape suivante.

6. Pour obtenir les valeurs **Tenant URL** (URL de locataire) et **Secret Token** (Jeton secret), accédez à **Administration** > **Authentication Settings** (Paramètres d’authentification) dans le portail Zscaler Two, puis sélectionnez **SAML** sous **Authentication Type** (Type d’authentification) :

    ![Paramètres d’authentification de Zscaler Two](./media/zscaler-two-provisioning-tutorial/secret-token-1.png)

    Sélectionnez **Configure SAML** (Configurer SAML) pour ouvrir la fenêtre **Configure SAML** :

    ![Fenêtre Configure SAML (Configurer SAML)](./media/zscaler-two-provisioning-tutorial/secret-token-2.png)

    Sélectionnez **Enable SCIM-Based Provisioning** (Activer le provisionnement SCIM), copiez les valeurs **Base URL** (URL de base) et **Bearer Token** (Jeton du porteur), puis enregistrez les paramètres. Dans le portail Azure, collez les valeurs **URL de base** dans le champ **URL de locataire** et la valeur **Jeton du porteur** dans le champ **Jeton secret**.

7. Après avoir entré les valeurs dans les champs **URL de locataire** et **Jeton secret**, sélectionnez **Tester la connexion** pour vérifier qu’Azure AD peut se connecter à Zscaler Two. Si la connexion échoue, vérifiez que votre compte Zscaler Two a les autorisations d’administrateur et réessayez.

    ![Tester la connexion](./media/zscaler-two-provisioning-tutorial/test-connection.png)

8. Dans le champ **E-mail de notification**, entrez l’adresse e-mail d’une personne ou d’un groupe qui doit recevoir les notifications d’erreur de provisionnement. Sélectionnez **Envoyer une notification par e-mail en cas de défaillance** :

    ![Configurer l’e-mail de notification](./media/zscaler-two-provisioning-tutorial/notification.png)

9. Sélectionnez **Enregistrer**.

10. Dans la section **Mappages**, sélectionnez **Synchronize Azure Active Directory Users to Zscaler Two** (Synchroniser les utilisateurs Azure Active Directory avec Zscaler Two) :

    ![Synchroniser les utilisateurs Azure AD](./media/zscaler-two-provisioning-tutorial/user-mappings.png)

11. Dans la section **Mappages des attributs**, passez en revue les attributs d’utilisateur qui sont synchronisés entre Azure AD et Zscaler Two. Les attributs sélectionnés comme propriétés de **Correspondance** sont utilisés pour la mise en correspondre des comptes d’utilisateur dans Zscaler Two dans le cadre des opérations de mise à jour. Sélectionnez **Enregistrer** pour valider les modifications.

    ![Mappages d’attributs](./media/zscaler-two-provisioning-tutorial/user-attribute-mappings.png)

12. Dans la section **Mappages**, sélectionnez **Synchronize Azure Active Directory Groups to Zscaler Two** (Synchroniser les groupes Azure Active Directory avec Zscaler Two) :

    ![Synchroniser les groupes Azure AD](./media/zscaler-two-provisioning-tutorial/group-mappings.png)

13. Dans la section **Mappages des attributs**, passez en revue les attributs de groupe qui sont synchronisés entre Azure AD et Zscaler Two. Les attributs sélectionnés comme propriétés de **Correspondance** sont utilisés pour la mise en correspondre des groupes dans Zscaler Two dans le cadre des opérations de mise à jour. Sélectionnez **Enregistrer** pour valider les modifications.

    ![Mappages d’attributs](./media/zscaler-two-provisioning-tutorial/group-attribute-mappings.png)

14. Pour configurer des filtres d’étendue, reportez-vous aux instructions fournies dans le [tutoriel sur les filtres d’étendue](./../active-directory-saas-scoping-filters.md).

15. Pour activer le service de provisionnement Azure AD pour Zscaler Two, définissez le paramètre **État du provisionnement** sur **Activé** dans la section **Paramètres** :

    ![État de l’approvisionnement](./media/zscaler-two-provisioning-tutorial/provisioning-status.png)

16. Définissez quels utilisateurs et/ou groupes vous voulez provisionner dans Zscaler Two en choisissant les valeurs appropriées sous **Étendue** dans la section **Paramètres** :

    ![Valeurs d’étendue](./media/zscaler-two-provisioning-tutorial/scoping.png)

17. Quand vous êtes prêt à effectuer le provisionnement, sélectionnez **Enregistrer** :

    ![Sélectionner Enregistrer](./media/zscaler-two-provisioning-tutorial/save-provisioning.png)

Cette opération démarre la synchronisation initiale de tous les utilisateurs et groupes définis sous **Étendue** dans la section **Paramètres**. La synchronisation initiale prend plus de temps que les synchronisations suivantes, qui ont lieu toutes les 40 minutes environ tant que le service de provisionnement Azure AD est en cours d’exécution. Vous pouvez surveiller la progression dans la section **Détails de la synchronisation**. Vous pouvez également suivre les liens fournis pour accéder à un rapport d’activité de provisionnement. Ce rapport décrit toutes les actions effectuées par le service de provisionnement Azure AD dans Zscaler Two.

Pour plus d’informations sur la lecture des journaux d’activité de provisionnement Azure AD, consultez [Créer des rapports sur le provisionnement automatique de comptes d’utilisateur](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Gestion de l’approvisionnement de comptes d’utilisateur pour les applications d’entreprise](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Étapes suivantes

* [Découvrez comment consulter les journaux d’activité et obtenir des rapports sur l’activité d’approvisionnement](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-03.png
