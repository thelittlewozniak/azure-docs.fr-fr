---
title: Proxy d’application Azure AD et Qlik Sense | Microsoft Docs
description: Activez le proxy d’application dans le portail Azure et installez les connecteurs pour le proxy inverse.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 09/06/2018
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f54e08e6c3b7b673541f124a90f32dbc860fa44
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65859540"
---
# <a name="application-proxy-and-qlik-sense"></a>Proxy d’application et Qlik Sens 
Le proxy d’application Azure Active Directory et Qlik Sense se sont associés afin de vous permettre d’utiliser facilement le Proxy d’application pour fournir l’accès à distance à votre déploiement Qlik sens.  

## <a name="prerequisites"></a>Conditions préalables 
Le reste de ce scénario suppose que vous avez effectué les opérations suivantes :
 
- Vous avez configuré [Qlik sens](https://community.qlik.com/docs/DOC-19822). 
- [Vous avez installé un connecteur Proxy d’application](application-proxy-add-on-premises-application.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Publier vos applications dans Azure 
Pour publier QlikSense, vous devez publier deux applications dans Azure.  

### <a name="application-1"></a>1ère application : 
Suivez ces étapes pour publier votre application. Pour obtenir plus de détails sur les étapes 1 à 8, consultez [Publier des applications avec le Proxy d’application Azure AD](application-proxy-add-on-premises-application.md). 


1. Connectez-vous au portail Azure en tant qu’administrateur général. 
2. Sélectionnez **Azure Active Directory (Azure Active Directory)** > **Enterprise applications (Applications d’entreprise)**. 
3. Sélectionnez **Ajouter** en haut du panneau. 
4. Sélectionnez **On-premises application (Application locale)**. 
5. Saisissez les informations concernant votre nouvelle application dans les champs requis. Suivez les conseils ci-dessous pour les paramètres : 
   - **URL interne** : Cette application doit avoir une URL interne qui est l’URL QlikSense proprement dite. Par exemple, **https&#58;//demo.qlikemm.com:4244** 
   - **Méthode de pré-authentification** : Azure Active Directory (recommandée mais pas obligatoire) 
1. Sélectionnez **Ajouter** en bas du panneau. Votre application est ajoutée et le menu de démarrage rapide s’ouvre. 
2. Dans le menu de démarrage rapide, sélectionnez **Assign a user for testing (Attribuer un utilisateur à des fins de test)**, et ajoutez au moins un utilisateur à l’application. Vérifiez que ce compte de test a accès à l’application locale. 
3. Sélectionnez **Affecter** pour enregistrer l’affectation de l’utilisateur de test. 
4. (Facultatif) Dans le panneau de gestion de l’application, sélectionnez Authentification unique. Choisissez **Délégation Kerberos contrainte** dans le menu déroulant et renseignez les champs obligatoires en fonction de votre configuration Qlik. Sélectionnez **Enregistrer**. 

### <a name="application-2"></a>2ème application : 
Suivez les mêmes étapes que pour la 1ère application, avec les exceptions suivantes : 

**Étape 5** : L’URL interne doit maintenant être l’URL QlikSense avec le port d’authentification utilisé par l’application. La valeur par défaut est **4244** pour HTTPS et 4248 pour HTTP. Par exemple, **https&#58;//demo.qlik.com:4244**</br></br> 
**Étape 10 :** Ne configurez pas l’authentification unique et laissez **l’authentification unique désactivée**
 
 
## <a name="testing"></a>Test 
Votre application est maintenant prête à être testée. Accédez à l’URL externe utilisée pour publier QlikSense dans la 1ère application et connectez-vous en tant qu’utilisateur affecté aux deux applications.  

## <a name="additional-references"></a>Références supplémentaires
Pour plus d’informations sur la publication Qlik Sense avec le Proxy d’Application, consultez les Articles de la Communauté Qlik qui suivent : 
- [Azure AD avec l’authentification Windows intégrée à l’aide d’une délégation Kerberos contrainte avec Qlik Sense](https://community.qlik.com/docs/DOC-20183)
- [Intégration de Qlik Sense avec Proxy d’Application Azure AD](https://community.qlik.com/t5/Technology-Partners-Ecosystem/Azure-AD-Application-Proxy/ta-p/1528396)

## <a name="next-steps"></a>Étapes suivantes

- [Publiez des applications avec le proxy d’application](application-proxy-add-on-premises-application.md)
- [Utiliser des connecteurs Proxy d’application](application-proxy-connector-groups.md)

