---
description: sysschemaarticles (Transact-SQL)
title: sysschemaarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysschemaarticles_TSQL
- sysschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysschemaarticles system table
ms.assetid: 67a1c039-c283-4a9c-bacc-b9b3973590c3
author: cawrites
ms.author: chadam
ms.openlocfilehash: ecefc7fe927f5013d5647c276cf4dc1e687c0ff0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102700"
---
# <a name="sysschemaarticles-transact-sql"></a>sysschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de suivre les articles de schéma uniquement pour des publications transactionnelles et d'instantané. Cette table est stockée dans la base de données de publication.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Identificateur de l'article.|  
|**creation_script**|**nvarchar(255)**|Chemin d'accès et nom d'un script de schéma d'article utilisé pour créer la table cible.|  
|**description**|**nvarchar(255)**|Entrée descriptive de l’article.|  
|**dest_object**|**sysname**|Nom de l'objet dans la base de données d'abonnement si l'article est un article de schéma uniquement, tel qu'une procédure stockée, vue ou fonction définie par l'utilisateur.|  
|**name**|**sysname**|Nom de l'article de schéma uniquement dans une publication.|  
|**objid**|**int**|Identificateur de l'objet de base de l'article. Il peut s’agir de l’identificateur d’objet d’une procédure, d’une vue, d’une vue, d’une vue ou d’une fonction définie par l’index.|  
|**pubid**|**int**|ID de la publication.|  
|**pre_creation_cmd**|**tinyint**|Indique l'action que doit entreprendre le système s'il détecte un objet existant de même nom sur l'Abonné lors de l'application de l'instantané pour cet article :<br /><br /> **0** = rien.<br /><br /> **1** = supprimer la table de destination.<br /><br /> **2** = supprimer la table de destination.<br /><br /> **3** = tronquer la table de destination.|  
|**statut**|**int**|Bitmap utilisé pour indiquer l'état de l'article.|  
|**type**|**tinyint**|Les valeurs indiquant le type d'article de schéma uniquement sont les suivantes :<br /><br /> **32** = procédure stockée.<br /><br /> **64** = vue ou vue indexée. <br /><br /> **96** = fonction d’agrégation.<br /><br /> **128** = fonction.|  
|**schema_option**|**Binary(8**|Masque de masque de l’option de génération de schéma pour l’article donné. Il spécifie la création automatique de la procédure stockée dans la base de données de destination pour toute syntaxe CALL/MCALL/XCALL, et il peut correspondre au résultat OR logique au niveau du bit d'une ou plusieurs des valeurs suivantes :<br /><br /> **0x00** = désactive les scripts par le agent d’instantané et utilise *creation_script*.<br /><br /> **0x01** = génère la création de l’objet (Create table, créer la procédure, etc.). Il s’agit de la valeur par défaut pour les Articles de procédure stockée.<br /><br /> **0x02** = génère des procédures stockées personnalisées pour l’article, s’il est défini.<br /><br /> **0x10** = génère un index cluster correspondant.<br /><br /> **0x20** = convertit les types de données définis par l’utilisateur en types de données de base.<br /><br /> **0x40**= génère des index non cluster correspondants.<br /><br /> **0x80**= comprend l’intégrité référentielle déclarée sur les clés primaires.<br /><br /> **0x73** = génère l’instruction CREATE TABLE, crée des index cluster et non-cluster, convertit les types de données définis par l’utilisateur en types de données de base et génère des scripts de procédure stockée personnalisés à appliquer à l’abonné. Cette valeur est la valeur par défaut pour tous les articles, à l'exception des articles de procédure stockée.<br /><br /> **0x100**= réplique les déclencheurs utilisateur sur un article de table, s’il est défini.<br /><br /> **0x200**= réplique les contraintes de clé étrangère. Si la table référencée ne fait pas partie d'une publication, aucune contrainte de clés étrangères appliquée à une table publiée n'est répliquée.<br /><br /> **0x400**= réplique les contraintes de validation.<br /><br /> **0x800**= réplique les valeurs par défaut.<br /><br /> **0x1000**= réplique le classement au niveau des colonnes.<br /><br /> **0x2000**= réplique les propriétés étendues associées à l’objet source de l’article publié.<br /><br /> **0x4000**= réplique les clés uniques si elles sont définies sur un article de table.<br /><br /> **0x8000**= réplique la clé primaire et les clés uniques sur un article de table en tant que contraintes à l’aide d’instructions ALTER TABLE.|  
|**dest_owner**|**sysname**|Propriétaire de la table dans la base de données de destination|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
