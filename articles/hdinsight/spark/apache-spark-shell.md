---
title: Utiliser un interpréteur de commandes Spark interactif dans Azure HDInsight
description: Un interpréteur de commandes Spark interactif fournit un processus de lecture-exécution-impression pour l’exécution des commandes Spark les unes après les autres et l’affichage des résultats.
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.openlocfilehash: 9044ed3ad9cf9ffa2f54d130bb50b37df121b86f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64696801"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Exécuter Apache Spark depuis l’interpréteur de commandes Spark

Un interpréteur de commandes [Apache Spark](https://spark.apache.org/) interactif fournit un environnement REPL (boucle de lecture-exécution-impression) pour l’exécution des commandes Spark les unes après les autres et l’affichage des résultats. Ce processus est utile pour le développement et le débogage. Spark fournit un interpréteur de commandes pour chacun des langages qu’il prend en charge : Scala, Python et R.

## <a name="get-to-an-apache-spark-shell-with-ssh"></a>Obtenir un interpréteur de commandes Apache Spark avec SSH

Accédez à un interpréteur de commandes Apache Spark sur HDInsight en vous connectant au nœud principal du cluster à l’aide de SSH :

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

Vous pouvez obtenir la commande SSH pour votre cluster depuis le portail Azure :

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Accédez au volet correspondant à votre cluster HDInsight Spark.
3. Sélectionnez Secure Shell (SSH).

    ![Volet HDInsight du portail Azure](./media/apache-spark-shell/hdinsight-spark-blade.png)

4. Copiez la commande SSH affichée et exécutez-la sur votre terminal.

    ![Volet HDInsight SSH du portail Azure](./media/apache-spark-shell/hdinsight-spark-ssh-blade.png)

Pour en savoir plus sur l’utilisation de SSH pour vous connecter à HDInsight, consultez [Utilisation de SSH avec HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-an-apache-spark-shell"></a>Exécuter un interpréteur de commandes Apache Spark

Spark fournit des interpréteurs de commandes pour Scala (spark-shell), Python (pyspark) et R (sparkR). Dans votre session SSH, au niveau du nœud principal de votre cluster HDInsight, entrez l’une des commandes suivantes :

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

Vous pouvez à présent entrer des commandes Spark dans la langue appropriée.

## <a name="sparksession-and-sparkcontext-instances"></a>Instances de SparkSession et SparkContext

Par défaut, lorsque vous exécutez l’interpréteur de commandes, des instances de SparkSession et SparkContext sont automatiquement instanciées pour vous.

Pour accéder à l’instance de SparkSession, entrez `spark`. Pour accéder à l’instance de SparkContext, entrez `sc`.

## <a name="important-shell-parameters"></a>Paramètres importants d’interpréteur de commandes

La commande d’interpréteur de commandes Spark (`spark-shell`, `pyspark` ou `sparkR`) prend en charge de nombreux paramètres de ligne de commande. Pour afficher une liste complète des paramètres, démarrez l’interpréteur de commandes Spark avec le commutateur `--help`. Notez que certains de ces paramètres peuvent uniquement s’appliquer à `spark-submit`, qui encapsule l’interpréteur de commandes Spark.

| commutateur | description | exemple |
| --- | --- | --- |
| --master MASTER_URL | Spécifie l’URL principale. Dans HDInsight, cette valeur est toujours `yarn`. | `--master yarn`|
| --jars JAR_LIST | Liste séparée par des virgules des fichiers JAR locaux à inclure dans les classpaths du pilote et de l’exécuteur. Dans HDInsight, cette liste est composée de chemins au système de fichiers par défaut dans Data Lake Storage ou Stockage Azure. | `--jars /path/to/examples.jar` |
| --packages MAVEN_COORDS | Liste séparée par des virgules des coordonnées Maven des fichiers JAR à inclure dans les classpaths du pilote et de l’exécuteur. Recherche dans le référentiel Maven local, puis dans le référentiel Maven central, et enfin dans tout autre référentiel distant spécifié avec `--repositories`. Le format des coordonnées est *groupId*:*artifactId*:*version*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py-files LIST | Pour Python uniquement, une liste séparée par des virgules des fichiers .zip, .egg ou .py à placer dans le PYTHONPATH. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Étapes suivantes

- Consultez [Présentation d’Apache Spark sur Azure HDInsight](apache-spark-overview.md) pour obtenir une vue d’ensemble.
- Consultez [Créer un cluster Apache Spark dans Azure HDInsight](apache-spark-jupyter-spark-sql.md) pour utiliser des clusters Spark et SparkSQL.
- Consultez [Vue d’ensemble d’Apache Spark Structured Streaming](apache-spark-streaming-overview.md) pour écrire des applications traitant des données de diffusion en continu avec Spark.
