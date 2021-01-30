---
description: sys.sp_cdc_get_ddl_history (Transact-SQL)
title: sys.sp_cdc_get_ddl_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_get_ddl_history
- sp_cdc_get_ddl_history_TSQL
- sys.sp_cdc_get_ddl_history_TSQL
- sys.sp_cdc_get_ddl_history
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], querying metadata
- sp_cdc_get_ddl_history
- sys.sp_cdc_get_ddl_history
ms.assetid: 4dee5e2e-d7e5-4fea-8037-a4c05c969b3a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b83e018c8e8e7c5e2f4daa704e5b7a13e7789f0b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205959"
---
# <a name="syssp_cdc_get_ddl_history-transact-sql"></a>sys.sp_cdc_get_ddl_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne l'historique des modifications de langage de définition de données (DDL) associé à l'instance de capture spécifiée depuis que la capture de données modifiées a été activée pour cette instance de capture. La capture des modifications de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_get_ddl_history [ @capture_instance = ] 'capture_instance'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @capture_instance =] '*capture_instance*'  
 Nom de l'instance de capture associée à une table source. *capture_instance* est de **type sysname** et ne peut pas avoir la valeur null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|Nom du schéma de table source.|  
|source_table|**sysname**|Nom de la table source.|  
|capture_instance|**sysname**|Nom de l'instance de capture.|  
|required_column_update|**bit**|Indique que la modification DDL a nécessité la modification d'une colonne dans la table de modifications pour refléter une modification de type de données effectuée sur la colonne source.|  
|ddl_command|**nvarchar(max)**|Instruction DDL appliquée à la table source.|  
|ddl_lsn|**binary(10)**|Numéro séquentiel dans le journal associé à la modification DDL.|  
|ddl_time|**datetime**|Heure associée à la modification DDL.|  
  
## <a name="remarks"></a>Notes  
 Les modifications DDL de la table source qui modifient la structure de colonne de la table source, telles que l’ajout ou la suppression d’une colonne ou la modification du type de données d’une colonne existante, sont conservées dans la table [CDC.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) . Ces modifications peuvent être signalées en utilisant cette procédure stockée. Les entrées dans cdc.ddl_history sont effectuées au moment où le processus de capture lit la transaction DDL dans le journal.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe db_owner pour retourner les lignes de toutes les instances de capture dans la base de données. Pour tous les autres utilisateurs, requiert l'autorisation SELECT sur toutes les colonnes capturées dans la table source et, si un rôle de régulation pour l'instance de capture a été défini, l'appartenance à ce rôle de base de données.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ajoute une colonne à la table source `HumanResources.Employee`, puis exécute la procédure stockée `sys.sp_cdc_get_ddl_history` pour signaler les modifications DDL qui s'appliquent à la table source associée à l'instance de capture `HumanResources_Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
ALTER TABLE HumanResources.Employee  
ADD Test_Column int NULL;  
GO  
-- Pause 10 seconds to allow the event to be logged.   
WAITFOR DELAY '00:00:10';  
GO   
EXECUTE sys.sp_cdc_get_ddl_history   
    @capture_instance = 'HumanResources_Employee';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  
