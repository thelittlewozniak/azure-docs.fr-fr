---
title: Créer une Function App à l’aide du Portail Azure | Microsoft Docs
description: Créez une Function App dans Azure App Service à l’aide du portail.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: ad9c50953447c1effee48eec5b0cb9f64386e6cc
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155570"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Créer une Function App à l’aide du Portail Azure

Azure Function App utilise l’infrastructure Azure App Service. Cette rubrique vous montre comment créer une Function App dans le Portail Azure. Une Function App est un conteneur qui héberge l’exécution de fonctions individuelles. Lorsque vous créez une Function App dans le plan d’hébergement App Service, elle peut utiliser toutes les fonctionnalités d’App Service.

## <a name="create-a-function-app"></a>Créer une application de fonction

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Lorsque vous créez une Function App, vous devez entrer un **nom d’application** valide, qui peut seulement contenir des lettres, des chiffres et des traits d’union. Le trait de soulignement ( **_** ) n’est pas un caractère autorisé.

Les noms des comptes de stockage doivent comporter entre 3 et 24 caractères, uniquement des lettres minuscules et des chiffres. Le nom de votre compte de stockage doit être unique dans Azure. 

Une fois la Function App créée, vous pouvez créer des fonctions individuelles à l’aide d’un ou plusieurs langages [en utilisant le portail](functions-create-first-azure-function.md#create-function), le [déploiement continu](functions-continuous-deployment.md) ou le [téléchargement par FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Plans de service

Azure Functions a deux plans de service différents : un plan Consommation et un plan App Service. Le plan de consommation alloue automatiquement la puissance de calcul lors de l’exécution du code, augmente la taille des instances quand c’est nécessaire pour gérer la charge, puis diminue leur taille lorsque le code n’est pas en cours d’exécution. Le plan App Service donne à votre Function App l’accès à toutes les fonctions d’App Service. Vous devez choisir votre plan de service lors de la création de votre Function App. Il ne peut pas être modifié actuellement. Pour plus d’informations, consultez [Choisir un plan d’hébergement Azure Functions](functions-scale.md).

Si vous prévoyez d’exécuter des fonctions JavaScript sur un plan App Service, vous devez choisir un plan avec moins de cœurs. Pour plus d’informations, consultez les [informations de référence sur JavaScript pour Functions](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Conditions requises pour le compte de stockage

Lorsque vous créez une Function App dans App Service, vous devez créer ou établir avec une liaison avec un compte de stockage Azure à usage général qui prend en charge le Stockage Blob, File d’attente et Table. En interne, Azure Functions utilise le stockage pour les opérations telles que la gestion des déclencheurs et la journalisation des exécutions de fonctions. Certains comptes de stockage ne prennent pas en charge les files d’attente et les tables, comme les comptes de stockage Blob uniquement, le stockage Premium Azure et les comptes de stockage à usage général avec la réplication ZRS. Ces comptes sont filtrés à partir du panneau du compte de stockage lors de la création d’une Function App.

>[!NOTE]
>Lorsque vous utilisez le plan d’hébergement de la consommation, les fichiers de code de fonction et de configuration de liaison sont stockés sur le Stockage Fichier Azure dans le compte de stockage principal. Lorsque vous supprimez le compte de stockage principal, ce contenu est supprimé et ne peut pas être récupéré.

Pour en savoir plus sur les types de compte de stockage, consultez [Présentation des services Stockage Azure](../storage/common/storage-introduction.md#azure-storage-services). 

## <a name="next-steps"></a>Étapes suivantes

Si le portail Azure permet de créer et essayer facilement Functions, nous vous recommandons le [développement local](functions-develop-local.md). Après avoir créé une Function App dans le portail, vous devez toujours ajouter une fonction. 

> [!div class="nextstepaction"]
> [Ajouter une fonction déclenchée via HTTP](functions-create-first-azure-function.md#create-function)
