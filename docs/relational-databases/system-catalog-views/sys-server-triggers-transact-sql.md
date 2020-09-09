---
description: sys.server_triggers (Transact-SQL)
title: sys. server_triggers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_triggers
- sys.server_triggers_TSQL
- server_triggers_TSQL
- sys.server_triggers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_triggers catalog view
ms.assetid: 25926ff4-9271-45bf-bc32-d5d3344bd47a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6850c9d406dd461f967d8297dcd5a49fdb5c3993
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89545033"
---
# <a name="sysserver_triggers-transact-sql"></a>sys.server_triggers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient l'ensemble de tous les déclencheurs DDL de niveau serveur avec un type object_type de valeur TR ou TA. Dans le cas des déclencheurs CLR, l’assembly doit être chargé dans la base de données **Master** . Tous les noms de déclencheurs DDL au niveau du serveur existent dans une étendue unique et globale.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du déclencheur.|  
|**object_id**|**int**|ID de l'objet.|  
|**parent_class**|**tinyint**|Classe du parent. Est toujours :<br /><br /> 100 = serveur|  
|**parent_class_desc**|**nvarchar(60)**|Description d'une classe de parent. Est toujours :<br /><br /> SERVER.|  
|**parent_id**|**int**|Toujours 0 pour les déclencheurs sur SERVER.|  
|**type**|**char(2)**|Type d’objet :<br /><br /> TA = Déclencheur assembly (CLR)<br /><br /> TR = Déclencheur SQL|  
|**type_desc**|**nvarchar(60)**|Description de la classe du type d'objet.<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**create_date**|**datetime**|Date de création du déclencheur.|  
|**modify_date**|**datetime**|Date de la dernière modification du déclencheur à l'aide de l'instruction ALTER.|  
|**is_ms_shipped**|**bit**|Déclencheur créé pour l'utilisateur par un composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interne.|  
|**is_disabled**|**bit**|1 = Déclencheur désactivé.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
