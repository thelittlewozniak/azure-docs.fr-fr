---
title: Didacticiel pour la sécurité X.509 dans Azure IoT Hub | Microsoft Docs
description: Commencez à utiliser la sécurité X.509 dans votre Azure IoT Hub dans un environnement simulé.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.openlocfilehash: 0bfb66f54ec09e86b46a41499211e93a0083e8d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65779927"
---
# <a name="set-up-x509-security-in-your-azure-iot-hub"></a>Configurer la sécurité X.509 dans votre Azure IoT Hub

Ce didacticiel simule la procédure que vous devez suivre pour sécuriser votre Azure IoT Hub à l’aide de *l’authentification de certificat X.509*. À titre d’illustration, nous indiquons ici comment utiliser l’outil open source OpenSSL pour créer des certificats localement sur votre machine Windows. Nous vous recommandons de n’utiliser ce didacticiel qu’à des fins de test. Pour un environnement de production, vous devez acheter les certificats auprès d’une *autorité de certification racine (CA)* .

## <a name="prerequisites"></a>Prérequis

Pour suivre ce didacticiel, vous devez disposer des ressources suivantes :

* Vous avez créé un IoT Hub avec votre abonnement Azure. Pour découvrir la procédure détaillée correspondante, consultez l’article [Création d’un IoT Hub à l’aide du portail Azure](iot-hub-create-through-portal.md).

* Assurez-vous que votre machine est équipée de [Visual Studio 2017 ou Visual Studio 2019](https://www.visualstudio.com/vs/).

## <a name="get-x509-ca-certificates"></a>Obtenir des certificats d’autorité de certification X.509

Pour mettre en place une sécurité basée sur un certificat X.509 dans le IoT Hub, vous devez commencer par une [chaîne d’approbation X.509](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), qui inclut le certificat racine, ainsi que tous les certificats intermédiaires jusqu’au certificat feuille.

Pour obtenir vos certificats, vous pouvez choisir l’une des méthodes suivantes :

* Achetez des certificats X.509 auprès d’une *autorité de certification racine*. Cette approche est recommandée pour les environnements de production.

* Créez vos propres certificats X.509 à l’aide d’un outil tiers, par exemple [OpenSSL](https://www.openssl.org/). Cette méthode se révèle parfaitement adaptée aux scénarios de test et de développement. Consultez [Gestion de certificats d’autorité de certification de test pour des exemples et tutoriels](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) pour obtenir des informations sur la génération de certificats d’autorité de certification de test à l’aide de PowerShell ou Bash. Le reste de ce tutoriel utilise des certificats d’autorité de certification de test générés en suivant les instructions fournies dans [Gestion de certificats d’autorité de certification de test pour des exemples et tutoriels](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md).

* Générez un [certificat d’autorité de certification intermédiaire X.509](iot-hub-x509ca-overview.md#sign-devices-into-the-certificate-chain-of-trust) signé par un certificat d’autorité de certification racine existant et chargez-le dans le service IoT Hub. Une fois le certificat intermédiaire chargé et vérifié, comme indiqué ci-dessous, il peut être utilisé à la place d’un certificat d’autorité de certification racine mentionné ci-dessous. Des outils tels qu’OpenSSL ([openssl req](https://www.openssl.org/docs/manmaster/man1/openssl-req.html) et [openssl ca](https://www.openssl.org/docs/manmaster/man1/openssl-ca.html)) peuvent être utilisés pour générer et signer un certificat d’autorité de certification intermédiaire.


## <a name="register-x509-ca-certificates-to-your-iot-hub"></a>Inscrire des certificats d’autorité de certification X.509 auprès de votre IoT Hub

Cette section décrit la procédure d’ajout d’une nouvelle autorité de certification à votre IoT Hub par le biais du portail.

1. Dans le portail Azure, accédez à votre IoT Hub, puis ouvrez le menu **PARAMÈTRES** > **Certificats**.

2. Cliquez sur **Ajouter** pour ajouter un nouveau certificat.

3. Entrez un nom d’affichage convivial pour votre certificat. Sélectionnez dans votre machine le fichier de certificat racine *RootCA.cer* créé à la section précédente. Cliquez sur **Télécharger**.

4. Une fois que vous avez obtenu une notification vous informant que votre certificat a été correctement chargé, cliquez sur **Enregistrer**.

    ![Téléchargement d’un certificat](./media/iot-hub-security-x509-get-started/add-new-cert.png)  

   Cette opération affiche votre certificat dans la liste **Explorateur de certificats**. Notez que la colonne **ÉTAT** de ce certificat présente la valeur *Non vérifié*.

5. Cliquez sur le certificat que vous avez ajouté à l’étape précédente.

6. Dans le panneau **Détails du certificat**, cliquez sur **Générer le code de vérification**.

7. Cette opération crée un **Code de vérification** permettant de valider la propriété du certificat. Copiez ce code dans le Presse-papiers.

   ![Vérifier le certificat](./media/iot-hub-security-x509-get-started/verify-cert.png)  

8. Vous devez ensuite signer ce *Code de vérification* avec la clé privée associée à votre certificat d’autorité de certification X.509, ce qui génère une signature. Vous disposez de plusieurs outils pour effectuer ce processus de signature, par exemple OpenSSL. Cette signature est désignée sous le terme de [preuve de possession](https://tools.ietf.org/html/rfc5280#section-3.1). L’étape 3 dans [Gestion de certificats d’autorité de certification de test pour des exemples et tutoriels](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) génère un code de vérification.

9. Chargez la signature générée à l’étape 8 ci-dessus dans votre IoT Hub sur le portail. Dans le panneau **Détails du certificat** du portail Azure, accédez au champ **Fichier .pem ou .cer du certificat de vérification**, puis sélectionnez la signature, par exemple le fichier *VerifyCert4.cer* créé par l’exemple de commande PowerShell, en utilisant l’icône _Explorateur de fichiers_ en regard de ce champ.

10. Une fois le chargement du certificat terminé, cliquez sur **Vérifier**. Dans le panneau **Certificats**, la colonne **ÉTAT** de votre certificat prend la valeur **_Vérifié_** . Si le panneau ne se met pas à jour automatiquement, cliquez sur **Actualiser**.

    ![Charger la vérification du certificat](./media/iot-hub-security-x509-get-started/upload-cert-verification.png)  

## <a name="create-an-x509-device-for-your-iot-hub"></a>Créer un appareil X.509 pour votre IoT Hub

1. Dans le portail Azure, accédez à la page **Explorateurs > Appareils IoT** de votre hub IoT.

2. Cliquez sur **+ Ajouter** pour ajouter un nouvel appareil.

3. Définissez un nom d’affichage convivial dans la zone **ID de l’appareil**, puis sous la zone **Type d’authentification**, sélectionnez **_X.509 Signé par une autorité de certification_** . Cliquez sur **Enregistrer**.

   ![Créer un appareil X.509 dans le portail](./media/iot-hub-security-x509-get-started/create-x509-device.png)

## <a name="authenticate-your-x509-device-with-the-x509-certificates"></a>Authentifier votre appareil X.509 avec les certificats X.509

Pour authentifier votre appareil X.509, vous devez commencer par signer l’appareil avec le certificat d’autorité de certification. La signature des appareils feuilles est généralement effectuée à l’usine de fabrication, dans laquelle les outils de fabrication ont été activés en conséquence. Lorsque l’appareil passe d’un fabricant à un autre, l’action de signature de chaque fabricant est capturée sous la forme d’un certificat intermédiaire dans la chaîne. Le résultat final est une chaîne d’approbation comprenant plusieurs certificats, depuis le certificat d’autorité de certification jusqu’au certificat feuille de l’appareil. L’étape 4 dans [Gestion de certificats d’autorité de certification de test pour des exemples et tutoriels](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) génère un certificat d’appareil.

Ensuite, nous vous indiquerons comment créer une application C# pour simuler l’appareil X.509 inscrit pour votre IoT Hub. Nous enverrons les valeurs de température et d’humidité de l’appareil simulé sur votre hub. Notez que dans ce didacticiel, nous créerons uniquement l’application de l’appareil. En guide d’exercice, les lecteurs seront chargés de créer l’application de service IoT Hub qui enverra la réponse aux événements envoyés par cet appareil simulé. L’application C# suppose que vous avez suivi les étapes décrites dans [Gestion de certificats d’autorité de certification de test pour des exemples et tutoriels](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md).

1. Dans Visual Studio, créez un projet Visual C# Bureau classique Windows en utilisant le modèle de projet Application de console. Nommez ce projet **SimulateX509Device**.

   ![Créer un projet d’appareil X.509 dans Visual Studio](./media/iot-hub-security-x509-get-started/create-device-project.png)

2. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **SimulateX509Device**, puis cliquez sur **Gérer les packages NuGet...** . Dans la fenêtre Gestionnaire de package NuGet, sélectionnez **Parcourir**, puis recherchez **microsoft.azure.devices.client**. Sélectionnez **Installer** pour installer le package **Microsoft.Azure.Devices.Client**, puis acceptez les conditions d’utilisation. Cette procédure télécharge, installe et ajoute une référence au package NuGet Azure IoT device SDK et ses dépendances.

   ![Ajouter le package NuGet device SDK dans Visual Studio](./media/iot-hub-security-x509-get-started/device-sdk-nuget.png)

3. Ajoutez les lignes de code ci-après au début du fichier *Program.cs* :

    ```CSharp
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using System.Security.Cryptography.X509Certificates;
    ```

4. Ajoutez les lignes de code ci-après dans la classe **Program** :

    ```CSharp
        private static int MESSAGE_COUNT = 5;
        private const int TEMPERATURE_THRESHOLD = 30;
        private static String deviceId = "<your-device-id>";
        private static float temperature;
        private static float humidity;
        private static Random rnd = new Random();
    ```

     Utilisez le nom d’appareil convivial que vous avez utilisé à la section précédente à la place de l’espace réservé _< your_device_id >_ .

5. Ajoutez la fonction ci-après pour créer des valeurs aléatoires pour la température et l’humidité et pour envoyer ces valeurs au hub :

    ```CSharp
    static async Task SendEvent(DeviceClient deviceClient)
    {
        string dataBuffer;
        Console.WriteLine("Device sending {0} messages to IoTHub...\n", MESSAGE_COUNT);

        for (int count = 0; count < MESSAGE_COUNT; count++)
        {
            temperature = rnd.Next(20, 35);
            humidity = rnd.Next(60, 80);
            dataBuffer = string.Format("{{\"deviceId\":\"{0}\",\"messageId\":{1},\"temperature\":{2},\"humidity\":{3}}}", deviceId, count, temperature, humidity);
            Message eventMessage = new Message(Encoding.UTF8.GetBytes(dataBuffer));
            eventMessage.Properties.Add("temperatureAlert", (temperature > TEMPERATURE_THRESHOLD) ? "true" : "false");
            Console.WriteLine("\t{0}> Sending message: {1}, Data: [{2}]", DateTime.Now.ToLocalTime(), count, dataBuffer);

            await deviceClient.SendEventAsync(eventMessage);
        }
    }
    ```

6. Pour finir, ajoutez les lignes de code ci-après à la fonction **Main** en remplaçant les espaces réservés _device-id_, _your-iot-hub-name_ et _absolute-path-to-your-device-pfx-file_ par les informations requises par votre configuration.

    ```CSharp
    try
    {
        var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
        var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
        var deviceClient = DeviceClient.Create("<your-iot-hub-name>.azure-devices.net", auth, TransportType.Amqp_Tcp_Only);

        if (deviceClient == null)
        {
            Console.WriteLine("Failed to create DeviceClient!");
        }
        else
        {
            Console.WriteLine("Successfully created DeviceClient!");
            SendEvent(deviceClient).Wait();
        }

        Console.WriteLine("Exiting...\n");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

   Ce code se connecte à votre IoT Hub en créant la chaîne de connexion pour votre appareil X.509. Une fois le code connecté, il envoie les événements de température et d’humidité au hub, puis attend la réponse de ce dernier. 
7. Étant donné que cette application accède à un fichier *.pfx*, vous pouvez être amené à exécuter cette opération en mode *Administrateur*. Créez la solution Visual Studio. Ouvrez une nouvelle fenêtre de commande en tant **qu’Administrateur** et accédez au dossier contenant cette solution. Accédez au chemin d’accès *bin/Debug* dans le dossier de solution. Exécutez l’application **SimulateX509Device.exe** à partir de la fenêtre de commande _Administrateur_. Votre appareil doit alors se connecter correctement au hub et envoyer les événements. 

   ![Exécuter l’application de l’appareil](./media/iot-hub-security-x509-get-started/device-app-success.png)

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur la sécurisation de votre solution IoT, consultez :

* [Meilleures pratiques de sécurité pour l’Internet des objets (IoT)](../iot-fundamentals/iot-security-best-practices.md)

* [Architecture de sécurité de l’Internet des objets (IoT)](../iot-fundamentals/iot-security-architecture.md)

* [Sécuriser votre déploiement IoT](../iot-fundamentals/iot-security-deployment.md)

Pour explorer davantage les capacités de IoT Hub, consultez :

* [Déploiement d’une IA sur des appareils de périmètre avec Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
