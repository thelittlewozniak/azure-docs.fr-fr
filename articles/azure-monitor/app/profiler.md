---
title: Profiler des applications Azure App Service en production avec Application Insights | Microsoft Docs
description: Profilez des applications en production sur Azure App Service avec Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: b8f6a2d12e1a9920421e6491432b516520ae110b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60730145"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Profiler des applications Azure App Service en production avec Application Insights

Vous pouvez exécuter Profiler sur des applications ASP.NET et ASP.NET Core qui s’exécutent sur Azure App Service avec le niveau de service De base ou un niveau supérieur. À l’heure actuelle, l’activation de Profiler sur Linux est uniquement possible via [cette méthode](profiler-aspnetcore-linux.md).

## <a id="installation"></a> Activer Profiler pour votre application
Pour activer Profiler pour une application, suivez les instructions ci-dessous. Si vous exécutez un autre type de service Azure, voici les instructions permettant d’activer Profiler sur d’autres plateformes prises en charge :
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Applications Service Fabric](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Machines virtuelles](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler est préinstallé dans le cadre de l’exécution d’App Service. Les étapes ci-dessous vous montrent comment l’activer pour votre App Service. Suivez ces étapes même si vous avez inclus le SDK App Insights dans votre application au moment de la génération.

1. Activez le paramètre « Always On » pour votre service d’application. Vous pouvez mettre à jour le paramètre sous Paramètres généraux dans la page Configuration de votre App Service.
1. Accédez au volet **App Services** dans le portail Azure.
1. Accédez au volet **Paramètres > Application Insights**.

   ![Activer App Insights sur le portail App Services](./media/profiler/AppInsights-AppServices.png)

1. Suivez les instructions dans le volet pour créer une ressource ou sélectionnez une ressource App Insights existante pour superviser votre application. Vérifiez également que le profileur est **activé**. Si votre ressource Application Insights se trouve dans un autre abonnement que votre App Service, vous ne pouvez pas utiliser cette page pour configurer Application Insights. Toutefois, vous pouvez toujours le faire manuellement en créant manuellement les paramètres d’application nécessaires. [La section suivante contient des instructions permettant d’activer manuellement Profiler.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![Ajouter l’extension de site Application Insights][Enablement UI]

1. Le profileur est maintenant activé à l’aide d’un paramètre d’application App Services.

    ![Paramètre d’application pour Profiler][profiler-app-setting]

## <a name="enable-profiler-manually-or-with-azure-resource-manager"></a>Activer Profiler manuellement ou avec Azure Resource Manager
Application Insights Profiler peut être activé en créant des paramètres d’application pour votre Azure App Service. La page avec les options affichées ci-dessus crée automatiquement ces paramètres d’application. Mais vous pouvez automatiser la création de ces paramètres à l’aide d’un modèle ou d’autres moyens. Ces paramètres fonctionnent également si votre ressource Application Insights se trouve dans un autre abonnement que votre Azure App Service.
Voici les paramètres nécessaires pour activer le profileur :

|Paramètre d'application    | Valeur    |
|---------------|----------|
|APPINSIGHTS_INSTRUMENTATIONKEY         | iKey pour votre ressource Application Insights    |
|APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
|DiagnosticServices_EXTENSION_VERSION | ~3 |


Vous pouvez définir ces valeurs à l’aide de [modèles Azure Resource Manager](../../azure-monitor/app/azure-web-apps.md#app-service-application-settings-with-azure-resource-manager), d’[Azure PowerShell](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp) et d’[Azure CLI](https://docs.microsoft.com/cli/azure/webapp/config/appsettings?view=azure-cli-latest).


## <a name="disable-profiler"></a>Désactiver Profiler

Pour arrêter ou redémarrer Profiler pour une instance d’application individuelle, accédez à la ressource d’application sous **Travaux web**. Pour supprimer Profiler, accédez à **Extensions**.

![Désactiver Profiler pour une tâche web][disable-profiler-webjob]

Nous vous recommandons d’activer dès que possible Profiler sur toutes vos applications afin de découvrir d’éventuels problèmes de performance.

Les fichiers de Profiler peuvent être supprimés quand vous utilisez WebDeploy pour déployer des changements sur votre application web. Vous pouvez empêcher la suppression en excluant le dossier App_Data des éléments étant supprimés au cours du déploiement. 


## <a name="next-steps"></a>Étapes suivantes

* [Utilisation d’Application Insights dans Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
