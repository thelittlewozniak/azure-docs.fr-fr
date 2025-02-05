---
title: Vue d’ensemble de l’API web protégée | Azure
description: Découvrez comment créer une API web protégée (vue d’ensemble).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5bc0075e6604bd1f94531040f2e4a0628e70667
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074889"
---
# <a name="scenario-protected-web-api"></a>Scénario : API web protégée

Dans ce scénario, nous allons vous montrer comment exposer une API web et comment la protéger afin que seuls les utilisateurs authentifiés puissent y accéder. Vous souhaiterez autoriser les utilisateurs authentifiés disposant à la fois de comptes professionnels et scolaires ou de comptes personnels Microsoft à utiliser votre API web.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Spécificités

Voici certaines caractéristiques que vous devez connaître pour protéger les API web :

- L’inscription de votre application doit exposer au moins une étendue. La version de jeton acceptée par votre API web dépend de l’audience de connexion.
- La configuration du code pour l’API web doit valider le jeton qui est utilisé lors de l’appel de l’API web.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Inscription d’application](scenario-protected-web-api-app-registration.md)
