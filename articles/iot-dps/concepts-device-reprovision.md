---
title: Concepts du reprovisionnement d’appareils pour le service Azure IoT Hub Device Provisioning | Microsoft Docs
description: Décrit les concepts du reprovisionnement d’appareils pour le service Azure IoT Hub Device Provisioning
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: fa8cb29f145c7658227f93d08a990c98563a0cfc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60730023"
---
# <a name="iot-hub-device-reprovisioning-concepts"></a>Concepts du reprovisionnement d’appareils IoT Hub

Durant le cycle de vie d’une solution IoT, il est fréquent d’avoir à déplacer des appareils d’un hub IoT à un autre. Voici quelques scénarios nécessitant le déplacement d’un appareil :

* **Géolocalisation / Géolatence** : Quand un appareil est déplacé entre différents emplacements, la latence réseau peut être améliorée s’il est migré vers un hub IoT plus proche.

* **Multilocation** : Un appareil peut être utilisé dans la même solution IoT, et être réaffecté à un nouveau client ou à un nouveau site client. Le service fourni à ce nouveau client peut utiliser un autre hub IoT.

* **Changement de solution** : Un appareil peut être déplacé dans une nouvelle solution IoT ou une solution IoT mise à jour. Cette réassignation peut nécessiter que l’appareil communique avec un nouveau hub IoT qui est connecté à d’autres composants back-end.

* **Mise en quarantaine** : Similaire à un changement de solution. Il est possible qu’un appareil défectueux, compromis ou obsolète soit réassigné à un hub IoT qui peut uniquement effectuer la mise à jour et la remise en conformité. Une fois que l’appareil fonctionne correctement, il est transféré à nouveau vers son hub principal.

La prise en charge du reprovisionnement dans le service Device Provisioning permet de gérer ces différents scénarios. Les appareils peuvent être automatiquement réassignés à de nouveaux hubs IoT selon la stratégie de reprovisionnement qui est configurée dans l’entrée d’inscription de l’appareil.

## <a name="device-state-data"></a>Données d’état de l’appareil

Les données d’état de l’appareil englobent les fonctionnalités de l’appareil et du [jumeau d’appareil](../iot-hub/iot-hub-devguide-device-twins.md). Ces données sont stockées dans l’instance du service Device Provisioning et dans le hub IoT auquel l’appareil est assigné.

![Provisionnement avec le service Device Provisioning](./media/concepts-device-reprovisioning/dps-provisioning.png)

Quand un appareil est initialement provisionné avec une instance du service Device Provisioning, les étapes suivantes sont effectuées :

1. L’appareil envoie une demande de provisionnement à l’instance du service Device Provisioning. L’instance du service authentifie l’identité de l’appareil par rapport à une entrée d’inscription et crée la configuration initiale des données d’état de l’appareil. L’instance du service assigne l’appareil à un hub IoT en fonction de la configuration de l’inscription, puis retourne cette assignation de hub IoT à l’appareil.

2. L’instance du service Device Provisioning transmet une copie des données d’état initiales de l’appareil au hub IoT assigné. L’appareil se connecte au hub IoT assigné et commence ses opérations.

Les données d’état de l’appareil sur le hub IoT peuvent être mises à jour au fur et à mesure par des [opérations sur l’appareil](../iot-hub/iot-hub-devguide-device-twins.md#device-operations) et des [opérations sur le backend](../iot-hub/iot-hub-devguide-device-twins.md#back-end-operations). Les données d’état initiales de l’appareil qui sont stockées dans l’instance du service Device Provisioning ne sont pas modifiées. Ces données correspondent à la configuration initiale.

![Provisionnement avec le service Device Provisioning](./media/concepts-device-reprovisioning/dps-provisioning-2.png)

Dans certains scénarios, quand un appareil est déplacé d’un hub IoT à un autre, il peut également être nécessaire de migrer l’état mis à jour de l’appareil de l’ancien hub IoT vers le nouveau hub IoT. Cette migration est gérée par les stratégies de reprovisionnement configurées dans le service Device Provisioning.

## <a name="reprovisioning-policies"></a>Stratégies de reprovisionnement

Selon le scénario, un appareil envoie généralement une demande à une instance du service Device Provisioning au redémarrage. Il prend aussi en charge une méthode pour déclencher manuellement le provisionnement à la demande. La stratégie de reprovisionnement sur une entrée d’inscription définit de quelle manière l’instance du service Device Provisioning gère ces demandes de provisionnement. La stratégie détermine également si les données d’état de l’appareil doivent être migrées au moment du reprovisionnement. Les mêmes stratégies sont disponibles pour les inscriptions individuelles et les groupes d’inscriptions :

* **Reprovisionner et migrer les données** : Il s’agit de la stratégie par défaut pour les nouvelles entrées d’inscription. Cette stratégie est appliquée quand des appareils associés à l’entrée d’inscription envoient une nouvelle demande (1). Selon la configuration de l’entrée d’inscription, l’appareil peut être réassigné à un autre hub IoT. Si l’appareil est déplacé vers un autre hub IoT, il est désinscrit du hub IoT initial. Les données d’état de l’appareil qui ont été mises à jour dans ce hub IoT initial sont alors migrées vers le nouveau hub IoT (2). Durant la migration, l’état de l’appareil indique **Assignation**.

    ![Provisionnement avec le service Device Provisioning](./media/concepts-device-reprovisioning/dps-reprovisioning-migrate.png)

* **Reprovisionner et rétablir la configuration initiale** : Cette stratégie est appliquée quand des appareils associés à l’entrée d’inscription envoient une nouvelle demande de provisionnement (1). Selon la configuration de l’entrée d’inscription, l’appareil peut être réassigné à un autre hub IoT. Si l’appareil est déplacé vers un autre hub IoT, il est désinscrit du hub IoT initial. Les données de configuration initiale que l’instance du service Device Provisioning a reçues au moment du provisionnement de l’appareil sont transmises au nouveau hub IoT (2). Durant la migration, l’état de l’appareil indique **Assignation**.

    Cette stratégie est souvent utilisée dans le cadre d’une réinitialisation aux paramètres d’usine sans changer les hubs IoT.

    ![Provisionnement avec le service Device Provisioning](./media/concepts-device-reprovisioning/dps-reprovisioning-reset.png)

* **Ne jamais reprovisionner** : L’appareil n’est jamais réaffecté à un autre hub. Cette stratégie est fournie pour gérer la compatibilité descendante.

### <a name="managing-backwards-compatibility"></a>Gestion de la compatibilité descendante

Avant septembre 2018, les assignations d’appareils à des hubs IoT avaient un comportement fixe. Quand un appareil suivait un processus de provisionnement, il était seulement réassigné au même hub IoT.

Pour les solutions qui dépendent de ce comportement, le service Device Provisioning gère la compatibilité descendante. Ce comportement est actuellement maintenu pour les appareils selon les critères suivants :

1. Les appareils se connectent avec une version d’API antérieure à la mise à disposition de la prise en charge native du reprovisionnement dans le service Device Provisioning. Reportez-vous au tableau des versions d’API ci-dessous.

2. Il n’existe pas de stratégie de reprovisionnement configurée dans l’entrée d’inscription des appareils.

Cette compatibilité garantit que les appareils précédemment déployés conservent le même comportement que celui qu’ils avaient lors des tests initiaux. Pour conserver le comportement initial, ne configurez pas de stratégie de reprovisionnement pour ces inscriptions. Si une stratégie de reprovisionnement est configurée, elle est prioritaire sur le comportement. Les clients peuvent alors mettre à jour le comportement de l’appareil sans avoir à réinitialiser ce dernier.

L’organigramme suivant aide à illustrer le processus du comportement :

![Organigramme de la compatibilité descendante](./media/concepts-device-reprovisioning/reprovisioning-compatibility-flow.png)

Le tableau suivant répertorie les versions d’API antérieures à la mise à disposition de la prise en charge native du reprovisionnement dans le service Device Provisioning :

| API REST | Kit de développement logiciel (SDK) C | Kit de développement logiciel (SDK) Python |  SDK Node | Kit de développement logiciel (SDK) Java | Kit de développement logiciel (SDK) .NET |
| -------- | ----- | ---------- | --------- | -------- | -------- |
| [2018-04-01 et versions antérieures](/rest/api/iot-dps/createorupdateindividualenrollment/createorupdateindividualenrollment#uri-parameters) | [1.2.8 et versions antérieures](https://github.com/Azure/azure-iot-sdk-c/blob/master/version.txt) | [1.4.2 et versions antérieures](https://github.com/Azure/azure-iot-sdk-python/blob/0a549f21f7f4fc24bc036c1d2d5614e9544a9667/device/iothub_client_python/src/iothub_client_python.cpp#L53) | [1.7.3 ou version antérieure](https://github.com/Azure/azure-iot-sdk-node/blob/074c1ac135aebb520d401b942acfad2d58fdc07f/common/core/package.json#L3) | [1.13.0 ou version antérieure](https://github.com/Azure/azure-iot-sdk-java/blob/794c128000358b8ed1c4cecfbf21734dd6824de9/device/iot-device-client/pom.xml#L7) | [1.1.0 ou version antérieure](https://github.com/Azure/azure-iot-sdk-csharp/blob/9f7269f4f61cff3536708cf3dc412a7316ed6236/provisioning/device/src/Microsoft.Azure.Devices.Provisioning.Client.csproj#L20)

> [!NOTE]
> Ces valeurs et ces liens sont susceptibles de changer. Il s’agit ici uniquement de tenter de déterminer là où les versions peuvent être définies par un client et les versions attendues.

## <a name="next-steps"></a>Étapes suivantes

* [Guide pratique pour reprovisionner des appareils](how-to-reprovision.md)
