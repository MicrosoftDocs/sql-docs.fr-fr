---
title: Collections de schémas SQL Server
description: Décrit les collections de schémas supplémentaires prises en charge par le fournisseur de données Microsoft SqlClient pour SQL Server.
ms.date: 11/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3d757c09717d885dbf9bf35dc6826234ef4ea59e
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771511"
---
# <a name="sql-server-schema-collections"></a>Collections de schémas SQL Server

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le fournisseur de données Microsoft SqlClient pour SQL Server prend en charge d’autres collections de schémas, en plus de celles fréquemment utilisées. Les collections de schémas varient légèrement selon la version de SQL Server que vous utilisez. Pour établir la liste des collections de schémas prises en charge, appelez la méthode **GetSchema** sans argument ou avec le nom de collection de schémas « MetaDataCollections ». Cette opération retourne un <xref:System.Data.DataTable> avec une liste des collections de schémas prises en charge, le nombre de restrictions qu’elles prennent en charge et le nombre d’éléments d’identification qu’elles utilisent.  

## <a name="databases"></a>Bases de données  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|database_name|Chaîne|Nom de la base de données.|  
|dbid|Int16|ID de la base de données.|  
|create_date|DateTime|Date de création de la base de données.|  

## <a name="foreign-keys"></a>Clés étrangères  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|CONSTRAINT_CATALOG|Chaîne|Catalogue auquel la contrainte appartient.|  
|CONSTRAINT_SCHEMA|Chaîne|Schéma contenant la contrainte.|  
|CONSTRAINT_NAME|Chaîne|Nom.|  
|TABLE_CATALOG|Chaîne|Nom de la table dont la contrainte fait partie.|  
|TABLE_SCHEMA|Chaîne|Schéma contenant la table.|  
|TABLE_NAME|Chaîne|Nom de la table|  
|CONSTRAINT_TYPE|Chaîne|Type de contrainte. Seul « FOREIGN KEY » est autorisé.|  
|IS_DEFERRABLE|Chaîne|Indique si la contrainte peut être différée. Retourne NO.|  
|INITIALLY_DEFERRED|Chaîne|Indique si la contrainte est initialement différée. Retourne NO.|  

## <a name="indexes"></a>Index  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|constraint_catalog|Chaîne|Catalogue auquel l'index appartient.|  
|constraint_schema|Chaîne|Schéma contenant l'index.|  
|constraint_name|Chaîne|Nom de l'index.|  
|table_catalog|Chaîne|Nom de la table auquel l'index est associé.|  
|table_schema|Chaîne|Schéma contenant la table à laquelle l'index est associé.|  
|table_name|Chaîne|Nom de table.|  
|index_name|Chaîne|Nom d’index.|  
|type_desc|Chaîne|Le type de l'index est l'un des suivants :<br /><br /> -   HEAP<br />-   CLUSTERED<br />-   NONCLUSTERED<br />-   XML<br />-   SPATIAL|  

## <a name="indexcolumns"></a>IndexColumns  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|constraint_catalog|Chaîne|Catalogue auquel l'index appartient.|  
|constraint_schema|Chaîne|Schéma contenant l'index.|  
|constraint_name|Chaîne|Nom de l'index.|  
|table_catalog|Chaîne|Nom de la table auquel l'index est associé.|  
|table_schema|Chaîne|Schéma contenant la table à laquelle l'index est associé.|  
|table_name|Chaîne|Nom de table.|  
|column_name|Chaîne|Nom de la colonne à laquelle l'index est associé.|  
|ordinal_position|Int32|Position ordinale de la colonne.|  
|KeyType|Byte|Type d'objet.|  
|index_name|Chaîne|Nom d’index.| 

## <a name="procedures"></a>Procédures  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|SPECIFIC_CATALOG|Chaîne|Nom spécifique du catalogue.|  
|SPECIFIC_SCHEMA|Chaîne|Nom spécifique du schéma.|  
|SPECIFIC_NAME|Chaîne|Nom spécifique du catalogue|  
|ROUTINE_CATALOG|Chaîne|Catalogue auquel appartient la procédure stockée.|  
|ROUTINE_SCHEMA|Chaîne|Schéma contenant la procédure stockée.|  
|ROUTINE_NAME|Chaîne|Nom de la procédure stockée.|  
|ROUTINE_TYPE|Chaîne|Retourne PROCEDURE pour les procédures stockées et FUNCTION pour les fonctions.|  
|CREATED|DateTime|Heure à laquelle la procédure a été créée.|  
|LAST_ALTERED|DateTime|Heure à laquelle la procédure a été modifiée pour la dernière fois.| 

## <a name="procedure-parameters"></a>Paramètres de procédure  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|SPECIFIC_CATALOG|Chaîne|Nom du catalogue de la procédure pour laquelle ceci est un paramètre.|  
|SPECIFIC_SCHEMA|Chaîne|Schéma contenant la procédure dont ce paramètre fait partie.|  
|SPECIFIC_NAME|Chaîne|Nom de la procédure dont ce paramètre fait partie.|  
|ORDINAL_POSITION|Int32|Position ordinale du paramètre en commençant à 1. Pour la valeur de retour d'une procédure, cela donne 0.|  
|PARAMETER_MODE|Chaîne|Retourne IN pour un paramètre d'entrée, OUT pour un paramètre de sortie et INOUT pour un paramètre d'entrée/sortie.|  
|IS_RESULT|Chaîne|Retourne YES s'il indique que le résultat de la procédure est une fonction. Dans le cas contraire, la valeur retournée est NO.|  
|AS_LOCATOR|Chaîne|Retourne YES si l'élément est déclaré comme localisateur. Dans le cas contraire, la valeur retournée est NO.|  
|PARAMETER_NAME|Chaîne|Nom du paramètre. NULL si ceci correspond à la valeur retournée d'une fonction.|  
|DATA_TYPE|Chaîne|Type de données fourni par le système.|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Longueur maximale en caractères des données de type binaire ou caractère. Dans le cas contraire, la valeur NULL est retournée.|  
|CHARACTER_OCTET_LENGTH|Int32|Longueur maximale en octets des données de type binaire ou caractère. Dans le cas contraire, la valeur NULL est retournée.|  
|COLLATION_CATALOG|Chaîne|Nom de catalogue du classement du paramètre. Retourne la valeur NULL si ce nom n'utilise pas l'un des types de caractères.|  
|COLLATION_SCHEMA|Chaîne|Retourne toujours la valeur Null.|  
|COLLATION_NAME|Chaîne|Nom du classement du paramètre. Retourne la valeur NULL si ce nom n'utilise pas l'un des types de caractères.|  
|CHARACTER_SET_CATALOG|Chaîne|Nom de catalogue du jeu de caractères du paramètre. Retourne la valeur NULL si ce nom n'utilise pas l'un des types de caractères.|  
|CHARACTER_SET_SCHEMA|Chaîne|Retourne toujours la valeur Null.|  
|CHARACTER_SET_NAME|Chaîne|Nom du jeu de caractères du paramètre. Retourne la valeur NULL si ce nom n'utilise pas l'un des types de caractères.|  
|NUMERIC_PRECISION|Byte|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|NUMERIC_PRECISION_RADIX|Int16|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|NUMERIC_SCALE|Int32|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|DATETIME_PRECISION|Int16|Précision en secondes fractionnelles si le type de paramètre est datetime ou smalldatetime. Dans le cas contraire, la valeur NULL est retournée.|  
|INTERVAL_TYPE|Chaîne|NULL. Réservé pour un usage futur de SQL Server.|  
|INTERVAL_PRECISION|Int16|NULL. Réservé pour un usage futur de SQL Server.| 

## <a name="tables"></a>Tables  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|Chaîne|Catalogue de la table.|  
|TABLE_SCHEMA|Chaîne|Schéma contenant la table.|  
|TABLE_NAME|Chaîne|Nom de la table.|  
|TABLE_TYPE|Chaîne|Type de table. Peut être VIEW ou BASE TABLE.|  

## <a name="columns"></a>Colonnes  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|Chaîne|Catalogue de la table.|  
|TABLE_SCHEMA|Chaîne|Schéma contenant la table.|  
|TABLE_NAME|Chaîne|Nom de la table.|  
|COLUMN_NAME|Chaîne|Nom de la colonne.|  
|ORDINAL_POSITION|Int32|Numéro d'identification de colonne.|  
|COLUMN_DEFAULT|Chaîne|Valeur par défaut de la colonne|  
|IS_NULLABLE|Chaîne|Valeur NULL possible dans la colonne. Si cette colonne autorise les valeurs NULL, elle retourne YES. Dans le cas contraire, elle retourne No.|  
|DATA_TYPE|Chaîne|Type de données fourni par le système.|  
|CHARACTER_MAXIMUM_LENGTH|Int32 – Sql8, Int16 – Sql7|Longueur maximale (en caractères) des données de type binaire, caractère, texte et image. Renvoie NULL dans les autres cas.|  
|CHARACTER_OCTET_LENGTH|Int32 – SQL8, Int16 – Sql7|Longueur maximale, en octets, des données de type binaire, caractère, texte et image. Renvoie NULL dans les autres cas.|  
|NUMERIC_PRECISION|Octet non signé|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|NUMERIC_PRECISION_RADIX|Int16|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|NUMERIC_SCALE|Int32|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|DATETIME_PRECISION|Int16|Code de sous-type pour le type de données datetime et pour le type de données interval de SQL-92. Renvoie NULL pour les autres types de données.|  
|CHARACTER_SET_CATALOG|Chaîne|Retourne une liste maître indiquant la base de données dans laquelle l'ensemble de caractères se trouve et si la colonne contient des données de type caractères ou texte. Renvoie NULL dans les autres cas.|  
|CHARACTER_SET_SCHEMA|Chaîne|Retourne toujours la valeur Null.|  
|CHARACTER_SET_NAME|Chaîne|Retourne le nom unique de l'ensemble de caractères si cette colonne contient des données de type caractère ou texte. Renvoie NULL dans les autres cas.|  
|COLLATION_CATALOG|Chaîne|Retourne une liste maître indiquant la base de données dans laquelle le classement est défini, si la colonne contient des données de type caractères ou texte. Sinon, cette colonne est NULL.|  
|IS_FILESTREAM|Chaîne|YES si la colonne présente l'attribut FILESTREAM.<br /><br /> NO si la colonne ne présente pas l'attribut FILESTREAM.|  
|IS_SPARSE|Chaîne|YES s'il s'agit d'une colonne fragmentée.<br /><br /> NO s'il ne s'agit pas d'une colonne fragmentée.|  
|IS_COLUMN_SET|Chaîne|YES s'il s'agit d'une colonne d'ensemble de colonnes.<br /><br /> NO s'il ne s'agit pas d'une colonne d'ensemble de colonnes.|  

## <a name="allcolumns"></a>AllColumns  

 La collection de schémas AllColumns sert à prendre en charge les colonnes éparses. AllColumns présente les mêmes restrictions et schéma DataTable résultant que la collection de schémas Columns. La seule différence vient du fait que la collection AllColumns inclut des colonnes d'ensembles de colonnes qui ne sont pas incluses dans la collection de schémas Columns. Le tableau suivant décrit ces colonnes.  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|Chaîne|Catalogue de la table.|  
|TABLE_SCHEMA|Chaîne|Schéma contenant la table.|  
|TABLE_NAME|Chaîne|Nom de la table.|  
|COLUMN_NAME|Chaîne|Nom de la colonne.|  
|ORDINAL_POSITION|Int32|Numéro d'identification de colonne.|  
|COLUMN_DEFAULT|Chaîne|Valeur par défaut de la colonne|  
|IS_NULLABLE|Chaîne|Valeur NULL possible dans la colonne. Si cette colonne autorise les valeurs NULL, elle retourne YES. Dans le cas contraire, elle renvoie NO.|  
|DATA_TYPE|Chaîne|Type de données fourni par le système.|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Longueur maximale (en caractères) des données de type binaire, caractère, texte et image. Renvoie NULL dans les autres cas.|  
|CHARACTER_OCTET_LENGTH|Int32|Longueur maximale, en octets, des données de type binaire, caractère, texte et image. Renvoie NULL dans les autres cas.|  
|NUMERIC_PRECISION|Octet non signé|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|NUMERIC_PRECISION_RADIX|Int16|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|NUMERIC_SCALE|Int32|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|DATETIME_PRECISION|Int16|Code de sous-type pour le type de données datetime et pour le type de données interval de SQL-92. Renvoie NULL pour les autres types de données.|  
|CHARACTER_SET_CATALOG|Chaîne|Retourne une liste maître indiquant la base de données dans laquelle l'ensemble de caractères se trouve et si la colonne contient des données de type caractères ou texte. Renvoie NULL dans les autres cas.|  
|CHARACTER_SET_SCHEMA|Chaîne|Retourne toujours la valeur Null.|  
|CHARACTER_SET_NAME|Chaîne|Retourne le nom unique de l'ensemble de caractères si cette colonne contient des données de type caractère ou texte. Renvoie NULL dans les autres cas.|  
|COLLATION_CATALOG|Chaîne|Retourne une liste maître indiquant la base de données dans laquelle le classement est défini, si la colonne contient des données de type caractères ou texte. Sinon, cette colonne est NULL.|  
|IS_FILESTREAM|Chaîne|YES si la colonne présente l'attribut FILESTREAM.<br /><br /> NO si la colonne ne présente pas l'attribut FILESTREAM.|  
|IS_SPARSE|Chaîne|YES s'il s'agit d'une colonne fragmentée.<br /><br /> NO s'il ne s'agit pas d'une colonne fragmentée.|  
|IS_COLUMN_SET|Chaîne|YES s'il s'agit d'une colonne d'ensemble de colonnes.<br /><br /> NO s'il ne s'agit pas d'une colonne d'ensemble de colonnes.|  

## <a name="columnsetcolumns"></a>ColumnSetColumns

La collection de schémas ColumnSetColumns est utilisée pour prendre en charge les colonnes éparses. La collection de schémas ColumnSetColumns retourne le schéma de toutes les colonnes dans un ensemble de colonnes. Le tableau suivant décrit ces colonnes.  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|Chaîne|Catalogue de la table.|  
|TABLE_SCHEMA|Chaîne|Schéma contenant la table.|  
|TABLE_NAME|Chaîne|Nom de la table.|  
|COLUMN_NAME|Chaîne|Nom de la colonne.|  
|ORDINAL_POSITION|Int32|Numéro d'identification de colonne.|  
|COLUMN_DEFAULT|Chaîne|Valeur par défaut de la colonne|  
|IS_NULLABLE|Chaîne|Valeur NULL possible dans la colonne. Si cette colonne autorise les valeurs NULL, elle retourne YES. Dans le cas contraire, elle renvoie NO.|  
|DATA_TYPE|Chaîne|Type de données fourni par le système.|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Longueur maximale (en caractères) des données de type binaire, caractère, texte et image. Renvoie NULL dans les autres cas.|  
|CHARACTER_OCTET_LENGTH|Int32|Longueur maximale, en octets, des données de type binaire, caractère, texte et image. Renvoie NULL dans les autres cas.|  
|NUMERIC_PRECISION|Octet non signé|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|NUMERIC_PRECISION_RADIX|Int16|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|NUMERIC_SCALE|Int32|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|DATETIME_PRECISION|Int16|Code de sous-type pour le type de données datetime et pour le type de données interval de SQL-92. Renvoie NULL pour les autres types de données.|  
|CHARACTER_SET_CATALOG|Chaîne|Retourne une liste maître indiquant la base de données dans laquelle l'ensemble de caractères se trouve et si la colonne contient des données de type caractères ou texte. Renvoie NULL dans les autres cas.|  
|CHARACTER_SET_SCHEMA|Chaîne|Retourne toujours la valeur Null.|  
|CHARACTER_SET_NAME|Chaîne|Retourne le nom unique de l'ensemble de caractères si cette colonne contient des données de type caractère ou texte. Renvoie NULL dans les autres cas.|  
|COLLATION_CATALOG|Chaîne|Retourne une liste maître indiquant la base de données dans laquelle le classement est défini, si la colonne contient des données de type caractères ou texte. Sinon, cette colonne est NULL.|  
|IS_FILESTREAM|Chaîne|YES si la colonne présente l'attribut FILESTREAM.<br /><br /> NO si la colonne ne présente pas l'attribut FILESTREAM.|  
|IS_SPARSE|Chaîne|YES s'il s'agit d'une colonne fragmentée.<br /><br /> NO s'il ne s'agit pas d'une colonne fragmentée.|  
|IS_COLUMN_SET|Chaîne|YES s'il s'agit d'une colonne d'ensemble de colonnes.<br /><br /> NO s'il ne s'agit pas d'une colonne d'ensemble de colonnes.|  

## <a name="users"></a>Utilisateurs  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|uid|Int16|ID d'utilisateur, unique dans cette base de données. 1 est le propriétaire de base de données.|  
|nom_utilisateur|Chaîne|Nom d'utilisateur ou nom de groupe, unique dans cette base de données.|  
|createdate|DateTime|Date de l'ajout du compte.|  
|updatedate|DateTime|Date de la dernière modification du compte.| 

## <a name="views"></a>Les vues  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|Chaîne|Catalogue de la vue.|  
|TABLE_SCHEMA|Chaîne|Schéma contenant la vue.|  
|TABLE_NAME|Chaîne|Nom de la vue.|  
|CHECK_OPTION|Chaîne|Type de WITH CHECK OPTION. Est CASCADE si la vue originale a été créée à l'aide de WITH CHECK OPTION. Renvoie NONE dans le cas contraire.|  
|IS_UPDATABLE|Chaîne|Spécifie si la vue peut être mise à jour. Renvoie toujours NO.|  

## <a name="viewcolumns"></a>ViewColumns  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|VIEW_CATALOG|Chaîne|Catalogue de la vue.|  
|VIEW_SCHEMA|Chaîne|Schéma contenant la vue.|  
|VIEW_NAME|Chaîne|Nom de la vue.|  
|TABLE_CATALOG|Chaîne|Catalogue de la table associée à cette vue.|  
|TABLE_SCHEMA|Chaîne|Schéma contenant la table associée à cette vue.|  
|TABLE_NAME|Chaîne|Nom de la table associée à cette vue. Table de base.|  
|COLUMN_NAME|Chaîne|Nom de la colonne.|  

## <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|assembly_name|Chaîne|Nom du fichier pour l'assembly.|  
|udt_name|Chaîne|Nom de la classe pour l'assembly.|  
|version_major|Object|Numéro de version principale.|  
|version_minor|Object|Numéro de version secondaire.|  
|version_build|Object|Numéro de build.|  
|version_revision|Object|Numéro de révision.|  
|culture_info|Object|Informations de culture associées à cet UDT.|  
|public_key|Object|Clé publique utilisée par cet assembly.|  
|is_fixed_length|Booléen|Indique si la longueur du type est toujours identique à max_length.|  
|max_length|Int16|Longueur maximale du type en octets.|  
|Create_Date|DateTime|Date à laquelle l'assembly a été créé/enregistré.|  
|Permission_set_desc|Chaîne|Nom convivial de l'ensemble d'autorisations/niveau de sécurité pour l'assembly.|  

## <a name="see-also"></a>Voir aussi

- [Extraction des informations de schéma de base de données](retrieving-database-schema-information.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
