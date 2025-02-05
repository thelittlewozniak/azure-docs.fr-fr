---
title: Développer des applications Azure Service Fabric en .NET Core avec Visual Studio Code | Microsoft Docs
description: Cet article montre comment générer, déployer et déboguer des applications Service Fabric en .NET Core à l’aide de Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 680c141e32333c4747ee69919229bd9381f536a4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60393745"
---
# <a name="develop-c-service-fabric-applications-with-visual-studio-code"></a>Développer des applications Service Fabric en C# avec Visual Studio Code

[L’extension Service Fabric Reliable Services pour VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) facilite la génération d’applications Service Fabric en .NET Core sur les systèmes d’exploitation Windows, Linux et macOS.

Cet article vous montre comment générer, déployer et déboguer une application Service Fabric en .NET Core à l’aide de Visual Studio Code.

## <a name="prerequisites"></a>Prérequis

Cet article suppose que vous avez déjà installé VS Code, l’extension Service Fabric Reliable Services pour VS Code et toutes les dépendances requises pour votre environnement de développement. Pour plus d’informations, consultez [Bien démarrer](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Télécharger l’exemple
Cet article utilise l’application CounterService dans le [dépôt GitHub des exemples de prise en main de Service Fabric avec .NET Core](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started). 

Pour cloner le dépôt sur votre machine de développement, exécutez la commande suivante à partir d’une fenêtre de terminal (fenêtre de commande sur Windows) :

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started.git
```

## <a name="open-the-application-in-vs-code"></a>Ouvrir l’application dans VS Code

### <a name="windows"></a>Windows
Dans le Menu Démarrer, cliquez avec le bouton droit sur l’icône VS Code, puis choisissez **Exécuter en tant qu’administrateur**. Pour attacher le débogueur à vos services, vous devez exécuter VS Code en tant qu’administrateur.

### <a name="linux"></a>Linux
À l’aide du terminal, accédez au chemin /service-fabric-dotnet-core-getting-started/Services/CounterService à partir du répertoire dans lequel l’application a été clonée localement.

Exécutez la commande suivante pour ouvrir VS Code en tant qu’utilisateur racine afin que le débogueur puisse être attaché à vos services.
```
sudo code . --user-data-dir='.'
```

L’application doit maintenant apparaître dans votre espace de travail VS Code.

![Application Counter Service dans l’espace de travail](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-application-in-workspace.png)

## <a name="build-the-application"></a>Créer l’application
1. Appuyez sur (Ctrl+Maj+p) pour ouvrir la **Palette de commandes** dans VS Code.
2. Recherchez puis sélectionnez la commande **Service Fabric : Générer une application**. La sortie de la génération est envoyée au terminal intégré.

   ![Commande Générer une application dans VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Déployer l’application sur le cluster local
Une fois que vous avez généré l’application, vous pouvez la déployer sur le cluster local. 

1. Dans la **Palette de commandes**, sélectionnez la commande **Service Fabric : Déployer l’application (Localhost)** . La sortie du processus d’installation est envoyée au terminal intégré.

   ![Commande Déployer une application dans VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-deploy-application.png)

4. Quand le déploiement est terminé, lancez un navigateur et ouvrez Service Fabric Explorer : http:\//localhost:19080/Explorer. Vous devez voir que l’application est en cours d’exécution. Cette opération pouvant prendre un certain temps, soyez patient. 

   ![Application Counter Service dans Service Fabric Explorer](./media/service-fabric-develop-csharp-applications-with-vs-code/sfx-verify-deploy.png)

4. Une fois que vous avez vérifié que l’application est en cours d’exécution, lancez un navigateur et ouvrez cette page : http:\//localhost:31002. Il s’agit du frontend web de l’application. Actualisez la page pour afficher la valeur actuelle du compteur quand elle est incrémentée.

   ![Application Counter Service dans le navigateur](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-running.png)

## <a name="debug-the-application"></a>Déboguer l’application
Quand vous déboguez une application dans VS Code, celle-ci doit être en cours d’exécution sur un cluster local. Vous pouvez ajouter des points d’arrêt au code.

Pour définir un point d’arrêt et déboguer, effectuez les étapes suivantes :
1. Dans Explorer, ouvrez le fichier */src/CounterServiceApplication/CounterService/CounterService.cs* et définissez un point d’arrêt au niveau de la ligne 62 à l’intérieur de la méthode `RunAsync`.
3. Cliquez sur l’icône de débogage dans la **barre d’activités** pour ouvrir la vue du débogueur dans VS Code. Cliquez sur l’icône en forme d’engrenage en haut de la vue du débogueur et sélectionnez **.NET Core** dans la liste déroulante des environnements. Le fichier launch.json s’ouvre. Vous pouvez fermer ce fichier. Maintenant, vous devez voir les options de configuration dans le menu de configuration du débogage situé en regard du bouton d’exécution (flèche verte).

   ![Icône de débogage dans l’espace de travail VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-icon-workspace.png)

2. Sélectionnez **.NET Core Attach** (Attacher .NET Core) dans le menu de configuration de débogage.

   ![Icône de débogage dans l’espace de travail VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-start.png)

3. Ouvrez Service Fabric Explorer dans un navigateur : http:\//localhost:19080/Explorer. Cliquez sur **Applications**, puis descendez dans la hiérarchie pour déterminer le nœud principal sur lequel CounterService est exécuté. Dans l’image sous le nœud principal pour CounterService se trouve le nœud 0.

   ![Nœud principal pour CounterService](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-primary-node.png)

4. Dans VS Code, cliquez sur l’icône d’exécution (flèche verte) en regard de la configuration de débogage **.NET Core Attach** (Attacher .NET Core). Dans la boîte de dialogue de sélection de processus, sélectionnez le processus CounterService qui s’exécute sur le nœud principal que vous avez identifié à l’étape 4.

   ![Processus principal](./media/service-fabric-develop-csharp-applications-with-vs-code/select-process.png)

5. Le point d’arrêt dans le fichier *CounterService.cs* est atteint très rapidement. Vous pouvez alors explorer les valeurs des variables locales. Utilisez la barre d’outils de débogage en haut de VS Code pour poursuivre l’exécution, sauter des lignes, effectuer un pas à pas détaillé dans des méthodes ou sortir de la méthode actuelle. 

   ![Valeurs de débogage](./media/service-fabric-develop-csharp-applications-with-vs-code/breakpoint-hit.png)

6. Pour mettre fin à la session de débogage, cliquez sur l’icône en forme de prise dans la barre d’outils de débogage en haut de VS Code.
   
   ![Se déconnecter du débogueur](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-bar-disconnect.png)
       
7. Lorsque vous avez terminé le débogage, vous pouvez utiliser la commande **Service Fabric : Supprimer l’application** pour supprimer l’application CounterService de votre cluster local. 

## <a name="next-steps"></a>Étapes suivantes

* Découvrez comment [développer et déboguer des applications Service Fabric en Java avec VS Code](./service-fabric-develop-java-applications-with-vs-code.md).



