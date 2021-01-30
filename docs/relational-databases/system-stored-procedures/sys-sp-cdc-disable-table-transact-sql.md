---
description: sys.sp_cdc_disable_table (Transact-SQL)
title: sys.sp_cdc_disable_table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_disable_table
- sp_cdc_disable_table
- sys.sp_cdc_disable_table_TSQL
- sp_cdc_disable_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_disable_table
- sys.sp_cdc_disable_table
- change data capture [SQL Server], disabling tables
ms.assetid: da2156c0-504e-4d76-b9a0-4448becf9bda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 53984d18923cef297e267374045f0f4cb4364419
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206017"
---
# <a name="syssp_cdc_disable_table-transact-sql"></a>sys.sp_cdc_disable_table (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Désactive la capture de données modifiées pour la table source spécifiée et l'instance de capture dans la base de données actuelle. La capture des modifications de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_disable_table   
  [ @source_schema = ] 'source_schema' ,   
  [ @source_name = ] 'source_name'  
  [ , [ @capture_instance = ] 'capture_instance' | 'all' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @source_schema = ] 'source\_schema'` Nom du schéma dans lequel se trouve la table source. *source_schema* est de **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
 *source_schema* doit exister dans la base de données active.  
  
`[ @source_name = ] 'source\_name'` Nom de la table source à partir de laquelle la capture de données modifiées doit être désactivée. *source_name* est de **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
 *source_name* doit exister dans la base de données active.  
  
`[ @capture_instance = ] 'capture\_instance' | 'all'` Nom de l’instance de capture à désactiver pour la table source spécifiée. *capture_instance* est de **type sysname** et ne peut pas avoir la valeur null.  
  
 Lorsque’all’est spécifié, toutes les instances de capture définies pour *source_name* sont désactivées.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sys.sp_cdc_disable_table** supprime la table de modifications de capture de données modifiées et les fonctions système associées à la table source et à l’instance de capture spécifiées. Elle supprime toutes les lignes associées à l’instance de capture spécifiée à partir des tables système de capture de données modifiées et définit la colonne **is_tracked_by_cdc** pour l’entrée de table dans l’affichage catalogue [sys. tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) sur 0.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant désactive la capture des données modifiées pour la table `HumanResources.Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_disable_table   
    @source_schema = N'HumanResources',   
    @source_name = N'Employee',  
    @capture_instance = N'HumanResources_Employee';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)  
  
  
