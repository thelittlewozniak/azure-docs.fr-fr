---
title: Bien démarrer avec les jumeaux d’appareils Azure IoT Hub (Java) | Microsoft Docs
description: Guide d’utilisation des jumeaux d’appareils Azure IoT Hub pour ajouter des balises, puis utiliser une requête IoT Hub. Vous utilisez Azure IoT device SDK pour Java afin d’implémenter l’application pour appareils et Azure IoT service SDK afin d’implémenter une application de service qui ajoute les balises et exécute la requête IoT Hub.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/04/2017
ms.openlocfilehash: bfb111b07db105190fc59f21b3255c2ea2b1471c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64574525"
---
# <a name="get-started-with-device-twins-java"></a>Bien démarrer avec les jumeaux d’appareils (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Dans ce tutoriel, vous allez créer deux applications de console Java :

* **add-tags-query**, application principale Java qui ajoute des balises et interroge les jumeaux d’appareils.
* **simulated-device**, application Java pour appareils qui se connecte à votre hub IoT et signale sa connectivité à l’aide d’une propriété signalée.

> [!NOTE]
> L’article relatif aux [Kits de développement logiciel (SDK) Azure IoT](iot-hub-devguide-sdks.md) fournit des informations sur les Kits de développement logiciel (SDK) Azure que l’on peut utiliser pour générer des applications pour périphérique et des applications principales.

Pour suivre ce didacticiel, vous avez besoin des éléments suivants :

* La version la plus récente de [Java SE Development Kit 8](https://aka.ms/azure-jdks)

* [Maven 3](https://maven.apache.org/install.html)

* Un compte Azure actif. (Si vous ne possédez pas de compte, vous pouvez créer un [compte gratuit](https://azure.microsoft.com/pricing/free-trial/) en quelques minutes seulement.)

## <a name="create-an-iot-hub"></a>Créer un hub IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Récupérer la chaîne de connexion pour le hub IoT

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Inscrire un nouvel appareil dans le hub IoT

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-the-service-app"></a>Créer l’application de service

Dans cette section, vous créez une application Java qui ajoute des métadonnées d’emplacement sous forme de balise au jumeau d’appareil associé à **myDeviceId** dans IoT Hub. L’application interroge IoT Hub pour lister d’abord les appareils situés aux États-Unis, puis les appareils qui signalent une connexion réseau cellulaire.

1. Sur votre ordinateur de développement, créez un dossier vide nommé `iot-java-twin-getstarted`.

2. Dans le dossier `iot-java-twin-getstarted`, créez un projet Maven nommé **add-tags-query** en lançant la commande ci-dessous dans votre invite de commandes. Il s’agit d’une commande unique et longue :

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

3. Dans votre invite de commandes, accédez au dossier `add-tags-query`.

4. Dans un éditeur de texte, ouvrez le fichier `pom.xml` dans le dossier `add-tags-query` et ajoutez la dépendance suivante au nœud **dependencies**. Cette dépendance vous permet d’utiliser le package **iot-service-client** dans votre application pour communiquer avec votre IoT Hub :

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Vous pouvez rechercher la dernière version de **iot-service-client** avec la [recherche Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

5. Ajoutez le nœud **build** suivant après le nœud **dependencies**. Cette configuration ordonne à Maven d’utiliser Java 1.8 pour générer l’application :

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

6. Enregistrez et fermez le fichier `pom.xml`.

7. Ouvrez le fichier `add-tags-query\src\main\java\com\mycompany\app\App.java` dans un éditeur de texte.

8. Ajoutez les instructions **import** suivantes au fichier :

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

9. Ajoutez les variables de niveau classe suivantes à la classe **App** . Remplacez `{youriothubconnectionstring}` par la chaîne de connexion de votre hub IoT, que vous avez notée dans la section *Créer un hub IoT* :

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

10. Mettez à jour la signature de la méthode **main** pour qu’elle inclue la clause `throws` suivante :

    ```java
    public static void main( String[] args ) throws IOException
    ```

11. Ajoutez le code suivant à la méthode **main** pour créer les objets **DeviceTwin** et **DeviceTwinDevice**. L’objet **DeviceTwin** gère la communication avec votre hub IoT. L’objet **DeviceTwinDevice** représente le jumeau de l’appareil avec ses propriétés et ses balises :

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

12. Ajoutez le bloc `try/catch` suivant à la méthode **main** :

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

13. Pour mettre à jour les balises **region** et **plant** du jumeau de l’appareil, ajoutez le code suivant dans le bloc `try` :

    ```java
    // Get the device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from the existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create the tags and attach them to the DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update the device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve the device twin with the tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

14. Pour interroger les jumeaux d’appareils dans IoT Hub, ajoutez le code suivant au bloc `try` après le code que vous avez ajouté à l’étape précédente. Le code exécute deux requêtes. Chaque requête retourne un maximum de 100 appareils :

    ```java
    // Query the device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct the query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run the query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct the query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run the query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

15. Enregistrez et fermez le fichier `add-tags-query\src\main\java\com\mycompany\app\App.java`.

16. Générez l’application **add-tags-query** et corrigez les erreurs éventuelles. Dans votre invite de commandes, accédez au dossier `add-tags-query` et exécutez la commande suivante :

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Créer une application pour appareils

Dans cette section, vous allez créer une application console Java qui définit une valeur de propriété signalée à envoyer à IoT Hub.

1. Dans le dossier `iot-java-twin-getstarted`, créez un projet Maven nommé **simulated-device** en lançant la commande suivante dans votre invite de commandes. Il s’agit d’une commande unique et longue :

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Dans votre invite de commandes, accédez au dossier `simulated-device`.

3. Dans un éditeur de texte, ouvrez le fichier `pom.xml` dans le dossier `simulated-device` et ajoutez les dépendances suivantes au nœud **dependencies**. Cette dépendance vous permet d’utiliser le package **iot-device-client** dans votre application pour communiquer avec votre IoT Hub :

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.14.2</version>
    </dependency>
    ```

    > [!NOTE]
    > Vous pouvez rechercher la dernière version de **iot-device-client** avec la [recherche Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

4. Ajoutez le nœud **build** suivant après le nœud **dependencies**. Cette configuration ordonne à Maven d’utiliser Java 1.8 pour générer l’application :

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

5. Enregistrez et fermez le fichier `pom.xml`.

6. Ouvrez le fichier `simulated-device\src\main\java\com\mycompany\app\App.java` dans un éditeur de texte.

7. Ajoutez les instructions **import** suivantes au fichier :

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

8. Ajoutez les variables de niveau classe suivantes à la classe **App** . Remplacez `{youriothubname}` par le nom de votre IoT Hub, et `{yourdevicekey}` par la valeur de clé de l’appareil que vous avez générée dans la section *Créer une identité d’appareil* :

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Cet exemple d’application utilise la variable **protocol** lorsqu’il instancie un objet **DeviceClient**. 

1. Ajoutez la méthode suivante à la classe **App** pour imprimer les informations sur les mises à jour de représentations :

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
          System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
      }
    ```

9. Ajoutez le code suivant à la méthode **main** pour :
    * créer un client d’appareil afin de communiquer avec IoT Hub.
    * créer un objet **Device** afin de stocker les propriétés du jumeau d’appareil.

      ```java
      DeviceClient client = new DeviceClient(connString, protocol);

      // Create a Device object to store the device twin properties
      Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed to " + propertyValue);
      }
      };
      ```

10. Ajoutez le code suivant à la méthode **main** pour créer une propriété signalée **connectivityType** et l’envoyer à IoT Hub :

    ```java
    try {
      // Open the DeviceClient and start the device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it to your IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

11. Ajoutez le code suivant à la fin de la méthode **main**. Le fait d’attendre la touche **Entrée** laisse le temps à IoT Hub de signaler l’état des opérations du jumeau d’appareil :

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Modifiez la signature de la méthode **main** pour inclure les exceptions suivantes :

     ```java
     public static void main(String[] args) throws URISyntaxException, IOException
     ```

1. Enregistrez et fermez le fichier `simulated-device\src\main\java\com\mycompany\app\App.java`.

13. Générez l’application **simulated-device** et corrigez les erreurs. Dans votre invite de commandes, accédez au dossier `simulated-device` et exécutez la commande suivante :

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-apps"></a>Exécuter les applications

Vous êtes maintenant prêt à exécuter les applications de console.

1. Dans une invite de commande, exécutez la commande suivante dans le dossier `add-tags-query` pour exécuter l’application de service **add-tags-query** :

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Application de service Java IoT Hub pour mettre à jour la valeur des balises et exécuter les requêtes des appareils](./media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Vous pouvez constater que les balises **plant** et **region** sont ajoutées au jumeau d’appareil. La première requête retourne votre appareil, la seconde non.

2. Dans une invite de commandes, exécutez la commande suivante dans le dossier `simulated-device` pour ajouter la propriété signalée **connectivityType** au jumeau d’appareil :

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Le client d’appareil ajoute la propriété signalée **connectivityType**](./media/iot-hub-java-java-twin-getstarted/device-app-1.png)

3. Dans une invite de commandes, exécutez la commande suivante dans le dossier `add-tags-query` pour exécuter de nouveau l’application de service **add-tags-query** :

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Application de service Java IoT Hub pour mettre à jour la valeur des balises et exécuter les requêtes des appareils](./media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Maintenant que votre appareil a envoyé la propriété **connectivityType** à IoT Hub, la deuxième requête retourne votre appareil.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez configuré un nouveau IoT Hub dans le portail Azure, puis créé une identité d’appareil dans le registre des identités de l’IoT Hub. Vous avez ajouté des métadonnées d’appareils sous forme de balises à partir d’une application principale et écrit une application pour appareils afin d’envoyer des informations sur la connectivité des appareils dans le jumeau d’appareil. Vous avez également appris à interroger les informations du jumeau d’appareil avec le langage de requête IoT Hub de type SQL.

Utilisez les ressources suivantes :

* Envoyez des données de télémétrie à partir d’appareils en suivant le didacticiel [Bien démarrer avec IoT Hub](quickstart-send-telemetry-java.md).

* Contrôlez les appareils de façon interactive (par exemple pour mettre en marche un ventilateur à partir d’une application contrôlée par l’utilisateur) en suivant le didacticiel [Utiliser des méthodes directes](quickstart-control-device-java.md).