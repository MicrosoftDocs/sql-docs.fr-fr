---
title: Gestion de la bibliothèque Spark
titleSuffix: SQL Server big data clusters
description: Gestion de la bibliothèque Spark
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 01/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 70611d3f7d4ed6825911908942d9ed707dabbd2e
ms.sourcegitcommit: 8bdb5a51f87a6ff3b94360555973ca0cd0b6223f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549966"
---
# <a name="spark-library-management"></a>Gestion de la bibliothèque Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article fournit des conseils sur l’importation et l’installation de packages pour une session Spark par le biais de configurations de sessions et de notebooks.

## <a name="built-in-tools"></a>Outils intégrés
Packages de base Spark et Hadoop, Python 3.7 et Python 2.7, Pandas, Sklearn, Numpy et d’autres packages de traitement de données.
Packages R et MRO Sparklyr

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Installer des packages à partir d’un référentiel Maven sur le cluster Spark au moment de l’exécution
Les packages Maven peuvent être installés sur votre cluster Spark en configurant des cellules de notebook au début de votre session Spark. Avant de démarrer une session Spark dans Azure Data Studio, exécutez le code suivant :

```
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-job-submission-time"></a>Installer des packages Python au moment de la soumission de travaux PySpark
1. Spécifiez le chemin d’un fichier requirements.txt dans HDFS à utiliser comme référence pour les packages à installer.
```
%%configure -f \
{"conf": {
    "spark.pyspark.virtualenv.enabled" : "true",
    "spark.pyspark.virtualenv.type" : "conda",
    "spark.pyspark.virtualenv.requirements" : "requirements.txt",
    "spark.pyspark.virtualenv.bin.path" : "/opt/mls/python/bin/conda"
    }, 
"files": ["hdfs://nmnode-0/tmp/requirements.txt"]
}
```
2. Créez un environnement virtuel Conda sans fichier requirements et ajoutez dynamiquement des packages pendant la session Spark.
```
%%configure -f \
{"conf": {
    'spark.pyspark.virtualenv.enabled' : 'true',
    'spark.pyspark.virtualenv.type' : 'conda',
    'spark.pyspark.virtualenv.bin.path' : '/opt/mls/python/bin/conda',
    'spark.pyspark.virtualenv.python_version': '3.6'
 }
 ```

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Importer un fichier .jar de HDFS à utiliser au moment de l’exécution
Importez le fichier jar au moment de l’exécution par le biais d’une configuration de cellule de notebook Azure Data Studio.

```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Importer le fichier jar au moment de l’exécution par le biais d’une configuration de cellule de notebook Azure Data Studio
```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le cluster Big Data SQL Server et les scénarios associés, consultez [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).