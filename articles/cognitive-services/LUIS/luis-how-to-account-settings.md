---
title: Gérer le compte et les clés
titleSuffix: Language Understanding - Azure Cognitive Services
description: Les deux informations clés d’un compte LUIS sont le compte d’utilisateur et la clé de création. Vos informations de connexion sont gérées sur account.microsoft.com. Votre clé de création est gérée sur la page Paramètres du portail LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: d5a1d7ee3b8b16631f7b919f3aece0848d662e62
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65523516"
---
# <a name="manage-account-and-authoring-key"></a>Gérer le compte et la clé de création

Les deux informations clés d’un compte LUIS sont le compte d’utilisateur et la clé de création. Vos informations de connexion sont gérées sur [account.microsoft.com](https://account.microsoft.com). Votre clé de création est gérée sur la page **Paramètres** du portail [LUIS](luis-reference-regions.md).

## <a name="authoring-key"></a>Clé de création

Sur la page **Paramètres**, cette unique clé de création régionale vous permet d’éditer toutes vos applications depuis le portail [LUIS](luis-reference-regions.md), ainsi que vos [API de création](https://go.microsoft.com/fwlink/?linkid=2092087). Chaque mois, pour des raisons pratiques, la clé de création est autorisée à effectuer un nombre [limité](luis-boundaries.md) de requêtes de point de terminaison.

[Page Paramètres LUIS![](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

La clé de création peut être utilisée pour chacune de vos applications ainsi que pour toutes les applications que vous avez listées comme collaborateur.

## <a name="authoring-key-regions"></a>Régions de clé de création

La clé de création est spécifique à la [région de création](luis-reference-regions.md#publishing-regions). La clé ne fonctionne pas dans une autre région.

## <a name="reset-authoring-key"></a>Réinitialiser la clé de création

Si votre clé de création est compromise, réinitialisez la clé. La clé est réinitialisée sur toutes vos applications depuis le portail [LUIS](luis-reference-regions.md). Si vous éditez vos applications par le biais des API de création, vous devez changer la valeur de `Ocp-Apim-Subscription-Key` pour la nouvelle clé.

## <a name="delete-account"></a>Supprimer le compte

Consultez la page [Stockage et suppression de données](luis-concept-data-storage.md#accounts) pour savoir quelles données sont supprimées lorsque vous supprimez votre compte.

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur votre [clé de création](luis-concept-keys.md#authoring-key).

