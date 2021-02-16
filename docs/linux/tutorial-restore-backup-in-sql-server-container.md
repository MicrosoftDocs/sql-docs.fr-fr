---
title: Restaurer une base de données SQL Server dans Docker
description: Ce tutoriel montre comment restaurer une sauvegarde de base de données SQL Server dans un nouveau conteneur Docker Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 1bb137243e5a297baae55e72afeaca356e8f4d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347359"
---
# <a name="restore-a-sql-server-database-in-a-linux-docker-container"></a>Restaurer une base de données SQL Server dans un conteneur Docker Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Ce tutoriel montre comment déplacer et restaurer un fichier de sauvegarde SQL Server dans une image conteneur SQL Server 2017 Linux exécutée sur Docker.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Ce tutoriel montre comment déplacer et restaurer un fichier de sauvegarde SQL Server dans une image conteneur SQL Server 2019 Linux exécutée sur Docker.

::: moniker-end

> [!div class="checklist"]
> * Extrayez et exécutez l’image conteneur Linux SQL Server la plus récente.
> * Copiez le fichier de base de données Wide World Importers dans le conteneur.
> * Restaurez la base de données dans le conteneur.
> * Exécutez les instructions Transact-SQL pour afficher et modifier la base de données.
> * Sauvegardez la base de données modifiée.

## <a name="prerequisites"></a>Conditions préalables requises

* Docker Engine 1.8+ sur n’importe quelle distribution Linux prise en charge ou Docker pour Mac/Windows. Pour plus d’informations, consultez [Installer Docker](https://docs.docker.com/engine/installation/).
* Au moins 2 Go d’espace disque
* Au moins 2 Go de RAM
* [Configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

## <a name="pull-and-run-the-container-image"></a>Extraire et exécuter l’image conteneur

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Ouvrez un terminal bash sur Linux/Mac ou une session PowerShell avec élévation de privilèges sur Windows.

1. Extrayez l’image conteneur Linux de SQL Server 2017 à partir du Hub Docker.

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   > [!TIP]
   > Tout au long de ce tutoriel, des exemples de commandes Docker sont fournis pour l’interpréteur de commandes bash (Linux/Mac) et PowerShell (Windows).

1. Pour exécuter l’image conteneur avec Docker, vous pouvez utiliser la commande suivante :

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   Cette commande crée un conteneur SQL Server 2017 avec l’édition Développeur (par défaut). Le port SQL Server **1433** est exposé sur l’hôte en tant que port **1401**. Le paramètre `-v sql1data:/var/opt/mssql` facultatif crée un conteneur de volumes de données nommé **sql1ddata**. Il est utilisé pour rendre persistantes les données créées par SQL Server.

   > [!IMPORTANT]
   > Cet exemple utilise un conteneur de volume de données dans Docker. Si vous choisissez plutôt de mapper un répertoire hôte, notez que cette approche est assortie de limitations sur Docker pour Mac et Windows. Pour plus d’informations, consultez [Configurer des images de conteneur SQL Server sur Docker](./sql-server-linux-docker-container-configure.md#persist).

1. Pour afficher vos conteneurs Docker, utilisez la commande `docker ps`.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. Si la colonne **ÉTAT** affiche **En cours d’exécution**, SQL Server est en cours d’exécution dans le conteneur et écoute sur le port spécifié dans la colonne **PORTS**. Si la colonne **ÉTAT** pour votre conteneur SQL Server affiche **Quitté**, consultez la [section Résolution des problèmes dans le guide de configuration](./sql-server-linux-docker-container-troubleshooting.md).

  ```bash
  $ sudo docker ps -a

  CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
  941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
  ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Ouvrez un terminal bash sur Linux/Mac ou une session PowerShell avec élévation de privilèges sur Windows.

1. Tirez (pull) l’image conteneur Linux de SQL Server 2019 à partir du hub Docker.

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   > [!TIP]
   > Tout au long de ce tutoriel, des exemples de commandes Docker sont fournis pour l’interpréteur de commandes bash (Linux/Mac) et PowerShell (Windows).

1. Pour exécuter l’image conteneur avec Docker, vous pouvez utiliser la commande suivante :

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   Cette commande crée un conteneur SQL Server 2019 avec l’édition Développeur (par défaut). Le port SQL Server **1433** est exposé sur l’hôte en tant que port **1401**. Le paramètre `-v sql1data:/var/opt/mssql` facultatif crée un conteneur de volumes de données nommé **sql1ddata**. Il est utilisé pour rendre persistantes les données créées par SQL Server.

1. Pour afficher vos conteneurs Docker, utilisez la commande `docker ps`.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. Si la colonne **ÉTAT** affiche **En cours d’exécution**, SQL Server est en cours d’exécution dans le conteneur et écoute sur le port spécifié dans la colonne **PORTS**. Si la colonne **ÉTAT** pour votre conteneur SQL Server affiche **Quitté**, consultez la [section Résolution des problèmes dans le guide de configuration](./sql-server-linux-docker-container-troubleshooting.md).

   ```bash
   $ sudo docker ps -a

   CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
   941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
   ```

::: moniker-end

## <a name="change-the-sa-password"></a>Changer le mot de passe AS

[!INCLUDE [Change docker password](../includes/sql-server-linux-change-docker-password.md)]

## <a name="copy-a-backup-file-into-the-container"></a>Copier un fichier de sauvegarde dans le conteneur

Ce tutoriel utilise l’[exemple de base de données World Wide Importers](../samples/wide-world-importers-what-is.md). Procédez comme suit pour télécharger et copier le fichier de sauvegarde de la base de données Wide World Importers dans votre conteneur SQL Server.

1. Tout d’abord, utilisez **dockr exec** pour créer un dossier de sauvegarde. La commande suivante crée un répertoire **/var/opt/mssql/backup** dans le conteneur SQL Server.

   ```bash
   sudo docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

   ```PowerShell
   docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

1. Ensuite, téléchargez le fichier [WideWorldImporters-Full.bak](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0) sur votre ordinateur hôte. Les commandes suivantes permettent d’accéder au répertoire d’accueil/d’utilisateur et de télécharger le fichier de sauvegarde sous le nom **wwi.bak**.

   ```bash
   cd ~
   curl -L -o wwi.bak 'https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak'
   ```

   ```PowerShell
   curl -OutFile "wwi.bak" "https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak"
   ```

1. Utilisez **dockr cp** pour copier le fichier de sauvegarde dans le conteneur du répertoire **/var/opt/mssql/backup**.

   ```bash
   sudo docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

   ```PowerShell
   docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

## <a name="restore-the-database"></a>Restaurer la base de données

Le fichier de sauvegarde se trouve maintenant dans le conteneur. Avant de restaurer la sauvegarde, il est important de connaître les noms de fichiers logiques et les types de fichiers à l’intérieur de la sauvegarde. Les commandes Transact-SQL suivantes inspectent la sauvegarde et effectuent la restauration à l’aide de **sqlcmd** dans le conteneur.

> [!TIP]
> Ce tutoriel utilise **sqlcmd** à l’intérieur du conteneur, car ce dernier est fourni avec cet outil préinstallé. Toutefois, vous pouvez également exécuter des instructions Transact-SQL avec d’autres outils clients en dehors du conteneur, par exemple [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) ou [SQL Server Management Studio](sql-server-linux-manage-ssms.md). Pour vous connecter, utilisez le port hôte qui a été mappé au port 1433 dans le conteneur. Dans cet exemple, il s’agit de **localhost,1401** sur l’ordinateur hôte et de **Host_IP_Address,1401** à distance.

1. Exécutez **sqlcmd** à l’intérieur du conteneur pour répertorier les noms de fichiers logiques et les chemins d’accès à l’intérieur de la sauvegarde. Cette opération s’effectue à l’aide de l’instruction Transact-SQL **RESTORE FILELISTONLY**.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost \
      -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE FILELISTONLY FROM DISK = "/var/opt/mssql/backup/wwi.bak"' \
      | tr -s ' ' | cut -d ' ' -f 1-2
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost `
      -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/wwi.bak'"
   ```

   Vous devez obtenir une sortie similaire à la suivante :

   ```
   LogicalName   PhysicalName
   ------------------------------------------
   WWI_Primary   D:\Data\WideWorldImporters.mdf
   WWI_UserData   D:\Data\WideWorldImporters_UserData.ndf
   WWI_Log   E:\Log\WideWorldImporters.ldf
   WWI_InMemory_Data_1   D:\Data\WideWorldImporters_InMemory_Data_1
   ```

1. Appelez la commande **RESTORE DATABASE** pour restaurer la base de données à l’intérieur du conteneur. Spécifiez de nouveaux chemins d’accès pour chacun des fichiers de l’étape précédente.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE DATABASE WideWorldImporters FROM DISK = "/var/opt/mssql/backup/wwi.bak" WITH MOVE "WWI_Primary" TO "/var/opt/mssql/data/WideWorldImporters.mdf", MOVE "WWI_UserData" TO "/var/opt/mssql/data/WideWorldImporters_userdata.ndf", MOVE "WWI_Log" TO "/var/opt/mssql/data/WideWorldImporters.ldf", MOVE "WWI_InMemory_Data_1" TO "/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1"'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE DATABASE WideWorldImporters FROM DISK = '/var/opt/mssql/backup/wwi.bak' WITH MOVE 'WWI_Primary' TO '/var/opt/mssql/data/WideWorldImporters.mdf', MOVE 'WWI_UserData' TO '/var/opt/mssql/data/WideWorldImporters_userdata.ndf', MOVE 'WWI_Log' TO '/var/opt/mssql/data/WideWorldImporters.ldf', MOVE 'WWI_InMemory_Data_1' TO '/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1'"
   ```

   Vous devez obtenir une sortie similaire à la suivante :

   ```
   Processed 1464 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   Processed 33 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   Processed 3862 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Converting database 'WideWorldImporters' from version 852 to the current version 869.
   Database 'WideWorldImporters' running the upgrade step from version 852 to version 853.
   Database 'WideWorldImporters' running the upgrade step from version 853 to version 854.
   Database 'WideWorldImporters' running the upgrade step from version 854 to version 855.
   Database 'WideWorldImporters' running the upgrade step from version 855 to version 856.
   Database 'WideWorldImporters' running the upgrade step from version 856 to version 857.
   Database 'WideWorldImporters' running the upgrade step from version 857 to version 858.
   Database 'WideWorldImporters' running the upgrade step from version 858 to version 859.
   Database 'WideWorldImporters' running the upgrade step from version 859 to version 860.
   Database 'WideWorldImporters' running the upgrade step from version 860 to version 861.
   Database 'WideWorldImporters' running the upgrade step from version 861 to version 862.
   Database 'WideWorldImporters' running the upgrade step from version 862 to version 863.
   Database 'WideWorldImporters' running the upgrade step from version 863 to version 864.
   Database 'WideWorldImporters' running the upgrade step from version 864 to version 865.
   Database 'WideWorldImporters' running the upgrade step from version 865 to version 866.
   Database 'WideWorldImporters' running the upgrade step from version 866 to version 867.
   Database 'WideWorldImporters' running the upgrade step from version 867 to version 868.
   Database 'WideWorldImporters' running the upgrade step from version 868 to version 869.
   RESTORE DATABASE successfully processed 58455 pages in 18.069 seconds (25.273 MB/sec).
   ```

## <a name="verify-the-restored-database"></a>Vérifier la base de données restaurée

Exécutez la requête suivante pour afficher une liste de noms de base de données dans votre conteneur :

```bash
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
   -Q 'SELECT Name FROM sys.Databases'
```

```PowerShell
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
   -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
   -Q "SELECT Name FROM sys.Databases"
```

**Wideworldimporters** doit apparaître dans la liste des bases de données.

## <a name="make-a-change"></a>Apporter une modification

Les étapes suivantes apportent une modification dans la base de données.

1. Exécutez une requête pour afficher les 10 premiers éléments de la table **Warehouse.StockItems**.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID"
   ```

   Vous devez voir une liste de noms et d’identificateurs d’élément :

   ```
   StockItemID StockItemName
   ----------- -----------------
             1 USB missile launcher (Green)
             2 USB rocket launcher (Gray)
             3 Office cube periscope (Black)
             4 USB food flash drive - sushi roll
             5 USB food flash drive - hamburger
             6 USB food flash drive - hot dog
             7 USB food flash drive - pizza slice
             8 USB food flash drive - dim sum 10 drive variety pack
             9 USB food flash drive - banana
            10 USB food flash drive - chocolate bar
   ```

1. Mettez à jour la description du premier élément à l’aide de l’instruction **UPDATE** suivante :

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName="USB missile launcher (Dark Green)" WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName='USB missile launcher (Dark Green)' WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   Vous devez obtenir une sortie similaire à la suivante :

   ```
   (1 rows affected)
   StockItemID StockItemName
   ----------- ------------------------------------
             1 USB missile launcher (Dark Green)
   ```

## <a name="create-a-new-backup"></a>Créer une nouvelle sauvegarde

Une fois que vous avez restauré votre base de données dans un conteneur, vous pouvez également créer régulièrement des sauvegardes de base de données dans le conteneur exécuté. Les étapes suivent un modèle similaire aux étapes précédentes, mais en sens inverse.

1. Utilisez la commande **BACKUP DATABASE** Transact-SQL pour créer une sauvegarde de base de données dans le conteneur. Ce tutoriel crée un fichier de sauvegarde **wwi_2.bak** dans le répertoire **/var/opt/mssql/backup** créé précédemment.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   Vous devez obtenir une sortie similaire à la suivante :

   ```
   10 percent processed.
   20 percent processed.
   30 percent processed.
   40 percent processed.
   50 percent processed.
   60 percent processed.
   70 percent processed.
   Processed 1200 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   80 percent processed.
   Processed 3865 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Processed 938 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   100 percent processed.
   BACKUP DATABASE successfully processed 59099 pages in 25.056 seconds (18.427 MB/sec).
   ```

1. Ensuite, copiez le fichier de sauvegarde à partir du conteneur et sur votre ordinateur hôte.

   ```bash
   cd ~
   sudo docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls -l wwi*
   ```

   ```PowerShell
   cd ~
   docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls wwi*
   ```

## <a name="use-the-persisted-data"></a>Utiliser les données persistantes

Outre la réalisation de sauvegardes de base de données pour protéger vos données, vous pouvez également utiliser des conteneurs de volume de données. Le début de ce tutoriel a créé le conteneur **sql1** avec le paramètre `-v sql1data:/var/opt/mssql`. Le conteneur de volume de données **sql1data** persiste les données **/var/opt/mssql**, même après la suppression du conteneur. Les étapes suivantes suppriment complètement le conteneur **sql1**, puis créent un conteneur **sql2** avec les données persistantes.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Arrêtez le conteneur **sql1**.

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. Supprimez le conteneur. Cela ne supprime pas le conteneur de volume de données **sql1data** précédemment créé ni les données persistantes qu’il contient.

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. Créez un conteneur **sql2** et réutilisez le conteneur de volume de données **sql1data**.

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

1. La base de données Wide World Importers se trouve à présent dans le nouveau conteneur. Exécutez une requête pour vérifier la modification que vous avez apportée précédemment.

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > Le mot de passe AS n’est pas le mot de passe que vous avez spécifié pour le conteneur **sql2**, `MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`. Toutes les données SQL Server ont été restaurées à partir de **sql1**, notamment le mot de passe modifié précédemment dans le tutoriel. En effet, certaines options comme celle-ci sont ignorées en raison de la restauration des données dans /var/opt/mssql. C’est la raison pour laquelle le mot de passe `<YourNewStrong!Passw0rd>` est affiché ici.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Arrêtez le conteneur **sql1**.

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. Supprimez le conteneur. Cela ne supprime pas le conteneur de volume de données **sql1data** précédemment créé ni les données persistantes qu’il contient.

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. Créez un conteneur **sql2** et réutilisez le conteneur de volume de données **sql1data**.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
       --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
       --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

1. La base de données Wide World Importers se trouve à présent dans le nouveau conteneur. Exécutez une requête pour vérifier la modification que vous avez apportée précédemment.

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > Le mot de passe AS n’est pas le mot de passe que vous avez spécifié pour le conteneur **sql2**, `MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`. Toutes les données SQL Server ont été restaurées à partir de **sql1**, notamment le mot de passe modifié précédemment dans le tutoriel. En effet, certaines options comme celle-ci sont ignorées en raison de la restauration des données dans /var/opt/mssql. C’est la raison pour laquelle le mot de passe `<YourNewStrong!Passw0rd>` est affiché ici.

::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Dans ce tutoriel, vous avez appris à sauvegarder une base de données sur Windows et à la déplacer vers un serveur Linux exécutant SQL Server 2017. Vous avez appris à :

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Dans ce tutoriel, vous avez appris à sauvegarder une base de données sur Windows et à la déplacer vers un serveur Linux exécutant SQL Server 2019. Vous avez appris à :

::: moniker-end

> [!div class="checklist"]
> * Créer des images conteneurs SQL Server Linux.
> * Copier les sauvegardes de base de données SQL Server dans un conteneur.
> * Exécuter les instructions Transact-SQL à l’intérieur du conteneur avec **sqlcmd**.
> * Créer et extraire des fichiers de sauvegarde à partir d’un conteneur.
> * Utiliser des conteneurs de volume de données dans Docker pour rendre persistantes les données SQL Server.

Ensuite, consultez les autres scénarios de configuration et de résolution des problèmes Docker :

> [!div class="nextstepaction"]
>[Guide de configuration pour SQL Server 2017 sur Docker](./sql-server-linux-docker-container-deployment.md)