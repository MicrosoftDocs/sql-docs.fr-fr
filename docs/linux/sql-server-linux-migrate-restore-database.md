---
title: Migrer une base de données SQL Server de Windows à Linux
description: Ce didacticiel montre comment effectuer une sauvegarde de base de données SQL Server sur Windows et la restaurer sur une machine Linux exécutant SQL Server.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/16/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9ac64d1a-9fe5-446e-93c3-d17b8f55a28f
ms.openlocfilehash: 2ae765c8bc1f274ab9b9f62978a85b5f09f6ca58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349081"
---
# <a name="migrate-a-sql-server-database-from-windows-to-linux-using-backup-and-restore"></a>Migrez une base de données SQL Server de Windows vers Linux à l’aide de la sauvegarde et de la restauration

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

La fonctionnalité de sauvegarde et de restauration de SQL Server est la méthode recommandée pour migrer une base de données de SQL Server sur Windows vers SQL Server sur Linux. Dans ce didacticiel, vous allez parcourir les étapes requises pour déplacer une base de données vers Linux avec des techniques de sauvegarde et de restauration.

> [!div class="checklist"]
> * Créer un fichier de sauvegarde sur Windows avec SSMS
> * Installer un interpréteur de commandes Bash sur Windows
> * Déplacer le fichier de sauvegarde vers Linux à partir de l’interpréteur de commandes Bash
> * Restaurer le fichier de sauvegarde sur Linux avec Transact-SQL
> * Exécuter une requête pour vérifier la migration

Vous pouvez également créer un groupe de disponibilité Always On SQL Server pour migrer une base de données SQL Server de Windows vers Linux. Consultez [sql-server-linux-availability-group-cross-platform](sql-server-linux-availability-group-cross-platform.md).

## <a name="prerequisites"></a>Conditions préalables requises

Pour exécuter ce didacticiel, vous devez réunir les conditions préalables suivantes :

* Machine Windows avec les éléments suivants :
  * [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) installé.
  * [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) installé.
  * Base de données cible à migrer.

* Machine Linux avec les éléments suivants installés :
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) ou [Ubuntu](quickstart-install-connect-ubuntu.md)) avec les outils de ligne de commande.

## <a name="create-a-backup-on-windows"></a>Créer une sauvegarde sur Windows

Il existe plusieurs façons de créer un fichier de sauvegarde d’une base de données sur Windows. Les étapes suivantes utilisent SQL Server Management Studio (SSMS).

1. Démarrez **SQL Server Management Studio** sur votre machine Windows.

1. Dans la boîte de dialogue connexion, entrez **localhost**.

1. Dans l’Explorateur d’objets, développez **Bases de données**.

1. Cliquez avec le bouton de droite sur la base de données cible, sélectionnez **Tâches**, puis cliquez sur **Sauvegarder...** .

   ![Utiliser SSMS pour créer un fichier de sauvegarde](./media/sql-server-linux-migrate-restore-database/ssms-create-backup.png)

1. Dans la boîte de dialogue **Sauvegarder la base de données**, vérifiez que **Type de sauvegarde** est défini sur **Complet** et **Sauvegarder sur** sur **Disque**. Notez le nom et l’emplacement du fichier. Par exemple, une base de données nommée **YourDB** sur SQL Server 2016 a un chemin de sauvegarde par défaut de `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak`.

1. Cliquez sur **OK** pour sauvegarder votre base de données.

> [!NOTE]
> Une autre option consiste à exécuter une requête Transact-SQL pour créer le fichier de sauvegarde. La commande Transact-SQL suivante effectue les mêmes actions que les étapes précédentes pour une base de données appelée **YourDB** :
>
> ```sql
> BACKUP DATABASE [YourDB] TO  DISK =
> N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak'
> WITH NOFORMAT, NOINIT, NAME = N'YourDB-Full Database Backup',
> SKIP, NOREWIND, NOUNLOAD, STATS = 10
> GO
> ```

## <a name="install-a-bash-shell-on-windows"></a>Installer un interpréteur de commandes Bash sur Windows

Pour restaurer la base de données, vous devez d’abord transférer le fichier de sauvegarde de la machine Windows vers la machine Linux cible. Dans ce didacticiel, nous allons déplacer le fichier vers Linux à partir d’un interpréteur de commandes Bash (fenêtre du terminal) exécuté sur Windows.

1. Installez un interpréteur de commandes Bash sur votre machine Windows qui prend en charge les commandes **scp** (copier sécurisée) et **ssh** (ouverture de session à distance). Les deux exemples comprennent :

   * Le [Sous-système Windows pour Linux](/windows/wsl/about) (Windows 10)
   * L’interpréteur de commandes Git Bash ([https://git-scm.com/downloads](https://git-scm.com/downloads))

1. Ouvrez une session Bash sur Windows.

## <a name="copy-the-backup-file-to-linux"></a><a id="scp"></a> Copier le fichier de sauvegarde sur Linux

1. Dans votre session Bash, accédez au répertoire contenant votre fichier de sauvegarde. Par exemple :

   ```bash
   cd 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\'
   ```

1. Utilisez la commande **scp** pour transférer le fichier vers la machine Linux cible. L’exemple suivant transfère **YourDB.bak** dans le répertoire de départ de *utilisateur1* sur le serveur Linux avec l’adresse IP *192.0.2.9* :

   ```bash
   scp YourDB.bak user1@192.0.2.9:./
   ```
   ![commande scp](./media/sql-server-linux-migrate-restore-database/scp-command.png)

> [!TIP]
> Il existe des alternatives à l’utilisation de scp pour le transfert de fichiers. La première consiste à utiliser [Samba](https://help.ubuntu.com/community/Samba) pour configurer un partage réseau SMB entre Windows et Linux. Pour obtenir une procédure pas à pas sur Ubuntu, consultez [Comment créer un partage réseau via Samba](https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated,%20Simple%20and%20Brief%20Way!). Une fois la connexion établie, vous pouvez y accéder en tant que partage de fichiers réseau à partir de Windows, par exemple le **partage\\\\machinenameorip\\** .

## <a name="move-the-backup-file-before-restoring"></a>Déplacer le fichier de sauvegarde avant la restauration

À ce stade, le fichier de sauvegarde se trouve sur votre serveur Linux dans le répertoire de départ de votre utilisateur. Avant de restaurer la base de données à SQL Server, vous devez placer la sauvegarde dans un sous-répertoire de **/var/opt/mssql**.

1. Dans la même session Bash Windows, connectez-vous à distance à votre machine Linux cible avec **ssh**. L’exemple suivant se connecte à la machine Linux **192.0.2.9** en tant qu’utilisateur **utilisateur1**.

   ```bash
   ssh user1@192.0.2.9
   ```

   Vous exécutez maintenant des commandes sur le serveur Linux distant.

1. Entrez en mode super utilisateur.

   ```bash
   sudo su
   ```

1. Créez un nouveau répertoire de sauvegarde. Le paramètre -p ne fait rien si le répertoire existe déjà.

   ```bash
   mkdir -p /var/opt/mssql/backup
   ```

1. Déplacez le fichier de sauvegarde vers ce répertoire. Dans l’exemple suivant, le fichier de sauvegarde se trouve dans le répertoire de départ de *utilisateur1*. Modifiez la commande pour qu’elle corresponde à l’emplacement et au nom de fichier de votre fichier de sauvegarde.

   ```bash
   mv /home/user1/YourDB.bak /var/opt/mssql/backup/
   ```

1. Quittez le mode super utilisateur.

   ```bash
   exit
   ```

## <a name="restore-your-database-on-linux"></a>Restaurer votre base de données sur Linux

Pour restaurer la sauvegarde de la base de données, vous pouvez utiliser la commande **RESTORE DATABASE** Transact-SQL (TQL).

> [!NOTE]
> Les étapes suivantes utilisent l'outil **sqlcmd**. Si vous n’avez pas installé les outils SQL Server, consultez [Installer des outils en ligne de commande SQL Server sur Linux](sql-server-linux-setup-tools.md).

1. Dans le même terminal, lancez **sqlcmd**. L’exemple suivant se connecte à l’instance de SQL Server locale avec l’utilisateur **SA**. Entrez le mot de passe lorsque vous y êtes invité, ou spécifiez le mot de passe en ajoutant le paramètre **-P**.

   ```bash
   sqlcmd -S localhost -U SA
   ```

1. À l'invite `>1`, entrez la commande **RESTORE DATABASE** suivante, en appuyant sur ENTRÉE après chaque ligne (vous ne pouvez pas copier et coller la totalité de la commande sur plusieurs lignes à la fois). Remplacez toutes les occurrences de `YourDB` par le nom de votre base de données.

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```

   Vous recevez un message indiquant que la base de données a été restaurée correctement.

   `RESTORE DATABASE` peut retourner une erreur comme dans l’exemple suivant :

   ```bash
   File 'YourDB_Product' cannot be restored to 'Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf'. Use WITH MOVE to identify a valid location for the file.
   Msg 5133, Level 16, State 1, Server servername, Line 1
   Directory lookup for the file "Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf" failed with the operating system error 2(The system cannot find the file specified.).
   ```
   
   Dans ce cas, la base de données contient des fichiers secondaires. Si ces fichiers ne sont pas spécifiés dans la clause `MOVE` de `RESTORE DATABASE`, la procédure de restauration essaiera de les créer dans le même chemin d’accès que le serveur d’origine. 

   Vous pouvez répertorier tous les fichiers inclus dans la sauvegarde :
   ```sql
   RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   GO
   ```
   Vous devez obtenir une liste comme celle ci-dessous (en répertoriant uniquement les deux premières colonnes) :
   ```sql
   LogicalName         PhysicalName                                                                 ..............
   ----------------------------------------------------------------------------------------------------------------------
   YourDB              Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB.mdf          ..............
   YourDB_Product      Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf  ..............
   YourDB_Customer     Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Customer.ndf ..............
   YourDB_log          Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Log.ldf      ..............
   ```
   
   Vous pouvez utiliser cette liste pour créer des clauses `MOVE` pour les fichiers supplémentaires. Dans cet exemple, `RESTORE DATABASE` est :

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Product' TO '/var/opt/mssql/data/YourDB_Product.ndf',
   MOVE 'YourDB_Customer' TO '/var/opt/mssql/data/YourDB_Customer.ndf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```


1. Vérifiez la restauration en répertoriant toutes les bases de données sur le serveur. La base de données restaurée doit être répertoriée.

   ```sql
   SELECT Name FROM sys.Databases
   GO
   ```

1. Exécutez d’autres requêtes sur votre base de données migrée. La commande suivante bascule le contexte dans la base de données **YourDB** et sélectionne les lignes de l’une de ses tables.

   ```sql
   USE YourDB
   SELECT * FROM YourTable
   GO
   ```

1. Quand vous avez fini d'utiliser **sqlcmd**, tapez `exit`.

1. Une fois que vous avez fini de travailler dans la session **ssh** à distance, tapez à nouveau `exit`.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à sauvegarder une base de données sur Windows et à la déplacer vers un serveur Linux exécutant SQL Server. Vous avez appris à :
> [!div class="checklist"]
> * utiliser SSMS et Transact-SQL pour créer un fichier de sauvegarde sur Windows
> * Installer un interpréteur de commandes Bash sur Windows
> * utiliser **scp** pour déplacer des fichiers de sauvegarde de Windows vers Linux
> * utiliser **ssh** pour se connecter à distance à votre machine Linux
> * déplacer le fichier de sauvegarde pour préparer la restauration
> * utiliser **sqlcmd** pour exécuter les commandes Transact-SQL
> * restaurer la sauvegarde de base de données avec la commande **RESTORE DATABASE** 
> * exécuter une requête pour vérifier la migration

Explorez ensuite d’autres scénarios de migration pour SQL Server sur Linux. 

> [!div class="nextstepaction"]
>[Migrer des bases de données vers SQL Server sur Linux](sql-server-linux-migrate-overview.md)