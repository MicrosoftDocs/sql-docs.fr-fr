---
title: 'Démarrage rapide : Sauvegarder et restaurer une base de données'
titleSuffix: SQL Server
description: Cet article décrit la création d’une base de données, la sauvegarde d’une base de données et la restauration de la sauvegarde dans SQL Server.
author: cawrites
ms.author: chadam
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.assetid: ''
ms.openlocfilehash: 66b4277674c1144a716e5153a9fa062f988c75c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340216"
---
# <a name="quickstart-backup-and-restore-a-sql-server-database-on-premises"></a>Démarrage rapide : Sauvegarder et restaurer une base de données SQL Server localement
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Dans ce guide démarrage rapide, vous allez créer une base de données, en effectuer une sauvegarde simple, puis la restaurer. 

Pour obtenir une procédure plus détaillée, consultez [Créer une sauvegarde de base de données complète](create-a-full-database-backup-sql-server.md) et [Restaurer une sauvegarde à l’aide de SSMS](restore-a-database-backup-using-ssms.md).

## <a name="prerequisites"></a>Prérequis
Pour suivre ce guide de démarrage rapide, vous avez besoin des éléments suivants : 

- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
- [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)

## <a name="create-a-test-database"></a>Créer une base de données de test 

1. Lancez [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) et connectez-vous à votre instance de SQL Server.
1. Ouvrez une fenêtre **Nouvelle requête**. 
1. Exécutez le code Transact-SQL (T-SQL) suivant pour créer votre base de données de test. Actualisez le nœud **Bases de données** dans l’**Explorateur d’objets** pour voir votre nouvelle base de données. 

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO


USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO
```
 
## <a name="take-a-backup"></a>Effectuer une sauvegarde
Pour effectuer une sauvegarde de votre base de données, effectuez les étapes suivantes : 

1. Lancez [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) et connectez-vous à votre instance de SQL Server.
1. Dans l’**Explorateur d’objets**, développez le nœud **Bases de données**.  
1. Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, puis sélectionnez **Sauvegarder**. 
1. Sous **Destination**, vérifiez que le chemin de votre sauvegarde est correcte. Si vous devez le changer, sélectionnez **Supprimer** pour supprimer le chemin existant, puis **Ajouter** pour taper un nouveau chemin. Vous pouvez utiliser les points de suspension pour accéder à un fichier spécifique. 
1. Sélectionnez **OK** pour créer une sauvegarde de votre base de données. 

![Effectuer la sauvegarde SQL](media/quickstart-backup-restore-database/backup-db-ssms.png)

Vous pouvez également exécuter la commande Transact-SQL suivante pour sauvegarder votre base de données : 

```sql
BACKUP DATABASE [SQLTestDB] 
TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' 
WITH NOFORMAT, NOINIT,  
NAME = N'SQLTestDB-Full Database Backup', SKIP, NOREWIND, NOUNLOAD,  STATS = 10
GO
```


## <a name="restore-a-backup"></a>Restaurer une sauvegarde
Pour restaurer votre base de données, effectuez les étapes suivantes : 

1. Lancez [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) et connectez-vous à votre instance de SQL Server.
1. Cliquez avec le bouton droit sur le nœud **Bases de données** dans l’**Explorateur d’objets** et sélectionnez **Restaurer la base de données**.

    ![Restaurer une base de données](media/quickstart-backup-restore-database/restore-db-ssms1.png)

1. Sélectionnez **Appareil :** , puis sélectionnez les points de suspension (...) pour localiser votre fichier de sauvegarde. 
1. Sélectionnez **Ajouter** et naviguez vers l’emplacement où se trouve votre fichier `.bak`. Sélectionnez le fichier `.bak`, puis **OK**. 
1. Sélectionnez **OK** pour fermer la boîte de dialogue **Sélectionner les unités de sauvegarde**. 
1. Sélectionnez **OK** pour restaurer la sauvegarde de votre base de données. 

    ![Restaurer la base de données](media/quickstart-backup-restore-database/restore-db-ssms2.png)

Vous pouvez également exécuter le script Transact-SQL suivant pour restaurer votre base de données :

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] 
FROM DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

### <a name="clean-up-resources"></a>Nettoyer les ressources
Exécutez la commande Transact-SQL suivante pour supprimer la base de données que vous avez créée, ainsi que son historique de sauvegarde dans la base de données MSDB :

```sql
EXEC msdb.dbo.sp_delete_database_backuphistory @database_name = N'SQLTestDB'
GO

USE [master]
DROP DATABASE [SQLTestDB]
GO
```

## <a name="see-more"></a>En savoir plus
[Vue d’ensemble de la sauvegarde et de la restauration](back-up-and-restore-of-sql-server-databases.md)
[Sauvegarder vers une URL](sql-server-backup-to-url.md)
[Créer une sauvegarde complète](create-a-full-database-backup-sql-server.md)
[Restaurer une sauvegarde de base de données](restore-a-database-backup-using-ssms.md)
