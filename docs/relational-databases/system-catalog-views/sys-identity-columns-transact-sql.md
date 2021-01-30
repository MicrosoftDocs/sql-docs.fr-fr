---
description: sys.identity_columns (Transact-SQL)
title: sys.identity_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- identity_columns
- sys.identity_columns
- sys.identity_columns_TSQL
- identity_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.identity_columns catalog view
ms.assetid: 97ee01e6-9c9e-4fd9-884b-68b4084669d5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3c375ef30ef9c5a3c76b61bb2eb06a4aa3bb611a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193823"
---
# <a name="sysidentity_columns-transact-sql"></a>sys.identity_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne par colonne d'identité.  
  
 La vue **sys.identity_columns** hérite des lignes de la vue **sys. Columns** . La vue **sys.identity_columns** retourne les colonnes de la **vue sys. Columns** , ainsi que les colonnes **seed_value**, **increment_value**, **LAST_VALUE** et **is_not_for_replication** . Pour plus d’informations, consultez [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<columns inherited from sys.columns>**||La vue **sys.identity_columns** retourne toutes les colonnes de la vue **sys. Columns** . Elle retourne également les colonnes supplémentaires décrites ci-après. Pour obtenir une description des colonnes que la vue **sys.identity_columns** hérite de **sys. Columns**, consultez [sys. Columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).|  
|**seed_value**|**sql_variant**|Valeur de départ de cette colonne d'identité. Le type de données de la valeur de départ est le même que celui de la colonne proprement dite.|  
|**increment_value**|**sql_variant**|Valeur d'incrément de cette colonne d'identité. Le type de données de la valeur de départ est le même que celui de la colonne proprement dite.|  
|**last_value**|**sql_variant**|Dernière valeur générée pour cette colonne d'identité. Le type de données de la valeur de départ est le même que celui de la colonne proprement dite.|  
|**is_not_for_replication**|**bit**|La colonne d'identité est déclarée NOT FOR REPLICATION. **Remarque :** Cette colonne ne s’applique pas à Azure Synapse Analytics.|  
  
> [!NOTE]  
>  Pour créer un numéro à incrémentation automatique qui peut être utilisé dans plusieurs tables ou être appelé par des applications sans faire référence à une table, consultez [Numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
