---
title: Utilisation d’Application Insights pour résoudre les problèmes liés aux stratégies personnalisées dans Azure Active Directory B2C | Microsoft Docs
description: configuration d’Application Insights pour suivre l’exécution de stratégies personnalisées.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: df5d710792d8c47e491f5b06d88f4050e8eb4a01
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66508072"
---
# <a name="azure-active-directory-b2c-collecting-logs"></a>Azure Active Directory B2C : collecte des journaux d’activité

Cet article explique comment collecter les journaux d’activité à partir d’Azure AD B2C afin que vous puissiez diagnostiquer les problèmes liés à vos stratégies personnalisées.

>[!NOTE]
>Actuellement, les journaux d’activité détaillés décrits ici sont conçus **UNIQUEMENT** pour faciliter le développement de stratégies personnalisées. N’utilisez pas le mode de développement en production.  Les journaux d’activité recueillent toutes les revendications envoyées par et aux fournisseurs d’identité au cours du développement.  En cas d’utilisation en production, le développeur assume la responsabilité des informations d’identification personnelle (PII) recueillies dans le journal Application Insights dont il est propriétaire.  Ces journaux d’activité détaillés ne sont collectés que si la stratégie est en **MODE DE DÉVELOPPEMENT**.


## <a name="use-application-insights"></a>Utiliser Application Insights

Azure AD B2C prend en charge une fonctionnalité d’envoi de données à Application Insights.  Application Insights permet de diagnostiquer les exceptions et de visualiser les problèmes de performances des applications.

### <a name="setup-application-insights"></a>Configurer Application Insights

1. Accédez au [portail Azure](https://portal.azure.com). Vérifiez que vous êtes sur le locataire avec votre abonnement Azure (pas votre locataire Azure AD B2C).
1. Cliquez sur **+ Nouveau** dans le menu de navigation de gauche.
1. Recherchez et sélectionnez **Application Insights**, puis cliquez sur **Créer**.
1. Remplissez le formulaire et cliquez sur **Créer**. Sélectionnez **Général** comme **Type d’application**.
1. Une fois que la ressource a été créée, ouvrez la ressource Application Insights.
1. Recherchez **Propriétés** dans le menu de gauche, puis cliquez dessus.
1. Copiez la **Clé d’instrumentation** et enregistrez-la pour la section suivante.

### <a name="set-up-the-custom-policy"></a>Configurer la stratégie personnalisée

1. Ouvrez le fichier RP (par exemple, SignUpOrSignin.xml).
1. Ajoutez les attributs suivants à l’élément `<TrustFrameworkPolicy>` :

   ```XML
   DeploymentMode="Development"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   ```

1. S’il n’existe pas déjà, ajoutez un nœud enfant `<UserJourneyBehaviors>` au nœud `<RelyingParty>`. Il doit être placé immédiatement après le `<DefaultUserJourney ReferenceId="UserJourney Id from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />`
2. Ajoutez le nœud suivant en tant qu’enfant de l’élément `<UserJourneyBehaviors>`. Veillez à remplacer `{Your Application Insights Key}` par la **Clé d’instrumentation** que vous avez obtenue à partir d’Application Insights dans la section précédente.

   ```XML
   <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
   ```

   * `DeveloperMode="true"` indique à ApplicationInsights d’accélérer l’envoi de la télémétrie via le pipeline de traitement, valable pour le développement, mais limité à des volumes élevés.
   * `ClientEnabled="true"` envoie le script côté client ApplicationInsights pour l’affichage de la page de suivi et les erreurs côté client (pas nécessaire).
   * `ServerEnabled="true"` envoie le JSON UserJourneyRecorder existant en tant qu’événement personnalisé à Application Insights.
   Exemple :

   ```XML
   <TrustFrameworkPolicy
    ...
    TenantId="fabrikamb2c.onmicrosoft.com"
    PolicyId="SignUpOrSignInWithAAD"
    DeploymentMode="Development"
    UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="UserJourney ID from your extensions policy, or equivalent (for example: SignUpOrSigninWithAzureAD)" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
   </TrustFrameworkPolicy>
   ```

3. Téléchargez la stratégie.

### <a name="see-the-logs-in-application-insights"></a>Afficher des journaux d’activité dans Application Insights

>[!NOTE]
> Il y a un court délai (moins de cinq minutes) avant que les nouveaux journaux d’activité s’affichent dans Application Insights.

1. Ouvrez la ressource Application Insights que vous avez créée sur le [portail Azure](https://portal.azure.com).
1. Dans le menu **Aperçu**, cliquez sur **Analytics**.
1. Ouvrez un nouvel onglet dans Application Insights.
1. Voici une liste de requêtes que vous pouvez utiliser pour afficher les journaux d’activité

| Requête | Description |
|---------------------|--------------------|
traces | Consultez tous les journaux d’activité générés par Azure AD B2C |
traces \| where timestamp > ago(1d) | Consultez tous les journaux d’activité générés par Azure AD B2C pour le dernier jour

Les entrées peuvent être longues.  Exporter au format CSV pour une étude plus approfondie.

Pour plus d’informations sur l’outil Analytics, cliquez [ici](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>La communauté a développé une visionneuse du parcours utilisateur pour aider les développeurs d’identité.  Elle n’est pas prise en charge par Microsoft et est mise à disposition tel quel.  Elle lit les données de votre instance d’Application Insights et présente les événements du parcours utilisateur d’une façon bien structurée.  Vous obtenez le code source et le déployez dans votre propre solution.

La version de la visionneuse qui lit les événements Application Insights se trouve [ici](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/wingtipgamesb2c/src/WingTipUserJourneyPlayerWebApplication).

>[!NOTE]
>Actuellement, les journaux d’activité détaillés décrits ici sont conçus **UNIQUEMENT** pour faciliter le développement de stratégies personnalisées. N’utilisez pas le mode de développement en production.  Les journaux d’activité recueillent toutes les revendications envoyées par et aux fournisseurs d’identité au cours du développement.  En cas d’utilisation en production, le développeur assume la responsabilité des informations d’identification personnelle (PII) recueillies dans le journal Application Insights dont il est propriétaire.  Ces journaux d’activité détaillés ne sont collectés que si la stratégie est en **MODE DE DÉVELOPPEMENT**.

[Référentiel GitHub pour exemples de stratégies personnalisées non pris en charge et outils connexes](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Étapes suivantes

Explorez les données d’Application Insights pour mieux comprendre la façon dont l’infrastructure d’expérience d’identité B2C sous-jacente fournit vos propres expériences d’identité.
