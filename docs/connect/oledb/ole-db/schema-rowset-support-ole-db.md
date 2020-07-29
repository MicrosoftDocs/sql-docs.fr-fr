---
title: Prise en charge des ensembles de lignes de schéma (OLE DB) | Microsoft Docs
description: Prise en charge des ensembles de lignes de schéma (OLE DB)
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- schema rowsets [OLE DB]
- OLE DB, schema rowsets
- OLE DB rowsets, schema
- OLE DB Driver for SQL Server, schema rowsets
- rowsets [OLE DB], schema
author: pmasl
ms.author: pelopes
ms.openlocfilehash: fdd00a0c53e2570ec9688665ce5348401571531b
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "86012826"
---
# <a name="schema-rowset-support-ole-db"></a>Prise en charge des ensembles de lignes de schéma (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Le pilote OLE DB pour SQL Server prend également en charge le retour des informations de schéma d’un serveur lié lors du traitement de requêtes distribuées [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  Bien que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prenne en charge les synonymes, OLE DB Driver pour SQL Server ne retourne pas les métadonnées de ces derniers.  
  
 Les tableaux suivants répertorient les ensembles de lignes de schéma et les colonnes de restriction prises en charge par le pilote OLE DB pour SQL Server.  
  
|Ensemble de lignes de schéma|Colonnes de restriction|  
|-------------------|-------------------------|  
|DBSCHEMA_CATALOGS|CATALOG_NAME|  
|DBSCHEMA_COLUMN_PRIVILEGES|Toutes les restrictions sont prises en charge.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME GRANTOR GRANTEE|  
|DBSCHEMA_COLUMNS|Toutes les restrictions sont prises en charge.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME<br /><br /> Les colonnes supplémentaires suivantes sont propres à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :<br /><br /> COLUMN_LCID désigne l'ID de paramètres régionaux du classement. COLUMN_LCID affiche une valeur identique à un LCID Windows.<br /><br /> COLUMN_COMPFLAGS définit les comparaisons prises en charge pour le classement. Le format de données est le même que DBPROB_FINDCOMPAREOPS.<br /><br /> COLUMN_SORTID désigne le style de tri de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour le classement.<br /><br /> COLUMN_TDSCOLLATION désigne le classement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de la colonne.<br /><br /> IS_COMPUTED est défini sur VARIANT_TRUE si la colonne est une colonne calculée et sur VARIANT_FALSE autrement.|  
|DBSCHEMA_FOREIGN_KEYS|Toutes les restrictions sont prises en charge.<br /><br /> PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME FK_TABLE_CATALOG FK_TABLE_SCHEMA FK_TABLE_NAME|  
|DBSCHEMA_INDEXES|Les restrictions 1, 2, 3 et 5 sont prises en charge.<br /><br /> TABLE_CATALOG TABLE_SCHEMA INDEX_NAME TABLE_NAME|  
|DBSCHEMA_PRIMARY_KEYS|Toutes les restrictions sont prises en charge.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|DBSCHEMA_PROCEDURE_PARAMETERS|Toutes les restrictions sont prises en charge.<br /><br /> PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PARAMETER_NAME|  
|DBSCHEMA_PROCEDURES|Les restrictions 1, 2, et 3 sont prises en charge.<br /><br /> PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME<br /><br /> DBSCHEMA_PROCEDURES retourne uniquement des procédures que l'utilisateur actuel peut exécuter ou pour lesquelles l'autorisation VIEW DEFINITION lui a été accordée.|  
|DBSCHEMA_PROVIDER_TYPES|Toutes les restrictions sont prises en charge.<br /><br /> DATA_TYPE BEST_MATCH|  
|DBSCHEMA_SCHEMATA|Toutes les restrictions sont prises en charge.<br /><br /> CATALOG_NAME SCHEMA_NAME SCHEMA_OWNER|  
|DBSCHEMA_STATISTICS|Toutes les restrictions sont prises en charge.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|DBSCHEMA_TABLE_CONSTRAINTS|Toutes les restrictions sont prises en charge.<br /><br /> CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME CONSTRAINT_TYPE|  
|DBSCHEMA_TABLE_PRIVILEGES|Toutes les restrictions sont prises en charge.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME GRANTOR GRANTEE|  
|DBSCHEMA_TABLES|Toutes les restrictions sont prises en charge.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
|DBSCHEMA_TABLES_INFO|Toutes les restrictions sont prises en charge.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
  
## <a name="in-this-section"></a>Dans cette section  
 [Prise en charge des requêtes distribuées dans les ensembles de lignes de schéma](../../oledb/ole-db/schema-rowsets-distributed-query-support.md)  
  
 [Ensemble de lignes LINKEDSERVERS &#40;OLE DB&#41;](../../oledb/ole-db/schema-rowsets-linkedservers-rowset.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation du pilote OLE DB pour SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)   
 [Utilisation de types définis par l’utilisateur](../../oledb/features/using-user-defined-types.md)  
  
  
