---
description: sys.sysprotects (Transact-SQL)
title: sys.sysprotège (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysprotects
- sys.sysprotects_TSQL
- sys.sysprotects
- sysprotects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysprotects compatibility view
- sysprotects system table
ms.assetid: 49c9658d-fb51-4c77-94a0-fba699b0102d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 764007fdbe5148b7077c8f9ffdab9c3e77244ef1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158507"
---
# <a name="syssysprotects-transact-sql"></a>sys.sysprotects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations relatives aux autorisations appliquées aux comptes de sécurité dans la base de données à l'aide des instructions GRANT et DENY.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l'objet auquel ces autorisations s'appliquent.|  
|**codé**|**smallint**|ID de l'utilisateur ou du groupe auquel ces autorisations s'appliquent. Déborde ou retourne la valeur NULL si le nombre d'utilisateurs et de rôles dépasse 32 767.|  
|**action**|**tinyint**|Peut présenter l'une des autorisations suivantes :<br /><br /> 26 = REFERENCES<br /><br /> 178 = CREATE FUNCTION<br /><br /> 193 = SELECT<br /><br /> 195 = INSERTION<br /><br /> 196 = SUPPRESSION<br /><br /> 197 = MISE À JOUR<br /><br /> 198 = CREATE TABLE<br /><br /> 203 = CREATE DATABASE<br /><br /> 207 = CREATE VIEW<br /><br /> 222 = CREATE PROCEDURE<br /><br /> 224 = EXECUTE<br /><br /> 228 = BACKUP DATABASE<br /><br /> 233 = CREATE DEFAULT<br /><br /> 235 = BACKUP LOG<br /><br /> 236 = CREATE RULE|  
|**protecttype**|**tinyint**|Peut avoir les valeurs suivantes :<br /><br /> 204 = GRANT_W_GRANT<br /><br /> 205 = GRANT<br /><br /> 206 = DENY|  
|**colonnes**|**varbinary(8000)**|Bitmap des colonnes auxquelles ces autorisations SELECT ou UPDATE s'appliquent.<br /><br /> Bit 0 = Toutes les colonnes.<br /><br /> Bit 1 = Les autorisations s'appliquent à cette colonne.<br /><br /> NULL = Aucune information.|  
|**fournisseur**|**smallint**|ID de l'utilisateur qui a octroyé les autorisations GRANT ou DENY. Déborde ou retourne la valeur NULL si le nombre d'utilisateurs et de rôles dépasse 32 767.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
