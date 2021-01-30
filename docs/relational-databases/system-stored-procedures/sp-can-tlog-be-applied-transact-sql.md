---
description: sp_can_tlog_be_applied (Transact-SQL)
title: sp_can_tlog_be_applied (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_can_tlog_be_applied_TSQL
- sp_can_tlog_be_applied
dev_langs:
- TSQL
helpviewer_keywords:
- sp_can_tlog_be_applied
ms.assetid: 9c143b6c-27ac-4ab7-98d1-3b7b265f3963
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9521c435d17e94a034aa1723f5cd023035cd1f87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197687"
---
# <a name="sp_can_tlog_be_applied-transact-sql"></a>sp_can_tlog_be_applied (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Vérifie si une sauvegarde du journal des transactions peut être appliquée à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **sp_can_tlog_be_applied** nécessite que la base de données soit dans l’état RESTORING.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_can_tlog_be_applied [ @backup_file_name = ] 'backup_file_name'   
        , [ @database_name = ] 'database_name'   
        , [ @result = ] result OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
`[ @backup_file_name = ] 'backup_file_name'` Nom d’un fichier de sauvegarde. *backup_file_name* est **de type nvarchar (128)**.  
  
`[ @database_name = ] 'database_name'` Nom de la base de données. *database_name* est de type **sysname**.  
  
`[ @result = ] _result_ OUTPUT` Indique si le journal des transactions peut être appliqué à la base de données. le *résultat* est un **bit**.  
  
 1 = Le journal peut être appliqué.  
  
 0 = Le journal ne peut pas être appliqué.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_can_tlog_be_applied**.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous déclare une variable locale, `@MyBitVar`, pour stocker le résultat.  
  
```  
USE master;  
GO  
DECLARE @MyBitVar BIT;  
EXEC sp_can_tlog_be_applied  
     @backup_file_name =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\AdventureWorks2012.bak',  
     @database_name = N'AdventureWorks2012',  
     @result = @MyBitVar OUTPUT;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
