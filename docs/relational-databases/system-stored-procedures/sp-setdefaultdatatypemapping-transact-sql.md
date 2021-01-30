---
description: sp_setdefaultdatatypemapping (Transact-SQL)
title: sp_setdefaultdatatypemapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_setdefaultdatatypemapping
- sp_setdefaultdatatypemapping_TSQL
helpviewer_keywords:
- sp_setdefaultdatatypemapping
ms.assetid: 7394e8ca-4ce1-4e99-a784-205007c2c248
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 46b17720b7d57110dc334a425976bf621a1395bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209248"
---
# <a name="sp_setdefaultdatatypemapping-transact-sql"></a>sp_setdefaultdatatypemapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Marque un mappage de type de données existant entre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] système de gestion de base de données (SGBD) non défini comme valeur par défaut. Cette procédure stockée est exécutée sur n’importe quelle base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_setdefaultdatatypemapping [ [ @mapping_id = ] mapping_id ]  
    [ , [ @source_dbms = ] 'source_dbms' ]  
    [ , [ @source_version = ] 'source_version' ]  
    [ , [ @source_type = ] 'source_type' ]   
    [ , [ @source_length_min = ] source_length_min ]  
    [ , [ @source_length_max = ] source_length_max ]  
    [ , [ @source_precision_min = ] source_precision_min ]  
    [ , [ @source_precision_max = ] source_precision_max ]  
    [ , [ @source_scale_min = ] source_scale_min ]  
    [ , [ @source_scale_max = ] source_scale_max ]  
    [ , [ @source_nullable = ] source_nullable ]  
    [ , [ @destination_dbms = ] 'destination_dbms' ]  
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' ]  
    [ , [ @destination_length = ] destination_length ]  
    [ , [ @destination_precision = ] destination_precision ]  
    [ , [ @destination_scale = ] destination_scale ]  
    [ , [ @destination_nullable = ] source_nullable ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @mapping_id = ] mapping_id` Identifie un mappage de type de données existant.  *mapping_id* est de **type int**, avec NULL comme valeur par défaut. Si vous spécifiez *mapping_id*, les autres paramètres ne sont pas requis.  
  
`[ @source_dbms = ] 'source_dbms'` Nom du SGBD à partir duquel les types de données sont mappés. *source_dbms* est de **type sysname** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Base de données source au format [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**SOLUTION**|Base de données Oracle source.|  
|NULL (par défaut)||  
  
 Vous devez spécifier ce paramètre si *mapping_id* a la valeur null.  
  
`[ @source_version = ] 'source_version'` Numéro de version du SGBD source. *source_version* est de type **varchar (10)**, avec NULL comme valeur par défaut.  
  
`[ @source_type = ] 'source_type'` Type de données dans le SGBD source. *source_type* est de **type sysname**. Vous devez spécifier ce paramètre si *mapping_id* a la valeur null.  
  
`[ @source_length_min = ] source_length_min` Longueur minimale du type de données dans le SGBD source. *source_length_min* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @source_length_max = ] source_length_max` Longueur maximale du type de données dans le SGBD source. *source_length_max* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @source_precision_min = ] source_precision_min` Précision minimale du type de données dans le SGBD source. *source_precision_min* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @source_precision_max = ] source_precision_max` Précision maximale du type de données dans le SGBD source. *source_precision_max* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @source_scale_min = ] source_scale_min` Est l’échelle minimale du type de données dans le SGBD source. *source_scale_min* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @source_scale_max = ] source_scale_max` Échelle maximale du type de données dans le SGBD source. *source_scale_max* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @source_nullable = ] source_nullable` Indique si le type de données du SGBD source prend en charge la valeur NULL. *source_nullable* est de type **bit**, avec NULL comme valeur par défaut. **1** signifie que les valeurs NULL sont prises en charge.  
  
`[ @destination_dbms = ] 'destination_dbms'` Nom du SGBD de destination. *destination_dbms* est de **type sysname** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destination.|  
|**SOLUTION**|Base de données Oracle de destination.|  
|**DB2**|Base de données IBM DB2 de destination.|  
|**INTERFACES**|Base de données Sybase de destination.|  
|NULL (par défaut)||  
  
`[ @destination_version = ] 'destination_version'` Version du produit du SGBD de destination. *destination_version* est de type **varchar (10)**, avec NULL comme valeur par défaut.  
  
`[ @destination_type = ] 'destination_type'` Type de données indiqué dans le SGBD de destination. *destination_type* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @destination_length = ] destination_length` Longueur du type de données dans le SGBD de destination. *destination_length* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @destination_precision = ] destination_precision` Précision du type de données dans le SGBD de destination. *destination_precision* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @destination_scale = ] destination_scale` Est l’échelle du type de données dans le SGBD de destination. *destination_scale* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @destination_nullable = ] destination_nullable` Indique si le type de données du SGBD de destination prend en charge une valeur NULL. *destination_nullable* est de type **bit**, avec NULL comme valeur par défaut. **1** signifie que les valeurs NULL sont prises en charge.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_setdefaultdatatypemapping** est utilisé dans tous les types de réplication entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SGBD non-.  
  
 Les mappages de types de données par défaut s'appliquent à toutes les topologies de réplication qui comprennent le SGBD spécifié.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_setdefaultdatatypemapping**.  
  
## <a name="see-also"></a>Voir aussi  
 [Spécifier des mappages de types de données pour un serveur de publication Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [sp_getdefaultdatatypemapping &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)   
 [sp_helpdatatypemap &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)  
  
  
