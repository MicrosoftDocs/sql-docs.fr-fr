---
description: sys.sp_cdc_help_change_data_capture (Transact-SQL)
title: sys.sp_cdc_help_change_data_capture (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_help_change_data_capture_TSQL
- sys.sp_cdc_help_change_data_capture_TSQL
- sp_cdc_help_change_data_capture
- sys.sp_cdc_help_change_data_capture
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], querying metadata
- sys.sp_cdc_help_change_data_capture
- sp_cdc_help_change_data_capture
ms.assetid: 91fd41f5-1b4d-44fe-a3b5-b73eff65a534
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 79c12ab4b1bdfb3a955c83ef3d724a026f24f1ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159780"
---
# <a name="syssp_cdc_help_change_data_capture-transact-sql"></a>sys.sp_cdc_help_change_data_capture (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne la configuration de capture de données modifiées pour chaque table activée pour la capture de données modifiées dans la base de données actuelle. Jusqu'à deux lignes peuvent être retournées pour chaque table source, une ligne pour chaque instance de capture. La capture des modifications de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_help_change_data_capture   
  [ [ @source_schema = ] 'source_schema' ]  
  [, [ @source_name = ] 'source_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @source_schema =] '*source_schema*'  
 Nom du schéma auquel appartient la table source. *source_schema* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque *source_schema* est spécifié, *source_name* doit également être spécifié.  
  
 Si la valeur est non NULL, *source_schema* doit exister dans la base de données active.  
  
 Si *source_schema* est non null, *source_name* doit également être non null.  
  
 [ @source_name =] '*source_name*'  
 Nom de la table source. *source_name* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque *source_name* est spécifié, *source_schema* doit également être spécifié.  
  
 Si la valeur est non NULL, *source_name* doit exister dans la base de données active.  
  
 Si *source_name* est non null, *source_schema* doit également être non null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|Nom du schéma de table source.|  
|source_table|**sysname**|Nom de la table source.|  
|capture_instance|**sysname**|Nom de l'instance de capture.|  
|object_id|**int**|ID de la table de modifications associée à la table source.|  
|source_object_id|**int**|ID de la table source.|  
|start_lsn|**binary(10)**|Numéro séquentiel dans le journal qui représente le point de terminaison inférieur pour interroger la table de modifications.<br /><br /> NULL = le point de terminaison inférieur n'a pas été établi.|  
|end_lsn|**binary(10)**|Numéro séquentiel dans le journal qui représente le point de terminaison supérieur pour interroger la table de modifications. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], cette colonne a toujours pour valeur NULL.|  
|supports_net_changes|**bit**|La prise en charge des modifications nettes est activée.|  
|has_drop_pending|**bit**|Inutilisé dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].|  
|role_name|**sysname**|Nom du rôle de base de données utilisé pour contrôler l'accès aux données modifiées.<br /><br /> NULL = aucun rôle n'est utilisé.|  
|index_name|**sysname**|Nom de l'index utilisé pour identifier de façon unique des lignes dans la table source.|  
|filegroup_name|**sysname**|Nom du groupe de fichiers qui contient la table de modifications.<br /><br /> NULL = la table de modifications se trouve dans le groupe de fichiers par défaut de la base de données.|  
|create_date|**datetime**|Date d'activation de l'instance de capture.|  
|index_column_list|**nvarchar(max)**|Liste des colonnes d'index utilisées pour identifier de façon unique des lignes dans la table source.|  
|captured_column_list|**nvarchar(max)**|Liste des colonnes sources capturées.|  
  
## <a name="remarks"></a>Notes  
 Si *source_schema* et *source_name* par défaut la valeur null, ou si la valeur null est explicitement définie, cette procédure stockée retourne des informations pour toutes les instances de capture de base de données pour lesquelles l’appelant a un accès SELECT. Lorsque *source_schema* et *source_name* n’ont pas la valeur null, seules les informations sur la table nommée activée spécifique sont retournées.  
  
## <a name="permissions"></a>Autorisations  
 Lorsque *source_schema* et *source_name* ont la valeur null, l’autorisation de l’appelant détermine les tables activées qui sont incluses dans le jeu de résultats. Les appelants doivent avoir l'autorisation SELECT sur toutes les colonnes capturées de l'instance de capture et être membres d'un rôle de régulation défini pour les informations de table à inclure. Les membres du rôle de base de données db_owner peuvent afficher des informations concernant toutes les instances de capture définies. Lorsque des informations pour une table activée spécifique sont demandées, les mêmes critères SELECT et d'appartenance sont appliqués pour la table nommée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-change-data-capture-configuration-information-for-a-specified-table"></a>R. Retour d'informations de configuration de capture des données modifiées pour une table spécifiée  
 L'exemple suivant retourne la configuration de capture des données modifiées pour la table `HumanResources.Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_help_change_data_capture   
    @source_schema = N'HumanResources',   
    @source_name = N'Employee';  
GO  
```  
  
### <a name="b-returning-change-data-capture-configuration-information-for-all-tables"></a>B. Retour d'informations de configuration de capture des données modifiées pour toutes les tables  
 L'exemple suivant retourne des informations de configuration pour toutes les tables activées dans la base de données qui contient des données modifiées auxquelles l'appelant est autorisé à accéder.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_help_change_data_capture;  
GO  
```  
  
