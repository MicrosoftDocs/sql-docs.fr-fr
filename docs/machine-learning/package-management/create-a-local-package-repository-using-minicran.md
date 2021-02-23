---
title: Créer un référentiel avec miniCRAN
titleSuffix: SQL machine learning
description: Découvrez comment installer des packages R hors connexion en utilisant le package miniCRAN pour créer un référentiel local de packages et de dépendances.
ms.prod: sql
ms.technology: machine-learning
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: f93a1180f25f764330db44306457e775961bc0da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350694"
---
# <a name="create-a-local-r-package-repository-using-minicran"></a>Créer un référentiel de packages R local à l’aide de miniCRAN
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Cet article explique comment installer des packages R hors connexion en utilisant [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) pour créer un référentiel local de packages et de dépendances. **miniCRAN** identifie les packages et les dépendances, puis les télécharge dans un dossier unique que vous pouvez copier sur d’autres ordinateurs afin d’installer le package R hors connexion.

Vous pouvez spécifier un ou plusieurs packages, puis **miniCRAN** lit de manière récursive l’arborescence des dépendances pour ces packages. Il télécharge ensuite uniquement les packages répertoriés et leurs dépendances à partir de CRAN ou de référentiels similaires.

Une fois l’opération terminée, **miniCRAN** crée un référentiel cohérent en interne qui comprend les packages sélectionnés et toutes les dépendances requises. Vous pouvez déplacer ce référentiel local vers le serveur et procéder à l’installation des packages sans connexion Internet.

Les utilisateurs expérimentés de R recherchent souvent la liste des packages dépendants dans le fichier DESCRIPTION d’un package téléchargé. Toutefois, les packages répertoriés dans **Importations** peuvent présenter des dépendances de second niveau. C’est pourquoi nous vous recommandons d’utiliser **miniCRAN** pour assembler toute la collection des packages requis.

## <a name="why-create-a-local-repository"></a>Pourquoi créer ou ajouter un référentiel local

La création d’un référentiel de packages local a pour but de fournir un emplacement unique utilisable par un administrateur de serveur ou d’autres utilisateurs de l’organisation afin d’installer de nouveaux packages R sur un serveur, en particulier ceux qui n’ont pas accès à Internet. Après avoir créé le référentiel, vous pouvez le modifier en ajoutant de nouveaux packages ou en mettant à niveau la version des packages existants.

Les référentiels de packages sont utiles dans les scénarios suivants :

- **Sécurité** : Beaucoup d’utilisateurs de R sont habitués à télécharger et à installer de nouveaux packages R selon leurs besoins à partir de CRAN ou de l’un de ses sites miroir. Toutefois, pour des raisons de sécurité, les serveurs de production qui exécutent [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ne disposent généralement pas d’une connectivité Internet.

- **Installation hors connexion simplifiée** : Pour installer un package sur un serveur hors connexion, vous devez également télécharger toutes les dépendances de package. Avec miniCRAN, il est plus facile d’extraire toutes les dépendances dans le format approprié et d’éviter les erreurs de dépendance.

- **Gestion des versions améliorée** : Dans un environnement multi-utilisateur, il existe de bonnes raisons d’éviter d’installer négligemment plusieurs versions des packages sur le serveur. Utilisez un référentiel local pour proposer un ensemble cohérent de packages à vos utilisateurs.

## <a name="install-minicran"></a>Installer miniCRAN

Le package **miniCRAN** dépend lui-même de 18 autres packages CRAN. Vous trouverez notamment le package **RCurl** qui présente une dépendance système envers le package **curl-devel**. De manière similaire, le package **XML** présente une dépendance envers le package **libxml2-devel**. Pour résoudre les dépendances, nous vous recommandons de créer votre référentiel local sur un ordinateur avec un accès complet à Internet en premier lieu.

Exécutez les commandes suivantes sur un ordinateur avec une version R de base, des outils R et une connexion Internet. Nous partons du principe que vous n’utilisez pas votre ordinateur SQL Server. Les commandes suivantes permettent d’installer les packages **miniCRAN** et **igraph**. Cet exemple vérifie si le package est déjà installé, mais vous pouvez ignorer les instructions `if` et installer les packages directement.

```R
if(!require("miniCRAN")) install.packages("miniCRAN") 
if(!require("igraph")) install.packages("igraph") 
library("miniCRAN")
```

## <a name="set-the-cran-mirror-and-mran-snapshot"></a>Définir le miroir CRAN et l’instantané MRAN

Spécifiez un site miroir à utiliser pour récupérer les packages. Par exemple, vous pouvez utiliser le site MRAN ou un autre site de votre région qui contient les packages dont vous avez besoin. Si un téléchargement échoue, essayez un autre site miroir.

```R
CRAN_mirror <- c(CRAN = "https://cran.cnr.berkeley.edu")
```

## <a name="create-a-local-folder"></a>Créer un dossier local

Créez un dossier local dans lequel vous stockerez les packages collectés. Si vous réitérez régulièrement cette procédure, nous vous conseillons d’utiliser un nom descriptif comme « miniCRANZooPackages » ou « miniCRANMyRPackageV2 ».

Définissez le dossier comme référentiel local. La syntaxe R utilise une barre oblique pour les noms de chemin d’accès, ce qui correspond au sens inverse par rapport aux conventions Windows.

```R
local_repo <- "C:/miniCRANZooPackages"
```

## <a name="add-packages-to-the-local-repo"></a>Ajouter des packages au référentiel local

Une fois **miniCRAN** installé et chargé, créez une liste qui spécifie les packages supplémentaires que vous souhaitez télécharger.

**N’ajoutez pas** de dépendances à cette liste initiale. Le package **igraph** utilisé par **miniCRAN** génère automatiquement la liste des dépendances. Pour en savoir plus sur l’utilisation du graphique de dépendance généré, consultez [Using miniCRAN to identify package dependencies](https://cran.r-project.org/web/packages/miniCRAN/vignettes/miniCRAN-dependency-graph.html) (Utiliser miniCRAN pour identifier les dépendances de package).

1. Ajoutez les packages cibles « zoo » et « forecast » à une variable.

    ```R
    pkgs_needed <- c("zoo", "forecast")
    ```

2. Si vous le souhaitez, vous pouvez tracer le graphique de dépendance. Ce n’est pas obligatoire, mais cela peut être informatif.

    ```R
    plot(makeDepGraph(pkgs_needed))
    ```

3. Créez le référentiel local. Assurez-vous de modifier la version R (si nécessaire) en fonction de la version installée sur votre instance SQL Server. Si vous avez mis à niveau l’un de vos composants, votre version peut être plus récente que la version d’origine. Pour plus d’informations, consultez la section [Obtenir des informations sur le package R](../package-management/r-package-information.md).

    ```R
    pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);
    makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.3");
    ```

   À partir de ces informations, le package miniCRAN crée la structure de dossiers dont vous avez besoin pour copier les packages sur le [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ultérieurement.

À ce stade, vous disposez normalement d’un dossier contenant les packages dont vous avez besoin et tous les packages supplémentaires requis. Le dossier doit contenir une collection de packages compressés. N’extrayez pas les packages et ne renommez pas les fichiers.

Si vous le souhaitez, vous pouvez exécuter le code suivant pour répertorier les packages contenus dans le référentiel miniCRAN local.

```R
pdb <- as.data.frame(pkgAvail(local_repo, type = "win.binary", Rversion = "3.3"), stringsAsFactors = FALSE);
head(pdb);
pdb$Package;
pdb[, c("Package", "Version", "License")]
```

## <a name="add-packages-to-the-instance-library"></a>Ajouter des packages à la bibliothèque d’instances

Une fois que vous disposez d’un référentiel local contenant les packages dont vous avez besoin, déplacez le référentiel de packages vers l’ordinateur SQL Server. La procédure suivante décrit comment installer les packages à l’aide des outils R.

::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> La méthode recommandée pour l’installation de packages consiste à utiliser **sqlmlutils**. Consultez [Installer de nouveaux packages R avec sqlmlutils](install-additional-r-packages-on-sql-server.md).
::: moniker-end

1. Copiez l’intégralité du dossier contenant le référentiel miniCRAN sur le serveur où vous souhaitez installer les packages. Le dossier présente généralement la structure suivante : 

   `<miniCRAN root>/bin/windows/contrib/version/<all packages>`

   Dans cette procédure, nous partons du principe que le dossier est situé dans le lecteur racine.

2. Ouvrez un outil R associé à l’instance (par exemple, vous pouvez utiliser RGUI.exe). Cliquez avec le bouton droit et sélectionnez **Exécuter en tant qu’administrateur** pour autoriser l’outil à mettre à jour votre système.

   ::: moniker range="=sql-server-2016"
   - Par exemple, l’emplacement de fichier par défaut pour le fichier RGUI est `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   - Par exemple, l’emplacement de fichier pour le fichier RGUI est `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   - Par exemple, l’emplacement de fichier pour le fichier RGUI est `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

3. Copiez le chemin d’accès de la bibliothèque d’instances, puis ajoutez-le à la liste des chemins de bibliothèque.

   ::: moniker range="=sql-server-2016"
   Par exemple,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL13.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Par exemple,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   Par exemple,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL15.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

4. Spécifiez le nouvel emplacement sur le serveur où vous avez copié le référentiel **miniCRAN** en tant que `server_repo`.

    Dans cet exemple, nous partons du principe que vous avez copié le référentiel dans un dossier temporaire sur le serveur.

    ```R
    inputlib <- "C:/miniCRANZooPackages"
    ```

5. Étant donné que vous travaillez dans un nouvel espace de travail R sur le serveur, vous devez également fournir la liste des packages à installer.

    ```R
    mypackages <- c("zoo", "forecast")
    ```

6. Installez les packages en fournissant le chemin d’accès vers la copie locale du référentiel miniCRAN.

    ```R
    install.packages(mypackages, repos = file.path("file://", normalizePath(inputlib, winslash = "/")), lib = outputlib, type = "win.binary", dependencies = TRUE);
    ```

7. Dans la bibliothèque d’instances, vous pouvez afficher les packages installés à l’aide d’une commande similaire à ce qui suit :

    ```R
    installed.packages()
    ```

## <a name="see-also"></a>Voir aussi

+ [Obtenir des informations sur les packages R](../package-management/r-package-information.md)
+ [Tutoriels sur R](../tutorials/r-tutorials.md)