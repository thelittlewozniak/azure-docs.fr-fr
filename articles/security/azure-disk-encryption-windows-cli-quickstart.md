---
title: Créer et chiffrer une machine virtuelle Windows avec Azure CLI
description: Dans ce guide de démarrage rapide, vous allez apprendre à utiliser Azure CLI pour créer et chiffrer une machine virtuelle Windows
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: 3cacb2e179cb7abe99d62d91d396a56fdeaf1ca2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071854"
---
# <a name="quickstart-create-and-encrypt-a-windows-vm-with-the-azure-cli"></a>Démarrage rapide : Créer et chiffrer une machine virtuelle Windows avec Azure CLI

L’interface de ligne de commande (CLI) Azure permet de créer et gérer des ressources Azure à partir de la ligne de commande ou dans les scripts. Ce démarrage rapide explique comment utiliser Azure CLI pour créer et chiffrer une machine virtuelle Windows Server 2016.

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous choisissez d’installer et d’utiliser l’interface de ligne de commande en local, ce guide de démarrage rapide nécessite que vous exécutiez la version 2.0.30 d’Azure CLI, ou une version ultérieure. Exécutez `az --version` pour trouver la version. Si vous devez installer ou mettre à niveau, voir [Installer Azure CLI]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Créer un groupe de ressources

Créez un groupe de ressources avec la commande [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create). Un groupe de ressources Azure est un conteneur logique dans lequel les ressources Azure sont déployées et gérées. L’exemple suivant crée un groupe de ressources nommé *myResourceGroup* à l’emplacement *eastus* :

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Création d'une machine virtuelle

Créez une machine virtuelle avec la commande [az vm create](/cli/azure/vm?view=azure-cli-latest#az-vm-create). L’exemple suivant permet de créer une machine virtuelle nommée *myVM*. Cet exemple utilise le nom d’utilisateur administratif *azureuser* et le mot de passe *myPassword12*. 

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword12
```

La création de la machine virtuelle et des ressources de support ne nécessite que quelques minutes. L’exemple de sortie suivant illustre la réussite de l’opération de création d’une machine virtuelle.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Créer un coffre de clés configuré pour les clés de chiffrement

Azure Disk Encryption stocke sa clé de chiffrement dans Azure Key Vault. Créez un coffre de clés avec la commande [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create). Pour activer le stockage des clés de chiffrement dans Key Vault, utilisez le paramètre --enabled-for-disk-encryption.
> [!Important]
> Chaque coffre de clés doit avoir un nom unique. L’exemple suivant crée un coffre de clés nommé *myKV*, mais vous devez nommer le vôtre différemment.

```azurecli
az keyvault create --name "myKV" --resource-group "myResourceGroup" --location eastus --enabled-for-disk-encryption
```

## <a name="encrypt-the-virtual-machine"></a>Chiffrer la machine virtuelle

Chiffrez votre machine virtuelle avec [az vm encryption](/cli/azure/vm/encryption?view=azure-cli-latest), en fournissant votre coffre de clés unique nom dans le paramètre --disk-encryption-keyvault.

```azurecli-interactive
az vm encryption enable -g MyResourceGroup --name MyVM --disk-encryption-keyvault myKV
```

Vous pouvez vérifier que le chiffrement est activé sur votre machine virtuelle avec [az vm show](/cli/azure/vm/encryption#az-vm-encryption-show)

```azurecli-interactive
az vm show --name MyVM -g MyResourceGroup
```

Vous verrez les éléments suivants dans la sortie retournée :

```azurecli-interactive
"EncryptionOperation": "EnableEncryption"
```

## <a name="clean-up-resources"></a>Supprimer des ressources

Lorsque vous n’en avez plus besoin, vous pouvez utiliser la commande [az group delete](/cli/azure/group) pour supprimer le groupe de ressources, la machine virtuelle et Key Vault. 

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez créé une machine virtuelle, créé un coffre de clés qui a été activé pour les clés de chiffrement, et vous avez chiffré la machine virtuelle.  Passez à l’article suivant pour en savoir plus sur la configuration requise d’Azure Disk Encryption pour les machines virtuelles IaaS.

> [!div class="nextstepaction"]
> [Prérequis pour Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)

