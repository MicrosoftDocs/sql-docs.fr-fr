---
title: Prise en charge des API pour les améliorations de date et d’heure (fournisseur Native Client OLE DB)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
ms.assetid: e65c9253-bd99-4dc3-9cb8-7613f754c966
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4bb55ae2187b1406281572157457a49bda97752
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87247962"
---
# <a name="ole-db-api-support-for-date-and-time-enhancements-native-client-ole-db-provider"></a>Prise en charge des API OLE DB pour les améliorations de date et d’heure (fournisseur Native Client OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les API OLE DB suivantes prennent en charge les fonctionnalités de date/heure améliorées.  
  
|Fonction|Description|  
|--------------|-----------------|  
|IAccessor::CreateAccessor|Un indicateur est ajouté dans la structure DBBINDING pour permettre aux applications de distinguer les valeurs **datetime**, **datetime2** et **smalldatetime**. Pour plus d’informations, consultez [Métadonnées de paramètre et d'ensemble de lignes](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md).|  
|IBCPSession::BCPColFmt|Pour plus d’informations, consultez [modifications de copie en bloc pour les types de date et d’heure améliorés &#40;OLE DB et ODBC&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).|  
|ICommandWithParameters::GetParameterInfo|Pour plus d’informations, consultez [Métadonnées de paramètre et d'ensemble de lignes](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md).|  
|ICommandWithParameters::SetParameterinfo|Pour plus d’informations, consultez [Métadonnées de paramètre et d'ensemble de lignes](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md).|  
|IColumnsRowset::GetColumnsRowset|Pour plus d’informations, consultez [Métadonnées de paramètre et d'ensemble de lignes](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md).|  
|IColumnsInfo::GetColumnInfo|Pour plus d’informations, consultez [Métadonnées de paramètre et d'ensemble de lignes](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md).|  
|IDBSchemaRowset::GetRowset|Pour plus d’informations sur les ensembles de lignes de schéma affectés, consultez [Date et heure et ensembles de lignes de schéma](../../relational-databases/native-client-ole-db-date-time/metadata-date-and-time-and-schema-rowsets.md).|  
|IRowsetFastLoad|Cette interface prend en charge les nouveaux types date/heure, mais aucune modification n'a été apportée à son interface.|  
|ITableDefinition::CreateTable|Pour plus d’informations, consultez [Prise en charge des types de données pour les améliorations de date et d’heure OLE DB](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Améliorations des types de données de date et d’heure &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
