---
description: sp_helpfile (Transact-SQL)
title: sp_helpfile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpfile
- sp_helpfile_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpfile
ms.assetid: 1546e0ae-5a99-4e01-9eb9-d147fa65884c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 87bf389e2cd28fdf031308c116c32105d2f3c215
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204432"
---
# <a name="sp_helpfile-transact-sql"></a>sp_helpfile (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne les noms physiques et les attributs des fichiers associés à la base de données active. Utilisez cette procédure stockée pour déterminer les noms des fichiers à associer ou à dissocier du serveur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpfile [ [ @filename= ] 'name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @filename = ] 'name'` Nom logique d’un fichier de la base de données active. *Name* est de **type sysname**, avec NULL comme valeur par défaut. Si le *nom* n’est pas spécifié, les attributs de tous les fichiers de la base de données active sont retournés.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de fichier logique.|  
|**combinaison**|**smallint**|Identificateur numérique du fichier. N’est pas retourné si *Name* est spécifié *.*|  
|**extension**|**nchar (260)**|Nom de fichier physique.|  
|**fichiers**|**sysname**|Groupe de fichiers auquel le fichier appartient.<br /><br /> NULL = Le fichier est un fichier journal. Ils ne font jamais partie d'un groupe de fichiers.|  
|**size**|**nvarchar(15**|Taille du fichier en kilo-octets.|  
|**MaxSize**|**nvarchar(15**|Taille maximale du fichier. La valeur UNLIMITED indique que le fichier peut augmenter jusqu'à ce que le disque soit plein.|  
|**future**|**nvarchar(15**|Incrément de croissance du fichier. Cela indique la quantité d’espace ajoutée au fichier chaque fois que de l’espace supplémentaire est nécessaire.<br /><br /> 0 = La taille du fichier est fixe et n'augmente pas.|  
|**syntaxe**|**varchar (9)**|Pour un fichier de données, la valeur est **« données uniquement »** et, pour le fichier journal, la valeur est **« Journal uniquement »**.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les fichiers de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfile;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Groupes de fichiers et fichiers de base de données](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
