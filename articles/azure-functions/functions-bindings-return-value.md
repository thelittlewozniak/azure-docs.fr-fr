---
title: Utilisation d’une valeur de retour dans Azure Functions
description: Découvrez comment gérer les valeurs de retour pour Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 01/14/2019
ms.author: cshoe
ms.openlocfilehash: 03cf85ab12a8f64d639c09db5ea75002b258aa84
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480283"
---
# <a name="using-the-azure-function-return-value"></a>Utilisation de la valeur de retour Azure Functions

Cet article explique comment fonctionnent les valeurs de retour dans une fonction.

Dans les langages qui proposent une valeur de retour, vous pouvez lier une [liaison de sortie](./functions-triggers-bindings.md#binding-direction) de fonction à la valeur de retour :

* Dans une bibliothèque de classes C#, appliquez l’attribut de liaison de sortie à la valeur de retour de la méthode.
* Dans d’autres langages, définissez la propriété `name` dans *function.json* sur `$return`.

S’il existe plusieurs liaisons de sortie, utilisez la valeur de retour pour un seul d’entre eux.

En C# et dans les scripts C#, les autres méthodes pour envoyer des données à une liaison de sortie sont les paramètres `out` et les [objets collecteurs](functions-reference-csharp.md#writing-multiple-output-values).

Consultez l’exemple spécifique à un langage montrant l’utilisation de la valeur de retour :

* [C#](#c-example)
* [Script C# (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)
* [Python](#python-example)

## <a name="c-example"></a>Exemple en code C#

Voici du code C# qui utilise la valeur de retour pour une liaison de sortie, suivie d’un exemple asynchrone :

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

## <a name="c-script-example"></a>Exemple de script C#

Voici la liaison de sortie dans le fichier *function.json* :

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Voici le code d’un script C#, suivi d’un exemple asynchrone :

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

## <a name="f-example"></a>Exemple F#

Voici la liaison de sortie dans le fichier *function.json* :

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Voici le code F# :

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="javascript-example"></a>Exemple JavaScript

Voici la liaison de sortie dans le fichier *function.json* :

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Dans JavaScript, la valeur de retour apparaît dans le second paramètre pour `context.done` :

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

## <a name="python-example"></a>Exemple Python

Voici la liaison de sortie dans le fichier *function.json* :

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Voici le code Python :

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Gérer les erreurs liées aux liaisons Azure Functions](./functions-bindings-errors.md)
