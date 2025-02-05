---
title: Créer et Explorer des expériences de portail
titleSuffix: Azure Machine Learning service
description: Découvrez comment créer et gérer des expériences de portail d’apprentissage automatique
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tsikiksr
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/2019
ms.openlocfilehash: a2a281fda9272fb794692becb0ca08f3cf791458
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990143"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Créer et Explorer automatisé expériences machine learning dans le portail Azure (version préliminaire)

 Dans cet article, vous allez apprendre à créer, exécuter et explorez automatisé expériences machine learning dans le portail Azure sans une seule ligne de code. Apprentissage automatisé automatise le processus de sélection du meilleur algorithme à utiliser pour vos données spécifiques, afin de générer rapidement un modèle d’apprentissage automatique. [En savoir plus sur l’apprentissage automatique](concept-automated-ml.md).

 Si vous préférez une expérience plus basée sur le code, vous pouvez également [configurer vos expériences dans Python d’apprentissage automatique](how-to-configure-auto-train.md) avec la [kit SDK Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Conditions préalables

* Un abonnement Azure. Si vous n’avez pas d’abonnement Azure, créez un compte gratuit avant de commencer. Essayez la [version gratuite ou payante d’Azure Machine Learning service](https://aka.ms/AMLFree) dès aujourd’hui.

* Un espace de travail de service Microsoft Azure Machine Learning. Consultez [créer un espace de travail du service Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

## <a name="get-started"></a>Prise en main

Accédez au volet gauche de votre espace de travail. Sélectionnez automatisé Machine Learning sous la section Création (version préliminaire).

![Volet de navigation du portail Azure](media/how-to-create-portal-experiments/nav-pane.png)

 S’il s’agit de faire des expériences avec automatisé Machine Learning pour la première fois, vous verrez les éléments suivants :

![Page d’accueil d’expérience de portail Azure](media/how-to-create-portal-experiments/landing-page.png)

Sinon, vous verrez votre tableau de bord automatisée machine learning avec une vue d’ensemble de vos expériences, y compris ceux créés avec le Kit de développement logiciel d’apprentissage automatique. Ici, vous pouvez filtrer et Explorer vos exécutions par date, faire des essais nom, et état de l’exécution.

![Tableau de bord expérience de portail Azure](media/how-to-create-portal-experiments/dashboard.png)

## <a name="create-an-experiment"></a>Création d'une expérience

Sélectionnez le bouton Créer une expérience pour remplir le formulaire suivant.

![Créer un formulaire de l’expérience](media/how-to-create-portal-experiments/create-exp-name-compute.png)

1. Entrez le nom de votre expérience.

1. Sélectionnez un calcul pour le travail de formation et de profilage des données. Une liste de vos services de calcul existantes est disponible dans la liste déroulante. Pour créer un nouveau calcul, suivez les instructions à l’étape 3.

1. Sélectionnez le bouton Créer un nouveau calcul pour ouvrir le ci-dessous volet et configurer le contexte de calcul pour cette expérience.

    ![Créer nouveau calcul d’expérimentation](media/how-to-create-portal-experiments/create-new-compute.png)

    Champ|Description 
    ---|---
    Nom de la capacité de calcul| Entrez un nom unique qui identifie votre contexte de calcul.
    Taille de la machine virtuelle| Sélectionnez la taille de machine virtuelle pour votre calcul.
    Paramètres supplémentaires| *Nœud de min*: Entrez le nombre minimal de nœuds de votre ordinateur. Le nombre minimal de nœuds de calcul AML est 0. Pour activer le profilage de données, vous devez disposer de 1 ou plusieurs nœuds. <br> *Nœud de max*: Entrez le nombre maximal de nœuds de votre ordinateur. La valeur par défaut est 6 nœuds pour un calcul AML.

      Pour démarrer la création de votre nouveau calcul, sélectionnez **créer**. Cela peut prendre quelques instants.

      >[!NOTE]
      > Le nom de votre calcul indique si les ressources de calcul que vous sélectionnez/créez est *profilage activé*. (Voir 7 b pour plus d’informations sur le profilage des données).

1. Sélectionnez un compte de stockage pour vos données. Version préliminaire publique prend uniquement en charge les chargements de fichiers local et les comptes de stockage d’objets Blob Azure.

1. Sélectionnez un conteneur de stockage.

1. Sélectionnez un fichier de données à partir de votre conteneur de stockage, ou télécharger un fichier à partir de votre ordinateur local vers le conteneur.

    ![Sélectionnez le fichier de données pour l’expérience](media/how-to-create-portal-experiments/select-file.png)

1. Utiliser les onglets Aperçu et de profil pour configurer vos données pour cette expérience.

    1. Sous l’onglet Aperçu, indiquer si vos données incluent des en-têtes et sélectionnez les fonctionnalités (colonnes) pour l’utilisation de formation la **inclus** basculer les boutons dans chaque colonne de fonctionnalité.

        ![Aperçu des données](media/how-to-create-portal-experiments/data-preview.png)

    1. Sous l’onglet profil, vous pouvez afficher le [profil de données](#profile) par fonctionnalité, ainsi que la distribution, type et des statistiques de synthèse (moyenne, médiane, max./min et ainsi de suite) de chacun d’eux.

        ![Onglet profil de données](media/how-to-create-portal-experiments/data-profile.png)

        >[!NOTE]
        > Le message d’erreur suivant s’affiche si votre contexte de calcul est **pas** profilage activé : *Profilage des données est disponible uniquement pour les cibles de calcul qui sont déjà en cours d’exécution*.

1. Sélectionnez le type de travail de formation : classification, la régression ou la prévision.

1. Sélectionnez la colonne cible. La colonne à laquelle vous souhaitez effectuer les prédictions sur.

1. Pour la prévision de :
    1. Sélectionnez la colonne d’heure : Cette colonne contient les données de temps à utiliser.
    1. Sélectionnez l’horizon de prévision : Indiquer le nombre d’unités de temps (minutes/heures/jours/semaines/mois/années) le modèle sera en mesure de prédire l’avenir. Plus le modèle est requis pour prédire l’avenir, moins précise deviendra. [En savoir plus sur la prévision et les prévisions horizon](https://docs.microsoft.com/azure/machine-learning/service/how-to-auto-train-forecast#configure-experiment).

1. (Facultatif) Les paramètres avancés : les paramètres supplémentaires que vous pouvez utiliser pour mieux contrôler le travail de formation.

    Paramètres avancés|Description 
    ------|------
    Métrique principal| Mesure principale utilisée pour calculer les scores votre modèle. [En savoir plus sur les métriques de modèles](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train#explore-model-metrics).
    Critères de sortie| Lorsqu’un de ces critères sont remplie, le travail de formation se termine avant son achèvement complet. <br> *Le temps de travail (en minutes) d’apprentissage*: Combien de temps pour permettre l’exécution du travail de formation.  <br> *Nombre maximal d’itérations*: Nombre maximal de pipelines (itérations) à tester dans le travail de formation. Le travail ne sera pas exécuté supérieur au nombre spécifié d’itérations. <br> *Métrique de seuil du score*:  Métrique de score minimal pour tous les pipelines. Cela garantit que si vous avez une mesure cible défini que vous souhaitez atteindre, vous ne passez pas plus de temps sur le travail de formation que nécessaire.
    Prétraitement| Sélectionnez cette option pour activer ou désactiver le prétraitement effectué par l’apprentissage automatique. Prétraitement inclut le nettoyage automatique des données, la préparation et transformation pour générer des fonctionnalités synthétiques. [En savoir plus sur le prétraitement](#preprocess).
    Validation| Sélectionnez une des options de validation croisée à utiliser dans le travail de formation. [En savoir plus sur la validation croisée](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train#cross-validation-split-options).
    Concurrence| Sélectionnez les limites de plusieurs cœurs à utiliser lors de l’utilisation du calcul de l’unité centrale multicœur.
    Algorithme bloqué| Sélectionner des algorithmes que vous souhaitez exclure le travail de formation.

   ![Formulaire de paramètres avancés](media/how-to-create-portal-experiments/advanced-settings.png)

> [!NOTE]
> Pour plus d’informations sur les champs, cliquez sur l’info-bulle d’informations.

<a name="profile"></a>

### <a name="data-profiling"></a>Profilage des données

Vous pouvez obtenir un vaste éventail de statistiques de synthèse dans votre jeu de données pour vérifier si votre jeu de données est prête ML. Pour les colonnes non numériques, elles incluent uniquement les statistiques telles que min, max et nombre d’erreurs. Pour les colonnes numériques, vous pouvez également consulter leurs statistiques instants et les quantiles estimé. Plus précisément, notre profil de données inclut :

* **Fonctionnalité**: nom de la colonne qui est en cours de synthèse.

* **Profil**: une visualisation en ligne en fonction du type déduit. Par exemple, chaînes, des valeurs booléennes et des dates aura des nombres de valeur, tandis que le nombre de décimales (valeurs numériques) ont approximative des histogrammes. Cela vous permet d’obtenir une présentation rapide de la distribution des données.

* **Type de distribution**: un nombre de valeurs d’en ligne de types au sein d’une colonne. Les valeurs NULL sont leur propre type, donc cette visualisation est utile pour détecter les valeurs impaires ou manquantes.

* **Type**: le type déduit de la colonne. Les valeurs possibles sont : chaînes, les valeurs booléennes, les dates et les décimales.

* **Min**: la valeur minimale de la colonne. Entrées vides apparaissent pour les fonctionnalités dont le type n’a pas un ordre inhérent (par exemple, les valeurs booléennes).

* **Max**: la valeur maximale de la colonne. Comme « min », entrées vides apparaissent pour les fonctionnalités avec les types non pertinentes.

* **Nombre**: le nombre total d’entrées manquants et non manquant dans la colonne.

* **Ne manque nombre**: le nombre d’entrées dans la colonne qui ne sont pas manquant. Notez que les erreurs et les chaînes vides sont traités en tant que valeurs, afin qu’ils ne contribueront pas à « nombre pas manquant ».

* **Quantiles** (à des intervalles de 0,1, 1, 5, 25, 50, 75, 95, 99 et 99,9 %) : les valeurs approximatives à chaque quantile pour fournir une idée de la distribution des données. Entrées vides apparaissent pour les fonctionnalités avec les types non pertinentes.

* **Signifie**: la moyenne arithmétique de la colonne. Entrées vides apparaissent pour les fonctionnalités avec les types non pertinentes.

* **Écart type**: l’écart type de la colonne. Entrées vides apparaissent pour les fonctionnalités avec les types non pertinentes.

* **Variance**: la variance de la colonne. Entrées vides apparaissent pour les fonctionnalités avec les types non pertinentes.

* **Asymétrie**: l’asymétrie de la colonne. Entrées vides apparaissent pour les fonctionnalités avec les types non pertinentes.

* **Aplatissement**: l’aplatissement de la colonne. Entrées vides apparaissent pour les fonctionnalités avec les types non pertinentes.

<a name="preprocess"></a>

### <a name="advanced-preprocessing"></a>Advanced prétraitement

Lorsque vous configurez vos expériences, vous pouvez activer le paramètre avancé `Preprocess`. Cela signifie que les étapes de prétraitement et la caractérisation données suivantes sont effectuées automatiquement.

|Prétraitement&nbsp;étapes| Description  |
| ------------- | ------------- |
|Suppression des cardinalités élevées ou aucune caractéristique de variance|Supprimer à partir de jeux d’apprentissage et de validation, y compris les fonctionnalités avec toutes les valeurs manquantes, la même valeur pour toutes les lignes ou présentant une cardinalité très élevée (par exemple, hachages, ID ou GUID).|
|Imputer des valeurs manquantes|Pour les fonctionnalités numériques, imputer avec la moyenne des valeurs dans la colonne.<br/><br/>Pour les fonctionnalités catégorielles, imputer avec la valeur la plus fréquente.|
|Générer des caractéristiques supplémentaires|Pour les caractéristiques de type date/heure : Année, Mois, Jour, Jour de la semaine, Jour de l’année, Trimestre, Semaine de l’année, Heure, Minute, Seconde.<br/><br/>Pour les caractéristiques de type texte : Fréquence des termes selon unigrammes, bi-grammes et triple-caractère-grammes.|
|Transformer et encoder |Fonctionnalités numériques avec quelques valeurs uniques sont transformées en fonctions catégorielles.<br/><br/>Un encodage linéaire est effectué pour une faible cardinalité par catégorie ; pour une cardinalité élevée, hachage chaud un encodage.|
|Des incorporations de mots|Texte préapprentissage qui convertit les vecteurs de jetons de texte en vecteurs de phrase à l’aide d’un modèle préentraîné. Vecteur d’incorporation de chaque mot dans un document est agrégée pour produire un vecteur de fonctionnalité de document.|
|Encodages de cible|Pour les fonctionnalités catégorielles, mappe chaque catégorie avec la valeur cible moyenne pour les problèmes de régression et à la probabilité de classe pour chaque classe pour les problèmes de classification. Sur la fréquence de pondération et k-pli de validation croisée est appliquée pour réduire au fil de l’ajustement du mappage et les bruits catégories de données éparses.|
|Encodage de texte cible|Entrée de texte, un modèle linéaire empilé avec jeu de mots est utilisé pour générer la probabilité de chaque classe.|
|Poids des preuves (les écueils)|Calcule les écueils comme une mesure de corrélation des colonnes catégorielles à la colonne cible. Il est calculé en tant que le journal du ratio de probabilités hors de la classe de Visual Studio de classe. Cette étape génère une colonne de fonctionnalité numérique par la classe et supprime la nécessité d’explicitement imputer les valeurs manquantes et traitement des valeurs hors norme.|
|Distance de cluster|Effectue l’apprentissage d’un modèle de clustering k-means sur toutes les colonnes numériques.  Sorties k de nouvelles fonctionnalités, une nouvelle fonctionnalité numérique par cluster, contenant la distance de chacun des exemples pour le centroïde de chaque cluster.|

## <a name="run-experiment-and-view-results"></a>Exécutez l’expérience et afficher les résultats

Pour exécuter l’expérience, cliquez sur Démarrer. L’expérience de préparation des processus prend quelques minutes.

### <a name="view-experiment-details"></a>Afficher les détails expérience

Une fois la phase de préparation d’expérience est terminée, vous verrez l’écran de détails de l’exécution. Cela vous donne une liste complète des modèles créés. Par défaut, le modèle qui marque le nombre le plus élevé selon vos paramètres s’affiche en haut de la liste. Comme le travail de formation tente des modèles de plus, ils sont ajoutés à la liste d’itération et le graphique. Pour obtenir une comparaison rapide des mesures pour les modèles de produit jusqu'à présent, utilisez le graphique de l’itération.

Travaux de formation peut prendre un certain temps pour chaque pipeline se termine en cours d’exécution.

![Tableau de bord de détails de l’exécution](media/how-to-create-portal-experiments/run-details.png)

### <a name="view-training-run-details"></a>Afficher la formation détails de l’exécution

Zoom avant sur un des modèles de sortie pour afficher les détails de l’exécution, telles que les graphiques de métriques et la distribution des performances de formation. [En savoir plus sur les graphiques](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments#understanding-automated-ml-charts).

![Détails de l’itération](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-model"></a>Déployer le modèle

Une fois que vous avez le meilleur modèle à portée de main, il est temps pour le déployer comme un service web pour prédire sur les nouvelles données.

ML automatisé vous aide à déployer le modèle sans écrire de code :

1. Vous avez quelques options pour le déploiement. 
    1. Si vous souhaitez déployer le meilleur modèle selon les critères de mesure que vous définissez pour l’expérience, sélectionnez **déployer un meilleur modèle** à partir de la **détails de l’exécution** page.

        ![Bouton modèle de déploiement](media/how-to-create-portal-experiments/deploy-model-button.png)

    1. Si vous souhaitez déployer une itération de modèle spécifique, Descendre sur le modèle pour ouvrir sa page de détail spécifique de l’exécution et sélectionnez **déployer Model**.

        ![Bouton modèle de déploiement](media/how-to-create-portal-experiments/deploy-model-button2.png)

1. Première étape consiste à inscrire le modèle dans le service. Sélectionnez « Enregistrer le modèle » et attendez la fin du processus d’inscription.

    ![Panneau de modèle de déploiement](media/how-to-create-portal-experiments/deploy-model-blade.png)

1. Une fois que le modèle est enregistré, vous serez en mesure de télécharger le script de notation (scoring.py) et le script d’environnement (condaEnv.yml) à utiliser lors du déploiement.

1. Lorsque le script de notation et le script d’environnement sont téléchargés, accédez à la **actifs** panneau du volet de navigation de gauche, sélectionnez **modèles**.

    ![Modèles de volet de navigation](media/how-to-create-portal-experiments/nav-pane-models.png)

1. Sélectionnez le modèle que vous avez enregistré et sélectionnez « Créer image ».

    Vous pouvez identifier le modèle de par sa description, qui inclut l’ID d’exécution, numéro d’itération, au format suivant : *< Run_ID > _ < Iteration_number > _Model*

    ![Modèles : Créer une image](media/how-to-create-portal-experiments/model-create-image.png)

1. Entrez un nom pour l’image. 
1. Sélectionnez le **Parcourir** bouton en regard de la zone « Fichier de notation » pour charger le fichier de score (scoring.py) vous avez précédemment téléchargé.

1. Sélectionnez le **Parcourir** bouton en regard de la zone « Fichier Conda » pour charger le fichier d’environnement (condaEnv.yml) vous avez précédemment téléchargé.

    Vous pouvez utiliser votre propre script de notation et le fichier conda, ainsi que télécharger des fichiers supplémentaires. [En savoir plus sur le script de score](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#script).

      >[!Important]
      > Noms de fichiers doivent être sous 32 caractères et doit commencer et se terminer par des caractères alphanumériques. Peut inclure des tirets, des traits de soulignement, des points et entre des caractères alphanumériques. Les espaces ne sont pas autorisés.

    ![Créer une image](media/how-to-create-portal-experiments/create-image.png)

1. Sélectionnez le bouton « Créer » pour démarrer la création d’image. Cela prendra quelques minutes pour terminer, une fois terminé, vous verrez un message dans la barre supérieure.
1. Accédez à l’onglet « Images », cochez la case en regard de l’image que vous souhaitez déployer, sélectionnez « Créer un déploiement ». [En savoir plus sur les déploiements](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where).

    Il existe 2 options de déploiement.
     + Instance de conteneur Azure (ACI) - cela utilisée plus pour le test objectif plutôt qu’opérationnelle déploiement à grande échelle. Veillez à renseigner les valeurs au moins un cœur pour _capacité de réserve de processeur_et au moins un gigaoctet (Go) pour _capacité de réserve de mémoire_
     + Azure Kubernetes Service (ACS)) - cette option est pour le déploiement à grande échelle. Vous devez disposer d’un calcul en fonction de AKS prêt.

     ![Images : Créer un déploiement](media/how-to-create-portal-experiments/images-create-deployment.png)

1. Quand vous avez terminé, sélectionnez **Créer**. Déploiement du modèle peut prendre plusieurs minutes pour chaque pipeline se termine en cours d’exécution.

1. Et voilà ! Vous avez un service web opérationnel pour générer des prédictions.

## <a name="next-steps"></a>Étapes suivantes

* [En savoir plus sur l’apprentissage automatisé](concept-automated-ml.md) et Azure Machine Learning.
* [Découvrez comment utiliser un service web](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
