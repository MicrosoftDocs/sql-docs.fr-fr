---
title: Installer de nouveaux packages R avec sqlmlutils
description: Découvrez comment utiliser sqlmlutils pour installer de nouveaux packages R sur une instance de SQL Server Machine Learning Services ou SQL Server R Services.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/15/2019
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 2c6fd8a9339756c6c22870e4eca6203064dc27f4
ms.sourcegitcommit: 00350f6ffb73c2c0d99beeded61c5b9baa63d171
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70190354"
---
# <a name="install-new-r-packages-with-sqlmlutils"></a>Installer de nouveaux packages R avec sqlmlutils

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Cet article explique comment utiliser les fonctions du package [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) pour installer de nouveaux packages R sur une instance de SQL Server Machine Learning Services ou SQL Server R services. Les packages que vous installez peuvent être utilisés dans des scripts R exécutés dans la base de données à l’aide de l’instruction T-SQL [SP-Execute-External-script-Transact-SQL](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) .

> [!NOTE]
> La commande r `install.packages` standard n’est pas recommandée pour l’ajout de packages R sur SQL Server. Utilisez plutôt **sqlmlutils** comme décrit dans cet article.

## <a name="prerequisites"></a>Prérequis

- Installez [R](https://www.r-project.org) et [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) sur l’ordinateur client que vous utilisez pour vous connecter à SQL Server. Vous pouvez utiliser n’importe quel IDE R pour exécuter les scripts, mais cet article suppose RStudio.

- Installez [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) ou [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) sur l’ordinateur client que vous utilisez pour vous connecter à SQL Server. Vous pouvez utiliser d’autres outils de gestion de base de données ou de requête, mais cet article suppose Azure Data Studio ou SSMS.

### <a name="other-considerations"></a>Autres points à considérer

- Le script R s’exécutant dans SQL Server peut utiliser uniquement des packages installés dans la bibliothèque d’instances par défaut. SQL Server ne peut pas charger les packages à partir de bibliothèques externes, même si cette bibliothèque est sur le même ordinateur. Cela comprend les bibliothèques R installées avec d’autres produits Microsoft.

- Dans un environnement de SQL Server renforcé, vous souhaiterez peut-être éviter ce qui suit:
  - Packages qui nécessitent un accès réseau
  - Packages nécessitant un accès avec système de fichiers élevé
  - Packages utilisés pour le développement Web ou d’autres tâches qui ne bénéficient pas de l’exécution dans SQL Server

## <a name="install-sqlmlutils-on-the-client-computer"></a>Installer sqlmlutils sur l’ordinateur client

Pour utiliser **sqlmlutils**, vous devez d’abord l’installer sur l’ordinateur client que vous utilisez pour vous connecter à SQL Server.

Le package **sqlmlutils** dépend du package **RODBCext** , et **RODBCext** dépend d’un certain nombre d’autres packages. Les procédures suivantes installent tous ces packages dans le bon ordre.

### <a name="install-sqlmlutils-online"></a>Installer sqlmlutils en ligne

Si l’ordinateur client a accès à Internet, vous pouvez télécharger et installer **sqlmlutils** et ses packages dépendants en ligne.

1. Téléchargez le fichier zip **sqlmlutils** le plus https://github.com/Microsoft/sqlmlutils/tree/master/R/dist récent à partir de sur l’ordinateur client. Ne Décompressez pas le fichier.

1. Ouvrez une **invite de commandes** et exécutez les commandes suivantes pour installer les packages **sqlmlutils** et **RODBCext**. Remplacez le chemin d’accès complet au fichier zip **sqlmlutils** que vous avez téléchargé (cet exemple suppose que le fichier se trouve dans votre dossier Documents). Le package **RODBCext** est trouvé en ligne et installé.

   ```console
   R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
   R CMD INSTALL %UserProfile%\Documents\sqlmlutils_0.7.1.zip
   ```

### <a name="install-sqlmlutils-offline"></a>Installer sqlmlutils hors connexion

Si l’ordinateur client n’a pas de connexion Internet, vous devez télécharger les packages **sqlmlutils** et **RODBCext** à l’avance à l’aide d’un ordinateur qui a accès à Internet. Vous pouvez ensuite copier les fichiers dans un dossier sur l’ordinateur client et installer les packages hors connexion.

Le package **RODBCext** a plusieurs packages dépendants, et l’identification de toutes les dépendances d’un package devient compliquée. Nous vous recommandons d’utiliser [**miniCRAN**](https://andrie.github.io/miniCRAN/) pour créer un dossier de référentiel local pour le package qui comprend tous les packages dépendants.
Pour plus d’informations, consultez [créer un référentiel de packages R local à l’aide de miniCRAN](create-a-local-package-repository-using-minicran.md).

Le package **sqlmlutils** se compose d’un seul fichier zip que vous pouvez copier sur l’ordinateur client et installer.

Sur un ordinateur disposant d’un accès à Internet:

1. Installez **miniCRAN**. Pour plus d’informations, consultez [installer miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran) .

1. Dans RStudio, exécutez le script R suivant pour créer un référentiel local du package **RODBCext**. Cet exemple crée le référentiel dans le dossier `c:\downloads\rodbcext`.

   ::: moniker range=">=sql-server-2016||=sqlallproducts-allversions"

   ```R
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/rodbcext"
   pkgs_needed <- "RODBCext"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```

   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15||=sqlallproducts-allversions"

   ```R
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/rodbcext"
   pkgs_needed <- "RODBCext"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```

   ::: moniker-end

   Pour la `Rversion` valeur, utilisez la version de R installée sur SQL Server. Pour vérifier la version installée, utilisez la commande T-SQL suivante.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Téléchargez le fichier zip **sqlmlutils** le plus https://github.com/Microsoft/sqlmlutils/tree/master/R/dist récent à partir de (ne Décompressez pas le fichier). Par exemple, téléchargez le fichier dans `c:\downloads\sqlmlutils_0.7.1.zip`.

1. Copiez l' intégralité du dossier de référentiel RODBCext`c:\downloads\rodbcext`() et le fichier zip sqlmlutils`c:\downloads\sqlmlutils_0.7.1.zip`() sur l’ordinateur client. Par exemple, copiez-les dans `c:\temp\packages` le dossier sur l’ordinateur client.

Sur l’ordinateur client que vous utilisez pour vous connecter à SQL Server, ouvrez une invite de commandes et exécutez les commandes suivantes pour installer **RODBCext** , puis **sqlmlutils**.

```console
R -e "install.packages('RODBCext', repos='c:\temp\packages\rodbcext')"
R CMD INSTALL c:\temp\packages\sqlmlutils_0.7.1.zip
```

## <a name="add-an-r-package-on-sql-server"></a>Ajouter un package R sur SQL Server

Dans l’exemple suivant, vous allez ajouter le package de [**Collage**](https://cran.r-project.org/web/packages/glue/) à SQL Server.

### <a name="add-the-package-online"></a>Ajouter le package en ligne

Si l’ordinateur client que vous utilisez pour vous connecter à SQL Server a accès à Internet, vous pouvez utiliser **sqlmlutils** pour rechercher le package de liaison et toutes les dépendances sur Internet, puis installer le package sur une instance de SQL Server à distance.

1. Sur l’ordinateur client, ouvrez RStudio et créez un nouveau fichier de **script R** .

1. Utilisez le script R suivant pour installer le package **Glue** à l’aide de **sqlmlutils**. Remplacez vos propres informations de connexion à la base de données SQL Server.

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server= "yourserver",
     database = "yourdatabase",
     uid = "yoursqluser",
     pwd = "yoursqlpassword")

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
   ```

   > [!TIP]
   > L' **étendue** peut être **publique** ou **privée**. L’étendue publique est utile pour que l’administrateur de base de données installe des packages que tous les utilisateurs peuvent utiliser. L’étendue privée rend le package disponible uniquement pour l’utilisateur qui l’installe. Si vous ne spécifiez pas l’étendue, l’étendue par défaut est **Private**.

### <a name="add-the-package-offline"></a>Ajouter le package hors connexion

Si l’ordinateur client n’a pas de connexion Internet, vous pouvez utiliser **miniCRAN** pour télécharger le package de liaison à l’aide d’un ordinateur qui a accès à Internet. Vous copiez ensuite le package sur l’ordinateur client sur lequel vous pouvez installer le package hors connexion.
Pour plus d’informations sur l’installation de **miniCRAN**, consultez [install miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran) .

Sur un ordinateur disposant d’un accès à Internet:

1. Exécutez le script R suivant pour créer un référentiel local à **coller**. Cet exemple crée le dossier de référentiel dans `c:\downloads\glue`.

   ::: moniker range=">=sql-server-2016||=sqlallproducts-allversions"

   ```R
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```

   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15||=sqlallproducts-allversions"

   ```R
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```

   ::: moniker-end


   Pour la `Rversion` valeur, utilisez la version de R installée sur SQL Server. Pour vérifier la version installée, utilisez la commande T-SQL suivante.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Copiez l' intégralité du dossier de dépôt de`c:\downloads\glue`la colle () sur l’ordinateur client. Par exemple, copiez-le dans `c:\temp\packages\glue`le dossier.

Sur l’ordinateur client:

1. Ouvrez RStudio et créez un nouveau fichier de **script R** .

1. Utilisez le script R suivant pour installer le package **Glue** à l’aide de **sqlmlutils**. Remplacez vos propres informations de connexion à la base de données SQL Server.

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server= "yourserver",
     database = "yourdatabase",
     uid = "yoursqluser",
     pwd = "yoursqlpassword")
   localRepo = "c:/temp/packages/glue"

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC", repos=paste0("file:///",localRepo))
   ```

   > [!TIP]
   > L' **étendue** peut être **publique** ou **privée**. L’étendue publique est utile pour que l’administrateur de base de données installe des packages que tous les utilisateurs peuvent utiliser. L’étendue privée rend le package disponible uniquement pour l’utilisateur qui l’installe. Si vous ne spécifiez pas l’étendue, l’étendue par défaut est **Private**.

## <a name="use-the-package"></a>Utiliser le package

Une fois le package de **Collage** installé, vous pouvez l’utiliser dans un script R dans SQL Server à l’aide de la commande T-SQL **sp_execute_external_script** .

1. Ouvrez Azure Data Studio ou SSMS et connectez-vous à votre base de données SQL Server.

1. Exécutez la commande suivante :

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
       , @script = N'
   library(glue)

   name <- "Fred"
   birthday <- as.Date("2020-06-14")
   text <- glue(''My name is {name} '',
   ''and my birthday is {format(birthday, "%A, %B %d, %Y")}.'')

   print(text)
         ';
   ```

    **Résultats**

    ```text
    My name is Fred and my birthday is Sunday, June 14, 2020.
    ```

## <a name="remove-the-package"></a>Supprimer le package

Si vous souhaitez supprimer le package de **Collage** , exécutez le script R suivant. Utilisez la même variable de **connexion** que celle que vous avez définie précédemment.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

## <a name="next-steps"></a>Étapes suivantes

- Pour plus d’informations sur les packages R installés, consultez [obtenir des informations](r-package-information.md) sur les packages r
- Pour obtenir de l’aide sur l’utilisation des packages R, consultez [conseils pour l’utilisation des packages r](tips-for-using-r-packages.md)
- Pour plus d’informations sur l’installation des packages Python, consultez [installer des packages Python avec PIP](install-additional-python-packages-on-sql-server.md)
- Pour plus d’informations sur SQL Server Machine Learning Services, consultez [qu’est-ce que SQL Server machine learning services (Python et R)?](../what-is-sql-server-machine-learning.md)