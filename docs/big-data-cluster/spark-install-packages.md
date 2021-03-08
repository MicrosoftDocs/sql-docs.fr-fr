---
title: Gestion de la bibliothèque Spark
titleSuffix: SQL Server big data clusters
description: Gestion de la bibliothèque Spark
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bed687cb003bfc7748aefa3c98ae5e19089f9685
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837056"
---
# <a name="spark-library-management"></a>Gestion de la bibliothèque Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article fournit des conseils sur l’importation et l’installation de packages pour une session Spark par le biais de configurations de sessions et de notebooks.

## <a name="built-in-tools"></a>Outils intégrés

Les packages Scala Spark (Scala 2.11) et Hadoop de base. 

PySpark (Python 3.7). Pandas, Sklearn, Numpy et autres traitements de données et packages du Machine Learning.

Packages MRO 3.5.2. Sparklyr et SparkR pour les charges de travail de R Spark.

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Installer des packages à partir d’un référentiel Maven sur le cluster Spark au moment de l’exécution

Les packages Maven peuvent être installés sur votre cluster Spark en configurant des cellules de notebook au début de votre session Spark. Avant de démarrer une session Spark dans Azure Data Studio, exécutez le code suivant :

```python
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-at-runtime"></a>Installer des packages Python sur PySpark au moment du runtime

La gestion des packages au niveau de la session et du travail garantit la cohérence et l’isolation de la bibliothèque. La configuration est une configuration de la bibliothèque standard Spark qui peut être appliquée sur les sessions Livy. __azdata Spark__ prend en charge ces configurations. Les exemples ci-dessous sont présentés comme des cellules de configuration __Notebooks Azure Data Studio__ qui doivent être exécutées après l’attachement à un cluster avec le noyau PySpark.

Si la configuration __«spark.pyspark.virtualenv.enabled » : « true »__ n’est pas définie, la session utilise le cluster python par défaut et les bibliothèques installées.

### <a name="sessionjob-configuration-with-requirementstxt"></a>Configuration de session/travail avec requirements.txt

Spécifiez le chemin vers un fichier requirements.txt dans HDFS à utiliser comme référence pour les packages à installer.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.7",
        "spark.pyspark.virtualenv.requirements" : "hdfs://user/project-A/requirements.txt"
    }
}
```

### <a name="sessionjob-configuration-with-different-python-versions"></a>Configuration de session/travail avec différentes versions de Python

Créez un environnement virtuel Conda sans fichier requirements et ajoutez dynamiquement des packages pendant la session Spark.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.6"
    }
}
```

### <a name="library-installation"></a>Installation d’une bibliothèque

Exécutez le __sc.install_packages__ pour installer des bibliothèques de manière dynamique dans votre session. Les bibliothèques sont installées dans le pilote et sur tous les nœuds de l’exécuteur.

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

Il est également possible d’installer plusieurs bibliothèques dans la même commande à l’aide d’un tableau.

 ```python
sc.install_packages(["numpy==1.11.0", "xgboost"])
import numpy as np
import xgboost as xgb
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Importer un fichier .jar de HDFS à utiliser au moment de l’exécution
Importez le fichier jar au moment de l’exécution par le biais d’une configuration de cellule de notebook Azure Data Studio.

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Importer le fichier jar au moment de l’exécution par le biais d’une configuration de cellule de notebook Azure Data Studio

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le cluster Big Data SQL Server et les scénarios associés, consultez [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).