---
description: sp_helpdb (Transact-SQL)
title: sp_helpdb (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdb
- sp_helpdb_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdb
ms.assetid: 4c3e3302-6cf1-4b2b-8682-004049b578c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb199d0fc846bc37a8f7c1272563de99507c795
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176434"
---
# <a name="sp_helpdb-transact-sql"></a>sp_helpdb (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche des informations sur une base de données précise ou sur toutes les bases de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdb [ [ @dbname= ] 'name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @dbname = ] 'name'` Nom de la base de données pour laquelle les informations sont signalées. *Name* est de **type sysname**, sans valeur par défaut. Si le *nom* n’est pas spécifié, **sp_helpdb** des rapports sur toutes les bases de données de l’affichage catalogue **sys. databases** .  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la base de données.|  
|**db_size**|**nvarchar (13)**|Taille totale de la base de données.|  
|**propriétaire**|**sysname**|Propriétaire de la base de données, tel que **sa**.|  
|**dbid**|**smallint**|ID de la base de données.|  
|**created**|**nvarchar(11)**|Date de création de la base de données.|  
|**statut**|**nvarchar (600)**|Liste de valeurs, séparées par des virgules, d'options de base de données actuellement définies pour la base de données.<br /><br /> Les options définies par des valeurs booléennes ne sont affichées que si elles sont activées. Les options non booléennes sont répertoriées avec leurs valeurs correspondantes sous la forme d' *option_name* = *valeur*.<br /><br /> Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).|  
|**compatibility_level**|**tinyint**|Niveau de compatibilité de la base de données : 60, 65, 70, 80 ou 90.|  
  
 Si *nom* est spécifié, un jeu de résultats supplémentaire indique l’allocation de fichiers pour la base de données spécifiée.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**nchar(128)**|Nom de fichier logique.|  
|**combinaison**|**smallint**|ID de fichier.|  
|**extension**|**nchar (260)**|Nom de fichier du système d'exploitation (nom de fichier physique).|  
|**fichiers**|**nvarchar(128)**|Groupe de fichiers auquel le fichier appartient.<br /><br /> NULL = Il s'agit d'un fichier journal. Ils ne font jamais partie d'un groupe de fichiers.|  
|**size**|**nvarchar (18)**|Taille du fichier exprimée en mégaoctets.|  
|**MaxSize**|**nvarchar (18)**|Taille maximale du fichier. La valeur UNLIMITED indique que le fichier peut augmenter jusqu'à ce que le disque soit plein.|  
|**future**|**nvarchar (18)**|Incrément de croissance du fichier. Cela indique la quantité d’espace ajoutée au fichier chaque fois que de l’espace supplémentaire est nécessaire.|  
|**syntaxe**|**varchar (9)**|Utilisation du fichier. Pour un fichier de données, la valeur est **« données uniquement »** et, pour le fichier journal, la valeur est **« Journal uniquement »**.|  
  
## <a name="remarks"></a>Notes  
 La colonne **État** dans le jeu de résultats indique les options qui ont été activées dans la base de données. Toutes les options de base de données ne sont pas signalées par la colonne **État** . Pour afficher la liste complète des paramètres d’option de base de données actuels, utilisez l’affichage catalogue **sys. databases** .  
  
## <a name="permissions"></a>Autorisations  
 Lorsqu’une base de données unique est spécifiée, l’appartenance au rôle **public** dans la base de données est requise. Si aucune base de données n’est spécifiée, l’appartenance au rôle **public** dans la base de données **Master** est requise.  
  
 Si une base de données n’est pas accessible, **sp_helpdb** affiche le message d’erreur 15622 et autant d’informations que possible sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-information-about-a-single-database"></a>R. Renvoi d'informations sur une base de données unique  
 Cet exemple affiche des informations sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_helpdb N'AdventureWorks2012';  
```  
  
### <a name="b-returning-information-about-all-databases"></a>B. Renvoi d’informations sur toutes les bases de données  
 L'exemple ci-dessous affiche les informations relatives à toutes les bases de données installées sur le serveur sur lequel s'exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
EXEC sp_helpdb;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
