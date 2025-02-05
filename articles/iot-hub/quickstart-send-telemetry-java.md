---
title: Démarrage rapide pour envoyer des données de télémétrie à Azure IoT Hub (Java) | Microsoft Docs
description: Dans ce guide de démarrage rapide, vous exécutez deux exemples d’applications Java pour envoyer des données de télémétrie simulées à un IoT Hub et lire les données de télémétrie provenant de l’IoT Hub à traiter dans le cloud.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/21/2019
ms.openlocfilehash: 52e221088a7b12551636ecdc81532448f38eb26c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330463"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-java"></a>Démarrage rapide : Envoyer des données de télémétrie d’un appareil à un hub IoT et les lire avec une application back-end (Java)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub est un service Azure qui vous permet de traiter de gros volumes de données de télémétrie envoyées par vos appareils IoT dans le cloud à des fins de stockage ou de traitement. Dans ce guide de démarrage rapide, vous envoyez des données de télémétrie à partir d’une application d’appareil simulé, via IoT Hub, à une application back-end pour traitement.

Ce démarrage rapide utilise deux applications Java prédéfinies, une pour envoyer des données de télémétrie et l’autre pour lire ces données de télémétrie provenant du concentrateur. Avant d’exécuter ces deux applications, vous créez un IoT Hub et inscrivez un appareil auprès du concentrateur.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Les deux exemples d’applications que vous exécutez dans ce guide de démarrage rapide sont écrits à l’aide de Java. Votre machine de développement doit disposer de Java SE 8 ou version ultérieure.

Vous pouvez télécharger Java pour plusieurs plateformes sur [Oracle](https://aka.ms/azure-jdks).

Vous pouvez vérifier la version actuelle de Java sur votre machine de développement à l’aide de la commande suivante :

```cmd/sh
java -version
```

Pour générer les exemples, vous devez installer Maven 3. Vous pouvez télécharger Maven pour plusieurs plateformes sur [Apache Maven](https://maven.apache.org/download.cgi).

Vous pouvez vérifier la version actuelle de Maven sur votre machine de développement à l’aide de la commande suivante :

```cmd/sh
mvn --version
```

Exécutez la commande suivante afin d’ajouter l’extension Microsoft Azure IoT pour Azure CLI à votre instance Cloud Shell. L’extension IoT ajoute des commandes IoT Hub, IoT Edge et IoT Device Provisioning Service (DPS) à Azure CLI.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Téléchargez l’exemple de projet Java à partir de https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip et extrayez l’archive ZIP.

## <a name="create-an-iot-hub"></a>Créer un hub IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Inscrire un appareil

Un appareil doit être inscrit dans votre hub IoT pour pouvoir se connecter. Dans ce démarrage rapide, vous utilisez Azure Cloud Shell pour inscrire un appareil simulé.

1. Exécutez les commandes suivantes dans Azure Cloud Shell pour créer l’identité d’appareil.

   **YourIoTHubName** : Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

   **MyJavaDevice** : Nom de l’appareil que vous inscrivez. Utilisez **MyJavaDevice** comme indiqué. Si vous choisissez un autre nom pour votre appareil, vous devez utiliser ce nom pour l’ensemble de cet article et mettre à jour le nom de l’appareil dans les exemples d’application avant de les exécuter.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyJavaDevice
    ```

2. Exécutez les commandes suivantes dans Azure Cloud Shell pour obtenir la _chaîne de connexion d’appareil_ de celui que vous venez d’inscrire : **YourIoTHubName: Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyJavaDevice --output table
    ```

    Notez la chaîne de connexion à l’appareil, qui ressemble à ce qui suit :

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Vous utiliserez cette valeur plus loin dans ce démarrage rapide.

3. Vous avez aussi besoin du _point de terminaison compatible Event Hubs_, du _chemin d’accès compatible Event Hubs_ et de la _clé principale du service_ à partir de votre IoT Hub pour permettre à l’application back-end de se connecter à votre IoT Hub et de récupérer les messages. Les commandes suivantes extraient ces valeurs pour votre IoT Hub :

     **YourIoTHubName: Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name YourIoTHubName

    az iot hub show --query properties.eventHubEndpoints.events.path --name YourIoTHubName

    az iot hub policy show --name service --query primaryKey --hub-name YourIoTHubName
    ```

    Prenez note de ces trois valeurs, vous les utiliserez plus loin dans ce démarrage rapide.

## <a name="send-simulated-telemetry"></a>Envoyer des données de télémétrie simulée

L’application d’appareil simulé se connecte à un point de terminaison spécifique de l’appareil sur votre IoT Hub et envoie les données de télémétrie simulée (température et humidité).

1. Dans une fenêtre de terminal local, accédez au dossier racine de l’exemple de projet Java. Ensuite, accédez au dossier **iot-hub\Quickstarts\simulated-device**.

2. Ouvrez le fichier **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** dans l’éditeur de texte de votre choix.

    Remplacez la valeur de la variable `connString` par la chaîne de connexion d’appareil que vous avez notée précédemment. Puis, enregistrez les modifications apportées au fichier **SimulatedDevice.java**.

3. Dans la fenêtre de terminal local, exécutez les commandes suivantes pour installer les bibliothèques requises et générer l’application d’appareil simulé :

    ```cmd/sh
    mvn clean package
    ```

4. Dans la fenêtre de terminal local, exécutez les commandes suivantes pour démarrer l’application d’appareil simulé :

    ```cmd/sh
    java -jar target/simulated-device-1.0.0-with-deps.jar
    ```

    La capture d’écran suivante présente la sortie lorsque l’application d’appareil simulé envoie des données de télémétrie à votre IoT Hub :

    ![Exécuter l’appareil simulé](media/quickstart-send-telemetry-java/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Lire les données de télémétrie envoyées par votre hub

L’application back-end se connecte au point de terminaison **Événements** du service sur votre IoT Hub. L’application reçoit les messages appareil-à-cloud envoyés à partir de votre appareil simulé. Une application back-end IoT Hub s’exécute généralement dans le cloud pour recevoir et traiter les messages appareil-à-cloud.

1. Dans une autre fenêtre de terminal local, accédez au dossier racine de l’exemple de projet Java. Puis, accédez au dossier **iot-hub\Quickstarts\read-d2c-messages**.

2. Ouvrez le fichier **src/main/java/com/microsoft/docs/iothub/samples/ReadDeviceToCloudMessages.java** dans l’éditeur de texte de votre choix. Mettez à jour les variables suivantes et enregistrez vos modifications dans le fichier.

    | Variable | Valeur |
    | -------- | ----------- |
    | `eventHubsCompatibleEndpoint` | Remplacez la valeur de la variable par le point de terminaison compatible Event Hubs que vous avez noté précédemment. |
    | `eventHubsCompatiblePath`     | Remplacez la valeur de la variable par le chemin d’accès compatible Event Hubs que vous avez noté précédemment. |
    | `iotHubSasKey`                | Remplacez la valeur de la variable par la clé principale de service que vous avez notée précédemment. |

3. Dans la fenêtre de terminal local, exécutez les commandes suivantes pour installer les bibliothèques requises et générer l’application back-end :

    ```cmd/sh
    mvn clean package
    ```

4. Dans la fenêtre de terminal local, exécutez les commandes suivantes pour exécuter l’application back-end :

    ```cmd/sh
    java -jar target/read-d2c-messages-1.0.0-with-deps.jar
    ```

    La capture d’écran suivante présente la sortie lorsque l’application back-end reçoit les données de télémétrie envoyées par l’appareil simulé au concentrateur :

    ![Exécuter l’application back-end](media/quickstart-send-telemetry-java/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Supprimer des ressources

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez configuré un IoT Hub, inscrit un appareil, envoyé des données de télémétrie simulée au concentrateur à l’aide d’une application Java et lu les données de télémétrie à partir du concentrateur à l’aide d’une application back-end simple.

Pour savoir comment contrôler votre appareil simulé à partir d’une application back-end, passez au démarrage rapide suivant.

> [!div class="nextstepaction"]
> [Démarrage rapide : Contrôler un appareil connecté à un hub IoT](quickstart-control-device-java.md)