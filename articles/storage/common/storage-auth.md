---
title: Autorisation d’accès au Stockage Azure | Microsoft Docs
description: Découvrez plus d’informations sur les différentes façons d’autoriser l’accès au Stockage Azure, notamment Azure Active Directory, l’authentification de clé partagée ou les signatures d’accès partagé.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 9e8c9755c444ca7b81891f5f83164bc51aa694eb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65147064"
---
# <a name="authorizing-access-to-azure-storage"></a>Autorisation d’accès au Stockage Azure

Chaque fois que vous accédez aux données de votre compte de stockage, le client envoie une demande sur HTTP/HTTPS au Stockage Azure. Chaque demande adressée à une ressource sécurisée doit être autorisée, pour que le service vérifie que le client a les autorisations nécessaires pour accéder aux données. Pour autoriser l’accès aux ressources sécurisées, le Stockage Azure propose ces options :

- **Intégration d’Azure Active Directory (Azure AD)** pour les objets blob et les files d’attente. Azure AD fournit un contrôle d’accès basé sur un rôle (RBAC) pour contrôler l’accès d’un client aux ressources dans un compte de stockage. Pour plus d’informations, consultez [Authentification des demandes au Stockage Azure à l’aide d’Azure Active Directory](storage-auth-aad.md).
- **Autorisation de clé partagée** pour les objets blob, les fichiers, les files d’attente et les tables. Un client qui utilise une clé partagée transmet avec chaque demande un en-tête signé à l’aide de la clé d’accès au compte de stockage. Pour plus d’informations, consultez [Autoriser avec une clé partagée](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/).
- **Signatures d’accès partagé** pour les objets blob, les fichiers, les files d’attente et les tables. Les signatures d’accès partagé (SAP) fournissent un accès délégué limité aux ressources de votre compte de stockage. L’ajout de contraintes à la période de validité de la signature ou aux autorisations qu’elle accorde garantit la souplesse de la gestion de l’accès. Pour plus d’informations, consultez la page [Utiliser des signatures d’accès partagé (SAP)](storage-dotnet-shared-access-signature-part-1.md).
- **Accès en lecture public anonyme** pour les conteneurs et les objets blob. L’autorisation n’est pas obligatoire. Pour plus d’informations, consultez [Gestion de l’accès en lecture anonyme aux conteneurs et aux objets blob](../blobs/storage-manage-access-to-resources.md).  

Par défaut, toutes les ressources dans le Stockage Azure sont sécurisées et disponibles uniquement pour le propriétaire du compte. Vous pouvez utiliser n’importe quelle stratégie d’autorisation décrite ci-dessus pour accorder aux clients l’accès aux ressources de votre compte de stockage, toutefois, Microsoft recommande d’utiliser Azure AD quand cela est possible pour optimiser la sécurité et la facilité d’utilisation. 



