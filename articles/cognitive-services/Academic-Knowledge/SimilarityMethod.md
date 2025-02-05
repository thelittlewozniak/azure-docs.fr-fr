---
title: Méthode Similarity - API Connaissances universitaires
titlesuffix: Azure Cognitive Services
description: Utilisez la méthode Similarity pour calculer la similarité universitaire entre deux chaînes.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 7f692c08f8af322bf7e6ab576e2e6f516594a6c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61336515"
---
# <a name="similarity-method"></a>Méthode Similarity

L’API REST **similarity** est utilisée pour calculer la similarité universitaire entre deux chaînes. 
<br>

**Point de terminaison REST :**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>Paramètres de la requête

Paramètre        |Type de données      |Obligatoire | Description
----------|----------|----------|------------
**s1**        |Chaîne   |OUI  |Chaîne* à comparer
**s2**        |Chaîne   |OUI  |Chaîne* à comparer

<sub> *La longueur maximale des chaînes à comparer est de 1 Mo. </sub>
<br>

## <a name="response"></a>response

Nom | Description
--------|---------
**SimilarityScore**        |Valeur à virgule flottante représentant la similarité cosinus de s1 et s2. Les valeurs proches de 1.0 indiquent une plus forte similarité et les valeurs proches de -1.0 indiquent une moindre similarité.

<br>

## <a name="successerror-conditions"></a>Conditions de réussite/d’erreur

État HTTP | Motif | response
-----------|----------|--------
**200**         |Succès | Nombre à virgule flottante
**400**         | Requête incorrecte ou non valide | Message d’erreur      
**500**         |Erreur interne du serveur | Message d’erreur
**Timed out**     | La requête a expiré.  | Message d’erreur

<br>

## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Exemple : Calcul de la similarité de deux abstracts partiels
#### <a name="request"></a>Demande :
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
Dans cet exemple, nous générons le score de similarité entre deux abstracts partiels à l’aide de l’API **similarity**.
#### <a name="response"></a>Réponse :
```
0.520
```
#### <a name="remarks"></a>Remarques :
Le score de similarité est déterminé en évaluant les concepts universitaires via l’incorporation de mots. Dans cet exemple, le score de 0.52 indique que les deux abstracts partiels ont quelques points de similitude.
<br>
