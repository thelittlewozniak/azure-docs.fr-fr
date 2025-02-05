---
title: Démarrage rapide (Android) pour contrôler un appareil à partir d’Azure IoT Hub | Microsoft Docs
description: Dans ce guide de démarrage rapide, vous exécutez deux exemples d’applications Java. Une application est une application de service qui peut contrôler à distance des appareils connectés à votre hub. L’autre application s’exécute sur un appareil physique ou simulé, connecté à votre hub qui peut être contrôlé à distance.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/21/2019
ms.author: wesmc
ms.openlocfilehash: d125328d903b419aa81c54ffecf1f549d4cb4fe2
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330787"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-android"></a>Démarrage rapide : contrôler un appareil connecté à un hub IoT (Android)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub est un service Azure qui vous permet de traiter de gros volumes de données de télémétrie envoyées par vos appareils IoT dans le cloud et de gérer vos appareils à partir du cloud. Dans ce guide de démarrage rapide, vous utilisez une *méthode directe* pour contrôler un appareil simulé connecté à votre IoT Hub. Vous pouvez utiliser les méthodes directes pour modifier à distance le comportement d’un appareil connecté à votre IoT Hub.

Ce démarrage rapide utilise deux applications Java prédéfinies :

* Une application d’appareil simulé qui répond aux méthodes directes appelées à partir d’une application de service back-end. Pour recevoir les appels de méthode directe, cette application se connecte à un point de terminaison spécifique à l’appareil sur votre IoT Hub.

* Une application de service qui appelle la méthode directe sur l’appareil Android. Pour appeler une méthode directe sur un appareil, cette application se connecte à un point de terminaison côté service sur votre IoT Hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="prerequisites"></a>Prérequis

* Android Studio à partir de https://developer.android.com/studio/. Pour plus d’informations sur l’installation d’Android Studio, consultez [android-installation](https://developer.android.com/studio/install).

* Android SDK 27 est utilisé par l’exemple dans cet article.

* Exécutez la commande suivante afin d’ajouter l’extension Microsoft Azure IoT pour Azure CLI à votre instance Cloud Shell. L’extension IoT ajoute des commandes IoT Hub, IoT Edge et IoT Device Provisioning Service (DPS) à Azure CLI.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

* Deux exemples d’applications sont requis pour ce guide de démarrage rapide : l’[exemple d’application Android du kit SDK d’appareil](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample) et l’[exemple d’application Android du kit SDK de service](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/service/AndroidSample). Ces deux exemples font partie du dépôt azure-iot-samples-java sur GitHub. Téléchargez ou clonez le dépôt [azure-iot-samples-java](https://github.com/Azure-Samples/azure-iot-samples-java).

## <a name="create-an-iot-hub"></a>Créer un hub IoT

Si vous avez terminé le précédent [Démarrage rapide : envoyer des données de télémétrie depuis un appareil à un hub IoT](quickstart-send-telemetry-android.md), vous pouvez ignorer cette étape et utiliser le hub IoT que vous avez déjà créé.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Inscrire un appareil

Si vous avez terminé le précédent [Démarrage rapide : envoyer des données de télémétrie depuis un appareil à un hub IoT](quickstart-send-telemetry-android.md), vous pouvez ignorer cette étape et utiliser l’appareil inscrit dans le guide de démarrage rapide précédent.

Un appareil doit être inscrit dans votre hub IoT pour pouvoir se connecter. Dans ce démarrage rapide, vous utilisez Azure Cloud Shell pour inscrire un appareil simulé.

1. Exécutez les commandes suivantes dans Azure Cloud Shell pour créer l’identité d’appareil.

   **YourIoTHubName** : Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

   **MyAndroidDevice** : cette valeur correspond au nom donné à l’appareil inscrit. Utilisez MyAndroidDevice comme indiqué. Si vous choisissez un autre nom pour votre appareil, vous pouvez également être amené à utiliser ce nom pour l’ensemble de cet article et à mettre à jour le nom de l’appareil dans les exemples d’application avant de les exécuter.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name YourIoTHubName --device-id MyAndroidDevice
    ```

2. Exécutez les commandes suivantes dans Azure Cloud Shell pour obtenir la _chaîne de connexion_ à l’appareil que vous venez d’inscrire :

   **YourIoTHubName** : Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name YourIoTHubName \
      --device-id MyAndroidDevice \
      --output table
    ```

    Notez la chaîne de connexion à l’appareil, qui ressemble à ce qui suit :

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    Vous utiliserez cette valeur plus loin dans ce démarrage rapide.

## <a name="retrieve-the-service-connection-string"></a>Récupérer la chaîne de connexion de service

Vous avez également besoin d’une _chaîne de connexion de service_ pour permettre aux applications de service back-end de se connecter à votre hub IoT afin d’exécuter les méthodes et de récupérer les messages. La commande suivante récupère la chaîne de connexion de service correspondant à votre hub IoT :

**YourIoTHubName** : Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

```azurecli-interactive
az iot hub show-connection-string --name YourIoTHubName --policy-name service --output table
```

Notez la chaîne de connexion de service, qui ressemble à ce qui suit :

`HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

Vous utiliserez cette valeur plus loin dans ce démarrage rapide. La chaîne de connexion de service est différente de la chaîne de connexion d’appareil.

## <a name="listen-for-direct-method-calls"></a>Écouter les appels de méthode directe

L’exemple d’application de kit SDK d’appareil peut être exécuté sur un appareil Android physique ou sur un émulateur Android. Cet exemple se connecte à un point de terminaison spécifique de l’appareil sur votre hub IoT, envoie les données de télémétrie simulée et écoute les appels de méthode directe provenant de votre hub. Dans ce démarrage rapide, l’appel de méthode directe à partir du concentrateur indique à l’appareil de modifier la fréquence à laquelle il envoie des données de télémétrie. L’appareil simulé renvoie un accusé de réception à votre concentrateur après l’exécution de la méthode directe.

1. Ouvrez l’exemple de projet Android GitHub dans Android Studio. Ce projet se trouve dans le répertoire suivant de votre copie clonée ou téléchargée du dépôt [azure-iot-sample-java](https://github.com/Azure-Samples/azure-iot-samples-java).

        \azure-iot-samples-java\iot-hub\Samples\device\AndroidSample

2. Dans Android Studio, ouvrez *gradle.properties* pour l’exemple de projet et remplacez l’espace réservé **Device_Connection_String** par votre chaîne de connexion d’appareil que vous avez notée précédemment.

    ```
    DeviceConnectionString=HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}
    ```

3. Dans Android Studio, cliquez sur **Fichier** > **Synchronisation du projet avec les fichiers Gradle**. Vérifiez que la build est terminée.

   > [!NOTE]
   > Si la synchronisation du projet échoue, c’est peut-être pour l’une des raisons suivantes :
   >
   > * La version du plug-in Android Gradle et celle de Gradle référencées dans le projet sont obsolètes pour votre version d’Android Studio. Suivez [ces instructions](https://developer.android.com/studio/releases/gradle-plugin) pour référencer et installer les bonnes versions du plug-in et de Gradle pour votre installation.
   > * Le contrat de licence du kit de développement logiciel Android SDK n’a pas été signé. Suivez les instructions dans la sortie de la build pour signer le contrat de licence et télécharger le SDK.

4. Une fois la build terminée, cliquez sur **Exécuter** > **Exécuter « application »** . Configurez l’application pour l’exécuter sur un appareil Android physique ou un émulateur Android. Pour plus d’informations sur l’exécution d’une application Android sur un appareil physique ou un émulateur, consultez [Exécuter votre application](https://developer.android.com/training/basics/firstapp/running-app).

5. Une fois l’application chargée, cliquez sur le bouton **Démarrer** pour commencer à envoyer les données de télémétrie à votre hub IoT :

    ![Exemple de capture d’écran d’application d’appareil android client](media/quickstart-control-device-android/sample-screenshot.png)

Cette application doit rester en cours d’exécution sur un appareil physique ou un émulateur pendant que vous exécutez l’exemple de kit SDK de service pour mettre à jour l’intervalle de télémétrie pendant l’exécution.

## <a name="read-the-telemetry-from-your-hub"></a>Lire les données de télémétrie envoyées par votre hub

Dans cette section, vous allez utiliser Azure Cloud Shell avec l’[extension IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) pour surveiller les messages d’appareil envoyés par l’appareil Android.

1. À l’aide d’Azure Cloud Shell, exécutez la commande suivante pour vous connecter et lire les messages à partir de votre hub IoT :

   **YourIoTHubName** : Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```

    La capture d’écran suivante montre la sortie lorsque le hub IoT reçoit les données de télémétrie envoyées par l’appareil Android :

      ![Lire les messages d’appareil à l’aide de l’interface Azure CLI](media/quickstart-control-device-android/read-data.png)

Par défaut, l’application de télémétrie envoie les données de télémétrie à partir de l’appareil Android toutes les 5 secondes. Dans la section suivante, vous allez utiliser un appel de méthode directe pour mettre à jour l’intervalle de télémétrie de l’appareil IoT Android.

## <a name="call-the-direct-method"></a>Appeler la méthode directe

L’application de service se connecte au point de terminaison côté service sur votre hub IoT. L’application effectue des appels de méthode directe à un appareil via votre IoT Hub et écoute les accusés de réception.

Exécutez cette application sur un appareil Android physique ou un émulateur Android distinct.

Une application de service back-end de hub IoT s’exécute généralement dans le cloud où il est plus facile d’atténuer les risques associés à la chaîne de connexion sensible qui contrôle tous les appareils sur un hub IoT. Dans cet exemple, nous l’exécutons comme une application Android uniquement à des fins de démonstration. Les versions de ce guide de démarrage rapide propres à d’autres langages fournissent d’autres exemples qui s’alignent plus étroitement avec une application de service back-end.

1. Ouvrez l’exemple de projet Android de service GitHub dans Android Studio. Ce projet se trouve dans le répertoire suivant de votre copie clonée ou téléchargée du dépôt [azure-iot-sample-java](https://github.com/Azure-Samples/azure-iot-samples-java).

        \azure-iot-samples-java\iot-hub\Samples\service\AndroidSample

2. Dans Android Studio, ouvrez *gradle.properties* pour l’exemple de projet et mettez à jour la valeur des propriétés **ConnectionString** et **DeviceId** avec votre chaîne de connexion de service que vous avez notée précédemment et l’ID d’appareil Android que vous avez inscrit.

    ```
    ConnectionString=HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}
    DeviceId=MyAndroidDevice
    ```

3. Dans Android Studio, cliquez sur **Fichier** > **Synchronisation du projet avec les fichiers Gradle**. Vérifiez que la build est terminée.

   > [!NOTE]
   > Si la synchronisation du projet échoue, c’est peut-être pour l’une des raisons suivantes :
   >
   > * La version du plug-in Android Gradle et celle de Gradle référencées dans le projet sont obsolètes pour votre version d’Android Studio. Suivez [ces instructions](https://developer.android.com/studio/releases/gradle-plugin) pour référencer et installer les bonnes versions du plug-in et de Gradle pour votre installation.
   > * Le contrat de licence du kit de développement logiciel Android SDK n’a pas été signé. Suivez les instructions dans la sortie de la build pour signer le contrat de licence et télécharger le SDK.

4. Une fois la build terminée, cliquez sur **Exécuter** > **Exécuter « application »** . Configurez l’application pour l’exécuter sur un appareil Android physique ou un émulateur Android distinct. Pour plus d’informations sur l’exécution d’une application Android sur un appareil physique ou un émulateur, consultez [Exécuter votre application](https://developer.android.com/training/basics/firstapp/running-app).

5. Une fois l’application chargée, mettez à jour la valeur **Définir l’intervalle de messagerie** sur **1000** et cliquez sur **Appeler**.

    L’intervalle de messagerie de télémétrie est exprimé en millisecondes. L’intervalle de télémétrie par défaut de l’exemple d’appareil est défini sur 5 secondes. Ce changement met à jour l’appareil IoT Android afin que les données de télémétrie soient envoyées à chaque seconde.

    ![Entrer l’intervalle de télémétrie](media/quickstart-control-device-android/enter-telemetry-interval.png)

6. L’application reçoit un accusé de réception indiquant si la méthode a été exécutée avec succès ou non.

    ![Accusé de réception de méthode directe](media/quickstart-control-device-android/direct-method-ack.png)

## <a name="clean-up-resources"></a>Supprimer des ressources

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez appelé une méthode directe sur un appareil à partir d’une application back-end et répondu à l’appel de méthode directe dans une application d’appareil simulé.

Pour savoir comment acheminer les messages appareil-à-cloud vers différentes destinations dans le cloud, passez au tutoriel suivant.

> [!div class="nextstepaction"]
> [Didacticiel : acheminer les données de télémétrie vers différents points de terminaison pour traitement](tutorial-routing.md)
