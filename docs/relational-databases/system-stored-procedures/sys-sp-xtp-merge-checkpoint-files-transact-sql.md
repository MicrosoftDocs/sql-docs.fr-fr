---
description: sys.sp_xtp_merge_checkpoint_files (Transact-SQL)
title: sys.sp_xtp_merge_checkpoint_files (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_xtp_merge_checkpoint_files_TSQL
- sys.sp_xtp_merge_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_merge_checkpoint_files
ms.assetid: da04df2a-f7a1-41e7-a1ef-2d5d68919892
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 85ecf2fbec4e3e35940862e9b6a71460fab781e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99102835"
---
# <a name="syssp_xtp_merge_checkpoint_files-transact-sql"></a>sys.sp_xtp_merge_checkpoint_files (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  **sys.sp_xtp_merge_checkpoint_files** fusionne tous les fichiers de données et delta dans la plage de transactions spécifiée.  
  
 Pour plus d’informations, consultez [création et gestion du stockage pour les objets Memory-Optimized](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
||  
|-|  
|**Remarque**: cette procédure stockée est déconseillée dans [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] . Il n’est plus nécessaire et ne peut pas être utilisé, à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] .|  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_xtp_merge_checkpoint_files database_name, @transaction_lower_bound, @transaction_upper_bound  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données dans laquelle la fusion doit être appelée. Si la base de données ne contient pas de tables en mémoire, cette procédure retourne une erreur utilisateur. Si la base de données est hors connexion, retourne une erreur.  
  
 *lower_bound_Tid*  
 La limite inférieure (bigint) des transactions pour un fichier de données, comme indiqué dans [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) correspondant au fichier de point de contrôle de démarrage de la fusion. Une erreur est générée pour une valeur transactionId non valide.  
  
 *upper_bound_Tid*  
 Limite supérieure (bigint) des transactions pour un fichier de données, comme indiqué dans [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md). Une erreur est générée pour une valeur transactionId non valide.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="cursors-returned"></a>Curseurs retournés  
 None  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle serveur fixe sysadmin et au rôle de base de données fixe db_owner.  
  
## <a name="remarks"></a>Notes  
 Fusionne tous les fichiers de données et delta dans la plage valide pour générer un seul fichier de données et delta. Cette procédure ne respecte pas la stratégie de fusion.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
