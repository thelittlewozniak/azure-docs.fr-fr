---
title: Gérer le trafic web - Azure CLI
description: Découvrez comment créer une passerelle d’application avec un groupe de machines virtuelles identiques pour gérer le trafic web à l’aide d’Azure CLI.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 5/1/2019
ms.author: victorh
ms.openlocfilehash: d60c756fcf0b527731b8a1f31a8d93f108c91665
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65146225"
---
# <a name="manage-web-traffic-with-an-application-gateway-using-the-azure-cli"></a>Gérer le trafic web avec une passerelle d’application à l’aide d’Azure CLI

La passerelle d’application est utilisée pour gérer et sécuriser le trafic web vers les serveurs que vous gérez. Vous pouvez utiliser Azure CLI pour créer une [passerelle d’application](overview.md) qui utilise un [groupe de machines virtuelles identiques](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) pour les serveurs backend. Dans cet exemple, le groupe identique contient deux instances de machines virtuelles. Le groupe identique est ajouté au pool principal par défaut de la passerelle d’application.

Dans cet article, vous apprendrez comment :

> [!div class="checklist"]
> * Configurer le réseau
> * Créer une passerelle Application Gateway
> * Créer un groupe de machines virtuelles identiques avec le pool backend par défaut

Si vous préférez, vous pouvez suivre cette procédure en utilisant [Azure PowerShell](tutorial-manage-web-traffic-powershell.md).

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous choisissez d’installer et d’utiliser l’interface de ligne de commande en local, ce guide de démarrage rapide nécessite que vous exécutiez la version 2.0.4 minimum d’Azure CLI. Pour connaître la version de l’interface, exécutez `az --version`. Si vous devez installer ou mettre à niveau, voir [Installer Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Créer un groupe de ressources

Un groupe de ressources est un conteneur logique dans lequel les ressources Azure sont déployées et gérées. Créez un groupe de ressources à l’aide de la commande [az group create](/cli/azure/group#az-group-create).

L’exemple suivant crée un groupe de ressources nommé *myResourceGroupAG* à l’emplacement *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Créer des ressources réseau 

Créez le réseau virtuel nommé *myVNet* et le sous-réseau nommé *myAGSubnet* à l’aide de la commande [az network vnet create](/cli/azure/network/vnet). Vous pouvez ensuite ajouter le sous-réseau nommé *myBackendSubnet* nécessaire aux serveurs backend à l’aide de la commande [az network vnet subnet create](/cli/azure/network/vnet/subnet). Créez l’adresse IP publique nommée *myAGPublicIPAddress* à l’aide de la commande [az network public-ip create](/cli/azure/network/public-ip).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24

az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24

az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Créer une passerelle Application Gateway

Utilisez la commande [az network application-gateway create](/cli/azure/network/application-gateway) pour créer la passerelle d’application nommée *myAppGateway*. Quand vous créez une passerelle d’application avec Azure CLI, vous spécifiez des informations de configuration, telles que la capacité, la référence SKU et les paramètres HTTP. La passerelle d’application est assignée aux *myAGSubnet* et *myPublicIPAddress* que vous avez créés. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

 La création de la passerelle d’application peut prendre plusieurs minutes. Une fois la passerelle d’application créée, vous voyez ces nouvelles fonctionnalités :

- *appGatewayBackendPool* : une passerelle d’application doit avoir au moins un pool d’adresses backend.
- *appGatewayBackendHttpSettings* : spécifie que le port 80 et le protocole HTTP sont utilisés pour la communication.
- *appGatewayHttpListener* : écouteur par défaut associé à *appGatewayBackendPool*.
- *appGatewayFrontendIP* - assigne *myAGPublicIPAddress* à *appGatewayHttpListener*.
- *rule1* : règle de routage par défaut associée à *appGatewayHttpListener*.

## <a name="create-a-virtual-machine-scale-set"></a>Crée un groupe de machines virtuelles identiques

Dans cet exemple, vous créez un groupe de machines virtuelles identiques qui fournit des serveurs pour le pool backend dans la passerelle d’application. Les machines virtuelles dans le groupe identique sont associées à *myBackendSubnet* et *appGatewayBackendPool*. Pour créer le groupe identique, utilisez la commande [az vmss create](/cli/azure/vmss#az-vmss-create).

```azurecli-interactive
az vmss create \
  --name myvmss \
  --resource-group myResourceGroupAG \
  --image UbuntuLTS \
  --admin-username azureuser \
  --admin-password Azure123456! \
  --instance-count 2 \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --vm-sku Standard_DS2 \
  --upgrade-policy-mode Automatic \
  --app-gateway myAppGateway \
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>Installer NGINX

Vous pouvez maintenant installer NGINX sur le groupe de machines virtuelles identiques de façon à tester la connectivité HTTP vers le pool principal.

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
```

## <a name="test-the-application-gateway"></a>Tester la passerelle d’application

Pour obtenir l’adresse IP publique de la passerelle d’application, utilisez la commande [az network public-ip show](/cli/azure/network/public-ip). Copiez l’adresse IP publique, puis collez-la dans la barre d’adresses de votre navigateur.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Tester l’URL de base dans la passerelle d’application](./media/tutorial-manage-web-traffic-cli/tutorial-nginxtest.png)

## <a name="clean-up-resources"></a>Supprimer des ressources

Lorsque vous n’en avez plus besoin, supprimez le groupe de ressources, la passerelle d’application et toutes les ressources associées.

```azurecli-interactive
az group delete --name myResourceGroupAG --location eastus
```

## <a name="next-steps"></a>Étapes suivantes

[Limiter le trafic web avec un pare-feu d’applications web](./tutorial-restrict-web-traffic-cli.md)
