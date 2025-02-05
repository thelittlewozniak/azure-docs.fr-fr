---
title: Se connecter à Dropbox - Azure Logic Apps
description: Charger et gérer des fichiers avec les API REST de Dropbox et Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 03/01/2019
tags: connectors
ms.openlocfilehash: 5a1bfe8ca38fc23f09b13195fb8ca5bd443a4afd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60312537"
---
# <a name="upload-and-manage-files-in-dropbox-by-using-azure-logic-apps"></a>Charger et gérer les fichiers de Dropbox à l'aide d'Azure Logic Apps

Le connecteur Dropbox et Azure Logic Apps vous permettent de créer des workflows automatisés qui chargent et gèrent les fichiers sur votre compte Dropbox. 

Cet article vous explique comment vous connecter à Dropbox à partir de votre application logique, puis comment ajouter le déclencheur Dropbox **Quand un fichier est créé** et l'action Dropbox **Obtenir le contenu du fichier à l'aide du chemin d'accès**.

## <a name="prerequisites"></a>Prérequis

* Un abonnement Azure. Si vous n’avez pas d’abonnement Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscrivez-vous pour bénéficier d’un compte Azure gratuit</a>.

* Un [compte Dropbox](https://www.dropbox.com/), que vous pouvez obtenir gratuitement. Les informations d'identification de votre compte sont nécessaires pour établir une connexion entre votre application logique et votre compte Dropbox.

* Des connaissances de base en [création d’applications logiques](../logic-apps/quickstart-create-first-logic-app-workflow.md). Pour cet exemple, vous avez besoin d’une application logique vide.

## <a name="add-trigger"></a>Ajouter un déclencheur

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Sous la zone de recherche, choisissez **Tout**. Dans la zone de recherche, entrez « dropbox » comme filtre.
Dans la liste des déclencheurs, sélectionnez ce déclencheur : **Quand un fichier est créé**

   ![Sélectionner un déclencheur Dropbox](media/connectors-create-api-dropbox/select-dropbox-trigger.png)

1. Connectez-vous avec vos informations d’identification de compte Dropbox et autorisez l’accès à vos données Dropbox pour Azure Logic Apps.

1. Fournissez les informations requises pour votre déclencheur. 

   Dans cet exemple, sélectionnez le dossier dans lequel vous souhaitez effectuer le suivi de la création de fichiers. Pour parcourir vos dossiers, sélectionnez l'icône de dossier en regard de la zone **Dossier**.

## <a name="add-action"></a>Ajouter une action

Ajoutez maintenant une action permettant de récupérer le contenu de tout nouveau fichier.

1. Sous le déclencheur, choisissez **Étape suivante**. 

1. Sous la zone de recherche, choisissez **Tout**. Dans la zone de recherche, entrez « dropbox » comme filtre.
Dans la liste des actions, sélectionnez cette action : **Obtenir le contenu d’un fichier à l’aide du chemin**

1. Si vous n'avez pas encore autorisé Azure Logic Apps à accéder à Dropbox, faites-le maintenant.

1. Pour accéder à l'emplacement que vous souhaitez utiliser, cliquez sur le bouton représentant des points de suspension ( **...** ) en regard de la zone **Chemin d'accès au fichier**. 

   Vous pouvez également cliquer dans la zone **Chemin d'accès au fichier** puis, dans la liste de contenu dynamique, sélectionner **Chemin d'accès au fichier**, dont la valeur est disponible en tant que sortie du déclencheur que vous avez ajouté à la section précédente.

1. Lorsque vous avez terminé, enregistrez votre application logique.

1. Pour déclencher votre application logique, créez un nouveau fichier dans Dropbox.

## <a name="connector-reference"></a>Référence de connecteur

Pour plus d'informations techniques, notamment sur les déclencheurs, les actions et les limites, comme décrit dans le fichier OpenAPI (anciennement Swagger) du connecteur, consultez la [page de référence du connecteur](/connectors/dropbox/).

## <a name="get-support"></a>Obtenir de l’aide

* Si vous avez des questions, consultez le [forum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Pour voter pour des idées de fonctionnalités ou pour en soumettre, visitez le [site de commentaires des utilisateurs Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur les autres [connecteurs d’applications logiques](../connectors/apis-list.md)
