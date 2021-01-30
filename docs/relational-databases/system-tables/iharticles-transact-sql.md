---
description: IHarticles (Transact-SQL)
title: IHarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHarticles
- IHarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHarticles system table
ms.assetid: 773ef9b7-c993-4629-9516-70c47b9dcf65
author: cawrites
ms.author: chadam
ms.openlocfilehash: d0fa94df7e518dd3c0348b5f729393ad0af64175
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212035"
---
# <a name="iharticles-transact-sql"></a>IHarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table système **IHarticles** contient une ligne pour chaque article en cours de réplication à partir d’un serveur de publication non-SQL Server à l’aide du serveur de distribution actuel. Cette table est stockée dans la base de données de distribution.  
  
## <a name="definition"></a>Définition  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|Colonne d'identité fournissant un numéro d'identification unique pour l'article|  
|**name**|**sysname**|Nom associé à l'article et unique dans la publication|  
|**publication_id**|**smallint**|Identificateur de la publication à laquelle appartient l'article|  
|**table_id**|**int**|ID de la table en cours de publication à partir de [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md).|  
|**publisher_id**|**smallint**|ID du serveur de publication non SQL Server.|  
|**creation_script**|**nvarchar(255)**|Script du schéma de l'article.|  
|**del_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des suppressions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**filter**|**int**|Cette colonne n’est pas utilisée et est incluse uniquement pour rendre la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la table **IHarticles** compatible avec la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilisée pour SQL Server Articles ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**filter_clause**|**ntext**|Clause WHERE de l'article, utilisée pour le filtrage horizontal et écrite dans une instruction Transact-SQL standard interprétable par le serveur de publication non SQL.|  
|**ins_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des insertions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**pre_creation_cmd**|**tinyint**|Commande à exécuter avant d'appliquer l'instantané initial lorsqu'un objet de même nom existe déjà sur l'Abonné.<br /><br /> **0** = aucun-une commande n’est pas exécutée.<br /><br /> **1** = supprimer la table de destination.<br /><br /> **2** = Delete-supprime les données de la table de destination.<br /><br /> **3** = tronquer-tronquer la table de destination.|  
|**statut**|**tinyint**|Masque de bits de l'état et des options d'article, qui peut être le résultat OR logique au niveau du bit d'au moins l'une des valeurs suivantes :<br /><br /> **0** = aucune propriété supplémentaire.<br /><br /> **1** = actif.<br /><br /> **8** = inclure le nom de colonne dans les instructions INSERT.<br /><br /> **16** = utiliser des instructions paramétrables.<br /><br /> Par exemple, un article actif utilisant des instructions paramétrables posséderait la valeur 17 dans cette colonne. La valeur 0 signifie que l'article est inactif et qu'aucune propriété supplémentaire n'est définie.|  
|**type**|**tinyint**|Type d'article :<br /><br /> **1** = article basé sur le journal.|  
|**upd_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des mises à jour avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**schema_option**|**Binary(8**|Bitmap de l'option de génération de schéma d'un article donné, qui peut être le résultat OR logique au niveau du bit d'au moins l'une des valeurs suivantes :<br /><br /> **0x00** = désactive les scripts par le agent d’instantané et utilise le CreationScript fourni.<br /><br /> **0x01** = générer la création de l’objet (Create table, créer la procédure, etc.).<br /><br /> **0x10** = générer un index cluster correspondant.<br /><br /> **0x40** = générer des index non cluster correspondants.<br /><br /> **0x80** = inclut l’intégrité référentielle déclarée sur les clés primaires.<br /><br /> **0x1000** = réplique le classement au niveau des colonnes. Remarque : cette option est définie par défaut pour les serveurs de publication Oracle afin d’activer les comparaisons sensibles à la casse.<br /><br /> **0x4000** = répliquer les clés uniques si elles sont définies sur un article de table.<br /><br /> **0x8000** = répliquer une clé primaire et des clés uniques sur un article de table en tant que contraintes à l’aide d’instructions ALTER TABLE.|  
|**dest_owner**|**sysname**|Propriétaire de la table dans la base de données de destination|  
|**dest_table**|**sysname**|Nom de la table de destination|  
|**tablespace_name**|**nvarchar(255)**|Identifie l'espace disque logique utilisé par la table d'enregistrement de l'article.|  
|**objid**|**int**|Cette colonne n’est pas utilisée et est incluse uniquement pour rendre la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la table **IHarticles** compatible avec la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilisée pour SQL Server Articles ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**sync_objid**|**int**|Cette colonne n’est pas utilisée et est incluse uniquement pour rendre la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la table **IHarticles** compatible avec la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilisée pour SQL Server Articles ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**description**|**nvarchar(255)**|Entrée descriptive de l’article.|  
|**publisher_status**|**int**|Est utilisé pour indiquer si la vue qui définit l’article publié a été définie en appelant [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md).<br /><br /> **0**  =  [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) a été appelée.<br /><br /> **1**  =  [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) n’a pas été appelé.|  
|**article_view_owner**|**nvarchar(255)**|Propriétaire de l'objet de synchronisation sur le serveur de publication utilisé par l'Agent de lecture du journal.|  
|**article_view**|**nvarchar(255)**|Objet de synchronisation sur le serveur de publication utilisé par l'Agent de lecture du journal.|  
|**ins_scripting_proc**|**int**|Cette colonne n’est pas utilisée et est incluse uniquement pour rendre la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la table **IHarticles** compatible avec la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilisée pour SQL Server Articles ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**del_scripting_proc**|**int**|Cette colonne n’est pas utilisée et est incluse uniquement pour rendre la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la table **IHarticles** compatible avec la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilisée pour SQL Server Articles ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**upd_scripting_proc**|**int**|Cette colonne n’est pas utilisée et est incluse uniquement pour rendre la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la table **IHarticles** compatible avec la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilisée pour SQL Server Articles ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**custom_script**|**int**|Cette colonne n’est pas utilisée et est incluse uniquement pour rendre la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la table **IHarticles** compatible avec la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilisée pour SQL Server Articles ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**fire_triggers_on_snapshot**|**bit**|Cette colonne n’est pas utilisée et est incluse uniquement pour rendre la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la table **IHarticles** compatible avec la vue [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilisée pour SQL Server Articles ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**instance_id**|**int**|Identifie l'instance active du journal d'article de la table publiée.|  
|**use_default_datatypes**|**bit**|Indique si l’article utilise des mappages de type de données par défaut ; la valeur **1** indique que les mappages de type de données par défaut sont utilisés.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)  
  
  
