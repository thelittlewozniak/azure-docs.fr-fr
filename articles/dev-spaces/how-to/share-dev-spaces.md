---
title: Guide pratique pour partager des espaces Azure Dev Spaces
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Développement Kubernetes rapide avec des conteneurs et des microservices sur Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, conteneurs, Helm, service Mesh, routage du service Mesh, kubectl, k8s '
ms.openlocfilehash: 62d4affa5ef49de7600f9ccc800ea6bf83e4bd49
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60686627"
---
# <a name="share-azure-dev-spaces"></a>Partager des espaces Azure Dev Spaces

Avec Azure Dev Spaces, vous pouvez partager votre espace de développement avec les autres membres de votre équipe. Chaque développeur peut travailler dans son propre espace sans craindre de compromettre le travail des autres. Par ailleurs, en collaborant dans un espace commun, vous pouvez tester du code de bout en bout sans avoir à créer des objets fictifs ou à simuler des dépendances. Pour plus d’informations, consultez le guide [Découvrir le développement en équipe](../team-development-nodejs.md).

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Configurer un espace de développement pour plusieurs développeurs

1. Créez un espace de développement dans Azure. Choisissez [.NET Core et VS Code](../get-started-netcore.md), [.NET Core et Visual Studio](../get-started-netcore-visualstudio.md) ou [Node.js et VS Code](../get-started-nodejs.md). Vous devez disposer d’un accès Propriétaire ou Contributeur pour pouvoir accéder à l’abonnement Azure sélectionné.
1. Configurez le **groupe de ressources** de l’espace Azure Dev Spaces de façon à [accorder un accès Contributeur](/azure/active-directory/role-based-access-control-configure) à chaque membre de l’équipe. Vous pouvez vérifier le groupe de ressources d’un espace de développement en exécutant cette commande : `azds list-up`.
1. Demandez aux membres de l’équipe de **sélectionner l’espace de développement** pour pouvoir y travailler.
   * **Ligne de commande ou VS Code** : Pour voir les espaces Azure Dev Spaces existants auxquels vous avez accès : `azds space list`. Pour sélectionner un espace de développement : `azds space select`.
   * **IDE Visual Studio** : Ouvrez un projet dans Visual Studio, sélectionnez **Azure Dev Spaces** dans la liste déroulante des paramètres de lancement. Dans la boîte de dialogue qui s’ouvre, sélectionnez un cluster existant.

     ![Liste déroulante des paramètres de lancement Visual Studio](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez le guide [Découvrir le développement en équipe](../team-development-nodejs.md).
