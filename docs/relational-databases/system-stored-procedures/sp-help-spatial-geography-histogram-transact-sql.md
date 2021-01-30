---
description: sp_help_spatial_geography_histogram (Transact-SQL)
title: sp_help_spatial_geography_histogram (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geography_histogram_TSQL
- sp_help_spatial_geography_histogram
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geography_histogram
ms.assetid: 5c5bd319-055d-4cd6-8c5a-06354cc056cc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3554c8b385a14f588c4f7f8dd6248ba36ad7b16d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212063"
---
# <a name="sp_help_spatial_geography_histogram-transact-sql"></a>sp_help_spatial_geography_histogram (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Facilite le codage des paramètres de grille pour un index spatial.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_spatial_geography_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @sample = ] 'tablesample' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @tabname = ] 'tabname'` Nom qualifié ou non qualifié de la table pour laquelle l’index spatial a été spécifié.  
  
 Les guillemets ne sont nécessaires que si une une table qualifiée est spécifiée. Si un nom qualifié complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données active. *tabname* est de **type sysname**, sans valeur par défaut.  
  
`[ @colname = ] 'columnname'` Nom de la colonne spatiale spécifiée. *ColumnName* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @resolution = ] 'resolution'` Résolution du cadre englobant. Les valeurs possibles sont comprises entre 10 et 5000. la *résolution* est de **type tinyint**, sans valeur par défaut.  
  
`[ @sample = ] 'sample'` Pourcentage de la table utilisée. Les valeurs valides sont comprises entre 0 et 100. *TABLESAMPLE* est de **type float**. La valeur par défaut est 100.  
  
## <a name="property-valuereturn-value"></a>Valeur de propriété/valeur de retour  
 Une valeur de table est retournée. La grille suivante décrit le contenu de colonne de la table.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|Représente l'ID unique de chaque cellule, avec un nombre initial de 1.|  
|**cellules**|**Geography**|Polygone rectangulaire qui représente chaque cellule. La forme de cellule est identique à la forme de cellule utilisée pour l'indexation spatiale.|  
|**row_count**|**bigint**|Indique le nombre d'objets spatiaux qui touchent ou contiennent la cellule.|  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit être membre du rôle **public** . Nécessite une autorisation READ ACCESS sur le serveur et l'objet.  
  
## <a name="remarks"></a>Notes  
 L'onglet spatial SSMS affiche une représentation graphique des résultats. Vous pouvez interroger les résultats dans la fenêtre spatiale afin d'obtenir un nombre approximatif d'éléments de résultat.  
  
> [!NOTE]  
>  Les objets dans la table peuvent couvrir plusieurs cellules ; par conséquent, la somme de cellules de la table peut être supérieure au nombre d'objets réels.  
  
 Le cadre englobant pour le type **Geography** est le monde entier.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant appelle  **sp_help_spatial_geography_histogram** sur la `Person.Address` table dans la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de données.  
  
```  
EXEC sp_help_spatial_geography_histogram @tabname = Person.Address, @colname = SpatialLocation, @resolution = 64, @sample = 30;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées d’index spatial &#40;Transact-SQL&#41;](./spatial-index-stored-procedures-arguments-and-properties.md)  
  
