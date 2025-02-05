---
title: Validation des alertes dans Azure Security Center | Microsoft Docs
description: Ce document est conçu pour vous aider à valider des alertes de sécurité dans Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2018
ms.author: rkarlin
ms.openlocfilehash: 009f5fe7243b8ce597c2be9f9c6874cdb56d103c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60706047"
---
# <a name="alerts-validation-in-azure-security-center"></a>Validation des alertes dans Azure Security Center
Ce document est conçu pour vous apprendre à vérifier si votre système est correctement configuré pour les alertes dans Azure Security Center.

## <a name="what-are-security-alerts"></a>Que sont les alertes de sécurité ?
Security Center collecte, analyse et intègre automatiquement les données de journaux provenant de vos ressources Azure, du réseau et des solutions partenaires connectées, telles que les solutions de protection des points de terminaison et des pare-feux, pour détecter et pour vous avertir en cas de menace. Pour plus d’informations sur les alertes de sécurité, lisez [Gestion et résolution des alertes de sécurité dans Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts), puis [Présentation des alertes de sécurité dans Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) pour en savoir plus sur les différents types d’alertes.

## <a name="alert-validation"></a>Validation de l’alerte
Après avoir installé l’agent de Security Center sur votre ordinateur, suivez les étapes ci-dessous depuis l’ordinateur sur lequel vous voulez configurer l’alerte de la ressource attaquée :

1. Copiez un exécutable (calc.exe, par exemple) sur le bureau de votre ordinateur ou dans le répertoire de votre choix.
2. Renommez ce fichier **ASC_AlertTest_662jfi039N.exe**.
3. Ouvrez l’invite de commandes et exécutez ce fichier avec un argument (un faux nom d’argument), tel que : *ASC_AlertTest_662jfi039N.exe -foo*
4. Patientez 5 à 10 minutes puis ouvrez les alertes dans Security Center. Vous devriez voir une alerte similaire à :

    ![Validation de l’alerte](./media/security-center-alert-validation/security-center-alert-validation-fig2.png)

En examinant cette alerte, veillez à ce que la valeur du champ Arguments Auditing Enabled (Audit pour arguments activé) soit true. S’il affiche la valeur false, vous devez activer la ligne de commande d’audit pour arguments. Pour activer cette option, utilisez la ligne de commande suivante :

*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*


> [!NOTE]
> Regardez la vidéo [Validation des alertes dans Azure Security Center](https://channel9.msdn.com/Blogs/Azure-Security-Videos/Alert-Validation-in-Azure-Security-Center) pour voir comment utiliser cette fonctionnalité.

## <a name="see-also"></a>Voir aussi
Cet article vous a présenté le processus de validation des alertes. Maintenant que vous êtes familiarisé avec la validation, vous pouvez consulter les articles suivants :

* [Gestion et résolution des alertes de sécurité dans Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Apprenez à gérer les alertes et à répondre aux incidents de sécurité dans Security Center.
* [Surveillance de l’intégrité de la sécurité dans Azure Security Center](security-center-monitoring.md). découvrez comment surveiller l’intégrité de vos ressources Azure.
* [Présentation des alertes de sécurité dans Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). En savoir plus sur les différents types d’alertes de sécurité.
* [Guide de résolution des problèmes d’Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Apprenez à résoudre les problèmes fréquents dans Azure Security Center.
* [FAQ du Centre de sécurité Azure](security-center-faq.md). forum aux questions concernant l’utilisation de ce service.
* [Blog sur la sécurité Azure](https://blogs.msdn.com/b/azuresecurity/). accédez à des billets de blog sur la sécurité et la conformité Azure.
