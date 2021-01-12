---
description: ALTER PARTITION SCHEME (Transact-SQL)
title: ALTER PARTITION SCHEME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER PARTITION SCHEME
- ALTER_PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PARTITION SCHEME statement
- partition schemes [SQL Server], modifying
- modifying partition schemes
- adding filegroups
- NEXT USED filegroups
ms.assetid: f01d6880-9800-4cfb-8d11-d4be21efc8ca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 084263fab973e11c1c0a14e340f18ae952ca5f44
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094734"
---
# <a name="alter-partition-scheme-transact-sql"></a>ALTER PARTITION SCHEME (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Ajoute un groupe de fichiers au schéma d'une partition ou modifie la désignation du groupe de fichiers NEXT USED pour le schéma de la partition. 

>[!NOTE]
>Dans Azure SQL Database, seuls les groupes de fichiers primaires sont pris en charge.  
  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien d’article") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER PARTITION SCHEME partition_scheme_name   
NEXT USED [ filegroup_name ] [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *partition_scheme_name*  
 Nom du schéma de partition à modifier.  
  
 *filegroup_name*  
 Spécifie le groupe de fichiers que le schéma de partition doit marquer comme NEXT USED. Cela signifie que le groupe de fichiers accepte une nouvelle partition créée à l’aide d’une instruction [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md).  
  
 Dans un schéma de partition, un seul groupe de fichiers peut être désigné comme NEXT USED. Un groupe de fichiers non vide peut être spécifié. Si l’argument *filegroup_name* est spécifié tandis qu’aucun groupe de fichiers n’est marqué comme NEXT USED, *filegroup_name* est marqué comme NEXT USED. Si *filegroup_name* est spécifié et qu’il existe déjà un groupe de fichiers marqué avec la propriété NEXT USED, cette propriété est transférée du groupe de fichiers existant au groupe *filegroup_name*.  
  
 Si *filegroup_name* n’est pas spécifié et qu’il existe déjà un groupe de fichiers marqué avec la propriété NEXT USED, ce groupe de fichiers perd son état NEXT USED de façon à qu’il n’y ait plus de groupes de fichiers NEXT USED dans *partition_scheme_name*.  
  
 Si *filegroup_name* n’est pas spécifié et si aucun groupe de fichiers n’est marqué comme NEXT USED, ALTER PARTITION SCHEME retourne un avertissement.  
  
## <a name="remarks"></a>Remarques  
 Tout groupe de fichiers affecté par ALTER PARTITION SCHEME doit être en ligne.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations suivantes peuvent être utilisées pour exécuter ALTER PARTITION SCHEME :  
  
-   Autorisation ALTER ANY DATASPACE. Cette autorisation est attribuée par défaut aux membres du rôle de serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_ddladmin** .  
  
-   Autorisation CONTROL ou ALTER sur la base de données dans laquelle le schéma de partition a été créé.  
  
-   Autorisation CONTROL SERVER ou ALTER ANY DATABASE sur le serveur de la base de données dans laquelle le schéma de partition a été créé.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant suppose que le schéma de partition `MyRangePS1` et le groupe de fichiers `test5fg` existent dans la base de données active.  
  
```sql  
ALTER PARTITION SCHEME MyRangePS1  
NEXT USED test5fg;  
```  
  
 Le groupe de fichiers `test5fg` recevra toute partition supplémentaire d'une table ou d'un index partitionné à la suite d'une instruction ALTER PARTITION FUNCTION.  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md)   
 [DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
