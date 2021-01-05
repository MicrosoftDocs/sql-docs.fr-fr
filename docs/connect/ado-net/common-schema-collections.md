---
title: Collections de schémas courants
description: Décrit toutes les collections de schémas courants prises en charge par tous les fournisseurs managés .NET.
ms.date: 11/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 09e1a159d0b0ad1cb58c43bd23b1e7be500e7ae3
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771656"
---
# <a name="common-schema-collections"></a>Collections de schémas courants

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Les collections de schémas courants sont les collections de schémas implémentées par chacun des fournisseurs managés .NET. Vous pouvez interroger un fournisseur managé .NET pour établir la liste des collections de schémas prises en charge en appelant la méthode **GetSchema** sans argument ou avec le nom de collection de schémas « MetaDataCollections ». Cette opération retourne un <xref:System.Data.DataTable> avec une liste des collections de schémas prises en charge, le nombre de restrictions qu’elles prennent en charge et le nombre d’éléments d’identification qu’elles utilisent. Ces collections décrivent toutes les colonnes requises. Les fournisseurs sont libres d'ajouter des colonnes s'ils le souhaitent. Par exemple, le fournisseur de données Microsoft SqlClient pour SQL Server ajoute ParameterName à la collection de restrictions.  

Si un fournisseur est incapable de déterminer la valeur d'une colonne obligatoire, il retourne la valeur null.  
  
Pour plus d’informations sur l’utilisation des méthodes **GetSchema**, consultez [Collections GetSchema et Schema](getschema-and-schema-collections.md).  

## <a name="metadatacollections"></a>MetaDataCollections  

Cette collection de schémas expose des informations sur toutes les collections de schémas prises en charge par le fournisseur de données Microsoft SqlClient pour SQL Server actuellement utilisé pour se connecter à la base de données.  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|CollectionName|string|Nom de la collection à passer à la méthode **GetSchema** pour retourner la collection.|  
|NumberOfRestrictions|int|Nombre de restrictions qui peuvent être spécifiées pour la collection.|  
|NumberOfIdentifierParts|int|Nombre de parties dans le nom d'objet identificateur/base de données composite. Par exemple, dans SQL Server, ce serait 3 pour les tables et 4 pour les colonnes.|  

## <a name="datasourceinformation"></a>DataSourceInformation  

Cette collection de schémas expose des informations sur la source de données à laquelle le fournisseur de données Microsoft SqlClient pour SQL Server est actuellement connecté.  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|CompositeIdentifierSeparatorPattern|string|Expression régulière pour mettre en correspondance les séparateurs composites dans un identificateur composite. Par exemple, «\\.» (pour SQL Server).<br /><br /> Un identificateur composite est généralement utilisé pour un nom d’objet de base de données, par exemple : pubs.dbo.authors ou pubs\@dbo.authors.<br /><br /> Pour SQL Server, utilisez l’expression régulière « \\. ». |  
|DataSourceProductName|string|Nom du produit auquel accède le fournisseur, par exemple « SQLServer ».|  
|DataSourceProductVersion|string|Indique la version du produit auquel accède le fournisseur, dans le format natif des sources de données et non dans un format Microsoft.<br /><br /> Dans certains cas, DataSourceProductVersion et DataSourceProductVersionNormalized ont la même valeur. |  
|DataSourceProductVersionNormalized|string|Version normalisée pour la source de données, telle qu'elle peut être comparée à `String.Compare()`. Son format est identique pour toutes les versions du fournisseur afin d'empêcher la version 10 d'opérer un tri entre les versions 1 et 2.<br /><br /> Par exemple, SQL Server utilise le format « nn.nn.nnnn » Microsoft classique.<br /><br /> Dans certains cas, DataSourceProductVersion et DataSourceProductVersionNormalized ont la même valeur. |  
|GroupByBehavior|<xref:System.Data.Common.GroupByBehavior>|Spécifie la relation entre les colonnes dans une clause GROUP BY et les colonnes non agrégées dans la liste de sélection.|  
|IdentifierPattern|string|Expression régulière qui correspond à un identificateur et dont la valeur de correspondance est l'identificateur. Par exemple, « [A-Za-z0-9_#$] ».|  
|IdentifierCase|<xref:System.Data.Common.IdentifierCase>|Indique si des identificateurs non entourés de guillemets sont traités ou non comme respectant la casse.|  
|OrderByColumnsInSelect|bool|Spécifie si les colonnes d'une clause ORDER BY doivent figurer dans la liste de sélection. Une valeur true indique qu'elles doivent obligatoirement figurer dans la liste de sélection ; une valeur false indique qu'elles ne doivent pas obligatoirement figurer dans la liste de sélection.|  
|ParameterMarkerFormat|string|Chaîne de format représentant la manière de formater un paramètre.<br /><br /> Si les paramètres nommés sont pris en charge par la source de données, le premier espace réservé dans cette chaîne doit être l'emplacement où le nom de paramètre doit être formaté.<br /><br /> Par exemple, si la source de données attend des paramètres nommés et précédés d’un préfixe « : », cela donne « :{0} ». En cas de formatage avec un nom de paramètre « p1 », la chaîne obtenue est « :p1 ».<br /><br /> Si la source de données attend des paramètres précédés du préfixe « \@ », mais qu’ils sont déjà présents dans les noms, cela donne « {0} » et le résultat du formatage d'un paramètre nommé « \@p1 » est simplement « \@p1 ».<br /><br /> Pour les sources de données qui n’attendent pas de paramètres nommés et attendent l’utilisation du caractère « ? », la chaîne de format peut être spécifiée simplement comme « ? ». Dans ce cas, le nom de paramètre est ignoré. |  
|ParameterMarkerPattern|string|Expression régulière représentant un marqueur de paramètre. Elle a pour valeur de correspondance éventuelle le nom de paramètre.<br /><br /> Par exemple, si les paramètres nommés sont pris en charge avec un caractère initial « \@ » qui sera inclus dans le nom de paramètre, cela donne : « (\@[A-Za-z0-9_$#]*) ».<br /><br /> En revanche, si les paramètres nommés sont pris en charge avec un « : » comme caractère initial ne faisant pas partie du nom de paramètre, cela donne : « :([A-Za-z0-9_$#]\*) ».<br /><br /> Bien sûr, si la source de données ne prend pas en charge les paramètres nommés, cela donne simplement « ? ».|  
|ParameterNameMaxLength|int|Longueur maximale d'un nom de paramètre en caractères. Si les noms de paramètres sont pris en charge, Visual Studio attend que la valeur minimale de longueur maximale soit de 30 caractères.<br /><br /> Si la source de données ne prend pas en charge les paramètres nommés, cette propriété retourne zéro.|  
|ParameterNamePattern|string|Expression régulière représentant les noms de paramètre valides. Les différentes sources de données ont des règles différentes concernant les caractères qui peuvent être utilisés pour les noms de paramètre.<br /><br /> Si les noms de paramètre sont pris en charge, Visual Studio attend que les caractères « \p{Lu}\p{Ll}\p{Lt}\p{Lm}\p{Lo}\p{Nl}\p{Nd} » correspondent à l'ensemble minimal pris en charge de caractères valides pour les noms de paramètre.|  
|QuotedIdentifierPattern|string|Expression régulière qui correspond à un identificateur entre guillemets et qui a pour valeur de correspondance l'identificateur proprement dit, sans les guillemets. Par exemple, si la source de données utilise des guillemets doubles pour identifier des identificateurs entre guillemets, cela donne : « (([^\\"]&#124;\\"\\")*) ».|  
|QuotedIdentifierCase|<xref:System.Data.Common.IdentifierCase>|Indique si des identificateurs entourés de guillemets sont traités ou non comme respectant la casse.|  
|StatementSeparatorPattern|string|Expression régulière représentant le séparateur d'instruction.|  
|StringLiteralPattern|string|Expression régulière qui correspond à un littéral de chaîne et dont la valeur de correspondance est le littéral proprement dit. Par exemple, si la source de données utilise des guillemets simples pour identifier des chaînes, cela donne : "('([^']&#124;'')*')"'|  
|SupportedJoinOperators|<xref:System.Data.Common.SupportedJoinOperators>|Spécifie les types d'instructions SQL jointes prises en charge par la source de données.|  

## <a name="datatypes"></a>DataTypes  

Cette collection de schémas expose des informations sur les types de données pris en charge par la base de données à laquelle le fournisseur de données Microsoft SqlClient pour SQL Server est actuellement connecté.  

|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|TypeName|string|Nom du type de données spécifique au fournisseur.|  
|ProviderDbType|int|Valeur de type spécifique du fournisseur qui doit être utilisée au moment de spécifier le type d’un paramètre. Par exemple, SqlDbType.Money.|  
|ColumnSize|long|La longueur d'une colonne ou d'un paramètre non numérique fait référence à la longueur maximale ou à la longueur définie pour ce type par le fournisseur.<br /><br /> Pour les données de type caractère, il s'agit de la longueur maximale ou de la longueur en unités définie par la source de données. <br /><br /> Pour les données de type date-heure, il s'agit de la longueur de la représentation de chaîne (en supposant la précision maximale autorisée de la partie fractions de secondes).<br /><br /> Si les données sont de type numérique, il s'agit de la limite supérieure de la précision maximale du type de données.|  
|CreateFormat|string|Chaîne de format représentant la manière d'ajouter cette colonne à une instruction de définition de données, telle que CREATE TABLE. Chaque élément dans le tableau CreateParameter doit être représenté par un « marqueur de paramètre » dans la chaîne de format.<br /><br /> Par exemple, le type de données SQL DECIMAL nécessite une précision et une échelle. Dans ce cas, la chaîne de format est « DECIMAL({0},{1}) ».|  
|CreateParameters|string|Paramètres de création à spécifier lors de la création d'une colonne de ce type de données. Les paramètres de création sont répertoriés dans la chaîne, avec des virgules de séparation, dans l'ordre dans lequel ils doivent être fournis.<br /><br /> Par exemple, le type de données SQL DECIMAL nécessite une précision et une échelle. Dans ce cas, les paramètres de création doivent contenir la chaîne « precision, scale ».<br /><br /> Dans une commande de texte destinée à créer une colonne DECIMAL avec une précision de 10 et une échelle de 2, la valeur de la colonne CreateFormat peut être « DECIMAL({0},{1}) » et la spécification de type complète est DECIMAL(10,2).|  
|DataType|string|Nom du type .NET du type de données.|  
|IsAutoincrementable|bool|true — Les valeurs de ce type de données peuvent être auto-incrémentées.<br /><br /> false — Les valeurs de ce type de données ne peuvent pas être auto-incrémentées.<br /><br /> Notez que cela indique simplement si une colonne de ce type de données peut être auto-incrémentée, pas que toutes les colonnes de ce type le sont.|  
|IsBestMatch|bool|true – Le type de données est la correspondance la plus proche entre tous les types de données du magasin de données et le type de données .NET indiqué par la valeur de la colonne DataType.<br /><br /> false — Le type de données n'est pas la meilleure correspondance.<br /><br /> Pour chaque ensemble de lignes dans lequel la valeur de la colonne DataType est identique, la colonne IsBestMatch est définie comme true dans une seule ligne.|  
|IsCaseSensitive|bool|true — Le type de données est un type de caractère respectant la casse.<br /><br /> false — Le type de données n'est pas un type de caractère ou ne respecte pas la casse.|  
|IsFixedLength|bool|true — Les colonnes de ce type de données créées par la DDL sont de longueur fixe.<br /><br /> false — Les colonnes de ce type de données créées par la DDL sont de longueur variable.<br /><br /> DBNull.Value — Il est impossible de déterminer si le fournisseur mappera ce champ avec une colonne de longueur fixe ou variable.|  
|IsFixedPrecisionScale|bool|true — Le type de données a une précision et une échelle fixes.<br /><br /> false — Le type de données n'a pas de précision ni d'échelle fixes.|  
|IsLong|bool|true — Le type de données contient des données très longues ; la définition de données très longues est spécifique au fournisseur.<br /><br /> false — Le type de données ne contient pas de données très longues.|  
|IsNullable|bool|true — Le type de données est Nullable.<br /><br /> false — Le type de données n'est pas Nullable.<br /><br /> DBNull.Value — Il est impossible de déterminer si le type de données est Nullable.|  
|IsSearchable|bool|true — Le type de données peut être utilisé dans une clause WHERE avec tout opérateur, à l'exception du prédicat LIKE.<br /><br /> false — Le type de données ne peut pas être utilisé dans une clause WHERE avec un opérateur, à l’exception du prédicat LIKE.|  
|IsSearchableWithLike|bool|true — Le type de données peut être utilisé avec le prédicat LIKE.<br /><br /> false — Le type de données ne peut pas être utilisé avec le prédicat LIKE.|  
|IsUnsigned|bool|true — Le type de données n'est pas signé.<br /><br /> false — Le type de données est signé.<br /><br /> DBNull.Value — Non applicable au type de données.|  
|MaximumScale|short|Si l'indicateur de type est un type numérique, il correspond au nombre maximal de chiffres autorisés à droite de la virgule décimale. Sinon, c'est DBNull.Value.|  
|MinimumScale|short|Si l'indicateur de type est un type numérique, il correspond au nombre minimal de chiffres autorisés à droite de la virgule décimale. Sinon, c'est DBNull.Value.|  
|IsConcurrencyType|bool|true — Le type de données est mis à jour par la base de données à chaque modification de la ligne et la valeur de la colonne diffère de toutes les valeurs précédentes.<br /><br /> false — Le type de données n'est pas mis à jour par la base de données à chaque modification de la ligne.<br /><br /> DBNull.Value — La base de données ne prend pas en charge ce type de données.|  
|IsLiteralSupported|bool|true — Le type de données peut être exprimé comme littéral.<br /><br /> false — Le type de données ne peut pas être exprimé comme littéral.|  
|LiteralPrefix|string|Préfixe appliqué à un littéral donné.|  
|LiteralSuffix|string|Suffixe appliqué à un littéral donné.|  

## <a name="restrictions"></a>Restrictions 

Cette collection de schémas exposait des informations sur les restrictions prises en charge par le fournisseur de données Microsoft SqlClient pour SQL Server actuellement utilisé pour se connecter à la base de données.  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|CollectionName|string|Nom de la collection à laquelle ces restrictions s'appliquent.|  
|RestrictionName|string|Nom de la restriction dans la collection.|  
|RestrictionDefault|string|Ignoré.|  
|RestrictionNumber|int|Emplacement réel des restrictions de collections dans lequel figure cette restriction particulière.|  

## <a name="reservedwords"></a>ReservedWords  

Cette collection de schémas expose des informations sur les mots réservés par la base de données à laquelle le fournisseur de données Microsoft SqlClient pour SQL Server est actuellement connecté.  
  
|ColumnName|DataType|Description|  
|----------------|--------------|-----------------|  
|ReservedWord|string|Mot réservé spécifique du fournisseur.|  

## <a name="see-also"></a>Voir aussi

- [Extraction des informations de schéma de base de données](retrieving-database-schema-information.md)
- [Collections GetSchema et Schema](getschema-and-schema-collections.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
