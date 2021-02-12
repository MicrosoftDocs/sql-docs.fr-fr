---
description: Accéder aux schémas d’inventaire (AccessToSQL)
title: Accéder aux schémas d’inventaire (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- columns table
- databases table
- exported metadata
- exporting, schemas of exported metadata
- foreign keys table
- forms table
- indexes, table
- inventory schemas
- macros table
- modules table
- queries table
- reference, inventory schemas
- reports table
- schemas of exported metadata
- schemas, exported metadata
- SSMA_Access_InventoryColumns
- SSMA_Access_InventoryDatabases
- SSMA_Access_InventoryForeignKeys
- SSMA_Access_InventoryForms
- SSMA_Access_InventoryIndexes
- SSMA_Access_InventoryMacros
- SSMA_Access_InventoryModules
- SSMA_Access_InventoryQueries
- SSMA_Access_InventoryReports
- SSMA_Access_InventoryTables
- tables, inventory
ms.assetid: fdd3cff2-4d62-4395-8acf-71ea8f17f524
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: bddb9d2329c20203aca602e7aec37f235c4b6850
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077670"
---
# <a name="access-inventory-schemas-accesstosql"></a>Accéder aux schémas d’inventaire (AccessToSQL)
Les sections suivantes décrivent les tables créées par SSMA lorsque vous exportez des schémas d’accès vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="databases"></a>Bases de données  
Les métadonnées de base de données sont exportées vers la table **SSMA_Access_InventoryDatabases** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|GUID qui identifie de façon unique chaque base de données. Cette colonne est également la clé primaire de la table.|  
|**DatabaseName**|**nvarchar(4000)**|Nom de la base de données Access.|  
|**ExportTime**|**datetime**|Date et heure auxquelles ces métadonnées ont été créées par SSMA.|  
|**Cheminfichier**|**nvarchar(4000)**|Chemin d’accès complet et nom de fichier de la base de données Access.|  
|**FileSize**|**bigint**|Taille de la base de données Access, en Ko.|  
|**FileOwner**|**nvarchar(4000)**|Compte Windows qui est spécifié en tant que propriétaire de la base de données Access.|  
|**DateCreated**|**datetime**|Date et heure de création de la base de données Access.|  
|**DateModified**|**datetime**|Date et heure de la dernière modification de la base de données Access.|  
|**TablesCount**|**int**|Nombre de tables dans la base de données Access.|  
|**QueriesCount**|**int**|Nombre de requêtes dans la base de données Access.|  
|**FormsCount**|**int**|Nombre de formulaires dans la base de données Access.|  
|**ModulesCount**|**int**|Nombre de modules dans la base de données Access.|  
|**ReportsCount**|**int**|Nombre de rapports dans la base de données Access.|  
|**MacrosCount**|**int**|Nombre de macros dans la base de données Access.|  
|**AccessVersion**|**nvarchar(4000)**|Version d’accès de la base de données.|  
|**Classement**|**nvarchar(4000)**|Classement de la base de données Access. Les classements déterminent la façon dont une base de données trie et compare les chaînes.|  
|**JetVersion**|**nvarchar(4000)**|Version du moteur de base de données Jet. Les bases de données Access utilisent le moteur de base de données Jet sous-jacent.|  
|**IsUpdatable**|**bit**|Indique si la base de données peut être mise à jour. Si la valeur est 1, la base de données peut être mise à jour. Si la valeur est 0, la base de données est en lecture seule.|  
|**QueryTimeout**|**int**|Valeur du délai d’expiration de la requête ODBC configurée pour la base de données, en secondes. La valeur par défaut est de 60 secondes.|  
  
## <a name="tables"></a>Tables  
Les métadonnées de table sont exportées vers la table **SSMA_Access_InventoryTables** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cette table.|  
|**TableId**|**uniqueidentifier**|GUID qui identifie de façon unique la table. Cette colonne est également la clé primaire de la table.|  
|**TableName**|**nvarchar(4000)**|Nom de la table.|  
|**RowsCount**|**int**|Nombre de lignes du tableau.|  
|**ValideSi**|**nvarchar(4000)**|Règle définissant une entrée valide pour la table. Si aucune règle de validation n’existe, le champ contient une chaîne vide.|  
|**LinkedTable**|**nvarchar(4000)**|Une autre table, le cas échéant, qui est liée à la table. La liaison de tables permet d’effectuer des ajouts, des suppressions et des mises à jour de l’autre table à l’aide de cette table.|  
|**ExternalSource**|**nvarchar(4000)**|La source de données, le cas échéant, qui est associée à la table. Si une table est liée, elle possède une source de données externe spécifiée dans ce champ.|  
  
## <a name="columns"></a>Colonnes  
Les métadonnées de colonne sont exportées vers la table **SSMA_Access_InventoryColumns** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cette colonne.|  
|**TableId**|**uniqueidentifier**|Identifie la table qui contient cette colonne.|  
|**ColumnId**|**int**|Entier incrémentiel qui identifie la colonne. **ColumnID** est la clé primaire de la table.|  
|**ColumnName**|**nvarchar(4000)**|Nom de la colonne.|  
|**IsNullable**|**bit**|Spécifie si la colonne peut contenir des valeurs NULL. Si la valeur est 1, la colonne peut contenir des valeurs NULL. Si la valeur est égale à 0, la colonne ne peut pas contenir de valeurs NULL. Notez que la règle de validation peut également être utilisée pour empêcher les valeurs NULL.|  
|**DataType**|**nvarchar(4000)**|Type de données Access de la colonne, tel que **Text** ou **long**.|  
|**IsAutoIncrement**|**bit**|Spécifie si la colonne incrémente automatiquement les valeurs entières. Si la valeur est 1, les entiers s’incrémentent automatiquement.|  
|**Ordinal**|**smallint**|Ordre de la colonne dans la table, en commençant à zéro.|  
|**DefaultValue**|**nvarchar(4000)**|Valeur par défaut de la colonne.|  
|**ValideSi**|**nvarchar(4000)**|Règle utilisée pour valider les données ajoutées ou mises à jour dans la colonne.|  
  
## <a name="indexes"></a>Index  
Les métadonnées d’index sont exportées vers la table **SSMA_Access_InventoryIndexes** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cet index.|  
|**TableId**|**uniqueidentifier**|Identifie la table qui contient cet index.|  
|**IndexId**|**int**|Entier incrémentiel qui identifie l’index. Cette colonne est la clé primaire de la table.|  
|**IndexName**|**nvarchar(4000)**|Nom de l’index.|  
|**ColumnsIncluded**|**nvarchar(4000)**|Répertorie les colonnes incluses dans l’index. Les noms de colonnes sont séparés par un point-virgule.|  
|**IsUnique**|**bit**|Spécifie si chaque élément de l’index doit être unique. Sur un index à plusieurs colonnes, la combinaison des valeurs doit être unique. Si la valeur est 1, l’index applique des valeurs uniques.|  
|**IsPK**|**bit**|Spécifie si l’index a été créé automatiquement dans le cadre de la définition de la clé primaire.|  
|**IsClustered**|**bit**|Spécifie si l’index est ordonné en clusters. Un index cluster réorganise le stockage physique des données. Une table ne peut avoir qu’un seul index cluster.|  
  
## <a name="foreign-keys"></a>Clés étrangères  
Les métadonnées de clé étrangère sont exportées vers la table **SSMA_Access_InventoryForeignKeys** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cette clé étrangère.|  
|**TableId**|**uniqueidentifier**|Identifie la table qui contient cette clé étrangère.|  
|**ForeignKeyId**|**int**|Entier incrémentiel qui identifie la clé étrangère. Cette colonne est la clé primaire de la table.|  
|**ForeignKeyName**|**nvarchar(4000)**|Nom de l’index.|  
|**ReferencedTableId**|**uniqueidentifier**|Identifie la table qui contient les colonnes sources.|  
|**SourceColumns**|**nvarchar(4000)**|Répertorie la ou les colonnes de clé étrangère.|  
|**ReferencedColumns**|**nvarchar(4000)**|Répertorie la ou les colonnes de clé primaire référencées par la clé étrangère.|  
|**IsCascadeForUpdate**|**bit**|Spécifie que si la valeur de clé primaire est mise à jour, toutes les lignes qui font référence à cette valeur de clé sont également mises à jour.|  
|**IsCascadeForDelete**|**bit**|Spécifie que si la valeur de clé primaire est supprimée, toutes les lignes qui font référence à cette valeur de clé sont également supprimées.|  
|**IsEnforced**|**bit**|Spécifie que la contrainte de clé étrangère est appliquée.|  
  
## <a name="queries"></a>Requêtes  
Les métadonnées de requête sont exportées vers la table **SSMA_Access_InventoryQueries** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient cette requête.|  
|**QueryId**|**int**|Entier incrémentiel qui identifie la requête. Cette colonne est la clé primaire de la table.|  
|**NomRequête**|**nvarchar(4000)**|Nom de la requête.|  
|**QueryText**|**nvarchar(4000)**|Code de requête SQL, par exemple une instruction SELECT.|  
|**IsUpdateable**|**bit**|Spécifie si la requête peut être mise à jour ou en lecture seule.|  
|**QueryType**|**nvarchar(4000)**|Spécifie le type de requête, par exemple **Select** ou **SetOperation**.|  
|**ExternalSource**|**nvarchar(4000)**|Si la requête fait référence à une source de données externe, il s’agit de la chaîne de connexion utilisée par la requête.|  
  
## <a name="forms"></a>Formulaires  
Les métadonnées de formulaire sont exportées vers la table **SSMA_Access_InventoryForms** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient ce formulaire.|  
|**FormId**|**int**|Entier incrémentiel qui identifie le formulaire. Cette colonne est la clé primaire de la table.|  
|**NomFormulaire**|**nvarchar(4000)**|Nom du formulaire.|  
  
## <a name="macros"></a>Macros  
Les métadonnées de macro sont exportées vers la table **SSMA_Access_InventoryMacros** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient la macro.|  
|**MacroId**|**int**|Entier incrémentiel qui identifie la macro. Cette colonne est la clé primaire de la table.|  
|**Nommacro**|**nvarchar(4000)**|Nom de la macro.|  
  
## <a name="reports"></a>Rapports  
Les métadonnées de rapport sont exportées vers la table **SSMA_Access_InventoryReports** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient le rapport.|  
|**ReportId**|**int**|Entier incrémentiel qui identifie le rapport. Cette colonne est la clé primaire de la table.|  
|**ReportName**|**nvarchar(4000)**|Nom du rapport.|  
  
## <a name="modules"></a>Modules  
Les métadonnées de module sont exportées vers la table **SSMA_Access_InventoryModules** . Ce tableau contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifie la base de données qui contient le module.|  
|**ModuleId**|**int**|Entier incrémentiel qui identifie le module. Cette colonne est la clé primaire de la table.|  
|**ModuleName**|**nvarchar(4000)**|Nom du module.|  
  
## <a name="see-also"></a>Voir aussi  
[Exportation d’un inventaire Access](exporting-an-access-inventory-accesstosql.md)  
  
