---
description: sp_showrowreplicainfo (Transact-SQL)
title: sp_showrowreplicainfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_showrowreplicainfo_TSQL
- sp_showrowreplicainfo
helpviewer_keywords:
- sp_showrowreplicainfo
ms.assetid: 6a9dbc1a-e1e1-40c4-97cb-8164a2288f76
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f2a66e8e6123adef864ca2beadef31cd4a9cbffe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193491"
---
# <a name="sp_showrowreplicainfo-transact-sql"></a>sp_showrowreplicainfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche des informations concernant une ligne d'une table utilisée en tant qu'article dans une réplication de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_showrowreplicainfo [ [ @ownername = ] 'ownername' ]  
    [ , [ @tablename =] 'tablename' ]   
        , [ @rowguid =] rowguid   
    [ , [ @show = ] 'show' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @ownername = ] 'ownername'` Nom du propriétaire de la table. *OwnerName* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre est utile pour différencier les tables si une base de données contient plusieurs tables du même nom, chacune de ces tables ayant un propriétaire différent.  
  
`[ @tablename = ] 'tablename'` Nom de la table qui contient la ligne pour laquelle les informations sont retournées. *TableName* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @rowguid = ] rowguid` Identificateur unique de la ligne. *rowguid* est de type **uniqueidentifier** et n’a pas de valeur par défaut.  
  
`[ @show = ] 'show'` Détermine la quantité d’informations à retourner dans le jeu de résultats. *Show* est de type **nvarchar (20),** avec les deux valeurs par défaut. Si la **ligne** est, seules les informations de version de ligne sont retournées. Si les **colonnes** sont, seules les informations de version de colonne sont retournées. Dans **les deux** cas, les informations de ligne et de colonne sont retournées.  
  
## <a name="result-sets-for-row-information"></a>Ensemble de résultats pour les informations de ligne  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**server_name**|**sysname**|Nom du serveur hébergeant la base de données qui a effectué l'entrée de la version de ligne.|  
|**db_name**|**sysname**|Nom de la base de données qui a effectué cette entrée.|  
|**db_nickname**|**binary(6)**|Surnom de la base de données qui a effectué cette entrée.|  
|**version**|**int**|Version de l'entrée.|  
|**current_state**|**nvarchar (9)**|Retourne des informations sur l'état actuel de la ligne.<br /><br /> les données de ligne **y** représentent l’état actuel de la ligne.<br /><br /> les données de **n** lignes ne représentent pas l’état actuel de la ligne.<br /><br /> **\<n/a>** -Non applicable.<br /><br /> **\<unknown>** -Impossible de déterminer l’état actuel.|  
|**rowversion_table**|**nchar (17)**|Indique si les versions de ligne sont stockées dans la table [MSmerge_contents](../../relational-databases/system-tables/msmerge-contents-transact-sql.md) ou dans la table [MSmerge_tombstone](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md) .|  
|**Commentaire**|**nvarchar(255)**|Informations supplémentaires concernant l'entrée de version de cette ligne. En général, ce champ est vide.|  
  
## <a name="result-sets-for-column-information"></a>Ensemble de résultats pour les informations de colonne  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**server_name**|**sysname**|Nom du serveur hébergeant la base de données qui a effectué l'entrée de la version de colonne.|  
|**db_name**|**sysname**|Nom de la base de données qui a effectué cette entrée.|  
|**db_nickname**|**binary(6)**|Surnom de la base de données qui a effectué cette entrée.|  
|**version**|**int**|Version de l'entrée.|  
|**colname**|**sysname**|Nom de la colonne d'article que l'entrée de la version de colonne représente.|  
|**Commentaire**|**nvarchar(255)**|Informations supplémentaires concernant l'entrée de version de cette colonne. En général, ce champ est vide.|  
  
## <a name="result-set-for-both"></a>Ensemble de résultats pour la ligne et la colonne  
 Si la valeur **both** est choisie pour *Show*, les jeux de résultats de ligne et de colonne sont retournés.  
  
## <a name="remarks"></a>Notes  
 **sp_showrowreplicainfo** est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 **sp_showrowreplicainfo** ne peut être exécutée que par les membres du rôle de base de données fixe **db_owner** sur la base de données de publication ou par les membres de la liste d’accès à la publication (PAL) sur la base de données de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Détecter et résoudre les conflits de réplication de fusion](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
