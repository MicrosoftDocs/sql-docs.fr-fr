---
title: Restaurer une base de données et la lier à un pool de ressources | Microsoft Docs
description: Découvrez la restauration d'une base de données avec des tables optimisées en mémoire dans SQL Server. Suivez les meilleures pratiques en liant la base de données à un pool de ressources nommé.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 0d20a569-8a27-409c-bcab-0effefb48013
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4f5035a7dd7818e14ec594d04edabad138242527
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89546926"
---
# <a name="restore-a-database-and-bind-it-to-a-resource-pool"></a>Restaurer une base de données et la lier à un pool de ressources
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Même si vous disposez de suffisamment de mémoire pour restaurer une base de données avec des tables optimisées en mémoire, suivez les meilleures pratiques et liez la base de données à un pool de ressources nommé. Comme la base de données doit exister pour que vous puissiez la lier au pool, la restauration de votre base de données s'effectue en plusieurs étapes. Cette rubrique vous guide pas à pas dans ce processus.  
  
## <a name="restoring-a-database-with-memory-optimized-tables"></a>Restauration d'une base de données avec des tables optimisées en mémoire  
 Les étapes suivantes permettent de restaurer complètement la base de données IMOLTP_DB et de la lier à Pool_IMOLTP.  
  
1.  [Effectuer la restauration avec NORECOVERY](../../relational-databases/in-memory-oltp/restore-a-database-and-bind-it-to-a-resource-pool.md#bkmk_NORECOVERY)  
  
2.  [Créer le pool de ressources](../../relational-databases/in-memory-oltp/restore-a-database-and-bind-it-to-a-resource-pool.md#bkmk_createPool)  
  
3.  [Lier la base de données et le pool de ressources](../../relational-databases/in-memory-oltp/restore-a-database-and-bind-it-to-a-resource-pool.md#bkmk_bind)  
  
4.  [Effectuer la restauration avec RECOVERY](../../relational-databases/in-memory-oltp/restore-a-database-and-bind-it-to-a-resource-pool.md#bkmk_RECOVERY)  
  
5.  [Surveillance des performances des pools de ressources](../../relational-databases/in-memory-oltp/restore-a-database-and-bind-it-to-a-resource-pool.md#bkmk_Monitor)  
  
###  <a name="restore-with-norecovery"></a><a name="bkmk_NORECOVERY"></a> Effectuer la restauration avec NORECOVERY  
 Lorsque vous restaurez une base de données avec l'option NORECOVERY, la base de données est créée et l'image disque est restaurée sans consommer de mémoire.  
  
```sql  
RESTORE DATABASE IMOLTP_DB   
   FROM DISK = 'C:\IMOLTP_test\IMOLTP_DB.bak'  
   WITH NORECOVERY  
```  
  
###  <a name="create-the-resource-pool"></a><a name="bkmk_createPool"></a> Créer le pool de ressources  
 Le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant crée un pool de ressources nommé Pool_IMOLTP avec la moitié de la mémoire disponible.  Une fois le pool créé, Resource Governor est reconfiguré afin d'inclure Pool_IMOLTP.  
  
```sql  
CREATE RESOURCE POOL Pool_IMOLTP WITH (MAX_MEMORY_PERCENT = 50);  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
###  <a name="bind-the-database-and-resource-pool"></a><a name="bkmk_bind"></a> Lier la base de données et le pool de ressources  
 Utilisez la fonction système `sp_xtp_bind_db_resource_pool` pour lier la base de données au pool de ressources. La fonction accepte deux paramètres : le nom de la base de données suivi du nom du pool de ressources.  
  
 Le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant définit une liaison de la base de données IMOLTP_DB au pool de ressources Pool_IMOLTP. La liaison ne prend pas effet tant que vous n'avez pas terminé l'étape suivante.  
  
```sql  
EXEC sp_xtp_bind_db_resource_pool 'IMOLTP_DB', 'Pool_IMOLTP'  
GO  
```  
  
###  <a name="restore-with-recovery"></a><a name="bkmk_RECOVERY"></a> Effectuer la restauration avec RECOVERY  
 Lorsque vous restaurez la base de données avec récupération, la base de données est mise en ligne et toutes les données sont restaurées.  
  
```sql  
RESTORE DATABASE IMOLTP_DB   
   WITH RECOVERY  
```  
  
###  <a name="monitor-the-resource-pool-performance"></a><a name="bkmk_Monitor"></a> Surveillance des performances des pools de ressources  
 Une fois que la base de données est liée au pool de ressources nommé et qu'elle est restaurée avec récupération, surveillez l'objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Statistiques des pools de ressources. Pour plus d'informations consultez [SQL Server, objet Statistiques des pools de ressources](../../relational-databases/performance-monitor/sql-server-resource-pool-stats-object.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Lier une base de données avec des tables mémoire optimisées à un pool de ressources](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)   
 [sys.sp_xtp_bind_db_resource_pool &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-bind-db-resource-pool-transact-sql.md)   
 [SQLServer, objet Statistiques des pools de ressources](../../relational-databases/performance-monitor/sql-server-resource-pool-stats-object.md)   
 [sys.dm_resource_governor_resource_pools](../../relational-databases/system-stored-procedures/sys-sp-xtp-unbind-db-resource-pool-transact-sql.md)  
  
  
