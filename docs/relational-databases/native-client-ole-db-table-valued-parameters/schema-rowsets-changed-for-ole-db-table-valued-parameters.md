---
description: Les ensembles de lignes de schéma ont été modifiés pour les paramètres de Table-Valued OLE DB dans SQL Server Native Client
title: Ensembles de lignes de schéma, OLE DB Table-Valued paramètres
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- schema rowsets [OLE DB]
- table-valued parameters (OLE DB), schema rowsets changed for (OLE DB)
ms.assetid: 581e3ead-53db-44da-8718-f3fc4b5108f1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a5daac97d996eee396607fb62b19fe4352f3995f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469260"
---
# <a name="schema-rowsets-changed-for-ole-db-table-valued-parameters-in-sql-server-native-client"></a>Les ensembles de lignes de schéma ont été modifiés pour les paramètres de Table-Valued OLE DB dans SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le tableau suivant répertorie les ensembles de lignes de schéma qui ont été modifiés ou ajoutés pour prendre en charge des paramètres table.  
  
|Ensemble de lignes de schéma|Description|  
|-------------------|-----------------|  
|DBSCHEMA_PROCEDURE_PARAMETERS|Deux nouvelles colonnes ont été ajoutées à la fin de l'ensemble de lignes nommé SS_TYPE_CATALOG_NAME et SS_TYPE_SCHEMANAME. Ces colonnes peuvent être réutilisées pour des types futurs. Les colonnes TYPE_NAME et LOCAL_TYPE_NAME contiendront le nom du type TABLE de paramètre table. La colonne DATA_TYPE aura la valeur DBTYPE_TABLE = 143 pour les paramètres table.|  
|DBSCHEMA_TABLE_TYPES|Cet ensemble de lignes a été ajouté pour prendre en charge des paramètres table. Il est identique à DBSCHEMA_TABLES, à la différence près qu'il retourne uniquement des métadonnées pour des types de table plutôt que pour des tables, des vues ou des synonymes. La colonne TABLE_TYPE aura la valeur « TABLE TYPE ».|  
|DBSCHEMA_TABLE_TYPE_PRIMARY_KEYS|Cet ensemble de lignes a été ajouté pour prendre en charge des paramètres table. Il est identique à DBSCHEMA_PRIMARY_KEYS, à la différence près qu'il retourne uniquement des métadonnées de clés primaires pour des types de table plutôt que pour des tables.|  
|DBSCHEMA_TABLE_TYPE_COLUMNS|Cet ensemble de lignes a été ajouté pour prendre en charge des paramètres table. Il est identique à DBSCHEMA_COLUMNS, à la différence près qu'il retourne uniquement des métadonnées de colonne pour des types de table plutôt que pour des tables, des vues ou des synonymes.|  
|||

## <a name="see-also"></a>Voir aussi  
 [Paramètres table &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Utiliser les paramètres table &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
