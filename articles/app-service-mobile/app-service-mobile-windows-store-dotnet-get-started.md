---
title: Créer une plateforme Windows universelle (UWP) sur Azure Mobile Apps | Microsoft Docs
description: Suivez ce didacticiel pour commencer à utiliser des services principaux d’applications mobiles Azure pour le développement d’applications UWP en C#, Visual Basic ou JavaScript.
services: app-service\mobile
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: be579b631fd910c56f2c360d6aace5b8d35c22e5
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235983"
---
# <a name="create-a-windows-app-with-an-azure-backend"></a>Créer une application Windows avec un back-end Azure

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Vue d'ensemble

Ce didacticiel présente l’ajout d’un service principal cloud à une application de plateforme Windows universelle (UWP). Pour plus d’informations, consultez [Que sont les applications Mobile Apps ?](app-service-mobile-value-prop.md). Voici les captures d’écran générées à partir de l’application terminée :

![Application de bureau terminée](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)

Vous devez suivre ce didacticiel avant de pouvoir suivre les autres didacticiels Mobile App pour les applications UWP.

## <a name="prerequisites"></a>Conditions préalables

Pour réaliser ce didacticiel, vous avez besoin des éléments suivants :

* Un compte Azure actif. Si vous n'avez pas de compte, vous pouvez vous inscrire pour une évaluation d'Azure et obtenir jusqu'à 10 applications mobiles gratuites que vous pourrez conserver après l'expiration de votre période d'évaluation. Pour plus d'informations, consultez la page [Version d'évaluation gratuite d'Azure](https://azure.microsoft.com/pricing/free-trial/).
* Windows 10.
* Visual Studio Community 2017.
* Connaissance du développement d’applications UWP. Consultez la [documentation UWP](https://docs.microsoft.com/windows/uwp/) pour savoir comment vous [préparer](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) à créer des applications UWP.

## <a name="create-a-new-azure-mobile-app-backend"></a>Créer un serveur principal d'applications mobiles Azure

Suivez ces étapes pour créer un serveur principal d’application mobile.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Créer une connexion de base de données et de configurer le projet client et serveur
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-client-project"></a>Exécutez le projet client

1. Ouvrez le projet UWP.

2. Accédez à la [Azure portal](https://portal.azure.com/) et accédez à l’application mobile que vous avez créé. Sur le `Overview` panneau, recherchez l’URL qui est le point de terminaison public pour votre application mobile. Exemple - sitename pour mon nom de l’application « test123 » sera https://test123.azurewebsites.net.

3. Ouvrez le fichier `App.xaml.cs` dans ce dossier - windows-uwp-cs/ZUMOAPPNAME /. Le nom de l’application est `ZUMOAPPNAME`.

4. Dans `App` classe, remplacez `ZUMOAPPURL` paramètre avec le point de terminaison public ci-dessus.

    `public static MobileServiceClient MobileService = new MobileServiceClient("ZUMOAPPURL");`

    devient
    
    `public static MobileServiceClient MobileService = new MobileServiceClient("https://test123.azurewebsites.net");`

5. Appuyez sur la touche F5 pour déployer et exécuter l’application.

6. Dans l’application, tapez un texte explicite, comme *Suivre le didacticiel*, dans la zone de texte **Insérer un TodoItem**, puis cliquez sur **Enregistrer**.

    ![Démarrage rapide Windows - Bureau terminé](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Ceci envoie une demande POST vers le nouveau backend d'application mobile qui est hébergé dans Azure.
