---
title: 'Démarrage rapide : Traduire du texte, Java - API de traduction de texte Translator Text'
titleSuffix: Azure Cognitive Services
description: Dans ce guide de démarrage rapide, vous allez apprendre à traduire une chaîne de texte de l’anglais vers l’italien et l’allemand à l’aide de Java et de l’API REST traduction de texte Translator Text.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: f8846e8dc6df8753562a36c05342f04307dd886e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445086"
---
# <a name="quickstart-use-the-translator-text-api-to-translate-a-string-using-java"></a>Démarrage rapide : Utiliser l’API de traduction de texte Translator Text et Java pour traduire une chaîne

Dans ce guide de démarrage rapide, vous allez apprendre à traduire une chaîne de texte de l’anglais vers l’italien et l’allemand à l’aide de Java et de l’API REST traduction de texte Translator Text.

Pour suivre ce démarrage rapide, vous devrez disposer d’un [compte Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) avec une ressource Traduction de texte Translator Text. Si vous n’avez pas de compte, vous pouvez utiliser la [version d’évaluation gratuite](https://azure.microsoft.com/try/cognitive-services/) pour obtenir une clé d’abonnement.

## <a name="prerequisites"></a>Prérequis

* [JDK 7 ou ultérieur](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)
* Une clé d’abonnement Azure pour Translator Text

## <a name="initialize-a-project-with-gradle"></a>Initialiser un projet avec Gradle

Commencez par créer un répertoire de travail pour ce projet. À partir de la ligne de commande (ou d’une session Terminal Server), exécutez cette commande :

```console
mkdir translator-sample
cd translator-sample
```

Ensuite, initialisez un projet Gradle. Cette commande crée les fichiers de build nécessaires pour Gradle, dont le plus important est le fichier `build.gradle.kts`, qui est utilisé au moment de l’exécution pour créer et configurer votre application. Exécutez cette commande à partir de votre répertoire de travail :

```console
gradle init --type basic
```

Quand vous êtes invité à choisir un **DSL**, sélectionnez **Kotlin**.

## <a name="configure-the-build-file"></a>Configurer le fichier de build

Recherchez le fichier `build.gradle.kts` et ouvrez-le dans votre IDE ou éditeur de texte habituel. Copiez-y ensuite cette configuration de build :

```
plugins {
    java
    application
}
application {
    mainClassName = "Translate"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Notez que cet exemple présente des dépendances sur OkHttp pour les requêtes HTTP, et sur Gson pour gérer et analyser JSON. Pour en savoir plus sur les configurations de build, consultez [Creating New Gradle Builds](https://guides.gradle.org/creating-new-gradle-builds/).

## <a name="create-a-java-file"></a>Créer un fichier Java

Créez maintenant un dossier pour votre exemple d’application. À partir de votre répertoire de travail, exécutez cette commande :

```console
mkdir -p src/main/java
```

Ensuite, dans ce dossier, créez un fichier nommé `Translate.java`.

## <a name="import-required-libraries"></a>Importer les bibliothèques nécessaires

Ouvrez `Translate.java` et ajoutez-y les instructions import suivantes :

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;
```

## <a name="define-variables"></a>Définir des variables

Vous devez tout d’abord créer une classe publique pour votre projet :

```java
public class Translate {
  // All project code goes here...
}
```

Ajoutez ces lignes à la classe `Translate`. Vous remarquerez qu’en plus d’`api-version`, deux paramètres supplémentaires ont été ajoutés à `url`. Ces paramètres servent à définir les sorties de traduction. Dans cet exemple, il s’agit de l’allemand (`de`) et de l’italien (`it`). N’oubliez pas de mettre à jour la valeur de clé d’abonnement.

```java
String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
String url = "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=de,it";
```

Si vous utilisez un abonnement multiservice Cognitive Services, vous devez également inclure la `Ocp-Apim-Subscription-Region` dans vos paramètres de requête. [En savoir plus sur l’authentification sur l’abonnement multiservice](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication). 

## <a name="create-a-client-and-build-a-request"></a>Créer un client et générer une requête

Ajoutez cette ligne à la classe `Translate` pour instancier `OkHttpClient` :

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Ensuite, générez la requête POST. Au besoin, changez le texte pour la traduction. Le texte doit être placé dans une séquence d’échappement.

```java
// This function performs a POST request.
public String Post() throws IOException {
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType,
            "[{\n\t\"Text\": \"Welcome to Microsoft Translator. Guess how many languages I speak!\"\n}]");
    Request request = new Request.Builder()
            .url(url).post(body)
            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey)
            .addHeader("Content-type", "application/json").build();
    Response response = client.newCall(request).execute();
    return response.body().string();
}
```

## <a name="create-a-function-to-parse-the-response"></a>Créer une fonction pour analyser la réponse

Cette fonction simple analyse et agrémente la réponse JSON retournée par le service de traduction de texte Translator Text.

```java
// This function prettifies the json response.
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonElement json = parser.parse(json_text);
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="put-it-all-together"></a>Assemblage

La dernière étape consiste à effectuer une requête et à obtenir une réponse. Ajoutez ces lignes à votre projet :

```java
public static void main(String[] args) {
    try {
        Translate translateRequest = new Translate();
        String response = translateRequest.Post();
        System.out.println(prettify(response));
    } catch (Exception e) {
        System.out.println(e);
    }
}
```

## <a name="run-the-sample-app"></a>Exécution de l'exemple d'application

Voilà, vous êtes prêt à exécuter votre exemple d’application. À partir de la ligne de commande (ou d’une session Terminal Server), accédez à la racine de votre répertoire de travail, puis exécutez cette commande :

```console
gradle build
```

Une fois la génération terminée, exécutez :

```console
gradle run
```

## <a name="sample-response"></a>Exemple de réponse

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Willkommen bei Microsoft Translator. Erraten Sie, wie viele Sprachen ich spreche!",
        "to": "de"
      },
      {
        "text": "Benvenuti a Microsoft Translator. Indovinate quante lingue parlo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Étapes suivantes

Explorez l’exemple de code pour ce démarrage rapide et d’autres, y compris la translittération et l’identification de la langue, ainsi que d’autres exemples de projets de l’API de traduction de texte Translator Text sur GitHub.

> [!div class="nextstepaction"]
> [Explorer des exemples Java sur GitHub](https://aka.ms/TranslatorGitHub?type=&language=java)

## <a name="see-also"></a>Voir aussi

* [Translittérer du texte](quickstart-java-transliterate.md)
* [Identifier la langue par entrée](quickstart-java-detect.md)
* [Obtenir des traductions alternatives](quickstart-java-dictionary.md)
* [Obtenir une liste des langues prises en charge](quickstart-java-languages.md)
* [Déterminer la longueur des phrases depuis une entrée](quickstart-java-sentences.md)
