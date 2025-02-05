---
title: Fichier Include
description: Fichier Include
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/27/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cae108a1d4226e8c0fe39f9cd1cedc1e6a024ffc
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465450"
---
## <a name="sign-in-to-azure"></a>Connexion à Azure 

Connectez-vous au portail Azure sur https://portal.azure.com.

> [!NOTE]
> Si vous vous êtes inscrit pour utiliser mes Galeries d’images partagées pendant la préversion, vous devrez peut-être inscrire à nouveau le fournisseur `Microsoft.Compute`. Ouvrez [Cloud Shell](https://shell.azure.com/bash) et saisissez : `az provider register -n Microsoft.Compute`

## <a name="create-an-image-gallery"></a>Créer une galerie d’images

Une galerie d’images est la ressource principale utilisée pour activer le partage d’image. Les caractères autorisés pour le nom de galerie sont les lettres majuscules ou minuscules, les chiffres et les points. Le nom de galerie ne peut pas contenir de tirets.  Les noms de galerie doivent être uniques dans votre abonnement. 

L’exemple suivant crée une galerie nommée *myGallery* dans le groupe de ressources *myGalleryRG*.

1. Sélectionnez **Créer une ressource** dans le coin supérieur gauche du portail Azure.
1. Utilisez le type **Galerie d’images partagées** dans la zone de recherche, puis sélectionnez **Galerie d’images partagées** dans les résultats.
1. Sur la page **Galerie d’images partagées**, cliquez sur **Créer**.
1. Sélectionnez l’abonnement approprié.
1. Dans **Groupe de ressources**, sélectionnez **Créer** et saisissez *myResourceGroup* comme nom.
1. Dans **Nom**, saisissez *myGallery* comme nom de la galerie.
1. Laissez la valeur par défaut pour **Région**.
1. Vous pouvez saisir une brève description de la galerie, par exemple *Ma galerie d’images pour le test.* Cliquez ensuite sur **Vérifier + créer**.
1. Une fois la validation réussie, sélectionnez **Créer**.
1. Une fois le déploiement terminé, sélectionnez **Accéder à la ressource**.
   
## <a name="create-an-image-definition"></a>Créer une définition d’image 

Les définitions d’image créent un regroupement logique des images. Elles sont utilisées pour gérer les informations sur les versions d’image créées au sein de celles-ci. Les noms de définition d’image peuvent contenir des lettres majuscules ou minuscules, des chiffres, des tirets et des points. Pour plus d’informations sur les valeurs que vous pouvez spécifier pour une définition d’image, consultez [Définitions d’image](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Créez la définition de l’image de galerie à l’intérieur de votre galerie. Dans cet exemple, l’image de galerie est nommée *myImageDefinition*.

1. Sur la page de votre nouvelle galerie d’images, sélectionnez **Ajouter une nouvelle définition d’image** à partir du haut de la page. 
1. Pour **Nom de la définition d’image**, saisissez *myImageDefinition*.
1. Pour **Système d’exploitation**, sélectionnez l’option appropriée en fonction de votre image source.
1. Pour **Éditeur**, saisissez *myPublisher*. 
1. Pour **Offre**, saisissez *myOffer*.
1. Pour **SKU**, saisissez *mySKU*.
1. Quand vous avez terminé, sélectionnez **Vérifier + créer**.
1. Une fois la définition de l’image validée, sélectionnez **Créer**.
1. Une fois le déploiement terminé, sélectionnez **Accéder à la ressource**.


## <a name="create-an-image-version"></a>Créer une version d’image

Créer une version de l’image à partir d’une image managée. Dans cet exemple, la version d'image, *1.0.0*, elle est répliquée dans les deux centres de données *USA Centre-Ouest* et *USA Centre Sud*. Lors du choix des régions cibles pour la réplication, n’oubliez pas que vous devez également inclure la région *source* en tant que cible pour la réplication.

Les caractères autorisés pour la version d’image sont les nombres et les points. Les nombres doivent être un entier 32 bits. Format : *MajorVersion*.*MinorVersion*.*Patch*.

1. Sur la page de votre définition d’image, sélectionnez **Ajouter une version** à partir du haut de la page.
1. Dans **Région**, sélectionnez la région où est stockée votre image managée. Les versions d’images doivent être créées dans la même région que l’image managée à partir de laquelle elles sont créées.
1. Pour **Nom**, saisissez *1.0.0*. Le nom de version d’image doit respecter le format *version majeure*.*version mineure*.*correctif* en utilisant des nombres entiers. 
1. Dans **Image source**, sélectionnez votre image managée source à partir de la liste déroulante.
1. Dans **Exclure de la plus récente**, conservez la valeur par défaut *Non*.
1. Pour **Date de fin de vie**, sélectionnez une date dans le calendrier dans quelques mois.
1. Dans **Réplication**, laissez le **Nombre de réplicas par défaut** sur 1. Vous devez répliquer vers la région source, donc laissez le premier réplica à la valeur par défaut et choisissez comme deuxième région de réplica *USA Est*.
1. Quand vous avez terminé, sélectionnez **Vérifier + créer**. Azure validera la configuration.
1. Une fois la définition d’image validée, sélectionnez **Créer**.
1. Une fois le déploiement terminé, sélectionnez **Accéder à la ressource**.

La réplication de l’image à l’ensemble des régions cibles peut prendre un certain temps.

## <a name="share-the-gallery"></a>Partager la galerie

Nous vous recommandons de partager l’accès au niveau de la galerie d’image. Les procédures suivantes vous guident dans les étapes de partage de la galerie que vous venez de créer.

1. Ouvrez le [portail Azure](https://portal.azure.com).
1. Dans le menu de gauche, sélectionnez **Groupes de ressources**. 
1. Dans la liste des groupes de ressources, sélectionnez **myGalleryRG**. Le panneau de votre groupe de ressources s’ouvre.
1. Dans le menu à gauche de la page **myGalleryRG**, sélectionnez **Contrôle d’accès (IAM)** . 
1. Sous **Ajouter une attribution de rôle**, sélectionnez **Ajouter**. Le volet **Ajouter une attribution de rôle** s’ouvre. 
1. Sous **Rôle**, sélectionnez **Lecteur**.
1. Sous **Attribuer l’accès à**, laissez la valeur par défaut **Utilisateur, groupe ou principal du service Azure AD**.
1. Sous **Sélectionner**, saisissez l’adresse de messagerie de la personne que vous souhaitez inviter.
1. Si l’utilisateur se trouve en dehors de votre organisation, vous verrez le message **Cet utilisateur recevra un e-mail qui lui permettra de collaborer avec Microsoft.** Sélectionnez l’utilisateur avec son adresse e-mail, puis cliquez sur **Enregistrer**.

Si l’utilisateur se trouve en dehors de votre organisation, il recevra un message d’invitation à rejoindre l’organisation. L’utilisateur doit accepter l’invitation, puis il pourra voir la galerie et toutes les définitions et versions d’image versions dans sa liste de ressources.

