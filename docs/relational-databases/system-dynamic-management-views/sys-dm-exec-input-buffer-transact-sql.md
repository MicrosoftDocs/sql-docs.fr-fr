---
description: sys.dm_exec_input_buffer (Transact-SQL)
title: sys.dm_exec_input_buffer (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_input_buffer
- sys.dm_exec_input_buffer _tsql
- dm_exec_input_buffer
- dm_exec_input_buffer_tsql
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_input_buffer dynamic management function
ms.assetid: fb34a560-bde9-4ad9-aa96-0d4baa4fc104
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e6b15da477c8fb5a4eb8cdc7ef302ebd8a41fae6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099736"
---
# <a name="sysdm_exec_input_buffer-transact-sql"></a>sys.dm_exec_input_buffer (Transact-SQL)

[!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)]

Retourne des informations sur les instructions soumises à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="syntax"></a>Syntaxe

```
sys.dm_exec_input_buffer ( session_id , request_id )
```

## <a name="arguments"></a>Arguments

*session_id* ID de session exécutant le lot à rechercher. *session_id* est de type **smallint**. *session_id* peut être obtenu à partir des objets de gestion dynamique suivants :

- [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
- [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)
- [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)

*request_id* Request_id à partir de [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md). *request_id* est de **type int**.

## <a name="table-returned"></a>Table retournée

|Nom de la colonne|Type de données|Description|
|-----------------|---------------|-----------------|
|**event_type**|**nvarchar (256)**|Type d’événement dans la mémoire tampon d’entrée pour le SPID donné.|
|**parameters**|**smallint**|Tous les paramètres fournis pour l’instruction.|
|**event_info**|**nvarchar(max)**|Texte de l’instruction dans la mémoire tampon d’entrée pour le SPID donné.|

## <a name="permissions"></a>Autorisations

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Si l’utilisateur dispose de l’autorisation View Server State, il verra toutes les sessions en cours d’exécution sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; sinon, l’utilisateur verra uniquement la session active.

> [!IMPORTANT]
> L’exécution de cette DMV en dehors de SQL Server Management Studio par rapport SQL Server sans les autorisations VIEW SERVER STATE (comme dans un déclencheur, une procédure stockée ou une fonction) lève une erreur d’autorisation sur la base de données Master.

Sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , si l’utilisateur est le propriétaire de la base de données, l’utilisateur verra toutes les sessions en cours d’exécution sur le [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ; sinon, l’utilisateur verra uniquement la session active.

> [!IMPORTANT]
> L’exécution de cette DMV en dehors de SQL Server Management Studio par rapport Azure SQL Database sans autorisations de propriétaire (comme dans un déclencheur, une procédure stockée ou une fonction) lève une erreur d’autorisation sur la base de données Master.

## <a name="remarks"></a>Notes

Cette fonction de gestion dynamique peut être utilisée conjointement avec sys.dm_exec_sessions ou sys.dm_exec_requests à l’aide de **Cross Apply**.

## <a name="examples"></a>Exemples

### <a name="a-simple-example"></a>R. Exemple simple

L’exemple suivant illustre le passage d’un ID de session (SPID) et d’un ID de demande à la fonction.

```sql
SELECT * FROM sys.dm_exec_input_buffer (52, 0);
GO
```

### <a name="b-using-cross-apply-to-additional-information"></a>B. Utilisation de Cross Apply pour obtenir des informations supplémentaires

L’exemple suivant répertorie la mémoire tampon d’entrée pour les sessions avec l’ID de session supérieur à 50.

```sql
SELECT es.session_id, ib.event_info
FROM sys.dm_exec_sessions AS es
CROSS APPLY sys.dm_exec_input_buffer(es.session_id, NULL) AS ib
WHERE es.session_id > 50;
GO
```

## <a name="see-also"></a>Voir aussi

- [Fonctions et vues de gestion dynamique relatives aux exécutions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
- [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)
- [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
- [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)
