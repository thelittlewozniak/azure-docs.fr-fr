---
title: 'Didacticiel : Intégration d’Azure Active Directory à Salesforce | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dffa40d4a34241f54b67fc28a1d4b7ba320347d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092510"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Didacticiel : Intégration d’Azure Active Directory à Salesforce

Dans ce didacticiel, vous allez apprendre à intégrer Salesforce à Azure Active Directory (Azure AD).
L’intégration de Salesforce dans Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à Salesforce.
* Vous pouvez permettre à vos utilisateurs de se connecter automatiquement à Salesforce (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Salesforce, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).
* Abonnement Salesforce pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Salesforce prend en charge l’authentification unique (SSO) initiée par le **fournisseur de services**

* Salesforce prend en charge l’attribution d’utilisateurs **juste-à-temps**

* Salesforce prend en charge [l’attribution d’utilisateurs **automatique**](salesforce-provisioning-tutorial.md)

## <a name="adding-salesforce-from-the-gallery"></a>Ajout de Salesforce à partir de la galerie

Pour configurer l’intégration de Salesforce avec Azure AD, vous devez ajouter Salesforce à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Salesforce à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)** , cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter une nouvelle application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, entrez **Salesforce**, sélectionnez **Salesforce** dans le volet des résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Salesforce dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous configurez et testez l’authentification unique Azure AD avec Salesforce au moyen d’un utilisateur de test appelé **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre un utilisateur Azure AD et l’utilisateur Salesforce associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec Salesforce, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique Salesforce](#configure-salesforce-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test Salesforce](#create-salesforce-test-user)** pour disposer, dans Salesforce, d’un équivalent de Britta Simon lié à la représentation Azure AD de l’utilisateur.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec Salesforce, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com/), sur la page d’intégration de l’application **Salesforce**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL Salesforce](common/sp-identifier.png)

    a. Dans la zone de texte **URL de connexion**, tapez la valeur au format suivant :

    Compte d’entreprise : `https://<subdomain>.my.salesforce.com`

    Compte de développeur : `https://<subdomain>-dev-ed.my.salesforce.com`

    b. Dans la zone de texte **Identificateur**, entrez la valeur au format suivant :

    Compte d’entreprise : `https://<subdomain>.my.salesforce.com`

    Compte de développeur : `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion et l’identificateur réels. Pour obtenir ces valeurs, contactez l’[équipe de support technique Salesforce](https://help.salesforce.com/support).

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies en fonction de vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer Salesforce**, copiez la ou les URL appropriées, selon vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-salesforce-single-sign-on"></a>Configurer l’authentification unique Salesforce

1. Ouvrez un nouvel onglet dans votre navigateur et connectez-vous à votre compte d’administrateur Salesforce.

2. Cliquez sur **Setup** sous **l’icône de paramètres** en haut à droite de la page.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/configure1.png)

3. Dans le volet de navigation, accédez à **SETTINGS** et cliquez sur **Identity** pour développer la section associée. Puis cliquez sur **Paramètres de l’authentification unique**.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/sf-admin-sso.png)

4. Sur la page **Paramètres de l’authentification unique**, cliquez sur le bouton **Modifier**.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Si vous ne pouvez pas activer les paramètres de l’authentification unique pour votre compte Salesforce, il vous faudra peut-être contacter [l’équipe du support technique de Salesforce](https://help.salesforce.com/support).

5. Sélectionnez **SAML activé**, puis cliquez sur **Enregistrer**.

      ![Configurer l'authentification unique](./media/salesforce-tutorial/sf-enable-saml.png)

6. Pour configurer vos paramètres d’authentification unique SAML, cliquez sur **Nouveau à partir d’un fichier de métadonnées**.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/sf-admin-sso-new.png)

7. Cliquez sur **Sélectionner le fichier** pour charger le fichier XML de métadonnées que vous avez téléchargé à partir du Portail Azure, puis cliquez sur **Créer**.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/xmlchoose.png)

8. Sur la page **Paramètres d’authentification unique SAML**, les champs se renseignent automatiquement et cliquez sur Enregistrer.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/salesforcexml.png)

9. Dans le volet de navigation de gauche de Salesforce, cliquez sur **Company Settings** pour développer la section associée, puis cliquez sur **My Domain**.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/sf-my-domain.png)

10. Faites défiler le contenu de la fenêtre jusqu’à la section **Configuration de l’authentification**, puis cliquez sur le bouton **Modifier**.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/sf-edit-auth-config.png)

11. Dans la section **Configuration de l’authentification**, cochez **AzureSSO** comme **Service d’authentification** de votre configuration SSO SAML, puis cliquez sur **Enregistrer**.

    ![Configurer l'authentification unique](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Si plusieurs services d’authentification sont sélectionnés, les utilisateurs sont invités à choisir le service d’authentification qu’ils préfèrent utiliser pour se connecter lors de l’initialisation d’une authentification unique sur votre environnement Salesforce. Si vous ne voulez pas que cela se produise, vous devez **décocher toutes les cases en regard des autres services d’authentification**.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](common/users.png)

2. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.

    ![Bouton Nouvel utilisateur](common/new-user.png)

3. Dans les propriétés de l’utilisateur, effectuez les étapes suivantes.

    ![Boîte de dialogue Utilisateur](common/user-properties.png)

    a. Dans le champ **Nom**, entrez **BrittaSimon**.
  
    b. Dans le champ **Nom d’utilisateur**, tapez `brittasimon\@yourcompanydomain.extension`. Par exemple : BrittaSimon@contoso.com.

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Salesforce.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **Salesforce**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Salesforce**.

    ![Lien Salesforce de la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-salesforce-test-user"></a>Créer un utilisateur de test Salesforce

Dans cette section, un utilisateur appelé Britta Simon est créé dans Salesforce. Salesforce prend en charge l’approvisionnement juste-à-temps, option activée par défaut. Vous n’avez aucune opération à effectuer dans cette section. Si un utilisateur n’existe pas dans Salesforce, un nouveau est créé lorsque vous tentez d’accéder à Salesforce. Salesforce prend également en charge l’attribution automatique d’utilisateurs. Des informations supplémentaires sur la configuration de l’attribution automatique d’utilisateurs sont disponibles [ici](salesforce-provisioning-tutorial.md).

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Le fait de cliquer sur la vignette Salesforce dans le panneau d’accès doit vous connecter automatiquement à l’application Salesforce pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configurer l’approvisionnement de l’utilisateur](salesforce-provisioning-tutorial.md)
