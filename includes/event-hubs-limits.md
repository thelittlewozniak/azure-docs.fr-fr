---
title: Fichier Include
description: Fichier Include
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: fa6b4d6d0db09f8c4955430d6dc227356416d915
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735944"
---
Le tableau suivant liste les quotas et les limites propres à [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Pour plus d’informations sur la tarification des hubs d’événements, consultez la rubrique [Tarification des hubs d’événements](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limite | Étendue | Notes | Valeur |
| --- | --- | --- | --- |
| Nombre d’espaces de noms Event Hubs par abonnement |Abonnement |- |100 |
| Nombre d’Event Hubs par espace de noms |Espace de noms |Les demandes suivantes de création d’un Event Hub sont rejetées. |10 |
| Nombre de partitions par Event Hub |Entité |- |32 |
| Nombre de groupes de consommateurs par Event Hub |Entité |- |20 |
| Nombre de connexions AMQP par espace de noms |Espace de noms |Les demandes suivantes de connexions supplémentaires sont rejetées et le code appelant reçoit une exception. |5 000 |
| Taille maximale de l’événement Event Hubs|Entité |- |1 Mo |
| Taille maximale du nom d’un Event Hub |Entité |- |50 caractères |
| Nombre de récepteurs non epoch par groupe de consommateurs |Entité |- |5\. |
| Période de rétention maximale des données d’événement |Entité |- |1 à 7 jours |
| Unités de débit maximales |Espace de noms |Le dépassement de la limite d’unités de débit entraîne la limitation de vos données et la génération d’une exception de [serveur occupé](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Vous pouvez demander une plus grande quantité d’unités de débit pour le niveau Standard en remplissant une [demande de support](/azure/azure-supportability/how-to-create-azure-support-request). Les [unités de débit supplémentaires](../articles/event-hubs/event-hubs-auto-inflate.md) sont disponibles par blocs de 20 sur la base d’un engagement d’achat ferme. |20 |
| Nombre de règles d’autorisation par espace de noms |Espace de noms|Les demandes suivantes pour la création de règle d’autorisation sont rejetées.|12 |
| Nombre d’appels à la méthode GetRuntimeInformation | Entité | - | 50 par seconde | 

### <a name="event-hubs-dedicated---quotas-and-limits"></a>Quotas et limites de l’offre Event Hubs Dedicated
L’offre Event Hubs Dedicated est facturée à un tarif mensuel fixe, avec un minimum de 4 heures d’utilisation. Le niveau Dedicated offre toutes les fonctionnalités du plan Standard, mais avec la capacité de mise à l’échelle de classe entreprise et les limites pour les clients avec des charges de travail exigeantes. 

| Fonctionnalité | limites |
| --- | ---|
| Bande passante |  20 CUS |
| Espaces de noms | 50 par unité de capacité |
| Event Hubs |  1 000 par espace de noms |
| Événements d’entrée | Inclus |
| Taille des messages | 1 million d’octets |
| Partitions | 2 000 par unité de capacité |
| Groupes de consommateurs | Aucune limite par unité de capacité, 1 000 par hub d’événements |
| Connexions réparties | 100 K inclus |
| Rétention des messages | Jusqu’à 7 jours (une rétention de 90 jours sera bientôt disponible), 10 To inclus par unité de capacité |
| Capture | Inclus |
