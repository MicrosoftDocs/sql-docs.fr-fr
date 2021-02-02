---
description: Modifier le nom d'une base de données
title: Renommer une base de données | Microsoft Docs
ms.custom: ''
ms.date: 10/02/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], renaming
- renaming databases
ms.assetid: 44c69d35-abcb-4da3-9370-5e0bc9a28496
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e22b6ae7e10347933b55ad643f6c69c6370340cf
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813012"
---
# <a name="rename-a-database"></a>Modifier le nom d'une base de données

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment renommer une base de données définie par l’utilisateur dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ou Azure SQL Database avec [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le nom de la base de données peut contenir n'importe quel caractère conforme aux règles applicables aux identificateurs.  
  
## <a name="in-this-topic"></a>Dans cette rubrique
  
- Avant de commencer :  
  
     [Limitations et restrictions](#limitations-and-restrictions)  
  
     [Sécurité](#security)  
  
- Pour renommer une base de données, utilisez :  
  
     [SQL Server Management Studio](#rename-a-database-using-sql-server-management-studio)  
  
     [Transact-SQL](#rename-a-database-using-transact-sql)  
  
- **Follow Up:**  [After renaming a database](#backup-after-renaming-a-database)  

> [!NOTE]
> Pour renommer une base de données dans Azure Synapse Analytics ou Parallel Data Warehouse, utilisez l’instruction [RENAME (Transact-SQL)](../../t-sql/statements/rename-transact-sql.md).
  
## <a name="before-you-begin"></a>Avant de commencer
  
### <a name="limitations-and-restrictions"></a>Limitations et restrictions  
  
- Les bases de données système ne peuvent pas être renommées.
- Le nom de la base de données ne peut pas être modifié tant que d’autres utilisateurs accèdent à la base de données. 
  - Dans SQL Server, vous pouvez définir une base de données en mode mono-utilisateur pour fermer toutes les connexions ouvertes. Pour plus d’informations, consultez [Définir la base de données en mode mono-utilisateur](../../relational-databases/databases/set-a-database-to-single-user-mode.md).
  - Dans Azure SQL Database, vous devez vérifier qu’aucun autre utilisateur n’a une connexion ouverte à la base de données à renommer.
  
### <a name="security"></a>Sécurité  
  
#### <a name="permissions"></a>Autorisations

Nécessite l'autorisation ALTER sur la base de données.  
  
## <a name="rename-a-database-using-sql-server-management-studio"></a>Renommer une base de données avec SQL Server Management Studio

Utilisez les étapes suivantes pour renommer une base de données SQL Server ou Azure SQL avec SQL Server Management Studio.

  
1. Dans **l’Explorateur d’objets**, connectez-vous à votre instance SQL.  
  
2. Vérifiez qu’il n’y a aucune connexion ouverte à la base de données. Si vous utilisez SQL Server, vous pouvez [définir la base de données en mode mono-utilisateur](../../relational-databases/databases/set-a-database-to-single-user-mode.md) pour fermer toutes les connexions ouvertes et empêcher les autres utilisateurs de se connecter pendant que vous modifiez le nom de la base de données.  
  
3. Dans l’Explorateur d’objets, développez **Bases de données**, cliquez avec le bouton droit sur la base de données à renommer, puis cliquez sur **Renommer**.  
  
4. Entrez le nouveau nom de la base de données, puis cliquez sur **OK**.  
  
5. Si besoin, si la base de données était votre base de données par défaut, consultez [Réinitialiser votre base de données par défaut après l’avoir renommée](#reset-your-default-database-after-rename).

## <a name="rename-a-database-using-transact-sql"></a>Renommer une base de données avec Transact-SQL  
  
### <a name="to-rename-a-sql-server-database-by-placing-it-in-single-user-mode"></a>Pour renommer une base de données SQL Server en la plaçant en mode mono-utilisateur

Utilisez les étapes suivantes pour renommer une base de données SQL Server avec T-SQL dans SQL Server Management Studio, qui comprennent notamment : placer la base de données en mode mono-utilisateur et, après le renommage, replacer la base de données en mode multi-utilisateur.
  
1. Connectez-vous à la base de données `master` pour votre instance.  
2. Ouvrez une fenêtre de requête.  
3. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple modifie le nom de la base de données `MyTestDatabase` en `MyTestDatabaseCopy`.
  
   ```sql
   USE master;  
   GO  
   ALTER DATABASE MyTestDatabase SET SINGLE_USER WITH ROLLBACK IMMEDIATE
   GO
   ALTER DATABASE MyTestDatabase MODIFY NAME = MyTestDatabaseCopy ;
   GO  
   ALTER DATABASE MyTestDatabaseCopy SET MULTI_USER
   GO
   ```  

4. Si besoin, si la base de données était votre base de données par défaut, consultez [Réinitialiser votre base de données par défaut après l’avoir renommée](#reset-your-default-database-after-rename).

### <a name="to-rename-an-azure-sql-database-database"></a>Pour renommer une base de données Azure SQL Database

Utilisez les étapes suivantes pour renommer une base de données Azure SQL avec T-SQL dans SQL Server Management Studio.
  
1. Connectez-vous à la base de données `master` pour votre instance.  
2. Ouvrez une fenêtre de requête.
3. Vérifiez que personne n’utilise la base de données.
4. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple modifie le nom de la base de données `MyTestDatabase` en `MyTestDatabaseCopy`.
  
   ```sql
   ALTER DATABASE MyTestDatabase MODIFY NAME = MyTestDatabaseCopy ;
   ```  

## <a name="backup-after-renaming-a-database"></a>Sauvegarde après le renommage d’une base de données  

Après avoir renommé une base de données dans SQL Server, sauvegardez la base de données `master`. Dans Azure SQL Database, ceci n’est pas nécessaire, car les sauvegardes sont effectuées automatiquement.  
  
## <a name="reset-your-default-database-after-rename"></a>Réinitialiser votre base de données par défaut après l’avoir renommée

Si la base de données que vous renommez était définie en tant que base de données par défaut, utilisez la commande suivante pour la réinitialiser :


```sql
USE [master]
GO
ALTER LOGIN [your-login] WITH DEFAULT_DATABASE=[new-database-name]
GO
```


## <a name="see-also"></a>Voir aussi

- [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)
- [Identificateurs de base de données](../../relational-databases/databases/database-identifiers.md)  
