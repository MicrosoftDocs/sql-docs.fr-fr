---
title: Sauvegarder et restaurer des bases de données SQL Server sur Linux
description: En savoir plus sur la sauvegarde et la restauration des bases de données SQL Server sur Linux. Découvrez également comment effectuer une sauvegarde et une restauration avec SQL Server Management Studio (SSMS).
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 11/14/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: d30090fb-889f-466e-b793-5f284fccc4e6
ms.openlocfilehash: 92210c7d111f44a965383a8c2099068b2030227e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338284"
---
# <a name="backup-and-restore-sql-server-databases-on-linux"></a>Sauvegarder et restaurer des bases de données SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Vous pouvez effectuer des sauvegardes de bases de données à partir de SQL Server 2017 sur Linux avec de nombreuses options différentes. Sur un serveur Linux, vous pouvez utiliser **sqlcmd** pour vous connecter à SQL Server et effectuer des sauvegardes. À partir de Windows, vous pouvez vous connecter à SQL Server sur Linux et effectuer des sauvegardes avec l’interface utilisateur. La fonctionnalité de sauvegarde est la même sur toutes les plateformes. Par exemple, vous pouvez sauvegarder les bases de données localement, sur des lecteurs distants ou sur le [service de stockage blob Microsoft Azure](../relational-databases/backup-restore/sql-server-backup-to-url.md).

> [!IMPORTANT]
> SQL Server sur Linux prend en charge seulement la sauvegarde vers Stockage Blob Azure avec des objets blob de blocs. L’utilisation d’une clé de stockage pour la sauvegarde et la restauration entraîne l’utilisation d’un objet blob de pages, ce qui n’est pas pris en charge. Utilisez à la place une signature d’accès partagé. Pour plus d’informations sur les objets blob de blocs et les objets blob de pages, consultez [Sauvegarde sur un objet blob de blocs et sur un objet blob de pages](../relational-databases/backup-restore/sql-server-backup-to-url.md#blockbloborpageblob).

## <a name="backup-a-database"></a>Sauvegarder une base de données

Dans l’exemple suivant, **sqlcmd** se connecte à l’instance locale et effectue une sauvegarde complète d'une base de données utilisateur appelée `demodb`.

```bash
sqlcmd -S localhost -U SA -Q "BACKUP DATABASE [demodb] TO DISK = N'/var/opt/mssql/data/demodb.bak' WITH NOFORMAT, NOINIT, NAME = 'demodb-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
```

Quand vous exécutez la commande, SQL Server vous invite à entrer un mot de passe. Une fois que vous avez entré le mot de passe, l’interpréteur de commandes retourne les résultats de la progression de la sauvegarde. Par exemple :

```
Password:
10 percent processed.
21 percent processed.
32 percent processed.
40 percent processed.
51 percent processed.
61 percent processed.
72 percent processed.
80 percent processed.
91 percent processed.
Processed 296 pages for database 'demodb', file 'demodb' on file 1.
100 percent processed.
Processed 2 pages for database 'demodb', file 'demodb_log' on file 1.
BACKUP DATABASE successfully processed 298 pages in 0.064 seconds (36.376 MB/sec).
```

### <a name="backup-the-transaction-log"></a>Sauvegarder le journal des transactions

Si votre base de données est en mode de récupération complète, vous pouvez également effectuer des sauvegardes de fichier journal pour des options de restauration plus granulaires. Dans l’exemple suivant, **sqlcmd** se connecte à l’instance locale et effectue une sauvegarde de fichier journal.

```bash
sqlcmd -S localhost -U SA -Q "BACKUP LOG [demodb] TO DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak' WITH NOFORMAT, NOINIT, NAME = N'demodb_LogBackup', NOSKIP, NOREWIND, NOUNLOAD, STATS = 5"
```

## <a name="restore-a-database"></a>Restaurer une base de données

Dans l’exemple suivant, **sqlcmd** se connecte à l’instance locale de SQL Server et restaure la base de données demobd. Notez que l'option `NORECOVERY` permet d’effectuer d’autres restaurations de sauvegardes de fichiers journaux. Si vous n’envisagez pas de restaurer des fichiers journaux supplémentaires, supprimez l’option `NORECOVERY`.

```bash
sqlcmd -S localhost -U SA -Q "RESTORE DATABASE [demodb] FROM DISK = N'/var/opt/mssql/data/demodb.bak' WITH FILE = 1, NOUNLOAD, REPLACE, NORECOVERY, STATS = 5"
```

> [!TIP]
> Si vous utilisez accidentellement NORECOVERY mais que vous n’avez pas de sauvegardes de fichiers journaux supplémentaires, exécutez la commande `RESTORE DATABASE demodb` sans paramètres supplémentaires. Cela termine la restauration et laisse votre base de données opérationnelle.

### <a name="restore-the-transaction-log"></a>Restaurer le journal des transactions

La commande suivante restaure la sauvegarde précédente de fichier journal.

```bash
sqlcmd -S localhost -U SA -Q "RESTORE LOG demodb FROM DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak'"
```

## <a name="backup-and-restore-with-sql-server-management-studio-ssms"></a>Sauvegarde et restauration avec SQL Server Management Studio (SSMS)

Vous pouvez utiliser SSMS à partir d’un ordinateur Windows pour vous connecter à une base de données Linux et effectuer une sauvegarde par le biais de l’interface utilisateur.

>[!NOTE] 
> Utilisez la dernière version de SSMS pour vous connecter à SQL Server. Pour télécharger et installer la dernière version, consultez [Télécharger SSMS](../ssms/download-sql-server-management-studio-ssms.md). Pour plus d’informations sur l’utilisation de SSMS, consultez [Utiliser SSMS pour gérer SQL Server sur Linux](sql-server-linux-manage-ssms.md).

Les étapes suivantes vous guident dans la réalisation d’une sauvegarde avec SSMS. 

1. Démarrez SSMS et connectez-vous à votre serveur dans SQL Server 2017 sur Linux.

1. Dans l’Explorateur d’objets, cliquez avec le bouton de droite sur votre base de données, cliquez sur **Tâches**, puis cliquez sur **Sauvegarder...**.

1. Dans la boîte de dialogue **Sauvegarder la base de données**, vérifiez les paramètres et les options, puis cliquez sur **OK**.
 
SQL Server termine la sauvegarde de la base de données.

### <a name="restore-with-sql-server-management-studio-ssms"></a>Restauration avec SQL Server Management Studio (SSMS) 

Les étapes suivantes vous guident tout au long de la restauration d’une base de données avec SSMS.

1. Cliquez avec le bouton de droite sur **Bases de données**, puis cliquez sur **Restaurer la base de données**. 

1. Sous **Source**, cliquez sur **Périphérique :**, puis cliquez sur les points de suspension (...).

1. Recherchez votre fichier de sauvegarde de bases de données, puis cliquez sur **OK**. 

1. Sous **Plan de restauration**, vérifiez le fichier de sauvegarde et les paramètres. Cliquez sur **OK**. 

1. SQL Server restaure la base de données. 

## <a name="see-also"></a>Voir aussi

* [Créer une sauvegarde complète de base de données (SQL Server)](../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
* [Sauvegarder un journal des transactions (SQL Server)](../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)
* [BACKUP (Transact-SQL)](../t-sql/statements/backup-transact-sql.md)
* [Sauvegarde SQL Server vers une URL](../relational-databases/backup-restore/sql-server-backup-to-url.md)
