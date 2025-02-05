---
title: Démarrage rapide pour Microsoft Azure Data Box Heavy | Microsoft Docs
description: Découvrez comment déployer rapidement votre Azure Data Box Heavy dans le portail Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: alkohli
ms.openlocfilehash: 3467b25c085fb86d4aed3918d5446d118f76ffb8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446732"
---
# <a name="quickstart-deploy-azure-data-box-heavy-using-the-azure-portal"></a>Démarrage rapide : Déployer Azure Data Box Heavy à l’aide du portail Azure

Ce démarrage rapide explique comment déployer Azure Data Box Heavy à l’aide du portail Azure. Ces étapes précisent comment brancher les câbles, puis configurer et copier des données vers Data Box Heavy afin de les charger dans Azure. Le démarrage rapide est effectué dans le portail Azure et sur l’interface utilisateur web locale de l’appareil.

Pour obtenir des instructions pas à pas sur le déploiement et le suivi, consultez [Tutoriel : Commander Azure Data Box Heavy](data-box-heavy-deploy-ordered.md)

## <a name="prerequisites"></a>Prérequis

Effectuez les prérequis de configuration suivants pour le site d’installation, le service Data Box et l’appareil avant de déployer de dernier.

### <a name="for-installation-site"></a>Pour le site d’installation

Avant de commencer, assurez-vous que :

- L’appareil peut passer dans toutes vos entrées. Les dimensions de l’appareil sont les suivantes : largeur : 26 pouces (66,04 cm) longueur : 48 pouces (121,92 cm) hauteur : 28 pouces (71,12 cm).
- Vous avez accès à l’appareil par le biais d’un ascenseur ou d’une rampe si vous prévoyez une installation à autre étage que le rez-de-chaussée.
- Deux personnes sont affectées à la gestion de l’appareil. L’appareil pèse environ 500 livres (226,8 kg). Il est équipé de roues.
- Vous disposez d’une surface plane dans le centre de données à proximité d’une connexion réseau disponible et pouvant loger un appareil ayant cet encombrement.

### <a name="for-service"></a>Pour le service

Avant de commencer, assurez-vous que :

- Vous disposez d’un compte de stockage Microsoft Azure doté d’informations d’identification d’accès.
- L’abonnement que vous utilisez dans le cadre du service Data Box service est de type [Accord Entreprise Microsoft (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), [Fournisseur de solutions cloud (CSP)](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview) ou [Microsoft Azure Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/).
- Vous disposez d’un accès propriétaire ou contributeur à l’abonnement pour créer une commande Data Box Heavy.

### <a name="for-device"></a>Pour l’appareil

Avant de commencer, assurez-vous que :

- Vous avez passé en revue les [consignes de sécurité pour votre Data Box Heavy](data-box-safety.md).
- Vous disposez d’un ordinateur hôte connecté au réseau du centre de données. Le Data Box Heavy va copier les données de cet ordinateur. Votre ordinateur hôte doit exécuter un [système d’exploitation pris en charge](data-box-heavy-system-requirements.md).
- Vous disposez d’un ordinateur portable équipé d’un câble RJ-45 pour vous connecter à l’interface utilisateur locale et configurer l’appareil. Utilisez l’ordinateur portable pour configurer une fois chaque nœud de l’appareil.
- Votre centre de données dispose d’un réseau haut débit avec une connexion 10 GbE minimum.
- Vous devez disposer d’un câble de 40 Gbits/s ou de 10 Gbits/s par nœud d’appareil. Choisissez des câbles compatibles avec l’interface réseau Mellanox MCX314A-BCCT :
    - Concernant le câble à 40 Gbits/s, l’extrémité du câble côté appareil doit être QSFP+.
    - Concernant le câble à 10 Gbits/s, vous devez disposer d’un câble SFP+ dont une extrémité est branchée à un commutateur 10 G, avec un adaptateur QSFP+ en SFP+ (ou un adaptateur QSA) pour l’extrémité qui est branchée sur l’appareil.
- Les câbles d’alimentation sont inclus et rangés dans un tiroir à l’arrière de l’appareil.

## <a name="sign-in-to-azure"></a>Connexion à Azure

Connectez-vous au portail Azure sur [https://portal.azure.com](https://portal.azure.com).

## <a name="order"></a>Ordre

Cette étape prend environ 5 minutes.

1. Créez une ressource Azure Data Box dans le portail Azure.
2. Sélectionnez un abonnement existant activé pour ce service et choisissez le type de transfert **Importation**. Fournissez le **pays source** où résident les données et la **région de destination Azure** pour le transfert des données.
3. Sélectionnez **Data Box Heavy**. La capacité utile maximale de la solution est de 770 To, et vous pouvez créer plusieurs commandes pour des tailles de données supérieures.
4. Entrez les détails de la commande et les informations d’expédition. Si le service est disponible dans votre région, fournissez les adresses e-mail de notification, vérifiez le résumé, puis créez la commande.

Une fois la commande créée, l’appareil est préparé pour l’expédition.

## <a name="cable-for-power"></a>Branchement des câbles d’alimentation

Cette étape prend environ 5 minutes.

Lorsque vous recevez la Data Box Heavy, procédez comme suit pour brancher et activer l’appareil.

1. S’il existe des preuves que l’appareil a été trafiqué ou endommagé, arrêtez-vous. Contactez le support Microsoft pour demander un appareil de remplacement.
2. Déplacez l’appareil sur l’emplacement d’installation et bloquez les roues arrière.
3. Connectez les quatre câbles aux blocs d’alimentation situés à l’arrière de l’appareil.
4. Utilisez les boutons d’alimentation situés sur le plan avant pour activer les nœuds de l’appareil.

## <a name="cable-first-node-for-network"></a>Brancher le premier nœud au réseau

Cette étape prend environ 10 à 15 minutes.

1. Utilisez le câble réseau RJ-45 CAT 6 pour connecter votre ordinateur hôte au port de gestion (MGMT) sur l'appareil.
2. Utilisez le câble en cuivre Twinax QSFP+ pour connecter au moins une interface réseau de 40 Gbit/s (de préférence supérieure à 1 Gbit/s), DATA 1 ou DATA 2 pour les données. En cas d’utilisation d’un commutateur 10 Gbits/s, utilisez un câble en cuivre Twinax SFP+ avec une carte QSFP+ à SFP+ (la carte QSA) pour connecter l’interface réseau 40 Gbits/s pour les données.
3. Branchez l’appareil comme indiqué ci-dessous.  

    ![Data Box Heavy branché](media/data-box-heavy-quickstart-portal/data-box-heavy-ports-cabled.png)  

## <a name="configure-first-node"></a>Configurer le premier nœud

Cette étape prend environ 5 à 7 minutes.

1. Pour obtenir le mot de passe du support, accédez à **Général > Détails du support** dans le [portail Azure](https://portal.azure.com). Le même mot de passe même est utilisé pour les deux nœuds de l’appareil.
2. Assignez une adresse IP statique 192.168.100.5 et le sous-réseau 255.255.255.0 à la carte Ethernet de l’ordinateur que vous utilisez pour vous connecter à Data Box Heavy. Accédez à l’interface utilisateur web locale de l’appareil à l’adresse `https://192.168.100.10`. Une fois l’appareil sous tension, la connexion peut prendre jusqu'à 5 minutes.
3. Connectez-vous avec le mot de passe à partir du portail Azure. Une erreur vous signale un problème avec le certificat de sécurité du site web. Suivez les instructions spécifiques au navigateur pour afficher la page web.
4. Par défaut, les paramètres réseau des interfaces (à l’exception de MGMT) sont configurés en mode DHCP. Si nécessaire, vous pouvez configurer ces interfaces en mode statique et fournir une adresse IP.

## <a name="cable-and-configure-the-second-node"></a>Brancher et configurer le second nœud

Cette étape prend environ 15 à 20 minutes.

Effectuez les étapes du premier nœud pour brancher et configurer le deuxième nœud sur l’appareil.  

## <a name="copy-data"></a>Copier des données

Le temps nécessaire à cette opération dépend de la taille de vos données et la vitesse du réseau sur lequel ces données sont copiées.
 
1. Copiez les données sur les deux nœuds de l’appareil en utilisant les deux interfaces de données 40 Gbit/s en parallèle.

    - Si vous utilisez un hôte Windows, utilisez un outil de copie de fichiers compatible avec SMB tel que [Robocopy](https://technet.microsoft.com/library/ee851678.aspx).
    - Pour l’hôte NFS, utilisez la commande `cp` ou `rsync` pour copier les données.
2. Connectez-vous aux partages sur l’appareil en utilisant le chemin d’accès :`\\<IP address of your device>\ShareName`. Pour obtenir les informations d'identification d'accès aux partages, accédez à la page **Connexion et copie** de l'interface utilisateur web locale de Data Box Heavy.
3. Assurez-vous que les noms des partages, des dossiers, et les données suivent les instructions décrites dans la section [Limites des services Stockage Azure et Data Box Heavy](data-box-heavy-limits.md).

## <a name="prepare-to-ship"></a>Préparer l’expédition

Le temps nécessaire à cette opération dépend de la taille de vos données.

1. Une fois la copie des données terminée sans erreur, accédez à la page **Préparation de l'expédition** dans l'interface utilisateur web locale et commencez la préparation de l'expédition.
2. Une fois la **préparation de l'expédition** terminée avec succès sur les deux nœuds, désactivez l’appareil à partir de l’interface utilisateur web locale.

## <a name="ship-to-azure"></a>Envoyer à Azure

Cette opération prend environ 15 à 20 minutes.

1. Débranchez les câbles et rangez-les dans le tiroir à l’arrière de l’appareil.
2. Planifiez un enlèvement avec votre transporteur régional.
3. Contactez le service des [Opérations Data Box](mailto:DataBoxOps@microsoft.com) pour l’informer de l’enlèvement et obtenir l’étiquette de réexpédition.
4. L’étiquette de retour doit être visible sur le panneau avant transparent de l’appareil.

## <a name="verify-data"></a>Vérifier les données

Le temps nécessaire à cette opération dépend de la taille de vos données.

1. Lorsque l’appareil Data Box Heavy est connecté au réseau du centre de données Azure, les données sont automatiquement chargées vers Azure.
2. Le service Data Box vous avertit que la copie des données est terminée via le portail Azure.

    1. Vérifiez les journaux d’activité d’erreurs pour connaître les échecs éventuels et prendre les mesures appropriées.
    2. Vérifiez que vos données se trouvent dans les comptes de stockage avant de les supprimer de la source.

## <a name="clean-up-resources"></a>Supprimer des ressources

Cette étape dure 2 à 3 minutes.

- Vous pouvez annuler la commande Data Box Heavy dans le portail Azure avant son traitement. Une fois que la commande est traitée, elle ne peut plus être annulée. La commande suit une progression jusqu’à la phase finale. Pour annuler la commande, accédez à **Vue d’ensemble** et cliquez sur **Annuler** à partir de la barre de commandes.

- Vous pouvez supprimer la commande une fois que l’état **Terminé** ou **Annulé** s’affiche dans le portail Azure. Pour supprimer la commande, accédez à **Vue d’ensemble** et cliquez sur **Supprimer** à partir de la barre de commandes.

## <a name="next-steps"></a>Étapes suivantes

Dans ce démarrage rapide, vous avez déployé un appareil Data Box Heavy pour faciliter l’importation de vos données dans Azure. Pour en savoir plus sur la gestion Azure Data Box Heavy, passez au didacticiel suivant :

> [!div class="nextstepaction"]
> [Use the Azure portal to administer Data Box Disk](data-box-portal-admin.md) (Utiliser le portail Azure pour administrer le Data Box Heavy)
