---
description: sys.dm_db_persisted_sku_features (Transact-SQL)
title: sys.dm_db_persisted_sku_features (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_persisted_sku_features_TSQL
- sys.dm_db_persisted_sku_features
- dm_db_persisted_sku_features_TSQL
- dm_db_persisted_sku_features
dev_langs:
- TSQL
helpviewer_keywords:
- editions [SQL Server]
- sys.dm_db_persisted_sku_features dynamic management view
ms.assetid: b4b29e97-b523-41b9-9528-6d4e84b89e09
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 89fd845ade31b7ca2de9559452bb04b15bc2c505
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171431"
---
# <a name="sysdm_db_persisted_sku_features-transact-sql"></a>sys.dm_db_persisted_sku_features (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Certaines fonctionnalités du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] modifient la façon dont les [!INCLUDE[ssDE](../../includes/ssde-md.md)] informations sont stockées dans les fichiers de la base de données. Ces fonctionnalités sont limitées à des éditions spécifiques de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une base de données qui contient ces fonctionnalités ne peut pas être déplacée vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne les prend pas en charge. Utilisez la sys.dm_db_persisted_sku_features vue de gestion dynamique pour répertorier les fonctionnalités spécifiques à l’édition qui sont activées dans la base de données actuelle.
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures).
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|feature_name|**sysname**|Nom externe de la fonctionnalité qui est activée dans la base de données, mais qui n'est pas prise en charge dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette fonctionnalité doit être supprimée avant que la base de données puisse être migrée vers toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles.|  
|feature_id|**int**|ID de fonctionnalité associée à la fonctionnalité. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DATABASE STATE sur la base de données.  
  
## <a name="remarks"></a>Notes  
 Si aucune fonctionnalité pouvant être restreinte par une édition spécifique n’est utilisée par la base de données, la vue ne retourne aucune ligne.  
  
 sys.dm_db_persisted_sku_features pouvez répertorier les fonctionnalités de modification de base de données suivantes, limitées à des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] éditions spécifiques :  
  
-   **ChangeCapture**: indique qu’une capture de données modifiées est activée pour une base de données. Pour supprimer la capture de données modifiées, utilisez la procédure stockée [sys.sp_cdc_disable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) . Pour plus d’informations, consultez [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md).  
  
-   **ColumnStoreIndex**: indique qu’au moins une table a un index ColumnStore. Pour permettre le déplacement d’une base de données vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne prend pas en charge cette fonctionnalité, utilisez l’instruction [Drop index](../../t-sql/statements/drop-index-transact-sql.md) ou [ALTER index](../../t-sql/statements/alter-index-transact-sql.md) pour supprimer l’index ColumnStore. Pour plus d’informations, consultez [index ColumnStore](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
    **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures).  
  
-   **Compression**: indique qu’au moins une table ou un index utilise la compression de données ou le format de stockage vardecimal. Pour permettre le déplacement d’une base de données vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne prend pas en charge cette fonctionnalité, utilisez l’instruction [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [ALTER index](../../t-sql/statements/alter-index-transact-sql.md) pour supprimer la compression de données. Pour supprimer le format de stockage Vardecimal, exécutez l'instruction sp_tableoption. Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).  
  
-   **MultipleFSContainers**: indique que la base de données utilise plusieurs conteneurs FileStream. La base de données a un groupe de fichiers FILESTREAM avec plusieurs conteneurs (fichiers). Pour plus d’informations, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
-   **InMemoryOLTP**: indique que la base de données utilise In-Memory OLTP. La base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA. Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
  **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures). 
  
-   **Partitionnement.** Indique que la base de données contient des tables partitionnées, des index partitionnés, des schémas de partition ou des fonctions de partition. Pour permettre le déplacement d'une base de données vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autre que les éditons Enterprise et Developer, il est insuffisant de modifier la table pour qu'elle se trouve sur une partition unique. Vous devez supprimer la table partitionnée. Si la table contient des données, utilisez SWITCH PARTITION pour convertir chaque partition en une table non partitionnée. Supprimez ensuite la table partitionnée, le schéma de partition et la fonction de partition.  
  
-   **TransparentDataEncryption.** Indique qu'une base de données est chiffrée à l'aide du chiffrement transparent des données. Pour supprimer le chiffrement transparent des données, exécutez l'instruction ALTER DATABASE. Pour plus d’informations, consultez [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  

> [!NOTE]
> À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Service Pack 1, ces fonctionnalités, à l’exception de **TransparentDataEncryption.** sont disponibles dans plusieurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] éditions et ne sont pas limitées aux éditions Enterprise ou Developer.

 Pour déterminer si une base de données utilise des fonctionnalités limitées à des éditions spécifiques, exécutez l'instruction suivante dans la base de données :  
  
```sql  
SELECT feature_name FROM sys.dm_db_persisted_sku_features;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
