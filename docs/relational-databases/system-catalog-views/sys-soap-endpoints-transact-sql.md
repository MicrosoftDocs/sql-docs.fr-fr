---
description: sys.soap_endpoints (Transact-SQL)
title: sys.soap_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- soap_endpoints_TSQL
- sys.soap_endpoints
- soap_endpoints
- sys.soap_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.soap_endpoints catalog view
ms.assetid: f50dcbfc-02ed-4a19-9c07-c78a5a1b3224
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0c02dba9a1d359517efc39ad73ebde3003c5bf3a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195605"
---
# <a name="syssoap_endpoints-transact-sql"></a>sys.soap_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Contient une ligne pour chaque point de terminaison du serveur qui supporte une charge utile de type SOAP. Pour chaque ligne de cette vue, il existe une ligne correspondante avec le même **endpoint_id** dans l’affichage catalogue **sys.http_endpoints** qui contient les métadonnées de configuration http.  
  
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**< les colonnes héritées>**||Pour obtenir la liste des colonnes héritées par cette vue, consultez [sys. endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**is_sql_language_enabled**|**bit**|1 = TRAITEMENTS = L'option ENABLED a été spécifiée, ce qui signifie que les traitements SQL ad-hoc sont autorisées sur le point de terminaison.|  
|**wsdl_generator_procedure**|**nvarchar(776**|Nom en trois parties de la procédure stockée qui implémente cette méthode.<br /><br /> Les noms des méthodes requièrent une syntaxe stricte en trois parties. Les noms composés d'une, de deux ou de quatre parties ne sont pas autorisés.|  
|**default_database**|**sysname**|Nom de la base de données par défaut indiqué dans l'option DATABASE =.<br /><br /> NULL = DEFAULT a été spécifié.|  
|**default_namespace**|**nvarchar(384**|Espace de noms par défaut spécifié dans l’option espace de noms = ou `https://tempuri.org` si la valeur par défaut a été spécifiée à la place.|  
|**default_result_schema**|**tinyint**|Valeur par défaut de l'option SCHEMA =.<br /><br /> 0 = AUCUN<br /><br /> 1 = STANDARD|  
|**default_result_schema_desc**|**nvarchar(60)**|Description de la valeur par défaut de l'option SCHEMA =.<br /><br /> Aucune<br /><br /> STANDARD|  
|**is_xml_charset_enforced**|**bit**|0 = CHARACTER_SET = L'option SQL a été spécifiée.<br /><br /> 1 = CHARACTER_SET = L'option XML a été spécifiée.|  
|**is_session_enabled**|**bit**|0 = SESSION = L'option DISABLE a été spécifiée.<br /><br /> 1 = SESSION = L'option ENABLED a été spécifiée.|  
|**session_timeout**|**int**|Valeur spécifiée dans l'option SESSION_TIMEOUT =.|  
|**login_type**|**nvarchar(60)**|Type d'authentification autorisé sur ce point de terminaison.<br /><br /> WINDOWS<br /><br /> MIXED|  
|**header_limit**|**int**|Taille maximale pouvant être autorisée pour l'en-tête SOAP.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue des points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
