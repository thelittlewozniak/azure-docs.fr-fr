---
title: Comprendre la prise en charge d’Azure IoT Hub AMQP | Microsoft Docs
description: Guide du développeur - prise en charge des appareils qui se connectent à IoT Hub des points de terminaison côté appareil et côté service en utilisant le protocole AMQP. Inclut des informations sur AMQP prise en charge dans les kits Azure IoT device SDK intégrée.
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: rezas
ms.openlocfilehash: d256faa42161e276e165f95c944b9f58ac4a8927
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297414"
---
# <a name="communicate-with-your-iot-hub-using-the-amqp-protocol"></a>Communiquer avec votre IoT hub à l’aide du protocole AMQP

IoT Hub prend en charge [AMQP version 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) de proposer un éventail de fonctionnalités via les points de terminaison côté appareil et côté service. Ce document décrit l’utilisation de clients AMQP pour se connecter à IoT Hub pour pouvoir utiliser la fonctionnalité IoT Hub.

## <a name="service-client"></a>Client de service

### <a name="connection-and-authenticating-to-iot-hub-service-client"></a>Connexion et l’authentification auprès d’IoT Hub (client de service)
Pour vous connecter à IoT Hub à l’aide d’AMQP, un client peut utiliser le [sécurité en fonction des revendications (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) ou [authentification Simple Authentication and Security Layer SASL ()](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

Les informations suivantes sont requises pour le client du service :

| Information | Valeur | 
|-------------|--------------|
| Nom d’hôte IoT Hub | `<iot-hub-name>.azure-devices.net` |
| Nom de la clé | `service` |
| Clé d’accès | Clé primaire ou secondaire associée au service |
| Signature d’accès partagé | SAP de courte durée de vie dans le format suivant : `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}` (le code pour générer cette signature peut être trouvé [ici](./iot-hub-devguide-security.md#security-token-structure)).


L’extrait de code ci-dessous utilise [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python) pour se connecter à IoT hub via un lien de l’expéditeur.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>' # e.g., '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoking-cloud-to-device-messages-service-client"></a>Appel de messages cloud-à-appareil (client de service)
L’échange de messages de cloud-à-appareil entre service et IoT Hub, ainsi qu’entre appareil et IoT Hub est décrite [ici](iot-hub-devguide-messages-c2d.md). Le client de service utilise deux liens décrites ci-dessous pour envoyer des messages et recevoir des commentaires pour les messages déjà envoyés à partir d’appareils.

| Créé par | Type de lien | Chemin d’accès du lien | Description |
|------------|-----------|-----------|-------------|
| de diffusion en continu | Lien de l’expéditeur | `/messages/devicebound` | Messages C2D destinés aux appareils sont envoyés à ce lien par le service. Les messages envoyés sur ce lien ont leur `To` propriété définie sur le chemin de lien du récepteur de l’appareil cible : par exemple, `/devices/<deviceID>/messages/devicebound`. |
| de diffusion en continu | Lien du récepteur | `/messages/serviceBound/feedback` | Saisie semi-automatique, de rejet et d’abandon des commentaires les messages provenant des appareils reçus sur ce lien par service. Pour plus d’informations sur les messages de commentaires, consultez [ici](./iot-hub-devguide-messages-c2d.md#message-feedback). |

L’extrait de code ci-dessous montre comment créer un message C2D et l’envoyer à un appareil à l’aide [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python).

```python
import uuid
# Create a message and set message property 'To' to the devicebound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message using the send client created and connected IoT Hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if not needed
send_client.close()
```

Pour recevoir des commentaires, client de service crée un lien du récepteur. L’extrait de code ci-dessous montre comment effectuer cette opération à l’aide de [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python).

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Recreate the URI using the feedback path above and authenticate
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
batch = receive_client.receive_message_batch(max_batch_size=10)
for msg in batch:
  print('received a message')
  # Check content_type in message property to identify feedback messages coming from device
  if msg.properties.content_type == 'application/vnd.microsoft.iothub.feedback.json':
    msg_body_raw = msg.get_data()
    msg_body_str = ''.join(msg_body_raw)
    msg_body = json.loads(msg_body_str)
    print(json.dumps(msg_body, indent=2))
    print('******************')
    for feedback in msg_body:
      print('feedback received')
      print('\tstatusCode: ' + str(feedback['statusCode']))
      print('\toriginalMessageId: ' + str(feedback['originalMessageId']))
      print('\tdeviceId: ' + str(feedback['deviceId']))
      print
  else:
    print('unknown message:', msg.properties.content_type)
```

Comme indiqué ci-dessus, un message de commentaire C2D a le type de contenu de `application/vnd.microsoft.iothub.feedback.json` et propriétés dans son corps JSON peuvent être utilisées pour déduire l’état de remise du message d’origine :
* Clé `statusCode` dans des commentaires corps a une des valeurs suivantes : `['Success', 'Expired', 'DeliveryCountExceeded', 'Rejected', 'Purged']`.
* Clé `deviceId` dans des commentaires corps a l’ID de l’appareil cible.
* Clé `originalMessageId` dans des commentaires corps a l’ID du message C2D d’origine envoyé par le service. Cela peut servir à mettre en corrélation les commentaires pour les messages C2D.

### <a name="receive-telemetry-messages-service-client"></a>Recevoir des messages de télémétrie (client de service)
Par défaut, IoT Hub stocke les messages de télémétrie d’appareil reçus dans un concentrateur d’événements intégré. Votre client de service peut utiliser le protocole AMQP pour recevoir les événements stockées.

Pour cela, le client du service doit d’abord se connecter au point de terminaison IoT Hub et recevoir une adresse de la redirection vers les Hubs d’événements intégrés. Client de service utilise ensuite l’adresse fournie pour vous connecter au concentrateur d’événements intégré.

Dans chaque étape, le client doit présenter les informations suivantes :
* Informations d’identification de service valide (jeton SAS du service).
* Un chemin d’accès correctement mise en forme pour la partition de groupe de consommateurs qu’elle souhaite extraire les messages de. Pour un ID de partition et de groupe de consommateurs donné, le chemin d’accès a le format suivant : `/messages/events/ConsumerGroups/<consumer_group>/Partitions/<partition_id>` (le groupe de consommateurs par défaut est `$Default`).
* Un prédicat de filtrage facultatif pour désigner un point de départ dans la partition (Cela peut être sous la forme d’un horodateur de nombre, offset ou en file d’attente de séquence).

L’extrait de code ci-dessous utilise [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python) de présenter les étapes ci-dessus.

```python
import json
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '/messages/events/ConsumerGroups/{consumer_group}/Partitions/{p_id}'.format(consumer_group='$Default', p_id=0)

username = '{policy_name}@sas.root.{iot_hub_name}'.format(policy_name=policy_name, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Optional filtering predicates can be specified using endpiont_filter
# Valid predicates include:
# - amqp.annotation.x-opt-sequence-number
# - amqp.annotation.x-opt-offset
# - amqp.annotation.x-opt-enqueued-time
# Set endpoint_filter variable to None if no filter is needed
endpoint_filter = b'amqp.annotation.x-opt-sequence-number > 2995'

# Helper function to set the filtering predicate on the source URI
def set_endpoint_filter(uri, endpoint_filter=''):
  source_uri = uamqp.address.Source(uri)
  source_uri.set_filter(endpoint_filter)
  return source_uri

receive_client = uamqp.ReceiveClient(set_endpoint_filter(uri, endpoint_filter), debug=True)
try:
  batch = receive_client.receive_message_batch(max_batch_size=5)
except uamqp.errors.LinkRedirect as redirect:
  # Once a redirect error is received, close the original client and recreate a new one to the re-directed address
  receive_client.close()

  sas_auth = uamqp.authentication.SASTokenAuth.from_shared_access_key(redirect.address, policy_name, access_key)
  receive_client = uamqp.ReceiveClient(set_endpoint_filter(redirect.address, endpoint_filter), auth=sas_auth, debug=True)

# Start receiving messages in batches
batch = receive_client.receive_message_batch(max_batch_size=5)
for msg in batch:
  print('*** received a message ***')
  print(''.join(msg.get_data()))
  print('\t: ' + str(msg.annotations['x-opt-sequence-number']))
  print('\t: ' + str(msg.annotations['x-opt-offset']))
  print('\t: ' + str(msg.annotations['x-opt-enqueued-time']))
```

Pour un ID d’appareil donné, IoT Hub utilise un hachage de l’ID d’appareil pour déterminer dans quelle partition pour stocker ses messages dans. L’extrait de code ci-dessus illustre la réception d’événements à partir d’une seule partition de ce type. Toutefois, notez que souvent une application classique doit récupérer des événements stockés dans toutes les partitions de hub d’événements.


## <a name="device-client"></a>Client de périphérique

### <a name="connection-and-authenticating-to-iot-hub-device-client"></a>Connexion et l’authentification auprès d’IoT Hub (client de périphérique)
Pour vous connecter à IoT Hub à l’aide d’AMQP, un appareil peut utiliser le [sécurité en fonction des revendications (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) ou [authentification Simple Authentication and Security Layer SASL ()](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

Les informations suivantes sont requises pour le client de périphérique :

| Information | Valeur | 
|-------------|--------------|
| Nom d’hôte IoT Hub | `<iot-hub-name>.azure-devices.net` |
| Clé d’accès | Clé primaire ou secondaire associée au périphérique |
| Signature d’accès partagé | SAP de courte durée de vie dans le format suivant : `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}` (le code pour générer cette signature peut être trouvé [ici](./iot-hub-devguide-security.md#security-token-structure)).


L’extrait de code ci-dessous utilise [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python) pour se connecter à IoT hub via un lien de l’expéditeur.

```python
import uamqp
import urllib
import uuid

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
device_id = '<device-id>'
access_key = '<primary-or-secondary-key>'
username = '{device_id}@sas.{iot_hub_name}'.format(device_id=device_id, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token('{hostname}/devices/{device_id}'.format(hostname=hostname, device_id=device_id), access_key, None)

operation = '<operation-link-name>' # e.g., '/devices/{device_id}/messages/devicebound'
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
send_client = uamqp.SendClient(uri, debug=True)
```

Les chemins de lien suivants sont pris en charge en tant qu’opérations de l’appareil :

| Créé par | Type de lien | Chemin d’accès du lien | Description |
|------------|-----------|-----------|-------------|
| Appareils | Lien du récepteur | `/devices/<deviceID>/messages/devicebound` | Messages C2D destinés aux appareils sont reçus sur ce lien par chaque périphérique de destination. |
| Appareils | Lien de l’expéditeur | `/devices/<deviceID>messages/events` | Les messages D2C envoyés à partir d’un appareil sont envoyées via ce lien. |
| Appareils | Lien de l’expéditeur | `/messages/serviceBound/feedback` | Commentaires de messages C2D envoyées au service sur ce lien par les appareils. |


### <a name="receive-c2d-commands-device-client"></a>Recevoir des commandes C2D (client de périphérique)
Commandes C2D envoyées aux appareils arrivent sur `/devices/<deviceID>/messages/devicebound` lien. Appareils peuvent recevoir ces messages par lots et utilisez la charge utile de données de message, les propriétés de message, annotations ou des propriétés de l’application dans le message en fonction des besoins.

L’extrait de code ci-dessous utilise [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python) pour recevoir les messages C2D par un appareil.

```python
# ... 
# Create a receive client for the C2D receive link on the device
operation = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
while True:
  batch = receive_client.receive_message_batch(max_batch_size=5)
  for msg in batch:
    print('*** received a message ***')
    print(''.join(msg.get_data()))

    # Property 'to' is set to: '/devices/device1/messages/devicebound',
    print('\tto:                     ' + str(msg.properties.to))

    # Property 'message_id' is set to value provided by the service
    print('\tmessage_id:             ' + str(msg.properties.message_id))

    # Other properties are present if they were provided by the service
    print('\tcreation_time:          ' + str(msg.properties.creation_time))
    print('\tcorrelation_id:         ' + str(msg.properties.correlation_id))
    print('\tcontent_type:           ' + str(msg.properties.content_type))
    print('\treply_to_group_id:      ' + str(msg.properties.reply_to_group_id))
    print('\tsubject:                ' + str(msg.properties.subject))
    print('\tuser_id:                ' + str(msg.properties.user_id))
    print('\tgroup_sequence:         ' + str(msg.properties.group_sequence))
    print('\tcontent_encoding:       ' + str(msg.properties.content_encoding))
    print('\treply_to:               ' + str(msg.properties.reply_to))
    print('\tabsolute_expiry_time:   ' + str(msg.properties.absolute_expiry_time))
    print('\tgroup_id:               ' + str(msg.properties.group_id))

    # Message sequence number in the built-in Event hub
    print('\tx-opt-sequence-number:  ' + str(msg.annotations['x-opt-sequence-number']))
```

### <a name="send-telemetry-messages-device-client"></a>Envoyer des messages de télémétrie (client de périphérique)
Données de télémétrie également être envoyés via AMQP à partir d’appareils. L’appareil peut éventuellement fournir un dictionnaire de propriétés de l’application ou divers messages propriétés telles qu’ID du message.

L’extrait de code ci-dessous utilise [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python) pour envoyer les messages D2C à partir d’un appareil.


```python
# ... 
# Create a send client for the D2C send link on the device
operation = '/devices/{device_id}/messages/events'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

send_client = uamqp.SendClient(uri, debug=True)

# Set any of the applicable message properties
msg_props = uamqp.message.MessageProperties()
msg_props.message_id = str(uuid.uuid4())
msg_props.creation_time = None
msg_props.correlation_id = None
msg_props.content_type = None
msg_props.reply_to_group_id = None
msg_props.subject = None
msg_props.user_id = None
msg_props.group_sequence = None
msg_props.to = None
msg_props.content_encoding = None
msg_props.reply_to = None
msg_props.absolute_expiry_time = None
msg_props.group_id = None

# Application properties in the message (if any)
application_properties = { "app_property_key": "app_property_value" }

# Create message
msg_data = b"Your message payload goes here"
message = uamqp.Message(msg_data, properties=msg_props, application_properties=application_properties)

send_client.queue_message(message)
results = send_client.send_all_messages()

for result in results:
    if result == uamqp.constants.MessageState.SendFailed:
        print result
```

## <a name="additional-notes"></a>Remarques supplémentaires
* Les connexions AMQP peuvent être interrompues en raison d’un réseau problème ou expiration du jeton d’authentification (généré dans le code). Le client du service doit gérer ces circonstances et rétablir la connexion et les liens si nécessaire. Dans le cas d’expiration du jeton d’authentification, le client peut renouveler également de manière proactive le jeton avant son expiration afin d’éviter une baisse de la connexion.
* Dans certains cas, votre client doit être en mesure de gérer correctement les redirections de liaison. Reportez-vous à la documentation de votre client AMQP sur la façon de gérer cette opération.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur le protocole AMQP, consultez le [spécification du protocole AMQP v1.0](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

Pour en savoir plus sur les messages IoT Hub, consultez :

* [Messages cloud-à-appareil](./iot-hub-devguide-messages-c2d.md)
* [Prise en charge des protocoles supplémentaires](iot-hub-protocol-gateway.md)
* [Prise en charge de protocole MQTT](./iot-hub-mqtt-support.md)
