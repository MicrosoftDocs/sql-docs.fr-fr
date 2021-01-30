---
description: sysextendedarticlesview (Transact-SQL)
title: sysextendedarticlesview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysextendedarticlesview_TSQL
- sysextendedarticlesview
dev_langs:
- TSQL
helpviewer_keywords:
- sysextendedarticlesview view
ms.assetid: 8bdd22f7-c268-49b6-820c-3fe603feb128
author: stevestein
ms.author: sstein
ms.openlocfilehash: 4eb45e9ab8113b6a66f6f7b5650546c2d6d7f06e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160316"
---
# <a name="sysextendedarticlesview-transact-sql"></a>sysextendedarticlesview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vue **sysextendedarticlesview** fournit des informations sur les articles publiés. Cette vue est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Colonne d'identité fournissant un numéro d'identification unique pour l'article|  
|**creation_script**|**nvarchar(255)**|Script de création du schéma de l'article.|  
|**del_cmd**|**nvarchar(255)**|Commande à exécuter en cas d'instruction DELETE, sinon création à partir du journal|  
|**description**|**nvarchar(255)**|Entrée descriptive de l’article.|  
|**dest_table**|**nvarchar(128)**|Nom de la table de destination|  
|**filter**|**int**|Identificateur d'objet de la procédure stockée utilisée pour le partitionnement horizontal.|  
|**filter_clause**|**ntext**|Clause WHERE de l'article utilisée pour le filtrage horizontal.|  
|**ins_cmd**|**nvarchar(255)**|Commande à exécuter lors d'une opération INSERT.|  
|**name**|**nvarchar(128)**|Nom associé à l'article et unique dans la publication|  
|**objid**|**int**|Identificateur de l'objet de la table publiée|  
|**pubid**|**int**|Identificateur de la publication à laquelle appartient l'article|  
|**pre_creation_cmd**|**tinyint**|Commande de précréation pour les instructions DROP TABLE, DELETE TABLE ou TRUNCATE :<br /><br /> **0** = aucun.<br /><br /> **1** = suppression.<br /><br /> **2** = suppression.<br /><br /> **3** = tronquer.|  
|**statut**|**int**|Masque de bits de l'état et des options d'article, qui peut être le résultat OR logique au niveau du bit d'au moins l'une des valeurs suivantes :<br /><br /> **1** = l’article est actif.<br /><br /> **8** = inclure le nom de colonne dans les instructions INSERT.<br /><br /> **16** = utiliser des instructions paramétrables.<br /><br /> **24** = les deux incluent le nom de colonne dans les instructions INSERT et utilisent des instructions paramétrables.<br /><br /> Par exemple, un article actif utilisant des instructions paramétrables posséderait la valeur 17 dans cette colonne. La valeur 0 signifie que l'article est inactif et qu'aucune propriété supplémentaire n'est définie.|  
|**sync_objid**|**int**|Identificateur de la table ou de la vue représentant la définition de l'article.|  
|**type**|**tinyint**|Type d'article :<br /><br /> **1** = article basé sur le journal.<br /><br /> **3** = article basé sur un journal avec filtre manuel.<br /><br /> **5** = article basé sur un journal avec vue manuelle.<br /><br /> **7** = article basé sur le journal avec filtre manuel et vue manuelle.|  
|**upd_cmd**|**nvarchar(255)**|Commande à exécuter en cas d'instruction UPDATE, sinon création à partir du journal|  
|**schema_option**|**binary**|Indique quelles sont les propriétés de l'objet publié qui donnent lieu à un script dans l'instantané. Pour obtenir la liste des options de schéma prises en charge, consultez [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**nvarchar(128)**|Propriétaire de la table dans la base de données de destination|  
|**ins_scripting_proc**|**int**|Identificateur d'objet de la procédure stockée ou du script personnalisé exécuté lorsqu'une instruction INSERT est répliquée.|  
|**del_scripting_proc**|**int**|Identificateur d'objet de la procédure stockée ou du script personnalisé exécuté lorsqu'une instruction DELETE est répliquée.|  
|**upd_scripting_proc**|**int**|Identificateur d'objet de la procédure stockée ou du script personnalisé exécuté lorsqu'une instruction UPDATE est répliquée.|  
|**custom_script**|**int**|Identificateur d'objet de la procédure ou du script personnalisé exécuté à l'activation d'un déclencheur DDL.|  
|**fire_triggers_on_snapshot**|**int**|Indique si les déclencheurs répliqués sont exécutés lorsque l'instantané est appliqué ; cette option peut prendre l'une des valeurs suivantes :<br /><br /> **0** = les déclencheurs ne sont pas exécutés.<br /><br /> **1** = les déclencheurs sont exécutés.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)  
  
  
