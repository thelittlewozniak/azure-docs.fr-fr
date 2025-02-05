---
title: Calcul haute performance - Machines virtuelles Azure | Microsoft Docs
description: En savoir plus sur le calcul haute performance dans Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: e8ff4147130dfeff14be41ed292b51ed34966df0
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537670"
---
# <a name="optimization-for-linux"></a>Optimisation pour Linux

Cet article démontre quelques techniques clés pour optimiser votre image de système d’exploitation. En savoir plus sur [l’activation d’InfiniBand](enable-infiniband.md) et l’optimisation des images de système d’exploitation.

## <a name="update-lis"></a>Mettre à jour LIS

Si vous déployez à l’aide d’une image personnalisée (par exemple, un système d’exploitation antérieur tel que CentOS/RHEL 7.4 ou 7.5), mettez à jour LIS sur la machine virtuelle.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Libérer de la mémoire

Améliorez l’efficacité en libérant automatiquement de la mémoire pour éviter tout accès à distance à la mémoire.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

Pour faire persister ceci après le redémarrage de la machine virtuelle :

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Désactiver le pare-feu et SELinux

Désactivez le pare-feu et SELinux.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Désactiver cpupower

Désactivez cpupower.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur [l’activation d’InfiniBand](enable-infiniband.md) et l’optimisation des images de système d’exploitation.

* En savoir plus sur [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) sur Azure.
