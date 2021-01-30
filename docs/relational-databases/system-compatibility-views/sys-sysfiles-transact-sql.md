---
description: sys.sysfiles (Transact-SQL)
title: Fichiers sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysfiles
- sys.sysfiles_TSQL
- sys.sysfiles
- sysfiles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysfiles system table
- sys.sysfiles compatibility view
ms.assetid: 3b47f38d-1cff-404d-89d3-9342c451c802
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34ec2c47be129384cca6be84894251a74cb996f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201378"
---
# <a name="syssysfiles-transact-sql"></a>sys.sysfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque fichier de la base de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**combinaison**|**smallint**|Numéro unique d'identification de fichier pour chaque base de données.|  
|**GroupID**|**smallint**|Numéro d'identification du groupe de fichiers.|  
|**size**|**int**|Taille du fichier, en pages de 8 Ko.|  
|**MaxSize**|**int**|Taille maximale du fichier, en pages de 8 Ko.<br /><br /> 0 = Croissance nulle.<br /><br /> -1 = Le fichier peut croître tant que le disque n'est pas saturé.<br /><br /> 268435456 = Le fichier journal peut croître pour atteindre une taille maximale de 2 To.<br /><br /> Remarque : les bases de données mises à niveau avec une taille de fichier journal illimitée signalent-1 pour la taille maximale du fichier journal.|  
|**future**|**int**|Taille de croissance de la base de données. Il peut s’agir du nombre de pages ou du pourcentage de la taille du fichier, en fonction de la valeur de l' **État**.<br /><br /> 0 = Croissance nulle.|  
|**statut**|**int**|Bits d’état de la valeur de **croissance** en mégaoctets (Mo) ou kilo-octets (Ko).<br /><br /> 0x2 = Fichier disque.<br /><br /> 0x40 = Fichier journal.<br /><br /> 0x100000 = Croissance. Cette valeur est un pourcentage et non le nombre de pages.|  
|**relevant**|**int**|Réservé.|  
|**name**|**sysname**|Nom logique du fichier.|  
|**extension**|**nvarchar(260)**|Nom de l'unité physique. Inclut le chemin d'accès complet du fichier.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
