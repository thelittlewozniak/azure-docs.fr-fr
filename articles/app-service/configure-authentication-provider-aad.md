---
title: Configurer l’authentification Azure Active Directory - Azure App Service
description: Découvrez comment configurer l'authentification Azure Active Directory pour votre application App Services.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/20/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: d687e770fae6c32ee351a597e12d1aca6094e5cb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60851373"
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-sign-in"></a>Configurer votre application App Service pour utiliser une connexion Azure Active Directory

> [!NOTE]
> À l’heure actuelle, AAD V2 (notamment MSAL) n’est pas pris en charge pour Azure App Services et Azure Functions. Revenez plus tard pour suivre l’évolution de la situation.
>

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Cet article montre comment configurer Azure App Services pour utiliser Azure Active Directory comme fournisseur d’authentification.

## <a name="express"> </a>Configurer avec des paramètres express

1. Dans le [portail Azure], accédez à votre application App Service. Dans la barre de navigation gauche, sélectionnez **Authentification / Autorisation**.
2. Si l'option **Authentification / Autorisation** n'est pas activée, sélectionnez **Activer**.
3. Sélectionnez **Azure Active Directory**, puis **Rapide** sous **Mode de gestion**.
4. Cliquez sur **OK** pour inscrire l'application App Service auprès d'Azure Active Directory. Une nouvelle inscription d’application est créée. Si vous choisissez une inscription d'app existante à la place, cliquez sur **Sélectionner une application existante** et recherchez le nom d’une inscription d'app précédemment créée au sein de votre client.
   Cliquez sur l'inscription d'app pour la sélectionner, puis sur **OK**. Cliquez ensuite sur **OK** dans la page des paramètres Azure Active Directory.
   Par défaut, App Service fournit une authentification, mais ne restreint pas l'accès autorisé à votre contenu et aux API de votre site. Vous devez autoriser les utilisateurs dans votre code d'application.
5. (Facultatif) Pour restreindre l’accès à votre site aux seuls utilisateurs authentifiés par Azure Active Directory, définissez **Action à exécuter quand une demande n’est pas authentifiée** sur **Se connecter avec Azure Active Directory**. Cela implique que toutes les demandes soient authentifiées. Toutes les demandes non authentifiées sont redirigées vers Azure Active Directory pour être authentifiées.
6. Cliquez sur **Enregistrer**.

## <a name="advanced"> </a>Configurer avec des paramètres avancés

Vous pouvez également fournir manuellement des paramètres de configuration. Il s’agit de la solution préférée si le locataire Azure Active Directory que vous voulez utiliser diffère de celui avec lequel vous vous connectez à Azure. Pour terminer la configuration, vous devez d’abord créer une inscription dans Azure Active Directory, puis fournir des informations d’inscription à App Service.

### <a name="register"> </a>Inscription de votre application App Service auprès d'Azure Active Directory

1. Connectez-vous au [portail Azure]et accédez à votre application App Service. Copiez l **'URL** de votre application. Elle vous permettra de configurer l'inscription de votre application Azure Active Directory.
2. Accédez à **Active Directory**, sélectionnez **Inscriptions des applications**, puis cliquez sur **Nouvelle inscription d’application** en haut pour démarrer une nouvelle inscription d’application. 
3. Dans la page **Créer**, entrez un **Nom** pour l'inscription de votre application, sélectionnez le type **Application web / API** et, dans la zone **URL de connexion**, collez l’URL de l’application (celle de l’étape 1). Cliquez ensuite sur **Créer**.
4. Au bout de quelques secondes, vous devez voir apparaître la nouvelle inscription d’application que vous venez de créer.
5. Une fois l'inscription de l’application ajoutée, cliquez sur le nom de l’inscription d’application, cliquez sur **Paramètres** en haut, puis cliquez sur **Propriétés** 
6. Dans la zone **URI d’ID d’application**, collez l’URI de l’application (celui de l’étape 1). Dans la zone **URL de la page d’accueil**, collez l’URL de l’application (celle de l’étape 1), puis cliquez sur **Enregistrer**
7. Cliquez maintenant sur **URL de réponse**, modifiez l’**URL de réponse**, collez l’URL d’application (de l’étape 1), puis ajoutez à la fin de l’URL, */.auth/login/aad/callback* (par exemple `https://contoso.azurewebsites.net/.auth/login/aad/callback`). Cliquez sur **Enregistrer**.

   > [!NOTE]
   > Vous pouvez utiliser la même inscription d’application pour plusieurs domaines en ajoutant des **URL de réponse** supplémentaires. Veillez à modéliser chaque instance d’App Service avec sa propre inscription. Ainsi, elle a ses propres autorisations et son propre consentement. Pensez également à utiliser des inscriptions d’applications distinctes pour des emplacements de site distincts. Cela permet d’éviter le partage des autorisations entre environnements. Ainsi, en cas de bogue dans le nouveau code que vous testez, l’environnement de production n’est pas affecté.
    
8. À ce stade, copiez **l’ID d’application** pour l’application. Gardez-le pour une utilisation ultérieure. Vous en aurez besoin pour configurer votre application App Service.
9. Fermez la page **Application inscrite**. Dans la page **Inscriptions d’applications**, cliquez en haut sur le bouton **Points de terminaison**, puis copiez l’URL **WS-FEDERATION SIGN-ON ENDPOINT**, mais supprimez la fin `/wsfed` de l’URL. Le résultat final doit ressembler à `https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000`. Le nom de domaine peut être différent dans le cas d’un cloud souverain. Il servira ultérieurement d’URL émettrice de certificat.

### <a name="secrets"> </a>Ajout d'informations Azure Active Directory à votre application App Service

1. Retournez dans le [portail Azure], puis accédez à votre application App Service. Cliquez sur **Authentification/autorisation**. Si la fonctionnalité Authentification/Autorisation n’est pas activée, positionnez le commutateur sur **Activé**. Sous Fournisseurs d’authentification, cliquez sur **Azure Active Directory** pour configurer votre application.

    (Facultatif) Par défaut, App Service fournit une authentification, mais ne restreint pas l’accès autorisé au contenu et aux API de votre site. Vous devez autoriser les utilisateurs dans votre code d'application. Définissez **Mesure à prendre quand une demande n’est pas authentifiée**, sur **Se connecter avec Azure Active Directory**. Cette option implique que toutes les demandes soient authentifiées. Toutes les demandes non authentifiées sont redirigées vers Azure Active Directory pour être authentifiées.
2. Dans la configuration de l’authentification Active Directory, cliquez sur **Avancé** sous **Mode de gestion**. Collez l’ID d’application dans la zone ID client (de l’étape 8) et collez l’URL (de l’étape 9) dans la valeur URL de l’émetteur. Cliquez ensuite sur **OK**.
3. Dans la page de configuration de l’authentification Active Directory, cliquez sur **Enregistrer**.

Vous êtes maintenant prêt à utiliser Azure Active Directory pour l'authentification dans votre application App Service.

## <a name="configure-a-native-client-application"></a>Configurer une application cliente native
Vous pouvez inscrire des clients natifs, ce qui offre un contrôle accru du mappage d’autorisations. C’est utile si vous souhaitez effectuer des connexions à l’aide d’une bibliothèque de client telle que **Bibliothèque d'authentification Active Directory**.

1. Accédez à **Azure Active Directory** dans le [portail Azure].
2. Dans le volet de navigation de gauche, sélectionnez **Inscriptions des applications**. Cliquez sur **Nouvelle inscription d'application** dans la partie supérieure.
4. Dans la page **Créer**, entrez un **Nom** pour l'inscription de votre application. Sélectionnez **Native** comme **type d'application**.
5. Dans la zone **URI de redirection**, entrez le point de terminaison */.auth/login/done* de votre site à l’aide du modèle HTTPS. Cette valeur doit être similaire à *https://contoso.azurewebsites.net/.auth/login/done* . Si vous créez une application Windows, utilisez plutôt le [SID de package](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) en tant qu’URI.
5. Cliquez sur **Créer**.
6. Une fois l'inscription de l'application ajoutée, sélectionnez-la pour l'ouvrir. Recherchez l’**ID de l'application** et notez-en la valeur.
7. Cliquez sur **Tous les paramètres** > **Autorisations requises** > **Ajouter** > **Sélectionner une API**.
8. Entrez le nom de l'application App Service que vous avez inscrite précédemment pour la rechercher, sélectionnez-la, puis cliquez sur **Sélectionner**.
9. Sélectionnez **Accéder à \<nom_app**. Puis cliquez sur **Sélectionner**. Cliquez ensuite sur **Terminé**.

Vous avez maintenant configuré une application cliente native qui peut accéder à votre application App Service.

## <a name="related-content"></a>Contenu connexe

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/configure-authentication-provider-aad/app-service-webapp-auth-config.png



<!-- URLs. -->

[Portail Azure]: https://portal.azure.com/
[alternative method]:#advanced
