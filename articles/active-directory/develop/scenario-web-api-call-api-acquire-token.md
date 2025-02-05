---
title: API web qui appelle d’autres API web (acquisition d’un jeton pour l’application) - Plateforme d’identités Microsoft
description: Découvrez comment générer une API web qui appelle d’autres API web (acquisition d’un jeton pour l’application).
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
ms.openlocfilehash: 986e2e0f8a481d61dc870af2548290658b44d2d3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65231099"
---
# <a name="web-api-that-calls-web-apis---acquire-a-token-for-the-app"></a>API web qui appelle d’autres API web - Acquisition d’un jeton pour l’application

Une fois que vous avez généré l’objet d’application cliente, utilisez-le pour acquérir un jeton qui vous servira à appeler une API web.

## <a name="code-in-the-controller"></a>Code dans le contrôleur

Voici un exemple du code qui sera appelé dans les actions des contrôleurs d’API, en appelant une API en aval (nommée todolist).

```CSharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...
}
```

`BuildConfidentialClient()` ressemble à ce que vous avez vu dans l’article [API web qui appelle des API - configuration de l’application](scenario-web-api-call-api-app-configuration.md). `BuildConfidentialClient()` instancie `IConfidentialClientApplication` avec un cache qui contient uniquement les informations d’un seul compte. Le compte est fourni par la méthode `GetAccountIdentifier`.

La méthode `GetAccountIdentifier` utilise les revendications associées à l’identité de l’utilisateur pour lequel l’API web a reçu le jeton JWT :

```CSharp
public static string GetMsalAccountId(this ClaimsPrincipal claimsPrincipal)
{
 string userObjectId = GetObjectId(claimsPrincipal);
 string tenantId = GetTenantId(claimsPrincipal);

 if (    !string.IsNullOrWhiteSpace(userObjectId)
      && !string.IsNullOrWhiteSpace(tenantId))
 {
  return $"{userObjectId}.{tenantId}";
 }

 return null;
}
```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Appel d’une API web](scenario-web-api-call-api-call-api.md)
