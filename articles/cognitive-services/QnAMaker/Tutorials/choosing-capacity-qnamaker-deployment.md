---
title: Capacité de ressources pour un déploiement - QnA Maker
titleSuffix: Azure Cognitive Services
description: Avant de créer votre service QnA Maker, vous devez décider quel niveau des services ci-dessus vous convient.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 3126613a9dca00d221610dc46116dd409c65d53d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446627"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Choisir la capacité de votre déploiement QnA Maker

Le service QnA Maker prend une dépendance sur trois ressources Azure :
1.  App Service (pour l’exécution)
2.  Recherche Azure (pour le stockage des QnA)
3.  App Insights (facultatif, pour le stockage des journaux d’activité de conversation et les données de télémétrie)

Avant de créer votre service QnA Maker, vous devez décider quel niveau des services ci-dessus vous convient. 

En général, il existe trois paramètres dont vous devez tenir compte :

1. **Débit dont vous avez besoin à partir du service** : sélectionnez le [plan App Service](https://azure.microsoft.com/pricing/details/app-service/plans/) approprié en fonction de vos besoins. Vous pouvez faire [monter en puissance](https://docs.microsoft.com/azure/app-service/web-sites-scale) ou faire descendre en puissance l’application. Ceci doit également influencer votre sélection de la référence SKU de la Recherche Azure. Consultez plus d’informations [ici](https://docs.microsoft.com/azure/search/search-sku-tier).

1. **Taille et nombre de bases de connaissances** : Choisissez la [référence SKU Recherche Azure](https://azure.microsoft.com/pricing/details/search/) adaptée à votre scénario. Vous pouvez publier N-1 bases de connaissances dans un niveau spécifique, où N correspond au nombre maximal d’index autorisé dans le niveau. Vérifiez également la taille maximale et le nombre de documents autorisés par niveau.

    Par exemple, si votre niveau comporte 15 index autorisés, vous pouvez publier 14 bases de connaissances (1 index par base de connaissances publiée). Le quinzième index est utilisé pour toutes les bases de connaissances à des fins de création et de test. 

1. **Nombre de documents comme sources** : la référence SKU gratuite du service de gestion QnA Maker limite à 3 le nombre de documents (d’une taille de 1 Mo chacun) que vous pouvez gérer via le portail et les API. La référence SKU Standard n’a aucune limite en termes de nombre de documents que vous pouvez gérer. Vous trouverez plus de détails [ici](https://aka.ms/qnamaker-pricing).

Le tableau suivant vous donne des indications d’ordre général.

|                        | Administration de QnA Maker | App Service | Recherche Azure | Limites                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Expérimentation        | Référence SKU Gratuite             | Niveau Gratuit   | Niveau Gratuit    | Publier jusqu’à 2 Ko, taille de 50 Mo  |
| Environnement de test/développement   | Référence SKU standard         | Partagé      | De base        | Publier jusqu’à 14 Ko, taille de 2 Go    |
| Environnement de production | Référence SKU standard         | De base       | standard     | Publier jusqu’à 49 Ko, taille de 25 Go |

Pour mettre à niveau votre pile QnA Maker, consultez [Mettre à niveau votre service QnA Maker](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Mettre à niveau votre service QnA Maker](../How-To/upgrade-qnamaker-service.md)
