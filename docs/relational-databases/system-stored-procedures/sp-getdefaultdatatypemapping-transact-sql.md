---
description: sp_getdefaultdatatypemapping (Transact-SQL)
title: sp_getdefaultdatatypemapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getdefaultdatatypemapping_TSQL
- sp_getdefaultdatatypemapping
helpviewer_keywords:
- sp_getdefaultdatatypemapping
ms.assetid: b8401de1-f135-41d0-ba79-ce8fe1f48c00
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 64345f8471caf5d757e0c8fbdbc843b2a18680ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183165"
---
# <a name="sp_getdefaultdatatypemapping-transact-sql"></a>sp_getdefaultdatatypemapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur le mappage par défaut pour le type de données spécifié entre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] système de gestion de base de données (SGBD) non-. Cette procédure stockée est exécutée sur n’importe quelle base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_getdefaultdatatypemapping [ @source_dbms = ] 'source_dbms'   
    [ , [ @source_version = ] 'source_version' ]  
        , [ @source_type = ] 'source_type'    
    [ , [ @source_length = ] source_length ]  
    [ , [ @source_precision = ] source_precision ]  
    [ , [ @source_scale = ] source_scale ]  
    [ , [ @source_nullable = ] source_nullable ]  
        , [ @destination_dbms = ] 'destination_dbms'   
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' OUTPUT ]  
    [ , [ @destination_length = ] destination_length OUTPUT ]  
    [ , [ @destination_precision = ] destination_precision OUTPUT ]  
    [ , [ @destination_scale = ] destination_scale OUTPUT ]  
    [ , [ @destination_nullable = ] source_nullable OUTPUT ]  
    [ , [ @dataloss = ] dataloss OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @source_dbms = ] 'source_dbms'` Nom du SGBD à partir duquel les types de données sont mappés. *source_dbms* est de **type sysname** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Base de données source au format [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**SOLUTION**|Base de données Oracle source.|  
  
 Ce paramètre est obligatoire.  
  
`[ @source_version = ] 'source_version'` Numéro de version du SGBD source. *source_version* est de type **varchar (10)**, avec NULL comme valeur par défaut.  
  
`[ @source_type = ] 'source_type'` Type de données dans le SGBD source. *source_type* est de **type sysname**, sans valeur par défaut.  
  
`[ @source_length = ] source_length` Longueur du type de données dans le SGBD source. *source_length* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @source_precision = ] source_precision` Précision du type de données dans le SGBD source. *source_precision* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @source_scale = ] source_scale` Est l’échelle du type de données dans le SGBD source. *source_scale* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @source_nullable = ] source_nullable` Indique si le type de données du SGBD source prend en charge la valeur NULL. *source_nullable* est de type **bit**, avec **1** comme valeur par défaut, ce qui signifie que les valeurs NULL sont prises en charge.  
  
`[ @destination_dbms = ] 'destination_dbms'` Nom du SGBD de destination. *destination_dbms* est de **type sysname** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destination.|  
|**SOLUTION**|Base de données Oracle de destination.|  
|**DB2**|Base de données IBM DB2 de destination.|  
|**INTERFACES**|Base de données Sybase de destination.|  
  
 Ce paramètre est obligatoire.  
  
`[ @destination_version = ] 'destination_version'` Version du produit du SGBD de destination. *destination_version* est de type **varchar (10)**, avec NULL comme valeur par défaut.  
  
`[ @destination_type = ] 'destination_type' OUTPUT` Type de données indiqué dans le SGBD de destination. *destination_type* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @destination_length = ] destination_length OUTPUT` Longueur du type de données dans le SGBD de destination. *destination_length* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @destination_precision = ] destination_precision OUTPUT` Précision du type de données dans le SGBD de destination. *destination_precision* est de type **bigint**, avec NULL comme valeur par défaut.  
  
`[ @destination_scale = ] _destination_scaleOUTPUT` Est l’échelle du type de données dans le SGBD de destination. *destination_scale* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @destination_nullable = ] _destination_nullableOUTPUT` Indique si le type de données du SGBD de destination prend en charge une valeur NULL. *destination_nullable* est de type **bit**, avec NULL comme valeur par défaut. **1** signifie que les valeurs NULL sont prises en charge.  
  
`[ @dataloss = ] _datalossOUTPUT` Indique si le mappage peut entraîner une perte de données. *perte* est de type **bit**, avec NULL comme valeur par défaut. **1** signifie qu’il existe un risque de perte de données.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_getdefaultdatatypemapping** est utilisé dans tous les types de réplication entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SGBD non-.  
  
 **sp_getdefaultdatatypemapping** retourne le type de données de destination par défaut qui correspond à la correspondance la plus proche du type de données source spécifié.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_getdefaultdatatypemapping**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_helpdatatypemap &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)   
 [sp_setdefaultdatatypemapping &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)   
 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)  
  
  
