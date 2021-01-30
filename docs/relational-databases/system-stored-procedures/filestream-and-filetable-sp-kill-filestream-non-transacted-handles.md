---
description: sp_kill_filestream_non_transacted_handles (Transact-SQL)
title: sp_kill_filestream_non_transacted_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_kill_filestream_non_transacted_handles_TSQL
- sp_kill_filestream_non_transacted_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sp_kill_filestream_non_transacted_handles
ms.assetid: 7188353e-ab29-49a0-8f25-7fb8ab122589
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dd3ba3d653b8f1626f89c7bb979580c666dd830a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165239"
---
# <a name="sp_kill_filestream_non_transacted_handles-transact-sql"></a>sp_kill_filestream_non_transacted_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ferme des descripteurs de fichiers non transactionnels aux données de FileTable.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
sp_kill_filestream_non_transacted_handles [[ @table_name = ] 'table_name', [[ @handle_id = ] @handle_id]]  
```  
  
## <a name="arguments"></a>Arguments  
 *table_name*  
 Nom de la table dans laquelle fermer des descripteurs non transactionnels.  
  
 Vous pouvez passer *table_name* sans *handle_id* pour fermer tous les descripteurs non transactionnels ouverts pour le filetable.  
  
 Vous pouvez passer NULL pour la valeur de *table_name* pour fermer tous les descripteurs non transactionnels ouverts pour tous les FileTables dans la base de données actuelle. La valeur par défaut est NULL.  
  
 *handle_id*  
 ID facultatif du descripteur individuel à fermer. Vous pouvez obtenir l' *handle_id* à partir de l’sys.dm_filestream_non_transacted_handles &#40;vue de gestion dynamique [&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md) . Chaque ID est unique dans une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous spécifiez *handle_id*, vous devez également fournir une valeur pour *table_name*.  
  
 Vous pouvez passer NULL pour la valeur de *handle_id* pour fermer tous les descripteurs non transactionnels ouverts pour le filetable spécifié par *table_name*. La valeur par défaut est NULL.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-set"></a>Jeu de résultats  
 Aucun.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 La *handle_id* requise par **sp_kill_filestream_non_transacted_handles** n’est pas liée à la session_id ou à l’unité de travail utilisée dans d’autres commandes **Kill** .  
  
 Pour plus d’informations, consultez [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md).  
  
## <a name="metadata"></a>Métadonnées  
 Pour plus d’informations sur les descripteurs de fichiers non transactionnels ouverts, interrogez la vue de gestion dynamique [sys.dm_filestream_non_transacted_handles &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Vous devez disposer de l’autorisation **View Database State** pour obtenir des descripteurs de fichiers à partir de la vue de gestion dynamique **sys.dm_FILESTREAM_non_transacted_handles** et pour exécuter **sp_kill_filestream_non_transacted_handles**.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants montrent comment appeler **sp_kill_filestream_non_transacted_handles** pour fermer les descripteurs de fichiers non transactionnels pour les données filetables.  
  
```sql  
-- Close all open handles in the current database.  
sp_kill_filestream_non_transacted_handles  
  
-- Close all open handles in myFileTable.  
sp_kill_filestream_non_transacted_handles @table_name = 'myFileTable'  
  
-- Close a specific handle in myFileTable.  
sp_kill_filestream_non_transacted_handles @table_name = 'myFileTable', @handle_id = 0xFFFAAADD  
```  
  
 L’exemple suivant montre comment utiliser un script pour obtenir un *handle_id* et le fermer.  
  
```sql  
DECLARE @handle_id varbinary(16);  
DECLARE @table_name sysname;  
  
SELECT TOP 1 @handle_id = handle_id, @table_name = Object_name(table_id)  
FROM sys.dm_FILESTREAM_non_transacted_handles;  
  
EXEC sp_kill_filestream_non_transacted_handles @dbname, @table_name, @handle_id;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md)  
 [Vues de gestion dynamique FileStream et filetable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
 <br>[Affichages catalogue FileStream et filetable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
 <br>[sp_filestream_force_garbage_collection (Transact-SQL)](filestream-and-filetable-sp-filestream-force-garbage-collection.md)
  
