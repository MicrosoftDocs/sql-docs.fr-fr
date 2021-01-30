---
description: sysarticles (vue système) (Transact-SQL)
title: sysarticles (vue système) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticles
- sysarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticles view
ms.assetid: 18f8c9b3-cab7-4e8f-8754-11ac38c3f789
author: stevestein
ms.author: sstein
ms.openlocfilehash: 21e1ca1548eee3fd9b17674cdf8ee7d32845b43f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160333"
---
# <a name="sysarticles-system-view-transact-sql"></a>sysarticles (vue système) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vue **sysarticles** expose les propriétés de l’article. Cette vue est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Colonne d'identité fournissant un numéro d'identification unique pour l'article|  
|**creation_script**|**nvarchar(255)**|Script du schéma de l'article.|  
|**del_cmd**|**nvarchar(255)**|Commande à exécuter en cas d'instruction DELETE, sinon création à partir du journal|  
|**description**|**nvarchar(255)**|Entrée descriptive de l’article.|  
|**dest_table**|**sysname**|Nom de la table de destination|  
|**filter**|**int**|Identificateur de la procédure stockée, utilisé pour la partition horizontale.|  
|**filter_clause**|**ntext**|Clause WHERE de l'article utilisée pour le filtrage horizontal.|  
|**ins_cmd**|**nvarchar(255)**|Commande à exécuter en cas d'instruction INSERT, sinon création à partir du journal.|  
|**name**|**sysname**|Nom associé à l'article et unique dans la publication|  
|**objid**|**int**|Identificateur de l'objet de la table publiée|  
|**pubid**|**int**|Identificateur de la publication à laquelle appartient l'article|  
|**pre_creation_cmd**|**tinyint**|Commande de précréation pour les instructions DROP TABLE, DELETE TABLE ou TRUNCATE :<br /><br /> **0** = aucun.<br /><br /> **1** = suppression.<br /><br /> **2** = suppression.<br /><br /> **3** = tronquer.|  
|**statut**|**tinyint**|Masque de bits de l'état et des options d'article, qui peut être le résultat OR logique au niveau du bit d'au moins l'une des valeurs suivantes :<br /><br /> **1** = l’article est actif.<br /><br /> **8** = inclure le nom de colonne dans les instructions INSERT.<br /><br /> **16** = utiliser des instructions paramétrables.<br /><br /> **24** = les deux incluent le nom de colonne dans les instructions INSERT et utilisent des instructions paramétrables.<br /><br /> **64** = la partition horizontale pour l’article est définie par un abonnement transformable.<br /><br /> Par exemple, un article actif utilisant des instructions paramétrables aurait la valeur **17** dans cette colonne. La valeur **0** signifie que l’article est inactif et qu’aucune propriété supplémentaire n’est définie.|  
|**sync_objid**|**int**|Identificateur de la table ou de la vue représentant la définition de l'article.|  
|**type**|**tinyint**|Type d'article :<br /><br /> **1** = article basé sur le journal.<br /><br /> **3** = article basé sur un journal avec filtre manuel.<br /><br /> **5** = article basé sur un journal avec vue manuelle.<br /><br /> **7** = article basé sur le journal avec filtre manuel et vue manuelle.<br /><br /> **8** = exécution de la procédure stockée.<br /><br /> **24** = exécution d’une procédure stockée sérialisable.<br /><br /> **32** = procédure stockée (schéma uniquement).<br /><br /> **64** = vue (schéma uniquement).<br /><br /> **128** = fonction (schéma uniquement).|  
|**upd_cmd**|**nvarchar(255)**|Commande à exécuter en cas d'instruction UPDATE, sinon création à partir du journal|  
|**schema_option**|**Binary(8**|Masque de bits des options de génération de schéma pour l'article, qui déterminent quelles sont les parties du schéma d'article devant donner lieu à un script pour la remise à l'Abonné. Pour plus d’informations sur les options de schéma, consultez [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Propriétaire de la table dans la base de données de destination|  
|**ins_scripting_proc**|**int**|Procédure stockée ou script personnalisé inscrit exécuté lorsqu'une instruction INSERT est répliquée.|  
|**del_scripting_proc**|**int**|Procédure stockée ou script personnalisé inscrit exécuté lorsqu'une instruction DELETE est répliquée.|  
|**upd_scripting_proc**|**int**|Procédure stockée ou script personnalisé inscrit exécuté lorsqu'une instruction UPDATE est répliquée.|  
|**custom_script**|**nvarchar(2048)**|Procédure stockée ou script personnalisé inscrit exécuté à la fin du déclencheur DDL.|  
|**fire_triggers_on_snapshot**|**bit**|Indique si les déclencheurs répliqués sont exécutés lorsque l'instantané est appliqué ; cette option peut prendre l'une des valeurs suivantes :<br /><br /> **0** = les déclencheurs ne sont pas exécutés.<br /><br /> **1** = les déclencheurs sont exécutés.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)  
  
  
