---
title: Comment rechercher une API spécifique requise pour une application personnalisée | Microsoft Docs
description: Comment configurer les autorisations dont vous avez besoin pour accéder à une API particulière dans l’application Azure AD que vous avez développée
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4a017c13008288b26ddb11bf58be1966d652bbae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540550"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Comment rechercher une API spécifique requise pour une application personnalisée

L’accès aux API suppose de configurer les étendues d’accès et les rôles. Si vous souhaitez exposer aux applications clientes les API web de vos applications de ressources, vous devez configurer les étendues d’accès et les rôles de l’API. Si vous voulez qu’une application cliente puisse accéder à une API web, vous devez configurer les autorisations d’accès à l’API lors de l’inscription de l’application.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configuration d’une application de ressource pour exposer les API web

Lorsque vous exposez vos API web, l’API s’affiche dans la liste **Sélectionner une API** lorsque vous ajoutez des autorisations à l’inscription d’une application. Pour ajouter des étendues d’accès, suivez les étapes décrites dans [Ajout d’étendues d’accès à votre application de ressources](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configuration d’une application cliente pour accéder aux API web

Lorsque vous ajoutez des autorisations à l’inscription de votre application, vous pouvez **ajouter un accès à l’API** à des API web exposées. Pour accéder aux API web, suivez les étapes décrites dans la section [Pour ajouter des informations d’identification ou des autorisations pour accéder aux API web](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

## <a name="next-steps"></a>Étapes suivantes

-   [Intégration d’applications dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Connaître le manifeste d’application Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


