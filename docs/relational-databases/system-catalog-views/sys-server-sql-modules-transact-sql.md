---
description: sys.server_sql_modules (Transact-SQL)
title: sys. server_sql_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.server_sql_modules
- sys.server_sql_modules_TSQL
- server_sql_modules_TSQL
- server_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_sql_modules catalog view
ms.assetid: 9ef9a8b9-c470-4a61-b0c4-ee24ad871d63
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7f74b41384c2aa42c26623fa69ef35c74f49eab3
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89539578"
---
# <a name="sysserver_sql_modules-transact-sql"></a>sys.server_sql_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient l'ensemble de modules SQL destinés aux déclencheurs de niveau serveur de type TR. Vous pouvez joindre cette relation à sys.server_triggers. Le tuple (object_id) est la clé de la relation.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Référence FOREIGN KEY au déclencheur de niveau serveur où ce module est défini.|  
|**définition**|**nvarchar(max)**|Texte SQL qui définit ce module.<br /><br /> NULL = chiffré|  
|**uses_ansi_nulls**|**bit**|Le module a été créé avec l'option ANSI NULLS définie sur ON.|  
|**uses_quoted_identifier**|**bit**|Le module a été créé avec l'option QUOTED IDENTIFIER définie sur ON.|  
|**execute_as_principal_id**|**int**|ID de l'instruction d'exécution en tant que principal de serveur (EXECUTE AS).<br /><br /> NULL par défaut ou si EXECUTE AS CALLER<br /><br /> ID du principal spécifié si EXECUTe AS SELF EXECUTe AS principal-2 = EXECUTe AS OWNER.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
