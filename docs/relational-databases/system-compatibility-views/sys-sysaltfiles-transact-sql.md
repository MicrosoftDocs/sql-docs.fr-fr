---
description: sys.sysaltfiles (Transact-SQL)
title: sys.sysaltfiles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysaltfiles_TSQL
- sys.sysaltfiles
- sysaltfiles_TSQL
- sysaltfiles
dev_langs:
- TSQL
helpviewer_keywords:
- sysaltfiles system table
- sys.sysaltfiles compatibility view
ms.assetid: 698dec23-5336-4108-87a5-f8e407f8da09
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f6139fb221ab1d960d8cfb455e592ac16fbcee09
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097796"
---
# <a name="syssysaltfiles-transact-sql"></a>sys.sysaltfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Sous certaines conditions, contient des lignes correspondant aux fichiers d'une base de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**combinaison**|**smallint**|Numéro d'identification du fichier. Il est unique pour chaque base de données.|  
|**GroupID**|**smallint**|Numéro d'identification du groupe de fichiers.|  
|**size**|**int**|Taille du fichier, en pages de 8 kilo-octets (Ko).|  
|**MaxSize**|**int**|Taille maximale du fichier, en pages de 8 Ko.<br /><br /> 0 = Croissance nulle.<br /><br /> -1 = Le fichier peut croître tant que le disque n'est pas saturé.<br /><br /> 268435456 = Le fichier journal peut croître pour atteindre une taille maximale de 2 To.<br /><br /> Remarque : les bases de données mises à niveau avec une taille de fichier journal illimitée signalent-1 pour la taille maximale du fichier journal.|  
|**future**|**int**|Taille de croissance de la base de données.<br /><br /> 0 = Croissance nulle. Il peut s'agir du nombre de pages ou du pourcentage de la taille du fichier selon la valeur de status. Si l' **État** est de la valeur de la taille de l’extension, la **croissance** est le pourcentage de taille de fichier ; dans le cas contraire, il s’agit du nombre de pages.|  
|**statut**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**relevant**|**int**|Réservé.|  
|**dbid**|**smallint**|Numéro d'identification de la base de données à laquelle le fichier appartient.|  
|**name**|**sysname**|Nom logique du fichier.|  
|**extension**|**nvarchar(260)**|Nom de l'unité physique. Inclut le chemin d'accès complet du fichier.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
