---
title: Personnaliser les règles de pare-feu d’applications web dans Azure Application Gateway - Azure CLI
description: Cet article fournit des informations sur la personnalisation des règles de pare-feu d’applications web dans Application Gateway avec Azure CLI.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: victorh
ms.openlocfilehash: 5e364c597b8c524e95297f279003462f2d16abe1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60832895"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-cli"></a>Personnaliser les règles de pare-feu d’applications web par le biais d’Azure CLI

Le pare-feu d’applications web (WAF) Azure Application Gateway fournit une protection pour les applications web. Ces protections sont fournies par le jeu de règles (Core Rule Set, CRS) de l’Open Web Application Security Project (OWASP). Certaines règles peuvent entraîner des faux positifs et bloquer le trafic réel. Par conséquent, Application Gateway permet de personnaliser des règles et des groupes de règles. Pour plus d’informations sur les règles et groupes de règles spécifiques, consultez la [List of web application firewall CRS Rule groups and rules](application-gateway-crs-rulegroups-rules.md) (Liste de règles et groupes de règles CRS de pare-feu d’applications web).

## <a name="view-rule-groups-and-rules"></a>Afficher les règles et groupes de règles

Voici des exemples de code montrant comment afficher les règles et les groupes de règles qui peuvent être configurés.

### <a name="view-rule-groups"></a>Afficher les groupes de règles

L’exemple suivant montre comment afficher les groupes de règles :

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

Voici un extrait de réponse issu de l’exemple précédent :

```json
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  },
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_2.2.9",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
   "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "crs_20_protocol_violations",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "2.2.9",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

### <a name="view-rules-in-a-rule-group"></a>Afficher les règles dans un groupe de règles

L’exemple suivant montre comment afficher les règles dans un groupe de règles spécifique :

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

Voici un extrait de réponse issu de l’exemple précédent :

```json
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": [
          {
            "description": "Rule 910011",
            "ruleId": 910011
          },
          ...
        ]
      }
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

## <a name="disable-rules"></a>Désactiver les règles

L’exemple suivant montre comment désactiver les règles `910018` et `910017` sur une passerelle d’application :

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="mandatory-rules"></a>Règles obligatoires

La liste suivante contient des conditions qui amènent le pare-feu WAF à bloquer la demande en mode de prévention (en mode de détection, elles sont consignées en tant qu’exceptions). Elles ne peuvent pas être configurées ni désactivées :

* L’échec d’analyse du corps de la demande entraîne le blocage de la demande, à moins que l’inspection du corps ne soit désactivée (XML, JSON, données de formulaire)
* La longueur des données du corps de la demande (sans fichier) est supérieure à la limite configurée
* Le corps de la demande (avec des fichiers) est supérieur à la limite
* Une erreur interne s’est produite dans le moteur WAF

Propre à CRS 3.x :

* Le score des anomalies entrantes a dépassé le seuil

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré vos règles désactivées, vous pouvez apprendre à afficher vos journaux d’activité WAF. Pour plus d’informations, consultez [Diagnostics Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
