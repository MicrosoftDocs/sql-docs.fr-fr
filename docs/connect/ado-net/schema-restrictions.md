---
title: Restrictions de schéma
description: Décrit les restrictions de schéma qui peuvent être utilisées avec GetSchema quand le fournisseur de données Microsoft SqlClient pour SQL Server est utilisé.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8d72e2dd020f1345ceb0b68249cb3d3acd1024dc
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051326"
---
# <a name="schema-restrictions"></a>Restrictions de schéma

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le deuxième paramètre facultatif de la méthode **GetSchema** correspond aux restrictions destinées à limiter la quantité d’informations de schéma retournées ; il est passé à la méthode **GetSchema** sous la forme d’un tableau de chaînes. La position dans le tableau détermine les valeurs que vous pouvez passer et équivaut au numéro de restriction.  
  
Par exemple, le tableau suivant décrit les restrictions prises en charge par la collection de schémas « Tables » en utilisant le fournisseur de données Microsoft SqlClient pour SQL Server. Des restrictions supplémentaires pour les collections de schémas SQL Server figurent à la fin de cette rubrique.  

|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|TABLE_CATALOG|1|  
|Propriétaire|@Owner|TABLE_SCHEMA|2|  
|Table de charge de travail|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
 
## <a name="specifying-restriction-values"></a>Spécification des valeurs de restriction  

Pour utiliser l’une des restrictions de la collection de schémas « Tables », créez simplement un tableau de chaînes contenant quatre éléments, puis placez une valeur dans l’élément correspondant au numéro de restriction. Par exemple, pour limiter les tables retournées par la méthode **GetSchema** à celles du schéma « Sales » uniquement, définissez le deuxième élément du tableau sur « Sales » avant de le passer à la méthode **GetSchema**.  
  
> [!NOTE]
> - Les collections de restrictions pour `SqlClient` présentent une colonne `ParameterName` supplémentaire. La colonne par défaut de la restriction est encore là pour la compatibilité ascendante, mais est désormais ignorée. Il convient d'utiliser des requêtes paramétrées plutôt qu'un remplacement de chaîne afin de minimiser le risque d'attaque par injection SQL lors de la spécification de valeurs de restriction.  
> - Le nombre d'éléments du tableau doit être inférieur ou égal au nombre de restrictions prises en charge pour la collection de schémas spécifiée, sans quoi une exception <xref:System.ArgumentException> est levée. Il peut y avoir moins de restrictions que le nombre maximal de restrictions. Les restrictions manquantes sont supposées avoir la valeur null (aucune restriction).  
  
Vous pouvez interroger le fournisseur de données Microsoft SqlClient pour SQL Server pour établir la liste des restrictions prises en charge en appelant la méthode **GetSchema** avec le nom de la collection de schémas de restrictions, « Restrictions ». Cette opération retourne un <xref:System.Data.DataTable> contenant une liste des noms de collections, des noms de restriction, des valeurs de restriction par défaut et des numéros de restriction.  
  
### <a name="example"></a>Exemple  

Les exemples suivants montrent comment utiliser la méthode <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> de la classe <xref:Microsoft.Data.SqlClient.SqlConnection> du fournisseur de données Microsoft SqlClient pour SQL Server afin d’extraire les informations de schéma sur toutes les tables contenues dans l’exemple de base de données **AdventureWorks** et de limiter les informations retournées exclusivement aux tables du schéma « Sales » :  

[!code-csharp[SqlClient GetSchema with restrictions#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Restriction.cs#1)]  

## <a name="sql-server-schema-restrictions"></a>Restrictions de schéma SQL Server  

 Les tableaux suivants énumèrent les restrictions pour les collections de schémas SQL Server.  
  
### <a name="users"></a>Utilisateurs  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|User_Name|@Name|name|1|  
  
### <a name="databases"></a>Bases de données  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Nom|@Name|Nom|1|  
  
### <a name="tables"></a>Tables  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|TABLE_CATALOG|1|  
|Propriétaire|@Owner|TABLE_SCHEMA|2|  
|Table de charge de travail|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
  
### <a name="columns"></a>Colonnes  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|TABLE_CATALOG|1|  
|Propriétaire|@Owner|TABLE_SCHEMA|2|  
|Table de charge de travail|@Table|TABLE_NAME|3|  
|Colonne|@Column|COLUMN_NAME|4|  
  
### <a name="structuredtypemembers"></a>StructuredTypeMembers  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|TABLE_CATALOG|1|  
|Propriétaire|@Owner|TABLE_SCHEMA|2|  
|Table de charge de travail|@Table|TABLE_NAME|3|  
|Colonne|@Column|COLUMN_NAME|4|  
  
### <a name="views"></a>Les vues  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|TABLE_CATALOG|1|  
|Propriétaire|@Owner|TABLE_SCHEMA|2|  
|Table de charge de travail|@Table|TABLE_NAME|3|  
  
### <a name="viewcolumns"></a>ViewColumns  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|VIEW_CATALOG|1|  
|Propriétaire|@Owner|VIEW_SCHEMA|2|  
|Table de charge de travail|@Table|VIEW_NAME|3|  
|Colonne|@Column|COLUMN_NAME|4|  
  
### <a name="procedureparameters"></a>ProcedureParameters  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|SPECIFIC_CATALOG|1|  
|Propriétaire|@Owner|SPECIFIC_SCHEMA|2|  
|Nom|@Name|SPECIFIC_NAME|3|  
|Paramètre|@Parameter|PARAMETER_NAME|4|  
  
### <a name="procedures"></a>Procédures  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|SPECIFIC_CATALOG|1|  
|Propriétaire|@Owner|SPECIFIC_SCHEMA|2|  
|Nom|@Name|SPECIFIC_NAME|3|  
|Type|@Type|ROUTINE_TYPE|4|  
  
### <a name="indexcolumns"></a>IndexColumns  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|db_name()|1|  
|Propriétaire|@Owner|user_name()|2|  
|Table de charge de travail|@Table|o.name|3|  
|ConstraintName|@ConstraintName|x.name|4|  
|Colonne|@Column|c.name|5|  
  
### <a name="indexes"></a>Index  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|db_name()|1|  
|Propriétaire|@Owner|user_name()|2|  
|Table de charge de travail|@Table|o.name|3|  
  
### <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|assembly_name|@AssemblyName|assemblies.name|1|  
|udt_name|@UDTName|types.assembly_class|2|  
  
### <a name="foreignkeys"></a>ForeignKeys  
  
|Nom de restriction|Nom du paramètre|Valeur par défaut de la restriction|Numéro de restriction|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogue|@Catalog|CONSTRAINT_CATALOG|1|  
|Propriétaire|@Owner|CONSTRAINT_SCHEMA|2|  
|Table de charge de travail|@Table|TABLE_NAME|3|  
|Nom|@Name|CONSTRAINT_NAME|4|  
  
## <a name="see-also"></a>Voir aussi

- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
