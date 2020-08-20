---
description: MSdbms_datatype_mapping (Transact-SQL)
title: MSdbms_datatype_mapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_datatype_mapping_TSQL
- MSdbms_datatype_mapping
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype_mapping system table
ms.assetid: 13289a0b-dfb0-4771-ad80-4c5f83cded99
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: bfdf9ab815a1623aa6c323c256b9f7a4d4e27263
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88488786"
---
# <a name="msdbms_datatype_mapping-transact-sql"></a>MSdbms_datatype_mapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSdbms_datatype_mapping** contient les mappages de type de données autorisés à partir du type de données du système de gestion de base de données source (SGBD) vers un ou plusieurs types de données spécifiques du SGBD de destination. Cette table est stockée dans la base de données **msdb** et est utilisée pour la réplication de bases de données hétérogènes.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**datatype_mapping_id**|**int**|Identifie chaque mappage de type de données unique.|  
|**map_id**|**int**|Identifie le type de données source.|  
|**dest_datatype_id**|**int**|Identifie le type de données de destination.|  
|**dest_precision**|**bigint**|Définit la précision du type de données de destination, où la valeur NULL signifie que la précision n’est pas utilisée et la valeur **-1** signifie que la précision du type de données source est utilisée.|  
|**dest_scale**|**int**|Définit l’échelle du type de données de destination, où la valeur NULL signifie que l’échelle n’est pas utilisée et la valeur **-1** signifie que l’échelle du type de données source est utilisée.|  
|**dest_length**|**bigint**|Définit la longueur du type de données de destination, où la valeur NULL signifie que la longueur n’est pas utilisée et la valeur **-1** signifie que la longueur du type de données source est utilisée.|  
|**dest_nullable**|**bit**|Indique si la colonne de destination dans le mappage autorise les valeurs NULL, où une valeur NULL signifie que cette définition n'est pas requise.|  
|**dest_createparams**|**int**|Bitmap décrivant la combinaison de longueur, précision et échelle applicable à chaque type de données, qui inclut :<br /><br /> **0x1** = précision.<br /><br /> **0X2** = échelle.<br /><br /> **0x4** = longueur.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Spécifier des mappages de types de données pour un serveur de publication Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
