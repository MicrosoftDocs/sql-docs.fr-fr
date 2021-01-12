---
description: sys.endpoint_webmethods (Transact-SQL)
title: sys.endpoint_webmethods (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.endpoint_webmethods_TSQL
- sys.endpoint_webmethods
- endpoint_webmethods_TSQL
- sys.http_soap_methods_TSQL
- endpoint_webmethods
- sys.http_soap_methods
dev_langs:
- TSQL
helpviewer_keywords:
- sys.endpoint_webmethods catalog view
ms.assetid: 7dad0cf6-eafa-47cf-98cc-75ba8d3c7959
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1c57d8319df8feb793587dc07d6d55248a0fca12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100609"
---
# <a name="sysendpoint_webmethods-transact-sql"></a>sys.endpoint_webmethods (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Contient une ligne pour chaque méthode SOAP définie sur un point de terminaison HTTP SOAP. La combinaison des colonnes endpoint_id et namespace est unique.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|endpoint_id|**int**|ID du point de terminaison sur lequel la méthode Web est définie.|  
|espace de noms|**nvarchar(384**|Espace de noms de la méthode Web.|  
|method_alias|**nvarchar (64)**|Alias de la méthode.<br /><br /> Remarque : les [!INCLUDE[tsql](../../includes/tsql-md.md)] identificateurs autorisent les caractères qui ne sont pas autorisés dans les noms de méthodes WSDL.<br /><br /> L'alias permet de mapper le nom exposé dans la description WSDL du point de terminaison avec l'objet exécutable [!INCLUDE[tsql](../../includes/tsql-md.md)] sous-jacent réel qui est appelé lorsque la méthode Web est invoquée.|  
|object_name|**nvarchar(776**|Nom d'objet vers lequel la méthode Web est redirigée, tel que spécifié dans l'option NAME =. Les parties du nom sont séparées par un point (.) et délimitées à l’aide de crochets, `[``]` .<br /><br /> Le nom d'objet doit être composé de trois parties, tel que spécifié dans l'option WSDL.|  
|result_schema|**tinyint**|Option qui détermine le schéma XSD éventuellement retourné avec une réponse.<br /><br /> 0 = Aucun(e)<br /><br /> 1 = standard<br /><br /> 2 = par défaut|  
|result_schema_desc|**nvarchar(60)**|Description de l'option qui détermine le schéma XSD éventuellement retourné avec une réponse.<br /><br /> NONE<br /><br /> STANDARD<br /><br /> DEFAULT|  
|result_format|**tinyint**|Option qui détermine la mise en forme des résultats dans la réponse.<br /><br /> 1 = ALL_RESULTS<br /><br /> 2 = ROWSETS_ONLY<br /><br /> 3 = NONE|  
|result_format_desc|**nvarchar(60)**|Description de l'option qui détermine la mise en forme des résultats dans la réponse.<br /><br /> ALL_RESULTS<br /><br /> ROWSETS_ONLY<br /><br /> NONE|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue des points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
