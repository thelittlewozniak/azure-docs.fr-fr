---
title: Activer automatiquement les paramètres de diagnostic à l’aide d’un modèle Resource Manager
description: Découvrez comment utiliser un modèle Resource Manager pour créer des paramètres de diagnostic qui activeront la diffusion en continu de vos journaux de diagnostic vers Event Hubs ou leur stockage dans un compte de stockage.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/26/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 7edce5175a1dda66abf3316cb8f0eb33e9f64ef7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60236032"
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Activer automatiquement les paramètres de diagnostic lors de la création de ressources à l’aide d’un modèle Resource Manager
Dans cet article, nous vous expliquons comment utiliser un [modèle Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md) pour configurer les paramètres de diagnostic d’une ressource lors de sa création. Cela vous permet de démarrer automatiquement la diffusion en continu de vos journaux de diagnostic et des mesures vers Event Hubs, leur archivage dans un compte de stockage ou leur envoi à un espace de travail Log Analytics lorsqu’une ressource est créée.

> [!WARNING]
> À compter du 1er novembre 2018, le format des données de journal dans le compte de stockage deviendra JSON Lines. [Consultez cet article pour en savoir plus sur les conséquences liées à ce changement et pour découvrir comment mettre à jour vos outils pour qu’ils gèrent ce nouveau format.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

La méthode d’activation des journaux de diagnostic à l’aide d’un modèle Resource Manager varie selon le type de ressource.

* Les ressources **Non-Compute** (comme Network Security Groups, Logic Apps, Automation) utilisent les [paramètres de diagnostic décrits dans cet article](../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings).
* **de calcul** (basées sur WAD/LAD) utilisent le [fichier de configuration de WAD/LAD décrit dans cet article](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

Dans cet article, nous expliquons comment configurer les diagnostics à l’aide de deux méthodes.

Procédure de base :

1. Créez un modèle sous la forme d’un fichier JSON qui décrit comment créer la ressource et activer les diagnostics.
2. [Déployez le modèle à l’aide de n’importe quelle méthode de déploiement](../../azure-resource-manager/resource-group-template-deploy.md).

Ci-dessous, nous vous donnons un exemple de modèle de fichier JSON que vous devez générer pour les ressources de calcul et non liées au calcul.

## <a name="non-compute-resource-template"></a>Modèle de ressource non liée au calcul
Pour les ressources non liées au calcul, vous devrez effectuer les deux opérations suivantes :

1. Ajouter des paramètres à l’objet blob de paramètres concernant le nom du compte de stockage, l’ID de règle d’autorisation Event Hubs et/ou l’ID d’espace de travail Log Analytics (activation de l’archivage des journaux de diagnostic dans un compte de stockage, du streaming des journaux d’activité vers Event Hubs et/ou de l’envoi de journaux d’activité à Azure Monitor).
   
    ```json
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Azure Resource ID of the Log Analytics workspace for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Ajouter une ressource de type `[resource namespace]/providers/diagnosticSettings`dans le tableau de ressources de la ressource pour laquelle vous souhaitez activer les journaux de diagnostic.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2017-05-01-preview",
        "properties": {
          "name": "[parameters('settingName')]",
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
          "eventHubName": "[parameters('eventHubName')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "category": "AllMetrics",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

L’objet blob de propriétés pour le paramètre de diagnostic suit [le format décrit dans cet article](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate). Ajouter la propriété `metrics` vous permettra d’envoyer également des mesures sur la ressource à ces mêmes sorties, à condition que [la ressource prenne en charge les mesures Azure Monitor](../../azure-monitor/platform/metrics-supported.md).

Voici un exemple complet qui crée une application logique et active la diffusion en continu vers Event Hubs et le stockage dans un compte de stockage.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "https://azure.microsoft.com/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting. Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2017-05-01-preview",
          "properties": {
            "name": "[parameters('settingName')]",
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
            "eventHubName": "[parameters('eventHubName')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Modèle de ressource de calcul
Pour activer les diagnostics pour une ressource de calcul, par exemple un cluster Service Fabric ou une machine virtuelle, vous devez :

1. Ajouter l’extension Diagnostics Azure à la définition de ressource de machine virtuelle.
2. Spécifier un compte de stockage et/ou un Event Hub comme paramètre.
3. Ajouter le contenu de votre fichier WADCfg XML dans la propriété XMLCfg, en échappant correctement tous les caractères XML.

> [!WARNING]
> Cette dernière étape peut être difficile à réaliser. [Consultez cet article](../../virtual-machines/extensions/diagnostics-template.md#diagnostics-configuration-variables) pour obtenir un exemple qui montre comment fractionner le schéma de configuration de diagnostics en variables échappées et mises en forme correctement.
> 
> 

L’intégralité du processus, y compris des exemples, est décrite [dans ce document](../../virtual-machines/extensions/diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Étapes suivantes
* [En savoir plus sur les journaux de diagnostic Azure](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Stream Azure Diagnostic Logs to Event Hubs (Diffuser en continu les journaux de diagnostic Azure vers Event Hubs)](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)


