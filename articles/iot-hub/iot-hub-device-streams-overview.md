---
title: Flux d’appareils Azure IoT Hub (préversion) | Microsoft Docs
description: Vue d’ensemble des flux d’appareils IoT Hub.
author: rezasherafat
manager: briz
services: iot-hub
ms.service: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 672b06dda41edb18cbf31352188b0fdd2a155782
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60401055"
---
# <a name="iot-hub-device-streams-preview"></a>Flux d'appareils IoT Hub (préversion)

## <a name="overview"></a>Vue d'ensemble
Les *flux d’appareils* Azure IoT Hub facilitent la création de tunnels TCP bidirectionnels sécurisés pour divers scénarios de communication cloud-à-appareil. Un flux d'appareil est régi par un *point de terminaison de streaming* IoT Hub qui fait office de proxy entre votre appareil et les points terminaison de service. Illustrée dans le diagramme ci-dessous, cette configuration est particulièrement utile quand les appareils sont situés derrière un pare-feu réseau ou à l’intérieur d’un réseau privé. Dès lors, les flux d’appareils IoT Hub permettent aux clients d'accéder aux appareils IoT de manière sécurisée via un pare-feu, sans devoir trop ouvrir les ports de pare-feu de réseau entrants ou sortants.

![Texte de remplacement](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png "Vue d’ensemble des flux d’appareils IoT Hub")


Grâce aux flux d’appareils IoT Hub, les appareils restent sécurisés et les connexions TCP sortantes vers le point de terminaison de streaming IoT Hub interviennent exclusivement sur le port 443. Une fois le flux établi, les applications côté service et côté appareil bénéficient chacune d’un accès programmatique à un objet client WebSocket afin d’échanger entre elles des octets bruts. Ce tunnel offre des garanties de fiabilité à égalité avec TCP.

## <a name="benefits"></a>Avantages
Les flux d'appareils IoT Hub offrent les avantages suivants :
- **Connectivité sécurisée via un pare-feu :** Les appareils IoT sont accessibles depuis les points de terminaison de service, sans ouverture du port du pare-feu entrant au niveau de l'appareil ou du réseau (seule une connectivité sortante vers IoT Hub est requise sur le port 443).

- **Authentication :** Le côté appareil et le côté service du tunnel doivent s'authentifier auprès de IoT Hub à l'aide des informations d'identification correspondantes.

- **Chiffrement :** Par défaut, les flux d'appareils IoT Hub utilisent des connexions TSL. Ainsi, le trafic est chiffré, indépendamment du fait que l'application utilise ou non le chiffrement.

- **Simplicité de la connectivité :** Dans de nombreux cas, grâce aux flux d’appareils, la connectivité aux appareils IoT ne requiert plus de configuration complexe des réseaux privés virtuels.

- **Compatibilité avec la pile TCP/IP :** Les flux d'appareils IoT Hub peuvent prendre en charge le trafic des applications TCP/IP. Ainsi, un large éventail de protocoles propriétaires et de protocoles basés sur des normes peuvent exploiter cette fonctionnalité.

- **Facilité d'utilisation dans des configurations de réseau privé :** Le service peut communiquer avec un appareil en référençant son ID d’appareil plutôt que son adresse IP. Cela s'avère particulièrement utile lorsqu'un appareil est situé à l'intérieur d’un réseau privé et dispose d'une adresse IP privée, ou que son adresse IP est attribuée de manière dynamique et n'est pas connue côté service.

## <a name="device-stream-workflows"></a>Workflows des flux d'appareils
Un flux d'appareil est initié lorsque le service demande à se connecter à un appareil en fournissant son ID d'appareil. Ce workflow s’adapte particulièrement au modèle de communication client/serveur, dont SSH et RDP, quand un utilisateur envisage de se connecter à distance au serveur SSH ou RDP en cours d’exécution sur l’appareil à l’aide d’un programme client SSH ou RDP.

Le processus de création de flux d'appareils implique une négociation entre l’appareil, le service, ainsi que le point de terminaison principal et le point de terminaison de streaming du hub IoT. Le point de terminaison principal du hub IoT orchestre la création d’un flux d'appareil et le point de terminaison de streaming gère le trafic entre le service et l'appareil.

### <a name="device-stream-creation-flow"></a>Flux de création de flux d'appareils
La création programmatique d'un flux d'appareil à l’aide du SDK implique les étapes suivantes, également représentées dans la figure ci-dessous :

![Texte de remplacement](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png "Processus de liaison de flux d'appareils")


1. L’application de l'appareil enregistre à l'avance un rappel pour être informé chaque fois qu'un nouveau flux d'appareil est initié vers l'appareil. Cette étape intervient généralement lorsque l'appareil démarre et se connecte à IoT Hub.

2. Si besoin, le programme côté service initie un flux d'appareil en fournissant l'ID d'appareil (_pas_ l’adresse IP).

3. IoT Hub informe le programme côté client en appelant le rappel enregistré à l’étape 1. L’appareil peut accepter ou rejeter la requête d’initiation de flux. Cette logique peut être spécifique à votre scénario d’application. Si la requête de flux est rejetée par l’appareil, IoT Hub en informe le service. Si elle est acceptée, les étapes ci-dessous s'ensuivent.

4. L’appareil crée une connexion TCP sortante sécurisée vers le point de terminaison de streaming sur le port 443, et met à niveau la connexion vers un WebSocket. L’URL du point de terminaison de streaming ainsi que les informations d’identification à utiliser à des fins d'authentification sont fournies à l'appareil par IoT Hub dans le cadre de la requête envoyée à l’étape 3.

5. Si l’appareil accepte le flux, le service en est informé et crée son propre client WebSocket vers le point de terminaison de streaming. De même, il reçoit les informations d’authentification et l'URL du point de terminaison de streaming de IoT Hub.

Dans le processus de liaison ci-dessus :
- Le processus de liaison doit se terminer sous 60 secondes (étapes 2 à 5), à défaut de quoi la liaison échoue et le service en est informé.

- Une fois le flux de création ci-dessus terminé, le point de terminaison de streaming fait office de proxy et transfert le trafic entre le service et l’appareil via leurs WebSockets respectifs.

- L'appareil et le service doivent disposer d'une connectivité sortante vers le point de terminaison principal IoT Hub, ainsi que vers le point de terminaison de streaming sur le port 443. L’URL de ces points de terminaison est disponible sous l’onglet *Vue d’ensemble* du portail IoT Hub.

- Un flux établi offre des garanties de fiabilité à égalité avec TCP.

- Toutes les connexions vers IoT Hub et le point de terminaison de streaming utilisent TLS et sont chiffrées.

### <a name="termination-flow"></a>Flux de fin
Un flux établi prend fin lorsqu'une des connexions TCP vers la passerelle est déconnectée (par le service ou l'appareil). Cela peut intervenir de manière volontaire, en fermant le WebSocket sur les programmes de l'appareil ou du service, ou de manière involontaire, en cas d'expiration du délai de connectivité réseau ou d'échec du processus. Lorsqu'une connexion de l'appareil ou du service vers le point de terminaison de streaming prend fin, l'autre connexion TCP est également interrompue (de manière forcée), et l'appareil et le service doivent recréer le flux, si besoin.


## <a name="connectivity-requirements"></a>Exigences de connectivité

Les côtés appareil et service d'un flux d'appareil doivent tous deux pouvoir établir des connexions TLS à IoT Hub et son point de terminaison de streaming. Cela nécessite une connectivité sortante sur le port 443 vers ces points de terminaison. Le nom d'hôte associé à ces points de terminaison est disponible sous l'onglet *Vue d’ensemble* de IoT Hub, comme illustré dans la figure ci-dessous : ![Texte de remplacement](./media/iot-hub-device-streams-overview/device-stream-portal.png "Points de terminaison des flux d'appareils")

Les informations relatives aux points de terminaison peuvent également être obtenues à l'aide d'Azure CLI, sous la section portant sur les propriétés du hub, et plus particulièrement, les touches `property.hostname` et `property.deviceStreams`.

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

La sortie est un objet JSON de tous les points de terminaison auxquels l’appareil et le service de votre concentrateur doivent se connecter afin d’établir un flux d’appareils.

```json
{
  "streamingEndpoints": [
    "https://<YourIoTHubName>.<region-stamp>.streams.azure-devices.net"
  ]
}
```

> [!NOTE]
> Assurez-vous d’avoir installé Azure CLI version 2.0.57 ou ultérieure. Vous pouvez télécharger la dernière version [ici](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
> 

## <a name="whitelist-device-streaming-endpoints"></a>Mettre sur liste verte les points de terminaison de streaming des appareils

Comme indiqué [précédemment](#overview), votre appareil crée une connexion sortante vers le point de terminaison IoT Hub lors du processus d'initiation des flux d'appareils. Les pare-feu de l’appareil ou de son réseau doivent autoriser les connexions sortantes vers la passerelle de streaming sur le port 443 (notez que cette communication s’effectue via une connexion WebSocket chiffrée à l’aide de TLS).

Le nom d'hôte du point de terminaison de streaming de l'appareil se trouve sous l'onglet Vue d'ensemble du portail Azure IoT Hub. ![Texte de remplacement](./media/iot-hub-device-streams-overview/device-stream-portal.PNG "Points de terminaison des flux d'appareils")

Ces informations sont également disponibles en utilisant Azure CLI :

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

> [!NOTE]
> Assurez-vous d’avoir installé Azure CLI version 2.0.57 ou ultérieure. Vous pouvez télécharger la dernière version [ici](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
> 

## <a name="troubleshoot-via-device-streams-activity-logs"></a>Détecter un problème via les journaux d’activité des flux d’appareils

Vous pouvez configurer les journaux d’activité Azure Monitor de manière à collecter le journal d’activité des flux d’appareils dans votre hub IoT. Ce journal peut s'avérer particulièrement utile à des fins de résolution des problèmes.

Pour configurer les journaux d’activité Azure Monitor pour les activités de flux d’appareils de votre hub IoT, procédez comme suit :

1. Accédez à l'onglet *Paramètres de diagnostic* dans votre hub IoT, puis cliquez sur le lien *Activer les diagnostics*.

   ![Texte de remplacement](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings.PNG "Activation des journaux de diagnostic")


2. Entrez un nom pour vos paramètres de diagnostic, puis sélectionnez l'option *Envoyer à Log Analytics*. Vous serez invité à sélectionner une ressource d’espace de travail Log Analytics existante ou à en créer une. Dans la liste, cochez également *DeviceStreams*.

    ![Texte de remplacement](./media/iot-hub-device-streams-overview/device-streams-diagnostics.PNG "Activer les journaux d’activité des flux d’appareils")

3. Vous pouvez maintenant accéder à vos journaux de flux d’appareils sous l’onglet *Journaux d’activité* de votre portail IoT Hub. Les journaux d’activité des flux d’appareils apparaissent dans la table `AzureDiagnostics` et ont `Category=DeviceStreams`.

    <p>
Comme indiqué ci-dessous, l’identité de l’appareil cible ainsi que le résultat de l’opération sont également disponibles dans les journaux d’activité.
    ![Texte de remplacement](./media/iot-hub-device-streams-overview/device-streams-log-analytics.PNG "Accéder aux journaux d’activité des flux d’appareils")


## <a name="regional-availability"></a>Disponibilité régionale

Pendant la phase de préversion publique, les flux d’appareils IoT Hub sont disponibles dans les régions EUAP USA Centre et USA Centre. Veillez à créer votre hub dans l’une de ces régions. 


## <a name="sdk-availability"></a>Disponibilité du kit de développement logiciel (SDK)

Deux côtés de chaque flux (côté appareil et côté service) utilisent le SDK IoT Hub pour établir le tunnel. Dans le cadre de la préversion publique, les clients peuvent sélectionner les langages de SDK suivants :
- Les SDK C et C# prennent en charge les flux d'appareils côté appareil.

- Les SDK NodeJS et C# prennent en charge les flux d'appareils côté service.


## <a name="iot-hub-device-stream-samples"></a>Exemples de flux d'appareils IoT Hub

Nous avons publié deux [exemples de démarrage rapide](/azure/iot-hub) illustrant l’utilisation des flux d’appareils par les applications.
* L’exemple *écho* illustre l’utilisation programmatique des flux d’appareils (en appelant directement les API du SDK).
* L’exemple *proxy local* illustre le tunneling du trafic d’application client/serveur du commerce (par exemple, SSH, RDP ou web) via les flux d’appareils.

Ces exemples sont traités plus en détail ci-dessous.

### <a name="echo-sample"></a>Exemple écho
L’exemple écho illustre l’utilisation programmatique des flux d’appareils pour envoyer et recevoir des octets entre les applications de service et d’appareil. Utilisez les liens ci-dessous pour accéder aux guides de démarrage rapide. Notez que vous pouvez utiliser des programmes de service et d’appareil dans différents langages, par exemple un programme d’appareil C peut fonctionner avec un programme de service C#.

| Kit SDK    | Programme de service                                          | Programme d'appareil                                           |
|--------|----------------------------------------------------------|----------------------------------------------------------|
| C#     | [Lien](quickstart-device-streams-echo-csharp.md) | [Lien](quickstart-device-streams-echo-csharp.md) |
| Node.js | [Lien](quickstart-device-streams-echo-nodejs.md) | -                                                        |
| C      | -                                                        | [Lien](quickstart-device-streams-echo-c.md)      |

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>Exemple de proxy local (pour SSH ou RDP)
L’exemple de proxy local illustre un moyen d'activer le tunneling d'un trafic d'application existante impliquant une communication entre un client et un programme de serveur. Cette configuration fonctionne pour les protocoles client/serveur tels que SSH et RDP, où le côté service agit en tant que client (exécutant les programmes clients SSH ou RDP), et le côté appareil en tant que serveur (exécutant le démon SSH ou les programmes de serveur RDP). 

Cette section décrit l’utilisation des flux d’appareils pour permettre à l’utilisateur d’exécuter SSH sur un appareil via des flux d’appareils (le cas de RDP ou d’une autre application client/serveur est similaire en utilisant le port correspondant du protocole).

La configuration s’appuie sur deux programmes de *proxy local* illustrés dans la figure ci-dessous, à savoir *proxy local appareil* et *proxy local service*. Les programmes de proxy local sont chargés d’effectuer la [liaison d’initiation des flux d’appareils](#device-stream-creation-flow) avec IoT Hub ainsi que d’interagir avec le client SSH et le démon SSH à l’aide de sockets client/serveur classiques.

![Texte de remplacement](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png "Configuration de proxy de flux d'appareil pour SSH/RDP")

1. L’utilisateur exécute un proxy local de service pour initier un flux vers l'appareil.

2. Le proxy local d’appareil accepte la demande d’initiation de flux et le tunnel est établi au point de terminaison de streaming IoT Hub (comme indiqué ci-dessus).

3. Le proxy local d'appareil se connecte au point de terminaison du démon SSH qui écoute sur le port 22 de l'appareil.

4. Le proxy local de service écoute sur un port désigné en attendant les nouvelles connexions SSH de l’utilisateur (le port 2222 est utilisé dans l’exemple, mais tout autre port disponible peut être configuré). L’utilisateur pointe le client SSH vers le port du proxy local de service sur l'hôte local.

### <a name="notes"></a>Notes
- Les étapes ci-dessus permettent d'établir un tunnel de bout en bout entre le client SSH (à droite) et le démon SSH (à gauche). Une partie de cette connectivité de bout en bout implique l’envoi de trafic sur un flux d’appareil à IoT Hub.

- Les flèches de la figure indiquent le sens dans lequel les connexions sont établies entre les points de terminaison. Plus précisément, notez qu’il n’existe aucune connexion entrante vers l’appareil (elles sont souvent bloquées par un pare-feu).

- L’utilisation du port 2222 sur le proxy local de service est un choix arbitraire. Le proxy peut être configuré pour utiliser tout autre port disponible.

- Le choix du port 22 dépend du protocole et est spécifique à SSH dans le cas présent. Pour RDP, le port 3389 doit être utilisé. Cela peut être configuré dans les exemples de programmes fournis.

Utilisez les liens ci-dessous pour obtenir des instructions sur l'exécution de programmes de proxy local dans le langage de votre choix. Comme pour l’[exemple écho](#echo-sample), vous pouvez exécuter des programmes de proxy local d’appareil et de service dans différents langages, car ils sont pleinement interopérables.

| Kit SDK    | Proxy local de service                                       | Proxy local d'appareil                                |
|--------|-----------------------------------------------------------|---------------------------------------------------|
| C#     | [Lien](quickstart-device-streams-proxy-csharp.md)  | [Lien](quickstart-device-streams-proxy-csharp.md) |
| NodeJS | [Lien](quickstart-device-streams-proxy-nodejs.md)         | -                                                 |
| C      | -                                                         | [Lien](quickstart-device-streams-proxy-c.md)      |

## <a name="next-steps"></a>Étapes suivantes

Utilisez les liens ci-dessous pour en savoir plus sur les flux d’appareils :

> [!div class="nextstepaction"]
> [Émission Flux d'appareils sur IoT (Channel 9)](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)
> [Essayer un guide de démarrage rapide de flux d'appareils](/azure/iot-hub)
