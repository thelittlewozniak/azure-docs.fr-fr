---
title: Définition de filtres dans Azure Media Services
description: Cette rubrique décrit comment créer des filtres pour que votre client puisse les utiliser pour diffuser des sections spécifiques d'un flux. Media Services crée des manifestes dynamiques pour obtenir cette diffusion sélective.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 05/23/2019
ms.author: juliako
ms.openlocfilehash: fdf29924da31db0347938df89e698cb258c2336b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66225407"
---
# <a name="filters"></a>Filtres

Quand vous distribuez votre contenu aux clients (diffusion en continu des événements ou vidéo à la demande) votre client peut-être davantage de flexibilité que celle décrite dans le fichier manifeste de la ressource par défaut. Azure Media Services propose [manifestes dynamiques](filters-dynamic-manifest-overview.md) selon les filtres prédéfinis. 

Les filtres sont des règles côté serveur qui permettent à vos clients d’effectuer des opérations comme : 

- Lire une seule section d'une vidéo (au lieu de la vidéo entière). Exemple :
  - Réduisez le manifeste pour afficher un sous-clip d'un événement en direct (« filtrage de sous-clip »), ou
  - Découpez le début d'une vidéo (« découpage d'une vidéo »).
- Distribuer uniquement les rendus spécifiés et/ou les pistes de langue spécifiées qui sont prises en charge par l’appareil utilisé pour lire le contenu (« filtrage de rendu »). 
- Ajustez la fenêtre de présentation (DVR) afin de fournir une longueur limitée de la fenêtre du DVR dans le lecteur (« ajustement de la fenêtre de présentation »).

Media Services vous permet de créer **compte des filtres** et **filtres d’éléments multimédias** pour votre contenu. En outre, vous pouvez associer vos filtres créés au préalable avec un **localisateur de diffusion en continu**.

## <a name="defining-filters"></a>Définition des filtres

Il existe deux types de filtres : 

* [Filtres de compte](https://docs.microsoft.com/rest/api/media/accountfilters) (globaux) : applicables à n'importe quel élément multimédia dans le compte Azure Media Services et dont la durée de vie correspond à celle du compte.
* [Filtres d’élément multimédia](https://docs.microsoft.com/rest/api/media/assetfilters) (locaux) : applicables uniquement à un élément multimédia avec lequel le filtre a été associé au moment de la création et dont la durée de vie correspond à celle de l'élément multimédia. 

**Compte des filtres** et **filtres d’éléments multimédias** types ont exactement les mêmes propriétés pour définir/qui décrit le filtre. Sauf lorsque vous créez le **filtre d’élément multimédia**, vous devez spécifier le nom de la ressource à laquelle vous souhaitez associer le filtre.

Selon votre scénario, vous choisissez le type de filtre le plus adapté (filtre d’élément multimédia ou de compte). Les filtres de compte conviennent aux profils d'appareil (filtrage de rendu) alors que les filtres d’élément multimédia peuvent être utilisés pour découper un élément multimédia spécifique.

Utilisez les propriétés suivantes pour décrire les filtres. 

|Nom|Description|
|---|---|
|firstQuality|Première qualité de vitesse de transmission du filtre.|
|presentationTimeRange|Plage horaire de présentation. Cette propriété sert à filtrer les points de début/fin du manifeste, la durée de la fenêtre de présentation, et la position de départ du flux en direct. <br/>Pour plus d’informations, consultez [PresentationTimeRange](#presentationtimerange).|
|tracks|Conditions de sélection des pistes. Pour plus d’informations, consultez [tracks](#tracks)|

### <a name="presentationtimerange"></a>presentationTimeRange

Utilisez cette propriété avec les **filtres d’élément multimédia**. Il n’est pas recommandé de définir la propriété avec des **filtres de compte**.

|Nom|Description|
|---|---|
|**endTimestamp**|Applicable à la vidéo à la demande (VoD).<br/>Pour la présentation de diffusion en continu, il est en mode silencieux ignoré et appliquée lorsque la fin de la présentation et le flux de données devient VoD.<br/>Il s’agit d’une valeur longue qui représente un point de terminaison absolu de la présentation, arrondi au début de GOP suivant le plus proche. L’unité étant l’échelle de temps, un endTimestamp de 1800000000 serait pendant 3 minutes.<br/>Utilisez startTimestamp et endTimestamp à découper les fragments qui figurera dans la liste de lecture (manifeste).<br/>Par exemple, startTimestamp = 40000000 et endTimestamp = 100000000 génère une sélection qui contient des fragments entre 4 secondes et 10 secondes de la présentation de la demande (VOD) à l’aide de l’échelle de temps par défaut. Si un fragment approche la limite, le fragment entier sera inclus dans le manifeste.|
|**forceEndTimestamp**|S’applique à la diffusion en continu en direct uniquement.<br/>Indique si la propriété endTimestamp doit être présente. Si la valeur est true, endTimestamp doit être spécifié ou un code de demande incorrecte est retourné.<br/>Valeurs autorisées : false, true.|
|**liveBackoffDuration**|S’applique à la diffusion en continu en direct uniquement.<br/> Cette valeur définit la dernière position dynamique qui peut rechercher un client.<br/>À l’aide de cette propriété, vous pouvez différer la position de lecture en direct et créer une mémoire tampon côté serveur pour les joueurs.<br/>L’unité de cette propriété est l’échelle de temps (voir ci-dessous).<br/>La valeur maximale en direct de durée d’interruption est de 300 secondes (3000000000).<br/>Par exemple, une valeur de 2000000000 signifie que le contenu le plus récent disponible est de 20 secondes différées à partir de la session réel en direct.|
|**presentationWindowDuration**|S’applique à la diffusion en continu en direct uniquement.<br/>Utilisez presentationWindowDuration pour appliquer une fenêtre glissante de fragments à inclure dans une sélection.<br/>L’unité de cette propriété est l’échelle de temps (voir ci-dessous).<br/>Par exemple, définissez presentationWindowDuration=1200000000 pour appliquer une fenêtre glissante de deux minutes. Les éléments multimédia situés à 2 minutes de la session en direct figureront dans la sélection. Si un fragment approche la limite, le fragment entier sera inclus dans la sélection. La durée minimale de la fenêtre de présentation est de 60 secondes.|
|**startTimestamp**|S’applique à la demande (VoD) ou la diffusion en continu de vidéo.<br/>Il s’agit d’une valeur longue qui représente un point de départ absolu du flux. Elle est arrondie au début GOP suivant le plus proche. L’unité étant l’échelle de temps, un startTimestamp de 150000000 serait pendant 15 secondes.<br/>Utilisez startTimestamp et endTimestampp à découper les fragments qui figurera dans la liste de lecture (manifeste).<br/>Par exemple, startTimestamp = 40000000 et endTimestamp = 100000000 génère une sélection qui contient des fragments entre 4 secondes et 10 secondes de la présentation de la demande (VOD) à l’aide de l’échelle de temps par défaut. Si un fragment approche la limite, le fragment entier sera inclus dans le manifeste.|
|**timescale**|S’applique à tous les horodatages et les durées dans un intervalle de temps de présentation, spécifié en tant que le nombre d’incréments d’une seconde.<br/>Valeur par défaut est 10000000 - dix millions incréments d’une seconde, où chaque incrément serait longue de 100 nanosecondes.<br/>Par exemple, si vous souhaitez définir un startTimestamp à 30 secondes, vous utiliseriez une valeur de 300000000 lors de l’utilisation de l’échelle de temps par défaut.|

### <a name="tracks"></a>Pistes

Vous spécifiez une liste de conditions de propriété de suivi de filtre (FilterTrackPropertyConditions) basé sur lequel les pistes de votre flux de données (diffusion en continu ou vidéo à la demande) doivent être inclus dans le manifeste créé dynamiquement. Les filtres sont combinés à l’aide d’une opération logique **AND** et **OR**.

Les conditions de propriétés de suivi de filtre décrivent les types de suivi, les valeurs (spécifiées dans le tableau suivant) et les opérations (Equal, NotEqual). 

|Nom|Description|
|---|---|
|**Bitrate**|Utilisez la vitesse de transmission de la piste pour le filtrage.<br/><br/>La valeur recommandée est une plage de vitesses de transmission, en bits par seconde. Par exemple, « 0-2427000 ».<br/><br/>Remarque : vous pouvez utiliser une valeur de vitesse de transmission spécifique, comme 250000 (bits par seconde), mais cette approche n'est pas recommandée car les vitesses de transmission exacts peuvent varier d’un élément multimédia à un autre.|
|**FourCC**|Utilisez la valeur FourCC de la piste pour le filtrage.<br/><br/>La valeur est le premier élément du format de codecs, tel que spécifié dans [RFC 6381](https://tools.ietf.org/html/rfc6381). À l’heure actuelle, les codecs suivants sont pris en charge : <br/>Pour la vidéo : « avc1 », « hev1 », « hvc1 »<br/>Pour l’audio : « mp4a », « ec-3 »<br/><br/>Pour déterminer les valeurs FourCC pour les pistes dans un élément multimédia, obtenir et examinez le fichier manifest.|
|**Langage**|Utilisez la langue de la piste pour le filtrage.<br/><br/>La valeur est la balise d’une langue que vous souhaitez inclure, comme indiqué dans RFC 5646. Exemple : « en ».|
|**Name**|Utilisez le nom de la piste pour le filtrage.|
|**Type**|Utilisez le type de la piste pour le filtrage.<br/><br/>Les valeurs suivantes sont autorisées : « video », « audio » ou « text ».|

### <a name="example"></a>Exemples

L’exemple suivant définit un filtre de diffusion en continu : 

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

## <a name="associating-filters-with-streaming-locator"></a>Associer des filtres de localisateur de diffusion en continu

Vous pouvez spécifier une liste de [compte d’actif ou filtres](filters-concept.md) sur votre [localisateur de diffusion en continu](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body). Le [l’empaquetage dynamique](dynamic-packaging-overview.md) s’applique à cette liste de filtres avec ceux de votre client spécifie l’URL. Cette combinaison génère une [manifeste dynamique](filters-dynamic-manifest-overview.md), qui est basé sur les filtres dans l’URL + filtres que vous spécifiez dans le localisateur de diffusion en continu. 

Regardez les exemples suivants :

* [Associer des filtres de localisateur de diffusion en continu - .NET](filters-dynamic-manifest-dotnet-howto.md#associate-filters-with-streaming-locator)
* [Associer des filtres de localisateur de diffusion en continu - CLI](filters-dynamic-manifest-cli-howto.md#associate-filters-with-streaming-locator)

## <a name="updating-filters"></a>La mise à jour de filtres
 
**Localisateurs de streaming** ne sont pas être mise à jour alors que les filtres peuvent être mis à jour. 

Il n’est pas recommandé pour mettre à jour la définition de filtres associés activement publiés **localisateur de diffusion en continu**, en particulier lorsque le CDN est activé. Diffusion en continu des serveurs et le CDN peut avoir des caches internes qui peuvent entraîner des données mises en cache obsolètes doit être retourné. 

Si la définition du filtre doit être modifiée envisagez de créer un filtre et en l’ajoutant à la **localisateur de diffusion en continu** URL ou la publication d’un nouveau **localisateur de diffusion en continu** qui fait directement référence au filtre.

## <a name="next-steps"></a>Étapes suivantes

Les articles suivants vous montrent comment créer des filtres par programmation.  

- [Créer des filtres avec les API REST](filters-dynamic-manifest-rest-howto.md)
- [Créer des filtres avec .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Créer des filtres avec l’interface CLI](filters-dynamic-manifest-cli-howto.md)

