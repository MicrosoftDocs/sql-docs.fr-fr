---
description: SchemaEnum
title: SchemaEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SchemaEnum
helpviewer_keywords:
- SchemaEnum enumeration [ADO]
ms.assetid: 21c97651-297f-469f-b5b5-c48af72b62a8
author: rothja
ms.author: jroth
ms.openlocfilehash: 55a55d39117f2367b1b13c1a25b3785fe2d9cacc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170304"
---
# <a name="schemaenum"></a>SchemaEnum
Spécifie le type de **jeu d’enregistrements** de schéma que la méthode [OpenSchema](./openschema-method.md) récupère.  
  
## <a name="remarks"></a>Notes  
 Vous trouverez des informations supplémentaires sur la fonction et les colonnes retournées pour chaque constante ADO dans les rubriques de l' [annexe B : ensembles de lignes de schéma](/previous-versions/windows/desktop/ms712921(v=vs.85)) de la OLE DB Guide de référence du programmeur. Le nom de chaque rubrique est indiqué entre parenthèses dans la section Description du tableau suivant.  
  
 Vous trouverez des informations supplémentaires sur la fonction et les colonnes retournées pour chaque ADO MD constante dans les rubriques dans [OLE DB pour les objets OLAP et les ensembles de lignes de schéma](/previous-versions/windows/desktop/ms723056(v=vs.85)) dans le OLE DB pour la documentation sur le traitement analytique en ligne (OLAP). Le nom de chaque rubrique est indiqué entre parenthèses dans la colonne Description du tableau suivant.  
  
 Vous pouvez convertir les types de données des colonnes de la documentation OLE DB en types de données ADO en faisant référence à la colonne Description de la rubrique [DATATYPEENUM](./datatypeenum.md) ADO. Par exemple, un OLE DB type de données de **DBTYPE_WSTR** est équivalent à un type de données ADO **adWChar**.  
  
 ADO génère des résultats de type schéma pour les constantes, **adSchemaDBInfoKeywords** et **adSchemaDBInfoLiterals**. ADO crée un **jeu d’enregistrements**, puis remplit chaque ligne avec les valeurs retournées respectivement par les méthodes **IDBInfo :: GetKeywords** et **IDBInfo :: GetLiteralInfo** . Vous trouverez des informations supplémentaires sur ces méthodes dans la section [IDBInfo](/previous-versions/windows/desktop/ms713663(v=vs.85)) du Guide de référence du programmeur OLE DB.  
  
|Constante|Valeur|Description|Colonnes de contrainte|  
|--------------|-----------|-----------------|------------------------|  
|**adSchemaAsserts**|0|Retourne les assertions définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes assertions)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCatalogs**|1|Retourne les attributs physiques associés aux catalogues accessibles à partir du SGBD.<br /><br /> (Ensemble de lignes de catalogue)|CATALOG_NAME|  
|**adSchemaCharacterSets**|2|Retourne les jeux de caractères définis dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes CHARACTER_SETS)|CHARACTER_SET_CATALOG CHARACTER_SET_SCHEMA CHARACTER_SET_NAME|  
|**adSchemaCheckConstraints**|5|Retourne les contraintes CHECK définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (CHECK_CONSTRAINTS) OpenXml|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCollations**|3|Retourne les classements de caractères définis dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes COLLATIONs)|COLLATION_CATALOG COLLATION_SCHEMA COLLATION_NAME|  
|**adSchemaColumnPrivileges**|13|Retourne les privilèges sur les colonnes de tables définies dans le catalogue qui sont disponibles ou accordées par un utilisateur donné.<br /><br /> (Ensemble de lignes COLUMN_PRIVILEGES)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME GRANTOR GRANTEE|  
|**adSchemaColumns**|4|Retourne les colonnes des tables (vues comprises) définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes COLUMNs)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaColumnsDomainUsage**|11|Retourne les colonnes définies dans le catalogue qui dépendent d'un domaine défini dans le catalogue et qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes COLUMN_DOMAIN_USAGE)|DOMAIN_CATALOG DOMAIN_SCHEMA DOMAIN_NAME COLUMN_NAME|  
|**adSchemaConstraintColumnUsage**|6|Retourne les colonnes utilisées par les contraintes référentielles, les contraintes uniques, les contraintes de validation et les assertions, définies dans le catalogue et détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes CONSTRAINT_COLUMN_USAGE)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaConstraintTableUsage**|7|Retourne les tables utilisées par les contraintes référentielles, les contraintes uniques, les contraintes de validation et les assertions, définies dans le catalogue et détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes CONSTRAINT_TABLE_USAGE)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaCubes**|32|Retourne des informations sur les cubes disponibles dans un schéma (ou le catalogue, si le fournisseur ne prend pas en charge les schémas).<br /><br /> (Ensemble de lignes CUBE *)|CATALOG_NAME SCHEMA_NAME CUBE_NAME|  
|**adSchemaDBInfoKeywords**|30|Retourne une liste de mots clés spécifiques au fournisseur.<br /><br /> (IDBInfo :: GetKeywords)|\<None>|  
|**adSchemaDBInfoLiterals**|31|Retourne la liste des littéraux propres au fournisseur utilisés dans les commandes de texte.<br /><br /> (IDBInfo :: GetLiteralInfo)|\<None>|  
|**adSchemaDimensions**|33|Retourne des informations sur les dimensions d’un cube donné. Elle possède une ligne pour chaque dimension.<br /><br /> (Ensemble de lignes de DIMENSIONS)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_NAME DIMENSION_UNIQUE_NAME|  
|**adSchemaForeignKeys**|27|Retourne les colonnes clés étrangères définies dans le catalogue par un utilisateur donné.<br /><br /> (Ensemble de lignes FOREIGN_KEYS)|PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME FK_TABLE_CATALOG FK_TABLE_SCHEMA FK_TABLE_NAME|  
|**adSchemaHierarchies**|34|Retourne des informations sur les hiérarchies disponibles dans une dimension.<br /><br /> (Ensemble de lignes HIERARCHIES)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_NAME HIERARCHY_UNIQUE_NAME|  
|**adSchemaIndexes**|12|Retourne les index définis dans le catalogue et détenus par un utilisateur donné.<br /><br /> (Ensemble de lignes d’INDEX)|TABLE_CATALOG TABLE_SCHEMA TYPE DE INDEX_NAME TABLE_NAME|  
|**adSchemaKeyColumnUsage**|8|Retourne les colonnes définies dans le catalogue qui sont restreintes en tant que clés par un utilisateur donné.<br /><br /> (Ensemble de lignes KEY_COLUMN_USAGE)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaLevels**|35|Retourne des informations sur les niveaux disponibles dans une dimension.<br /><br /> (Ensemble de lignes niveaux)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_NAME LEVEL_UNIQUE_NAME|  
|**adSchemaMeasures**|36|Retourne des informations sur les mesures disponibles.<br /><br /> (Ensemble de lignes de mesures)|CATALOG_NAME SCHEMA_NAME CUBE_NAME MEASURE_NAME MEASURE_UNIQUE_NAME|  
|**adSchemaMembers**|38|Retourne des informations sur les membres disponibles.<br /><br /> (Ensemble de lignes des membres)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME LEVEL_NUMBER Member_Name MEMBER_UNIQUE_NAME MEMBER_CAPTION MEMBER_TYPE. Pour plus d’informations, consultez OLE DB pour le traitement analytique en ligne (OLAP).|  
|**adSchemaPrimaryKeys**|28|Retourne les colonnes de clés primaires définies dans le catalogue par un utilisateur donné.<br /><br /> (Ensemble de lignes PRIMARY_KEYS)|PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME|  
|**adSchemaProcedureColumns**|29|Retourne des informations sur les colonnes de jeux de lignes retournés par des procédures.<br /><br /> (Ensemble de lignes PROCEDURE_COLUMNS)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME COLUMN_NAME|  
|**adSchemaProcedureParameters**|26|Retourne des informations sur les paramètres et les codes de retour de procédures.<br /><br /> (Ensemble de lignes PROCEDURE_PARAMETERS)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PARAMETER_NAME|  
|**adSchemaProcedures**|16|Retourne les procédures définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes de procédures)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PROCEDURE_TYPE|  
|**adSchemaProperties**|37|Retourne des informations sur les propriétés disponibles pour chaque niveau de la dimension.<br /><br /> (Ensemble de lignes PROPERTIES)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME MEMBER_UNIQUE_NAME PROPERTY_TYPE PROPERTY_NAME|  
|**adSchemaProviderSpecific**|-1|Utilisé si le fournisseur définit ses propres requêtes de schéma non standard.|\<Provider specific>|  
|**adSchemaProviderTypes**|22|Retourne les types de données (de base) pris en charge par le fournisseur de données.<br /><br /> (Ensemble de lignes PROVIDER_TYPES)|DATA_TYPE BEST_MATCH|  
|**AdSchemaReferentialConstraints**|9|Retourne les contraintes référentielles définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes REFERENTIAL_CONSTRAINTS)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaSchemata**|17|Retourne les schémas (objets de base de données) détenus par un utilisateur donné.<br /><br /> (Ensemble de lignes SCHEMATA)|CATALOG_NAME SCHEMA_NAME SCHEMA_OWNER|  
|**adSchemaSQLLanguages**|18|Retourne les niveaux de conformité, les options et les dialectes pris en charge par l'implémentation SQL traitant les données définies dans le catalogue.<br /><br /> (Ensemble de lignes SQL_LANGUAGES)|\<None>|  
|**adSchemaStatistics**|19|Retourne les statistiques définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes de statistiques)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaTableConstraints**|10|Retourne les contraintes de table définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes TABLE_CONSTRAINTS)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME CONSTRAINT_TYPE|  
|**adSchemaTablePrivileges**|14|Retourne les privilèges sur les tables définies dans le catalogue qui sont mis à la disposition d'un utilisateur ou accordés par celui-ci.<br /><br /> (Ensemble de lignes TABLE_PRIVILEGES)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME GRANTOR GRANTEE|  
|**adSchemaTables**|20|Retourne les tables (vues comprises) définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes de TABLES)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
|**adSchemaTranslations**|21|Retourne les traductions de caractères définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes TRANSLATIONs)|TRANSLATION_CATALOG TRANSLATION_SCHEMA TRANSLATION_NAME|  
|**adSchemaTrustees**|39|Réservé pour un usage futur.||  
|**adSchemaUsagePrivileges**|15|Retourne les privilèges d’utilisation sur les objets définis dans le catalogue qui sont accessibles à un utilisateur donné ou accordés par celui-ci.<br /><br /> (Ensemble de lignes USAGE_PRIVILEGES)|OBJECT_CATALOG OBJECT_SCHEMA OBJECT_NAME OBJECT_TYPE BÉNÉFICIAIRE DU GRANTATEUR|  
|**adSchemaViewColumnUsage**|24|Retourne les colonnes sur lesquelles les tables affichées, définies dans le catalogue et détenues par un utilisateur donné, sont dépendantes.<br /><br /> (Ensemble de lignes VIEW_COLUMN_USAGE)|VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
|**adSchemaViews**|23|Retourne les vues définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes VIEWS)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaViewTableUsage**|25|Retourne les tables dont dépendent les tables affichées, définies dans le catalogue et détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes VIEW_TABLE_USAGE)|VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. Schema. assertions|  
|AdoEnums. Schema. catalogues|  
|AdoEnums. Schema. CHARACTERSETS|  
|AdoEnums. Schema. CHECKCONSTRAINTS|  
|AdoEnums. Schema. COLLATIONs|  
|AdoEnums. Schema. COLUMNPRIVILEGES|  
|AdoEnums. Schema. COLUMNs|  
|AdoEnums. Schema. COLUMNSDOMAINUSAGE|  
|AdoEnums. Schema. CONSTRAINTCOLUMNUSAGE|  
|AdoEnums. Schema. CONSTRAINTTABLEUSAGE|  
|AdoEnums. Schema. CUBEs|  
|AdoEnums. Schema. DBINFOKEYWORDS|  
|AdoEnums. Schema. DBINFOLITERALS|  
|AdoEnums. Schema. DIMENSIONS|  
|AdoEnums. Schema. FOREIGNKEYS|  
|AdoEnums. Schema. HIERARCHIES|  
|AdoEnums. Schema. INDEXes|  
|AdoEnums. Schema. KEYCOLUMNUSAGE|  
|AdoEnums. Schema. LEVELs|  
|AdoEnums. Schema. mesures|  
|AdoEnums. Schema. MEMBERs|  
|AdoEnums. Schema. PRIMARYKEYS|  
|AdoEnums. Schema. PROCEDURECOLUMNS|  
|AdoEnums. Schema. PROCEDUREPARAMETERS|  
|AdoEnums. Schema. PROCEDUREs|  
|AdoEnums. Schema. PROPERTIES|  
|AdoEnums. Schema. PROVIDERSPECIFIC|  
|AdoEnums. Schema. PROVIDERTYPES|  
|AdoEnums. Schema. REFERENTIALCONTRAINTS|  
|AdoEnums. Schema. SCHEMATA|  
|AdoEnums. Schema. SQLLANGUAGES|  
|AdoEnums. Schema. STATISTICs|  
|AdoEnums. Schema. TABLECONSTRAINTS|  
|AdoEnums. Schema. TABLEPRIVILEGES|  
|AdoEnums. Schema. TABLES|  
|AdoEnums. Schema. TRANSLATIONs|  
|AdoEnums. Schema. TRUSTers|  
|AdoEnums. Schema. USAGEPRIVILEGES|  
|AdoEnums. Schema. VIEWCOLUMNUSAGE|  
|AdoEnums. Schema. VIEWS|  
|AdoEnums. Schema. VIEWTABLEUSAGE|  
  
## <a name="applies-to"></a>S'applique à  
 [OpenSchema, méthode](./openschema-method.md)