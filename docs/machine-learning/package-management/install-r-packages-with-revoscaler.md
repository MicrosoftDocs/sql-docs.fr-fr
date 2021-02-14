---
title: Utiliser RevoScaleR pour installer des packages R
description: Découvrez comment utiliser des fonctions RevoScaleR pour installer des packages R sur SQL Server avec Machine Learning Services ou Services R.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 99750753f7b74070ecb3bc8217d72b91255a0962
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082610"
---
# <a name="use-revoscaler-to-install-r-packages"></a>Utiliser RevoScaleR pour installer des packages R

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Cet article décrit comment utiliser les fonctions [RevoScaleR](../r/ref-r-revoscaler.md) (version 9.0.1 et version ultérieure) pour installer les packages R sur SQL Server avec Machine Learning Services ou Services R. Les fonctions RevoScaleR peuvent être utilisées par des non administrateurs distants pour installer des packages sur SQL Server sans accès direct au serveur.

::: moniker range="=sql-server-2016"
> [!NOTE]
> Les clients SQL Server R Services doivent procéder à une [mise à jour des composants](../install/upgrade-r-and-python.md) pour obtenir les fonctions de gestion des packages RevoScaleR. Pour obtenir des instructions sur comment récupérer la version et le contenu du package, consultez [Obtenir des informations sur le package R](../package-management/r-package-information.md).
::: moniker-end

## <a name="revoscaler-functions-for-package-management"></a>Fonctions RevoScaleR pour la gestion de packages

La table suivante décrit les fonctions utilisées pour l’installation et la gestion du package R.

| Fonction | Description |
|----------|-------------|
| [rxSqlLibPaths](/machine-learning-server/r-reference/revoscaler/rxsqllibpaths) | Déterminez le chemin d’accès à la bibliothèque d’instances sur SQL Server distant. |
| [rxFindPackage](/machine-learning-server/r-reference/revoscaler/rxfindpackage) | Obtient le chemin d’accès pour un ou plusieurs packages sur SQL Server distant. |
| [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) | Appelez cette fonction à partir d’un client R distant pour installer des packages dans un contexte de calcul SQL Server à partir d’un référentiel spécifié ou en lisant des packages zippés enregistrés localement. Cette fonction vérifie les dépendances et garantit la possibilité d’installation de tout package connexe sur SQL Server, exactement comme l’installation de packages R dans un contexte de calcul local. Pour utiliser cette option, vous devez activer la gestion des packages sur le serveur et la base de données. Les environnements client et serveur doivent avoir tous les deux la même version de RevoScaleR. |
| [rxInstalledPackages](/machine-learning-server/r-reference/revoscaler/rxinstalledpackages) | Obtient une liste des packages installés dans le contexte de calcul spécifié. |
| [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) | Copiez des informations sur une bibliothèque de packages entre le système de fichiers et la base de données, pour le contexte de calcul spécifié. |
| [rxRemovePackages](/machine-learning-server/r-reference/revoscaler/rxremovepackages) | Supprime des packages d’un contexte de calcul spécifié. Il calcul également les dépendances et assure que les packages qui ne sont plus utilisés par d’autres packages sur SQL Server sont supprimés pour libérer des ressources. |

## <a name="prerequisites"></a>Conditions préalables requises

+ La gestion à distance active SQL Server. Pour plus d’informations, consultez [Activer la gestion des packages R distants sur SQL Server](r-package-how-to-enable-or-disable.md).

+ Les versions RevoScaleR sont les mêmes sur les environnements client et serveur. Pour plus d’informations, consultez la section [Obtenir des informations sur le package R](../package-management/r-package-information.md).

+ Vous avez l’autorisation de vous connecter au serveur et à une base de données et d’exécuter les commandes R. Vous devez être membre d’un rôle de base de données qui vous autorise à installer des packages sur une instance et une base de données spécifiées.

  + Les packages de l’**étendue partagée** peuvent être installés par les utilisateurs appartenant au rôle `rpkgs-shared` dans une base de données spécifiée. Tous les utilisateurs de ce rôle peuvent désinstaller des packages partagés.

  + Des packages dans l’**étendue privée** peuvent être installés par tout utilisateur appartenant au rôle `rpkgs-private` dans une base de données. Néanmoins, les utilisateurs peuvent consulter et désinstaller uniquement leurs propres packages.

  + Les propriétaires des bases de données peuvent utiliser des packages partagés ou privés.

## <a name="client-connections"></a>Connexions clientes

Une station de travail client peut être [Microsoft R Client](/machine-learning-server/r-client/install-on-windows) ou un [Microsoft Machine Learning Server](/machine-learning-server/install/machine-learning-server-windows-install) (des scientifiques des données utilisent souvent l’édition de développeur gratuit) sur le même réseau.

Lors de l’appel de fonctions de gestion de packages à partir d’un client R distant, vous devez créer un objet de contexte de calcul, à l’aide de la fonction [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver). Par la suite, pour chaque fonction de gestion de packages que vous utilisez, transmettez le contexte de calcul en tant qu’argument.

L’identité de l’utilisateur est généralement spécifiée lors du paramétrage du contexte de calcul. Si vous ne spécifiez pas de nom d’utilisateur ni de mot de passe lorsque vous créez le contexte de calcul, l’identité de l’utilisateur exécutant le code R est utilisé.

1. À partir d’une ligne de commande, définissez une chaîne de connexion à l’instance et à la base de données.
2. Utilisez le constructeur [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver) pour définir un contexte de calcul SQL Server à l’aide de la chaîne de connexion.

    ```R
    sqlcc <- RxInSqlServer(connectionString = myConnString, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```
3. Créez une liste des packages que vous souhaitez installer et enregistrez la liste dans une variable de chaîne.

    ```R
    packageList <- c("e1071", "mice")
    ```

4. Appelez [rxInstallPackages ](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) et transmettez le contexte de calcul et la variable de chaîne contenant les noms des packages.

    ```R
    rxInstallPackages(pkgs = packageList, verbose = TRUE, computeContext = sqlcc)
    ```

    Si des packages dépendants sont requis, ils sont également installés, en supposant qu’une connexion Internet est disponible sur le client.
    
    Les packages sont installés à l’aide des informations d’identification de l’utilisateur qui effectue la connexion, dans l’étendue par défaut pour cet utilisateur.

## <a name="call-package-management-functions-in-stored-procedures"></a>Appeler des fonctions de gestion des packages dans des procédures stockées

Vous pouvez exécuter des fonctions de gestion des packages dans `sp_execute_external_script`. Dans ce cas, la fonction est exécutée à l’aide du contexte de sécurité de l’appelant de la procédure stockée.

## <a name="examples"></a>Exemples

Cette section fournit des exemples d’utilisation de ces fonctions à partir d’un client distant lors de la connexion à une instance ou une base de données en tant que contexte de calcul.

Pour tous les exemples, vous devez fournir une chaîne de connexion ou un contexte de calcul qui requiert une chaîne de connexion. Cet exemple fournit un moyen de créer un contexte de calcul pour SQL Server :

```R
instance_name <- "computer-name/instance-name";
database_name <- "TestDB";
sqlWait= TRUE;
sqlConsoleOutput <- TRUE;
connString <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
sqlcc <- RxInSqlServer(connectionString = connString, wait = sqlWait, consoleOutput = sqlConsoleOutput, numTasks = 4);
```

Selon l’emplacement du serveur et le modèle de sécurité, vous devrez peut-être fournir un domaine et une spécification de sous-réseau dans la chaîne de connexion ou utiliser une connexion SQL. Par exemple :

```R
connStr <- "Driver=SQL Server;Server=myserver.financeweb.contoso.com;Database=Finance;Uid=RUser1;Pwd=RUserPassword"
```

### <a name="get-package-path-on-a-remote-sql-server-compute-context"></a>Obtenir un chemin d’accès au package sur un contexte de calcul SQL Server distant

Cet exemple obtient le chemin d’accès au package **RevoScaleR** sur le contexte de calcul `sqlcc`.

```R
sqlPackagePaths <- rxFindPackage(package = "RevoScaleR", computeContext = sqlcc)
print(sqlPackagePaths)
```

**Résultats**

« C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library/RevoScaleR »

> [!TIP]
> Si vous avez activé l’option pour afficher la sortie de la console SQL, vous pouvez obtenir des messages d’état à partir de la fonction qui précède l’instruction `print`. Une fois que vous avez terminé de tester votre code, définissez `consoleOutput` sur FALSE dans le constructeur de contexte de calcul pour éliminer les messages.

### <a name="get-locations-for-multiple-packages"></a>Obtenir les emplacements de plusieurs packages

L’exemple suivant obtient les chemins d’accès des packages **RevoScaleR** et **lattice** sur le contexte de calcul `sqlcc`. Lors de la recherche d’informations sur plusieurs packages, transmettez un vecteur de chaînes contenant les noms des packages.

```R
packagePaths <- rxFindPackage(package = c("RevoScaleR", "lattice"), computeContext = sqlcc)
print(packagePaths)
```

### <a name="get-package-versions-on-a-remote-compute-context"></a>Obtenir des versions de package sur un contexte de calcul distant

Exécutez cette commande à partir d’une console R pour obtenir les numéros de build et de version des packages installés sur le contexte de calcul, *sqlServer*.

```R
sqlPackages <- rxInstalledPackages(fields = c("Package", "Version", "Built"), computeContext = sqlServer)
```

### <a name="install-a-package-on-sql-server"></a>Installer un package sur SQL Server

Cet exemple installe le package **prévisionnel** et ses dépendances dans le contexte de calcul.

  ```R
  pkgs <- c("forecast")
  rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="remove-a-package-from-sql-server"></a>Supprimer un package de SQL Server

Cet exemple supprime le package **prévisionnel** et ses dépendances du contexte de calcul.

  ```R
  pkgs <- c("forecast")
  rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="synchronize-packages-between-database-and-file-system"></a>Synchroniser des packages entre la base de données et le système de fichiers

L’exemple suivant vérifie la base de données **TestDB** et détermine si tous les packages sont installés dans le système de fichiers. Si certains packages sont manquants, ils sont installés dans le système de fichiers.

```R
# Instantiate the compute context
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )

# Synchronize the packages in the file system for all scopes and users
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

La synchronisation des packages fonctionne pour chaque base de données et par utilisateur. Pour plus d’informations, consultez [Synchronisation des packages R pour SQL Server](package-install-uninstall-and-sync.md).

### <a name="use-a-stored-procedure-to-list-packages-in-sql-server"></a>Utilisez une procédure stockée pour répertorier les packages dans SQL Server

Exécutez cette commande à partir de Management Studio ou d’un autre outil qui prend en charge T-SQL, pour obtenir la liste des packages installés sur l’instance actuelle, à l’aide de `rxInstalledPackages` dans une procédure stockée.

```sql
EXEC sp_execute_external_script 
  @language=N'R', 
  @script=N'
    myPackages <- rxInstalledPackages();
    OutputDataSet <- as.data.frame(myPackages);
    '
```

La fonction `rxSqlLibPaths` peut être utilisée pour déterminer la bibliothèque active utilisée par SQL Server Machine Learning Services. Ce script peut retourner uniquement le chemin d’accès de la bibliothèque pour le serveur actuel. 

```sql
declare @instance_name nvarchar(100) = @@SERVERNAME, @database_name nvarchar(128) = db_name();
exec sp_execute_external_script 
  @language = N'R',
  @script = N'
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    .libPaths(rxSqlLibPaths(connStr));
    print(.libPaths());
  ', 
  @input_data_1 = N'', 
  @params = N'@instance_name nvarchar(100), @database_name nvarchar(128)',
  @instance_name = @instance_name, 
  @database_name = @database_name;
```

## <a name="see-also"></a>Voir aussi

+ [Activer la gestion de packages R à distance](r-package-how-to-enable-or-disable.md)
+ [Synchroniser des packages R](package-install-uninstall-and-sync.md)
+ [Conseils pour l’utilisation de packages R](tips-for-using-r-packages.md)
+ [Obtenir des informations sur les packages R](../package-management/r-package-information.md)