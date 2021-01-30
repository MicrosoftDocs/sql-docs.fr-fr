---
description: sys.fulltext_catalogs (Transact-SQL)
title: sys.fulltext_catalogs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_catalogs_TSQL
- sys.fulltext_catalogs
- fulltext_catalogs
- fulltext_catalogs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_catalogs catalog view
ms.assetid: cf1489ff-4819-41fa-a62a-4ed797a16207
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 38420a12b6b6cab661644ef8d176c6e08c5e0c28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191522"
---
# <a name="sysfulltext_catalogs-transact-sql"></a>sys.fulltext_catalogs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque catalogue de texte intégral.  
  
> [!NOTE]  
>  Les colonnes suivantes seront supprimées dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **data_space_id**, **file_id** et **path**. Évitez d'utiliser ces colonnes dans de nouveaux travaux de développement et modifiez dès que possible les applications qui les utilisent actuellement.  
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|fulltext_catalog_id|**int**|Identificateur du catalogue de texte intégral. Il est unique parmi les catalogues de texte intégral de la base de données.|  
|name|**sysname**|Nom du catalogue. Unique dans la base de données.|  
|path|**nvarchar(260)**|Nom du répertoire du catalogue dans le système de fichiers.|  
|is_default|**bit**|Catalogue de texte intégral par défaut.<br /><br /> True = est la valeur par défaut.<br /><br /> False = n'est pas la valeur par défaut.|  
|is_accent_sensitivity_on|**bit**|Paramètre indiquant le respect des accents pour le catalogue.<br /><br /> True = respecte les accents.<br /><br /> False = ne respecte pas les accents.|  
|data_space_id|**int**|Groupe de fichiers dans lequel le catalogue a été créé.|  
|file_id|**int**|ID du fichier de texte intégral associé au catalogue.|  
|principal_id|**int**|ID du principal de base de données propriétaire du catalogue de texte intégral.|  
|is_importing|**bit**|Indique si le catalogue de texte intégral est en cours d'importation :<br /><br /> 1 = le catalogue est en cours d'importation.<br /><br /> 2 = le catalogue n'est pas en cours d'importation.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [ALTER FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)  
  
  
