---
title: Utiliser sparklyr à partir de RStudio
titleSuffix: SQL Server big data clusters
description: Découvrez comment utiliser sparklyr dans un cluster Big Data SQL Server pour vous connecter à Spark par le biais de l’interface R.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 8a15336a22f564dd22870de8843732cbb583d2f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043528"
---
# <a name="use-sparklyr-in-sql-server-big-data-cluster"></a>Utiliser sparklyr dans un cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Sparklyr fournit une interface R pour Apache Spark. Sparklyr est un moyen populaire permettant aux développeurs R d’utiliser Spark. Cet article explique comment utiliser sparklyr dans des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] à l’aide de RStudio.

## <a name="prerequisites"></a>Conditions préalables requises

- [Déployez un cluster Big Data SQL Server 2019](quickstart-big-data-cluster-deploy.md).

### <a name="install-rstudio-desktop"></a>Installer RStudio Desktop

Installez et configurez **RStudio Desktop** en procédant comme suit :

1. Si vous utilisez un client Windows, [téléchargez et installez R 3.4.4](https://cran.rstudio.com/bin/windows/base/old/3.4.4).

1. [Téléchargez et installez RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

1. Une fois l’installation terminée, exécutez les commandes suivantes au sein de RStudio Desktop pour installer les packages requis :

   ```RStudioDesktop
   install.packages("DBI", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("dplyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("sparklyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   ```

## <a name="connect-to-spark-in-a-big-data-cluster"></a>Se connecter à Spark dans un cluster Big Data

Vous pouvez utiliser sparklyr pour vous connecter à partir d’un client au cluster Big Data à l’aide de Livy et de la passerelle HDFS/Spark. 

Dans RStudio, créez un script R et connectez-vous à Spark comme dans l’exemple suivant :

> [!TIP]
> Pour les valeurs `<AZDATA_USERNAME>` et `<AZDATA_PASSWORD>`, utilisez le nom d’utilisateur et le mot de passe que vous définissez lors du déploiement du cluster Big Data.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Pour les valeurs `<IP>` et `<PORT>`, consultez la documentation sur la [connexion à un cluster Big Data](connect-to-big-data-cluster.md).

```r
library(sparklyr)
library(dplyr)
library(DBI)

#Specify the Knox username and password
config <- livy_config(user = "<AZDATA_USERNAME>", password = "<AZDATA_PASSWORD>")

httr::set_config(httr::config(ssl_verifypeer = 0L, ssl_verifyhost = 0L))

sc <- spark_connect(master = "https://<IP>:<PORT>/gateway/default/livy/v1",
                    method = "livy",
                    config = config)
```

## <a name="run-sparklyr-queries"></a>Exécuter des requêtes sparklyr

Après vous être connecté à Spark, vous pouvez exécuter sparklyr. L’exemple suivant exécute une requête sur un jeu de données Iris à l’aide de sparklyr :

```r
iris_tbl <- copy_to(sc, iris)

iris_count <- dbGetQuery(sc, "SELECT COUNT(*) FROM iris")

iris_count
```

## <a name="distributed-r-computations"></a>Calculs R distribués

L’une des fonctionnalités de sparklyr est la capacité à [distribuer des calculs R](https://spark.rstudio.com/guides/distributed-r/) avec [spark_apply](https://spark.rstudio.com/guides/distributed-r/#apply-an-r-function-to-a-spark-object).

Comme les clusters Big Data utilisent des connexions Livy, vous devez définir `packages = FALSE` dans l’appel à **spark_apply**. Pour plus d’informations, consultez la [section Livy](https://spark.rstudio.com/guides/distributed-r/#livy) de la documentation sparklyr sur les calculs R distribués. Avec ce paramètre, vous ne pouvez utiliser que les packages R déjà installés sur votre cluster Spark dans le code R passé à **spark_apply**. L’exemple suivant illustre cette fonctionnalité :

```r
iris_tbl %>% spark_apply(function(e) nrow(e), names = "nrow", group_by = "Species", packages = FALSE)
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data, consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).
