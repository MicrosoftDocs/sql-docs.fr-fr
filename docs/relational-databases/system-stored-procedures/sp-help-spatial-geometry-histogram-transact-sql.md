---
description: sp_help_spatial_geometry_histogram (Transact-SQL)
title: sp_help_spatial_geometry_histogram (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_histogram
- sp_help_spatial_geometry_histogram_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_histogram
ms.assetid: 036aaf61-df3e-40f7-aa4e-62983c5a37bd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 75609570d3c021b350fc06cbaf35f32dee4bd8ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183607"
---
# <a name="sp_help_spatial_geometry_histogram-transact-sql"></a>sp_help_spatial_geometry_histogram (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Facilite le codage du rectangle englobant et des paramètres de grille pour un index spatial.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_spatial_geometry_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @xmin = ] 'minx' ]   
     [ , [ @ymin = ] 'miny' ]   
     [ ,.[ @xmax = ] 'maxx' ]  
     [ , [ @ymax = ] 'maxy' ]  
     [ , [ @sample = ] 'sample' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @tabname = ] 'tabname'` Nom qualifié ou non qualifié de la table pour laquelle l’index spatial a été spécifié.  
  
 Les guillemets ne sont nécessaires que si une une table qualifiée est spécifiée. Si un nom qualifié complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données active. *tabname* est de **type sysname**, sans valeur par défaut.  
  
`[ @colname = ] 'colname'` Nom de la colonne spatiale spécifiée. *colname* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @resolution = ] 'resolution'` Résolution du cadre englobant. Les valeurs possibles sont comprises entre 10 et 5000. la *résolution* est de **type tinyint**, sans valeur par défaut.  
  
`[ @xmin = ] 'xmin'` Est la propriété de zone englobante minimale X. *xmin* est de **type float**, sans valeur par défaut.  
  
`[ @ymin = ] 'ymin'` Propriété de zone englobante minimale Y. *ymin* est de **type float**, sans valeur par défaut.  
  
`[ @xmax = ] 'xmax'` Est la propriété du cadre englobant X-maximum. *Xmax* est de **type float**, sans valeur par défaut.  
  
`[ @ymax = ] 'ymax'` Propriété du rectangle englobant Y-maximum. *ymax* est de **type float**, sans valeur par défaut.  
  
`[ @sample = ] 'sample'` Pourcentage de la table utilisée. Les valeurs valides sont comprises entre 0 et 100. l' *exemple* est un **float**. La valeur par défaut est 100.  
  
## <a name="property-valuereturn-value"></a>Valeur de propriété/valeur de retour  
 Une valeur de table est retournée. La grille suivante décrit le contenu de colonne de la table.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|Représente l'ID unique de chaque cellule ; le comptage démarre à 1.|  
|**cellules**|**geometry**|Polygone rectangulaire qui représente chaque cellule. La forme de cellule est identique à la forme de cellule utilisée pour l'indexation spatiale.|  
|**row_count**|**bigint**|Indique le nombre d'objets spatiaux qui touchent ou contiennent la cellule.|  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit être membre du rôle **public** . Nécessite une autorisation READ ACCESS sur le serveur et l'objet.  
  
## <a name="remarks"></a>Notes  
 L'onglet spatial SSMS affiche une représentation graphique des résultats. Vous pouvez interroger les résultats dans la fenêtre spatiale afin d'obtenir un nombre approximatif d'éléments de résultat. Les objets dans la table peuvent couvrir plusieurs cellules ; par conséquent, la somme de cellules peut être supérieure au nombre d'objets réels.  
  
 Une ligne supplémentaire peut être ajoutée au jeu de résultats qui conserve le nombre d'objets qui se trouvent en dehors du rectangle englobant ou qui touchent la bordure du rectangle englobant. Le **CellID** de cette ligne est 0 et la **cellule** de cette ligne contient un **LineString** qui représente le cadre englobant. Cette ligne représente l'espace entier à l'extérieur du rectangle englobant.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée un exemple de table, puis appelle **sp_help_spatial_geometry_histogram** sur la table.  
  
 `USE AdventureWorksDW2012`  
  
 `GO`  
  
 `-- Set database compatibility for circular arc segments`  
  
 `ALTER DATABASE AdventureWorksDW2012`  
  
 `SET COMPATIBILITY_LEVEL = 110;`  
  
 `GO`  
  
 `-- Create table to execute sp_help_spatial_geometry_histogram on`  
  
 `CREATE TABLE TownSites`  
  
 `(`  
  
 `Location geometry NULL,`  
  
 `SiteName nvarchar(50) NULL`  
  
 `)`  
  
 `GO`  
  
 `-- Insert site data into table`  
  
 `DECLARE @g geometry;`  
  
 `SET @g = geometry::Parse('POINT(0 0)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Booth Map';`  
  
 `SET @g = geometry::Parse('POLYGON((1 1, 1 2, 2 2, 2 1, 1 1))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Town Hall';`  
  
 `SET @g = geometry::Parse('CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-1 0, 0 -1, 1 0),(1 0, 1 2, -1 0)))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Park';`  
  
 `SET @g = geometry::Parse('CIRCULARSTRING(1 5, 2 2, 5 1)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Road';`  
  
 `-- Call proc to see data within bounding box`  
  
 `EXEC sp_help_spatial_geometry_histogram @tabname = TownSites, @colname = Location, @resolution = 64, @xmin = -2, @ymin = -2, @xmax = 3, @ymax = 3, @sample = 100;`  
  
 `GO`  
  
 `DROP TABLE TownSites;`  
  
 `GO`  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées d’index spatial &#40;Transact-SQL&#41;](./spatial-index-stored-procedures-arguments-and-properties.md)  
  
