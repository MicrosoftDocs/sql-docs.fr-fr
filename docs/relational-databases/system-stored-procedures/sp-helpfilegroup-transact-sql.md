---
description: sp_helpfilegroup (Transact-SQL)
title: sp_helpfilegroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpfilegroup_TSQL
- sp_helpfilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpfilegroup
ms.assetid: 619716b5-95dc-4538-82ae-4b90b9da8ebc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39899feae9ad7b4cd6f2ab45caf11ddd3e89cd98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204406"
---
# <a name="sp_helpfilegroup-transact-sql"></a>sp_helpfilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie le nom et les attributs des groupes de fichiers associés à la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpfilegroup [ [ @filegroupname = ] 'name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @filegroupname = ] 'name'` Nom logique d’un groupe de fichiers de la base de données active. *Name* est de **type sysname**, avec NULL comme valeur par défaut. Si le *nom* n’est pas spécifié, tous les groupes de fichiers de la base de données active sont répertoriés et seul le premier jeu de résultats affiché dans la section ensembles de résultats s’affiche.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**GroupName**|**sysname**|Nom du groupe de fichiers|  
|**GroupID**|**smallint**|Identificateur numérique du groupe de fichiers.|  
|**FileCount**|**int**|Nombre de fichiers appartenant au groupe de fichiers.|  
  
 Si le *nom* est spécifié, une ligne est retournée pour chaque fichier dans le groupe de fichiers.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**file_in_group**|**sysname**|Nom logique du fichier dans le groupe de fichiers.|  
|**combinaison**|**smallint**|Identificateur numérique du fichier.|  
|**extension**|**nchar (260)**|Nom physique du fichier, y compris le chemin d'accès du répertoire.|  
|**size**|**nvarchar(15**|Taille du fichier en kilo-octets.|  
|**MaxSize**|**nvarchar(15**|Taille maximale du fichier.<br /><br /> Taille maximale que peut atteindre le fichier. La valeur UNLIMITED indique que le fichier peut augmenter jusqu'à ce que le disque soit plein.|  
|**future**|**nvarchar(15**|Incrément de croissance du fichier. Quantité d'espace ajoutée au fichier chaque fois que de l'espace supplémentaire est nécessaire.<br /><br /> 0 = La taille du fichier est fixe et n'augmente pas.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-all-filegroups-in-a-database"></a>R. Renvoi de tous les groupes de fichiers d'une base de données  
 L'exemple suivant renvoie des informations sur les groupes de fichiers dans la base de données exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfilegroup;  
GO  
```  
  
### <a name="b-returning-all-files-in-a-filegroup"></a>B. Renvoi de tous les fichiers d'un groupe de fichiers  
 L'exemple suivant renvoie des informations sur tous les fichiers du groupe de fichiers `PRIMARY` dans la base de données exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfilegroup 'PRIMARY';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpfile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Groupes de fichiers et fichiers de base de données](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
