---
title: Afficher ou modifier les propriétés d’une base de données | Microsoft Docs
description: Découvrez comment afficher ou modifier les propriétés d’une base de données dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- displaying databases
- database viewing [SQL Server]
- databases [SQL Server], viewing
- viewing databases
ms.assetid: 9e8ac097-84b7-46c7-85e3-c1e79f94d747
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c7e50c35f5804227bc9716cd359e466f543373ea
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048856"
---
# <a name="view-or-change-the-properties-of-a-database"></a>Afficher ou modifier les propriétés d'une base de données
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette rubrique explique comment afficher ou modifier les propriétés d'une base de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Lorsque vous modifiez une propriété de base de données, la modification prend effet immédiatement.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour afficher ou modifier les propriétés d'une base de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Quand AUTO_CLOSE a la valeur ON, certaines colonnes de l’affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) et la fonction DATABASEPROPERTYEX retournent la valeur NULL, car la base de données est inaccessible et qu’aucune donnée ne peut être extraite. Pour résoudre ce problème, ouvrez la base de données.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l’autorisation ALTER sur la base de données pour modifier les propriétés de cette dernière. Requiert au minimum le rôle de base de données Public pour afficher les propriétés d’une base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-or-change-the-properties-of-a-database"></a>Pour afficher ou modifier les propriétés d'une base de données  
  
1.  Dans **l’Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], puis développez-la.  
  
2.  Développez **Bases de données**, cliquez avec le bouton droit sur la base de données à afficher, puis cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés de la base de données** , sélectionnez une page pour afficher les informations correspondantes. Par exemple, sélectionnez la page **Fichiers** pour afficher les informations sur les fichiers journaux et les fichiers de données.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Transact-SQL fournit plusieurs méthodes pour afficher et modifier les propriétés d’une base de données. Pour afficher les propriétés d’une base de données, vous pouvez utiliser la fonction [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md) et l’affichage catalogue [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) . Pour changer les propriétés d’une base de données, vous pouvez utiliser la version de l’instruction ALTER DATABASE correspondant à votre environnement :  [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) ou [ALTER DATABASE (Azure SQL Database)](../../t-sql/statements/alter-database-transact-sql.md). Pour afficher les propriétés étendues à la base de données, utilisez l’affichage catalogue [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) et pour modifier les propriétés étendues à la base de données, utilisez l’instruction [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) .  
  
#### <a name="to-view-a-property-of-a-database-by-using-the-databasepropertyex-function"></a>Afficher une propriété d’une base de données à l’aide de la fonction DATABASEPROPERTYEX  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] puis à la base de données dont vous souhaitez afficher les propriétés.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple utilise la fonction système [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) pour retourner l’état de l’option de base de données AUTO_SHRINK dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Une valeur de retour de 1 signifie que l'option est activée (ON), et une valeur de retour de 0 signifie que l'option est désactivée (OFF).  
  
    ```sql  
    SELECT DATABASEPROPERTYEX('AdventureWorks2012', 'IsAutoShrink');  
    ```  
  
#### <a name="to-view-the-properties-of-a-database-by-querying-sysdatabases"></a>Pour afficher les propriétés d'une base de données en interrogeant sys.databases  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] puis à la base de données dont vous souhaitez afficher les propriétés.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple suivant interroge l'affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) pour afficher plusieurs propriétés de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Cet exemple renvoie le numéro d'ID de base de données (`database_id`), indique si la base de données est en lecture seule ou en lecture-écriture (`is_read_only`), et renvoie le classement de la base de données (`collation_name`) et le niveau de compatibilité de la base de données (`compatibility_level`).  
  
    ```sql  
    SELECT database_id, is_read_only, collation_name, compatibility_level  
    FROM sys.databases WHERE name = 'AdventureWorks2012';  
    ```  
  
#### <a name="to-view-the-properties-of-a-database-scoped-configuration-by-querying-sysdatabases_scoped_configuration"></a>Afficher les propriétés d’une configuration étendue à la base de données en interrogeant sys.databases_scoped_configuration  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] puis à la base de données dont vous souhaitez afficher les propriétés.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple suivant interroge l'affichage catalogue [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) pour afficher plusieurs propriétés de la base de données actuelle.  
  
    ```sql  
    SELECT configuration_id, name, value, value_for_secondary  
    FROM sys.database_scoped_configurations;  
    ```  
  
     Pour plus d’exemples, consultez [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)  
  
#### <a name="to-change-the-properties-of-a-sql-server-2016-database-using-alter-database"></a>Modifier les propriétés d’une base de données SQL Server 2016 à l’aide de ALTER DATABASE  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête. L'exemple détermine l'état de l'isolement d'instantané sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , modifie l'état de la propriété, puis vérifie la modification.  
  
     Pour déterminer l'état de l'isolement d'instantané, sélectionnez la première instruction `SELECT` et cliquez sur **Exécuter**.  
  
     Pour modifier l'état de l'isolement d'instantané, sélectionnez l'instruction `ALTER DATABASE` , puis cliquez sur **Exécuter**.  
  
     Pour vérifier la modification, sélectionnez la deuxième instruction `SELECT` , puis cliquez sur **Exécuter**.  
  
     [!code-sql[DatabaseDDL#AlterDatabase9](../../relational-databases/databases/codesnippet/tsql/view-or-change-the-prope_1.sql)]  
  
#### <a name="to-change-the-database-scoped-properties-using-alter-database-scoped-configuration"></a>Modifier les propriétés étendues à la base de données à l’aide de ALTER DATABASE SCOPED CONFIGURATION  
  
1.  Connectez-vous à une base de données dans votre instance de SQL Server.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête. l’exemple suivant attribut au paramètre MAXDOP d’une base de données secondaire la valeur de la base de données primaire.  
  
    ```sql  
    ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = PRIMARY   
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER DATABASE (Azure SQL Database)](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
 [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)  

  
