---
title: Afficher les informations sur l’espace occupé par les données et par le journal d’une base de données
description: Découvrez comment afficher les informations relatives aux données et à l’espace du journal pour une base de données dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], space
- status information [SQL Server], space
- displaying space information
- disk space [SQL Server], displaying
- databases [SQL Server], space used
- viewing space information
- space allocation [SQL Server], displaying
- data space [SQL Server]
ms.assetid: c7b99463-4bab-4e9b-9217-fcb0898dc757
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 4f00c45e482e7a985b19b7fb4084407677d0908b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478400"
---
# <a name="display-data-and-log-space-information-for-a-database"></a>Afficher les informations sur l'espace occupé par les données et par le journal d'une base de données
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Cette rubrique explique comment afficher des informations sur l'espace occupé par les données et par le journal d'une base de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 L’autorisation d’exécuter **sp_spaceused** est accordée au rôle **public** . Seuls les membres du rôle de base de données fixe **db_owner** peuvent spécifier la paramètre **\@updateusage**.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-display-data-and-log-space-information-for-a-database"></a>Pour afficher les informations sur l'espace occupé par les données et par le journal d'une base de données  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et développez-la.  
  
2.  Développez **Bases de données**.  
  
3.  Cliquez avec le bouton droit sur une base de données, pointez sur **Rapports**, sur **Rapports standard**, puis cliquez sur **Utilisation du disque**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-display-data-and-log-space-information-for-a-database-by-using-sp_spaceused"></a>Pour afficher les informations sur l'espace occupé par les données et par le journal d'une base de données à l'aide de sp_spaceused  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple utilise la procédure stockée système [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) pour communiquer des informations sur l’espace disque pour la table `Vendor` et ses index.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused N'Purchasing.Vendor';  
GO  
```  
  
#### <a name="to-display-data-and-log-space-information-for-a-database-by-querying-sysdatabase_files"></a>Pour afficher les informations sur l'espace occupé par les données et par le journal d'une base de données en interrogeant sys.database_files  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L’exemple suivant interroge l’affichage catalogue [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) pour retourner des informations spécifiques sur les fichiers de données et les fichiers journaux de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name, type_desc, physical_name, size, max_size  
FROM sys.database_files ;  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [Ajouter des fichiers de données ou journaux à une base de données](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)   
 [Supprimer des fichiers de données ou des fichiers journaux d’une base de données](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)  
  
  
