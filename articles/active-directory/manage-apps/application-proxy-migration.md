---
title: Mise à niveau vers le proxy d’application Azure AD | Microsoft Docs
description: Choisissez la meilleure solution de proxy si vous effectuez une mise à niveau à partir de Microsoft Forefront ou de Unified Access Gateway.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9a3468d720cb04e73cb284abb20c7bcf6a392dd
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65859524"
---
# <a name="compare-remote-access-solutions"></a>Comparer les solutions d’accès à distance

Le proxy d’application Azure Active Directory est l’une des deux solutions d’accès à distance proposées par Microsoft. L’autre solution est le proxy d’application Web, la version locale. Ces deux solutions remplacent d’anciens produits proposés par Microsoft : Microsoft Forefront Threat Management Gateway (TMG) et Unified Access Gateway (UAG). Cet article vous permettra de comparer ces quatre solutions entre elles. Pour ceux qui utilisent toujours les solutions TMG ou UAG déconseillées, utilisez cet article pour planifier votre migration vers l’un des proxys d’application. 


## <a name="feature-comparison"></a>Comparaison des fonctionnalités

Ce tableau vous permettra de comparer les solutions Threat Management Gateway (TMG), Unified Access Gateway (UAG), de proxy d’application Web (WAP) et de proxy d’application Azure AD (AP) entre elles.

| Fonctionnalité | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Authentification par certificat | Oui | Oui | - | - |
| Publier de manière sélective les applications de navigateur | Oui | OUI | OUI | Oui |
| Préauthentification et authentification unique | Oui | OUI | OUI | Oui | 
| Pare-feu de couche 2/3 | Oui | Oui | - | - |
| Transférer des fonctionnalités de proxy | Oui | - | - | - |
| Fonctionnalités VPN | Oui | Oui | - | - |
| Prise en charge du protocole riche | - | Oui | Oui, en cas d’exécution sur HTTP | Oui, en cas d’exécution sur HTTP ou via la passerelle des services Bureau à distance |
| Sert de serveur proxy AD FS | - | Oui | Oui | - |
| Un portail pour l’accès de l’application | - | Oui | - | Oui |
| Traduction du lien de corps de réponse | Oui | OUI | - | Oui | 
| Authentification avec des en-têtes | - | Oui | - | Oui, avec PingAccess | 
| Sécurité à l’échelle du cloud | - | - | - | Oui | 
| Accès conditionnel | - | Oui | - | Oui |
| Aucun composant dans la zone démilitarisée (DMZ) | - | - | - | Oui |
| Aucune connexion entrante | - | - | - | Oui |

La plupart des scénarios, nous vous recommandons de Proxy d’Application Azure AD en tant que la solution moderne. Le proxy d’application Web est uniquement recommandé dans les scénarios qui requièrent un serveur proxy pour AD FS, et que vous ne pouvez pas utiliser de domaines personnalisés dans Azure Active Directory. 

Le proxy d’application Azure AD offre des avantages uniques par rapport à des produits similaires, notamment :

- L’extension d’Azure AD à des ressources locales
   - La protection et la sécurité à l’échelle du cloud
   - Des fonctionnalités telles que l’accès conditionnel et Multi-Factor Authentication faciles à activer
- Aucun composant dans la zone démilitarisée
- Aucune connexion entrante nécessaire
- Un panneau d’accès dans lequel vos utilisateurs peuvent se rendre pour toutes leurs applications, y compris les applications SaaS intégrées à Azure AD, les applications O365 et vos applications web locales. 


## <a name="next-steps"></a>Étapes suivantes

- [Utiliser le proxy d’application Azure AD pour offrir un accès à distance sécurisé aux applications locales](application-proxy.md)
