---
description: sysmergeschemaarticles (Transact-SQL)
title: sysmergeschemaarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergeschemaarticles_TSQL
- sysmergeschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemaarticles system table
ms.assetid: b5085979-2f76-48e1-bf3b-765a84003dd9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 579acde4f82556f176e5923a66db48c1faafe27f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210197"
---
# <a name="sysmergeschemaarticles-transact-sql"></a>sysmergeschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de suivre les articles de schéma exclusivement associés à la réplication de fusion. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de l'article de schéma exclusivement associé à la publication de fusion.|  
|**type**|**tinyint**|Type de l'article de schéma exclusivement, pouvant être :<br /><br /> **0x20** = article de schéma de procédure stockée uniquement.<br /><br /> **0x40** = article de schéma uniquement ou de vue indexée de schéma uniquement.|  
|**objid**|**int**|Identificateur de l'objet de base de l'article. L'identificateur d'objet peut être celui d'une procédure, vue, vue indexée ou fonction définie par l'utilisateur.|  
|**artid**|**uniqueidentifier**|Identificateur de l'article.|  
|**description**|**nvarchar(255)**|Description de l'article.|  
|**pre_creation_command**|**tinyint**|Action par défaut à effectuer lorsque l'article est créé dans la base de données d'abonnement :<br /><br /> **0 =** Aucun : si la table existe déjà sur l’abonné, aucune action n’est effectuée.<br /><br /> **1** = drop-supprime la table avant de la recréer.<br /><br /> **2** = DELETE : émet une suppression basée sur la clause WHERE dans le filtre de sous-ensemble.<br /><br /> **3** = tronquer-identique à **2**, mais supprime les pages au lieu des lignes. Toutefois, n'accepte pas la clause WHERE.|  
|**pubid**|**uniqueidentifier**|Identificateur unique de la publication.|  
|**statut**|**tinyint**|État de l'article de schéma exclusivement, pouvant être :<br /><br /> **1** = non synchronisé : le script de traitement initial pour publier la table s’exécute lors de la prochaine exécution du agent d’instantané.<br /><br /> **2** = actif : le script de traitement initial pour la publication de la table a été exécuté.<br /><br /> **5** = New_inactive à ajouter.<br /><br /> **6** = New_active à ajouter.|  
|**creation_script**|**nvarchar(255)**|Chemin d'accès et nom d'un script de précréation de schéma d'article facultatif utilisé pour créer une table cible.|  
|**schema_option**|**Binary(8**|Représentation graphique de l'option de génération de schéma relative à l'article de schéma exclusivement, pouvant être le résultat logique au niveau du bit OU le résultat d'une ou plusieurs des valeurs suivantes :<br /><br /> **0x00** = désactive les scripts par le agent d’instantané et utilise le CreationScript fourni.<br /><br /> **0x01** = générer la création de l’objet (Create table, créer la procédure, etc.).<br /><br /> **0x10** = générer un index cluster correspondant.<br /><br /> **0x20** = convertir les types de données définis par l’utilisateur en types de données de base.<br /><br /> **0x40** = générer des index ou index non cluster correspondants.<br /><br /> **0x80** = inclut l’intégrité référentielle déclarée sur les clés primaires.<br /><br /> **0x100** = répliquer les déclencheurs utilisateur sur un article de table, s’il est défini.<br /><br /> **0x200** = répliquer les contraintes de clé étrangère. Si la table référencée ne fait pas partie d'une publication, aucune contrainte de clé étrangère appliquée à une table publiée n'est répliquée.<br /><br /> **0x400** = répliquer les contraintes de validation.<br /><br /> **0x800** = répliquer les valeurs par défaut.<br /><br /> **0x1000** = répliquer le classement au niveau des colonnes.<br /><br /> **0x2000** = répliquer les propriétés étendues associées à l’objet source de l’article publié.<br /><br /> **0x4000** = répliquer les clés uniques si elles sont définies sur un article de table.<br /><br /> **0x8000** = répliquer une clé primaire et des clés uniques sur un article de table en tant que contraintes à l’aide d’instructions ALTER TABLE.<br /><br /> Pour plus d’informations sur les valeurs possibles de **schema_option**, consultez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nom de l'objet de destination dans la base de données d'abonnement. Cette valeur s'applique uniquement aux articles de schéma exclusivement, tels que les procédures stockées, les vues et les fonctions définies par l'utilisateur.|  
|**destination_owner**|**sysname**|Propriétaire de l’objet dans la base de données d’abonnement, s’il ne s’agit pas de **dbo**.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
