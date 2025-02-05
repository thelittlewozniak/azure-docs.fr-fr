---
title: Optimiser les hyperparamètres pour votre modèle
titleSuffix: Azure Machine Learning service
description: Ajustez avec efficacité les hyperparamètres de votre modèle d’apprentissage profond (deep learning) / machine learning avec le service Azure Machine Learning. Vous allez apprendre à définir l’espace de recherche de paramètres, à spécifier une métrique principale à optimiser et à arrêter de façon anticipée les exécutions peu performantes.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: cf9ac0271e140d719da9a72424e1c01021fdf6c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65957429"
---
# <a name="tune-hyperparameters-for-your-model-with-azure-machine-learning-service"></a>Optimiser les hyperparamètres de votre modèle avec le service Azure Machine Learning

Ajustez avec efficacité les hyperparamètres de votre modèle à l’aide d’Azure Machine Learning.  L’optimisation des hyperparamètres comprend les étapes suivantes :

* Définir l’espace de recherche de paramètres
* Spécifier une métrique principale à optimiser  
* Spécifier des critères d’arrêt anticipé pour les exécutions peu performantes
* Allouer des ressources pour l’optimisation des hyperparamètres
* Lancer une expérience avec la configuration ci-dessus
* Visualiser les exécutions d’entraînement
* Sélectionner la configuration la plus performante pour votre modèle

## <a name="what-are-hyperparameters"></a>Présentation des hyperparamètres

Les hyperparamètres sont des paramètres ajustables que vous choisissez pour entraîner un modèle qui régit le processus d’entraînement lui-même. Par exemple, pour entraîner un réseau neuronal profond, vous décidez du nombre de couches masquées dans le réseau et du nombre de nœuds dans chaque couche préalablement à l’entraînement du modèle. Ces valeurs restent généralement constantes au cours du processus d’entraînement.

Dans les scénarios d’apprentissage profond / machine learning, les performances d’un modèle dépendent fortement des valeurs sélectionnées pour les hyperparamètres. L’objectif de l’exploration des hyperparamètres est de rechercher parmi les différentes configurations des hyperparamètres celle qui offre les meilleures performances. En règle générale, le processus d’exploration des hyperparamètres est manuel et minutieux, car l’espace de recherche est vaste et l’évaluation de chaque configuration peut être coûteuse en termes de ressources.

Azure Machine Learning vous permet d’automatiser l’exploration des hyperparamètres de manière efficace et ainsi de bénéficier de gains de temps et de ressources significatifs. Vous pouvez spécifier la plage de valeurs des hyperparamètres, ainsi qu’un nombre maximal d’exécutions d’entraînement. Le système lance alors automatiquement plusieurs exécutions simultanées avec différentes configurations de paramètres, puis recherche la configuration qui offre les meilleures performances, mesurées en fonction de la métrique que vous choisissez. Les exécutions d’entraînement ayant obtenu des performances médiocres sont arrêtées de façon anticipée, ce qui réduit considérablement le gaspillage des ressources de calcul. Ces ressources sont alors utilisées pour explorer d’autres configurations des hyperparamètres.


## <a name="define-search-space"></a>Définir l’espace de recherche

Optimisez automatiquement les hyperparamètres en explorant la plage de valeurs définie pour chaque hyperparamètre.

### <a name="types-of-hyperparameters"></a>Types d’hyperparamètres

Chaque hyperparamètre peut être discret ou continu.

#### <a name="discrete-hyperparameters"></a>Hyperparamètres discrets 

Les hyperparamètres discrets sont spécifiés en tant que `choice` parmi des valeurs discrètes. Voici à quoi `choice` peut correspondre :

* une ou plusieurs valeurs séparées par une virgule
* un objet `range`
* un objet `list` arbitraire


```Python
    {
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

Dans ce cas, `batch_size` prend l’une des valeurs [16, 32, 64, 128] et `number_of_hidden_layers` prend l’une des valeurs [1, 2, 3, 4].

Des hyperparamètres discrets avancés peuvent également être spécifiés en utilisant une distribution. Les distributions prises en charge sont les suivantes :

* `quniform(low, high, q)` : retourne une valeur comme round(uniform(low, high) / q) * q
* `qloguniform(low, high, q)` : retourne une valeur comme round(exp(uniform(low, high)) / q) * q
* `qnormal(mu, sigma, q)` : retourne une valeur comme round(normal(mu, sigma) / q) * q
* `qlognormal(mu, sigma, q)` : retourne une valeur comme round(exp(normal(mu, sigma)) / q) * q

#### <a name="continuous-hyperparameters"></a>Hyperparamètres continus 

Les hyperparamètres continus sont spécifiés comme une distribution sur une plage continue de valeurs. Les distributions prises en charge sont les suivantes :

* `uniform(low, high)` : retourne une valeur uniformément distribuée entre low (valeur basse) et high (valeur haute)
* `loguniform(low, high)` : retourne une valeur calculée en fonction de exp(uniform(low, high)), de sorte que le logarithme de la valeur de retour est uniformément distribué
* `normal(mu, sigma)` : retourne une valeur réelle qui est normalement distribuée avec la moyenne mu et l’écart type sigma
* `lognormal(mu, sigma)` : retourne une valeur calculée en fonction de exp(normal(mu, sigma)), de sorte que le logarithme de la valeur de retour est normalement distribué

Voici un exemple de définition d’espace de paramètres :

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Ce code définit un espace de recherche avec deux paramètres : `learning_rate` et `keep_probability`. `learning_rate` présente une distribution normale avec une valeur moyenne de 10 et un écart type de 3. `keep_probability` présente une distribution uniforme avec une valeur minimale de 0,05 et une valeur maximale de 0,1.

### <a name="sampling-the-hyperparameter-space"></a>Échantillonnage de l’espace des hyperparamètres

Vous pouvez aussi spécifier la méthode d’échantillonnage des paramètres à utiliser dans la définition de l’espace des hyperparamètres. Le service Azure Machine Learning prend en charge l’échantillonnage aléatoire, l’échantillonnage par grille et l’échantillonnage bayésien.

#### <a name="random-sampling"></a>Échantillonnage aléatoire

Dans l’échantillonnage aléatoire, les valeurs des hyperparamètres sont sélectionnées de façon aléatoire à partir de l’espace de recherche défini. L’échantillonnage aléatoire permet à l’espace de recherche d’inclure à la fois des hyperparamètres discrets et des hyperparamètres continus.

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Échantillonnage par grille

L’échantillonnage par grille effectue une simple recherche de grille sur toutes les valeurs possibles dans l’espace de recherche défini. Il peut être utilisé seulement avec des hyperparamètres spécifiés en utilisant `choice`. Par exemple, l’espace suivant compte un total de six échantillons :

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Échantillonnage bayésien

L’échantillonnage bayésien est basé sur l’algorithme d’optimisation bayésienne et fait des choix intelligents quant aux prochaines valeurs d’hyperparamètres à échantillonner. Il sélectionne l’échantillon en fonction des performances des échantillons précédents, si bien que le nouvel échantillon améliore la métrique principale rapportée.

Quand vous utilisez l’échantillonnage bayésien, le nombre d’exécutions simultanées a un impact sur l’efficacité du processus d’optimisation. En règle générale, un nombre inférieur d’exécutions simultanées peut déboucher sur une meilleure convergence d’échantillonnage, car le plus faible degré de parallélisme accroît le nombre d’exécutions qui bénéficient des exécutions ayant abouti précédemment.

L’échantillonnage bayésien prend en charge seulement les distributions `choice` et `uniform` sur l’espace de recherche. 

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> L’échantillonnage bayésien ne prend pas en charge les stratégies d’arrêt anticipé (consultez [Spécifier une stratégie d’arrêt anticipé](#specify-early-termination-policy)). Quand vous utilisez l’échantillonnage de paramètres bayésien, définissez `early_termination_policy = None` ou laissez le paramètre `early_termination_policy` désactivé.

<a name='specify-primary-metric-to-optimize'/>

## <a name="specify-primary-metric"></a>Spécifier la métrique principale

Spécifiez la métrique principale que l’expérience d’optimisation des hyperparamètres doit optimiser. Chaque exécution d’entraînement est évaluée par rapport à la métrique principale. Les exécutions peu performantes (pour lesquelles la métrique principale ne répond pas aux critères définis par la stratégie d’arrêt anticipé) sont arrêtées. En plus de spécifier le nom de la métrique principale, vous devez aussi spécifier l’objectif de l’optimisation, c’est-à-dire s’il faut maximiser ou minimiser la métrique principale.

* `primary_metric_name`: nom de la métrique principale à optimiser. Le nom de la métrique principale doit correspondre exactement au nom de la métrique journalisée par le script d’entraînement. Consultez [Journaliser des métriques pour l’optimisation des hyperparamètres](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: il peut s’agir de `PrimaryMetricGoal.MAXIMIZE` ou de `PrimaryMetricGoal.MINIMIZE`. Il détermine si la métrique principale est maximisée ou minimisée lors de l’évaluation des exécutions. 

```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```

Optimisez les exécutions de façon à maximiser la précision (« accuracy »).  Veillez à journaliser cette valeur dans votre script d’entraînement.

<a name='log-metrics-for-hyperparameter-tuning'/>

### <a name="log-metrics-for-hyperparameter-tuning"></a>Journaliser des métriques pour l’optimisation des hyperparamètres

Le script d’entraînement de votre modèle doit journaliser les métriques importantes pendant l’entraînement du modèle. Au moment de configurer l’optimisation des hyperparamètres, vous spécifiez la métrique principale à utiliser pour l’évaluation des performances d’exécution. (Consultez [Spécifier une métrique principale à optimiser](#specify-primary-metric-to-optimize).)  Dans votre script d’entraînement, vous devez journaliser cette métrique de façon à la rendre accessible au processus d’optimisation des hyperparamètres.

Journalisez cette métrique dans votre script d’entraînement avec l’extrait de code suivant :

```Python
from azureml.core.run import Run
run_logger = Run.get_context()
run_logger.log("accuracy", float(val_accuracy))
```

Le script d’entraînement calcule la valeur de `val_accuracy` et la journalise comme valeur de précision (« accuracy »), qui est utilisée comme métrique principale. Chaque fois que la métrique est journalisée, elle est reçue par le service d’optimisation des hyperparamètres. Il incombe au développeur du modèle de déterminer la fréquence de collecte de cette métrique.

<a name='specify-early-termination-policy'/>

## <a name="specify-early-termination-policy"></a>Spécifier une stratégie d’arrêt anticipé

Une stratégie d’arrêt anticipé permet d’arrêter automatiquement les exécutions peu performantes. Cet arrêt permet de réduire le gaspillage de ressources, qui servent alors à explorer les autres configurations de paramètres.

Si vous utilisez une stratégie d’arrêt anticipé, vous pouvez configurer les paramètres suivants pour contrôler à quel moment elle s’applique :

* `evaluation_interval` : fréquence d’application de la stratégie. Chaque journalisation de la métrique principale par le script d’entraînement compte pour un intervalle. Ainsi, une valeur de 1 pour `evaluation_interval` applique la stratégie chaque fois que le script d’entraînement signale la métrique principale. Une valeur de 2 pour `evaluation_interval` applique la stratégie à chaque autre signalement de la métrique principale par le script d’entraînement. La valeur par défaut de `evaluation_interval` est 1 si ce paramètre n’est pas spécifié.
* `delay_evaluation` : retarde la première évaluation de la stratégie pour un nombre d’intervalles spécifié. Il s’agit d’un paramètre facultatif qui permet à toutes les configurations de s’exécuter pour un nombre minimal initial d’intervalles, évitant ainsi un arrêt prématuré des exécutions d’entraînement. S’il est spécifié, la stratégie s’applique à chaque multiple de evaluation_interval qui est supérieur ou égal à delay_evaluation.

Le service Azure Machine Learning prend en charge les stratégies d’arrêt anticipé suivantes.

### <a name="bandit-policy"></a>Stratégie Bandit

La stratégie Bandit est une stratégie d’arrêt basée sur le facteur de marge/marge totale et l’intervalle d’évaluation. Cette stratégie arrête de façon anticipée les exécutions quand la métrique principale se trouve en dehors du facteur de marge/marge totale spécifié par rapport à l’exécution d’entraînement la plus performante. Elle prend les paramètres de configuration suivants :

* `slack_factor` ou `slack_amount` : marge autorisée par rapport à l’exécution d’entraînement la plus performante. `slack_factor` spécifie la marge autorisée sous forme de ratio. `slack_amount` spécifie la marge autorisée en valeur absolue, au lieu d’un ratio.

    Par exemple, considérez une stratégie Bandit appliquée avec un intervalle de 10. Supposez que l’exécution la plus performante à l’intervalle 10 a signalé une métrique principale de 0,8 avec l’objectif de maximiser la métrique principale. Si la stratégie a été spécifiée avec un `slack_factor` de 0,2, les exécutions d’entraînement, dont la meilleure mesure à l’intervalle 10 est inférieure à 0,66 (0,8/(1+`slack_factor`)) seront arrêtées. Si au lieu de cela, la stratégie a été spécifiée avec un `slack_amount` de 0,2, les exécutions d’entraînement, dont la meilleure mesure à l’intervalle 10 est inférieure à 0,6 (0,8 - `slack_amount`) seront arrêtées.
* `evaluation_interval` : fréquence d’application de la stratégie (paramètre facultatif).
* `delay_evaluation` : retarde la première évaluation de la stratégie pour un nombre d’intervalles spécifié (paramètre facultatif).


```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

Dans cet exemple, la stratégie d’arrêt anticipé est appliquée à chaque intervalle quand des métriques sont signalées, en commençant à l’intervalle d’évaluation 5. Toute exécution dont la meilleure métrique est inférieure à (1/(1+0,1) ou 91 % de la meilleure exécution sera arrêtée.

### <a name="median-stopping-policy"></a>Stratégie d’arrêt médiane

La stratégie d’arrêt médiane est une stratégie d’arrêt anticipé basée sur les moyennes mobiles des métriques principales rapportées par les exécutions. Cette stratégie calcule les moyennes mobiles pour toutes les exécutions d’entraînement et arrête les exécutions dont les performances sont moins bonnes que la médiane des moyennes mobiles. Cette stratégie prend les paramètres de configuration suivants :
* `evaluation_interval` : fréquence d’application de la stratégie (paramètre facultatif).
* `delay_evaluation` : retarde la première évaluation de la stratégie pour un nombre d’intervalles spécifié (paramètre facultatif).


```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

Dans cet exemple, la stratégie d’arrêt anticipé est appliquée à chaque intervalle, en commençant à l’intervalle d’évaluation 5. Une exécution est interrompue à l’intervalle 5 si sa meilleure métrique principale est moins bonne que la médiane des moyennes mobiles sur les intervalles 1 à 5 pour toutes les exécutions d’entraînement.

### <a name="truncation-selection-policy"></a>Stratégie de sélection de troncation

La stratégie de sélection de troncation annule un pourcentage donné des exécutions les moins performantes à chaque intervalle d’évaluation. Les exécutions sont comparées en fonction de leur performance sur la métrique principale et les X % les moins bonnes sont arrêtées. Elle prend les paramètres de configuration suivants :

* `truncation_percentage` : le pourcentage des exécutions avec les performances les moins bonnes à arrêter à chaque intervalle d’évaluation. Spécifiez un nombre entier compris entre 1 et 99.
* `evaluation_interval` : fréquence d’application de la stratégie (paramètre facultatif).
* `delay_evaluation` : retarde la première évaluation de la stratégie pour un nombre d’intervalles spécifié (paramètre facultatif).


```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

Dans cet exemple, la stratégie d’arrêt anticipé est appliquée à chaque intervalle, en commençant à l’intervalle d’évaluation 5. Une exécution est arrêtée à l’intervalle 5 si sa performance à cet intervalle se trouve dans les 20 % des performances les moins bonnes de toutes les exécutions à l’intervalle 5.

### <a name="no-termination-policy"></a>Stratégie sans arrêt

Si vous voulez que toutes les exécutions d’entraînement s’exécutent jusqu’à leur terme, définissez la stratégie sur None (Aucune). Son effet est qu’aucune stratégie d’arrêt anticipé n’est appliquée.

```Python
policy=None
```

### <a name="default-policy"></a>Stratégie par défaut

Si aucune stratégie n’est spécifiée, le service d’optimisation des hyperparamètres permet à toutes les exécutions d’entraînement d’arriver à leur terme.

>[!NOTE] 
>Si vous cherchez une stratégie classique qui permet de réaliser des économies, sans arrêter les tâches prometteuses, vous pouvez utiliser une stratégie d’arrêt à la médiane avec un `evaluation_interval` de 1 et un `delay_evaluation` de 5. Il s’agit de valeurs prudentes, qui peuvent fournir approximativement 25 à 35 % d’économies sans perte sur la métrique principale (d’après nos évaluations).

## <a name="allocate-resources"></a>Allouer des ressources

Maîtrisez votre budget ressources pour votre expérience d’optimisation des hyperparamètres en spécifiant le nombre total maximal d’exécutions d’entraînement.  Spécifiez éventuellement la durée maximale de votre expérience d’optimisation des hyperparamètres.

* `max_total_runs`: nombre total maximal d’exécutions d’entraînement à créer. Il s’agit d’une limite supérieure : le nombre d’exécutions peut être inférieur, par exemple si l’espace des hyperparamètres est limité et compte moins d’échantillons. Doit être un nombre compris entre 1 et 1000.
* `max_duration_minutes`: durée maximale en minutes de l’expérience d’optimisation des hyperparamètres. Ce paramètre est facultatif, et s’il est présent, les exécutions qui s’exécutent au-delà de cette durée sont automatiquement annulées.

>[!NOTE] 
>Si `max_total_runs` et `max_duration_minutes` sont tous deux spécifiés, l’expérience d’optimisation des hyperparamètres s’arrête dès que le premier de ces deux seuils est atteint.

Par ailleurs, spécifiez le nombre maximal d’exécutions d’entraînement qui peuvent s’exécuter simultanément pendant votre recherche d’optimisation des hyperparamètres.

* `max_concurrent_runs`: nombre maximal d’exécutions à exécuter simultanément à un moment donné. Si aucune valeur n’est spécifiée, toutes les `max_total_runs` exécutions sont lancées en parallèle. S’il est spécifié, sa valeur doit être un nombre compris entre 1 et 100.

>[!NOTE] 
>Le nombre d’exécutions simultanées est limité par les ressources disponibles dans la cible de calcul spécifiée. Vous devez donc vérifier que la cible de calcul dispose des ressources nécessaires à l’accès concurrentiel souhaité.

Allouez des ressources pour l’optimisation des hyperparamètres :

```Python
max_total_runs=20,
max_concurrent_runs=4
```

Ce code configure l’expérience d’optimisation des hyperparamètres pour utiliser un maximum de 20 exécutions au total, en exécutant 4 configurations à la fois.

## <a name="configure-experiment"></a>Configurer une expérience

Configurez votre expérience d’optimisation des hyperparamètres en utilisant l’espace de recherche des hyperparamètres défini, la stratégie d’arrêt anticipé, la métrique principale et l’allocation de ressources, qui sont décrits dans les sections précédentes. Par ailleurs, fournissez un `estimator` qui sera appelé avec les hyperparamètres échantillonnés. Cet `estimator` décrit le script d’entraînement que vous exécutez, les ressources par travail (un seul GPU ou plusieurs) et la cible de calcul à utiliser. L’accès concurrentiel pour votre expérience d’optimisation des hyperparamètres étant limité par les ressources disponibles, vérifiez que la cible de calcul spécifiée dans `estimator` dispose de ressources suffisantes pour l’accès concurrentiel souhaité. (Pour plus d’informations sur les estimateurs, consultez [Comment entraîner des modèles](how-to-train-ml-models.md).)

Configurez votre expérience d’optimisation des hyperparamètres :

```Python
from azureml.train.hyperdrive import HyperDriveConfig
hyperdrive_run_config = HyperDriveConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-experiment"></a>Soumettre l’expérience

Une fois que vous avez défini votre configuration d’optimisation des hyperparamètres, soumettez une expérience :

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

`experiment_name` est le nom que vous voulez attribuer à votre expérience d’optimisation des hyperparamètres, et `workspace` est l’espace de travail dans lequel vous voulez créer l’expérience (pour plus d’informations sur les expériences, consultez [Fonctionnement du service Azure Machine Learning](concept-azure-machine-learning-architecture.md)).

## <a name="visualize-experiment"></a>Visualiser l’expérience

Le SDK Azure Machine Learning fournit un widget Notebook qui vous permet de visualiser la progression de vos exécutions d’entraînement. L’extrait de code suivant vous permet de visualiser toutes vos exécutions d’optimisation des hyperparamètres dans un notebook Jupyter :

```Python
from azureml.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Ce code présente un tableau avec des détails sur les exécutions d’entraînement pour chacune des configurations d’hyperparamètres.

![Tableau des optimisations des hyperparamètres](media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

Vous pouvez également visualiser les performances de chacune des exécutions au fil de l’entraînement. 

![Tracé de l’optimisation des hyperparamètres](media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

De plus, vous pouvez identifier visuellement la corrélation entre les performances et les valeurs des hyperparamètres individuels avec un tracé de coordonnées parallèles. 

![Coordonnées parallèles de l’optimisation des hyperparamètres](media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)

Vous pouvez aussi visualiser toutes vos exécutions d’optimisation des hyperparamètres dans le portail web Azure. Pour plus d’informations sur la visualisation d’une expérience sur le portail web, consultez le [guide pratique pour suivre les expériences](how-to-track-experiments.md#view-the-experiment-in-the-web-portal).

![Portail de l’optimisation des hyperparamètres](media/how-to-tune-hyperparameters/HyperparameterTuningPortal.png)

## <a name="find-the-best-model"></a>Trouver le meilleur modèle

Une fois que toutes les exécutions d’optimisation des hyperparamètres ont abouti, identifiez la configuration la plus performante et les valeurs des hyperparamètres correspondantes :

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Exemple de notebook
Consultez ces notebooks :
* [how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch) 
* [how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Étapes suivantes
* [Faire le suivi d’une expérience](how-to-track-experiments.md)
* [Déployer un modèle entraîné](how-to-deploy-and-where.md)
