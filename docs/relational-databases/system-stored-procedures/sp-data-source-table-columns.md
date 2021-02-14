---
title: sp_data_source_table_columns
description: sp_data_source_table_columns (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_table_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
author: MikeRayMSFT
ms.author: mikeray
ms.custom: ''
ms.date: 11/10/2020
ms.openlocfilehash: 31b0dedb8544cc6752eac8a9934aa92d74d424ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081870"
---
# <a name="sp_data_source_table_columns-transact-sql"></a>sp_data_source_table_columns (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Retourne la liste des colonnes dans la table de source de données externe.
  
> [!NOTE]
> Cette procédure est présentée dans [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5).

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sqlsyntax
sp_data_source_table_columns
         [ @data_source = ] 'data_source'
       , [ @table_location = ] 'table_location'
[ ; ]
```  

## <a name="arguments"></a>Arguments

`[ @data_source = ] 'data_source'`   
Nom de la source de données externe à partir de laquelle récupérer les métadonnées. Le type est `sysname` .

`[ @table_location = ] 'table_location'`   
Chaîne d’emplacement de table qui identifie la table. `table_location` le type est `nvarchar(max)` .

## <a name="returns"></a>retourne :

La procédure stockée retourne les informations suivantes :

|Nom de la colonne |Type de données |Description|
|---|---|---|
|NOM|nvarchar(max)|Nom de la colonne.
|TYPE|nvarchar(200)|Nom du type de SQL Server
|LENGTH|int|Longueur de la colonne
|PRECISION|int|Précision de la colonne
|SCALE|int|Échelle de la colonne
|COLLATION|nvarchar(200)|SQL Server le classement de la colonne
|IS_NULLABLE|bit|Cette colonne peut être null.
|SOURCE_TYPE_NAME|nvarchar(max)|Nom de type spécifique au serveur principal. Principalement utilisé pour le débogage. Pour les sources ODBC, cela correspond à la colonne de résultats TYPE_NAME pour SQLColumns ().
|REMARQUES|nvarchar(max)|Commentaires généraux ou description de la colonne. Actuellement, toujours NULL.|

## <a name="permissions"></a>Autorisations  

Exige l’autorisation ALTER ANY EXTERNAL DATA SOURCE.
  
## <a name="remarks"></a>Notes  

La fonctionnalité  [Polybase](../../relational-databases/polybase/polybase-guide.md) doit être installée sur l’instance SQL Server.

Cette procédure stockée prend en charge les connecteurs pour :

- SQL Server
- Oracle
- Teradata
- MongoDB
- CosmosDB

La procédure stockée ne prend pas en charge les connecteurs sources ODBC DTA génériques.

La notion de Empty et non Empty est associée au comportement du pilote ODBC et à la [ `SQLTables` fonction](../native-client-odbc-api/sqltables.md). Non vide indique qu’un objet contient des tables, et non des lignes. Par exemple, un schéma vide ne contient aucune table dans SQL Server. Une base de données vide contient sans tables dans Teradata. les résultats sont une représentation SQL Server du schéma principal tel qu’il est interprété par le connecteur Polybase pour le serveur principal. La distinction ici est que, au lieu de passer simplement les résultats de l’appel ODBC au backend, les résultats sont basés sur le résultat du code de mappage de type Polybase.

Utilisez [`sp_data_source_objects`](sp-data-source-objects.md) et `sp_data_source_table_columns` pour découvrir des objets externes. Ces procédures stockées système retournent le schéma des tables disponibles pour la virtualisation. Azure Data Studio utilise ces deux procédures stockées pour prendre en charge [la virtualisation des données](../../azure-data-studio/extensions/data-virtualization-extension.md). Utilisez `sp_data_source_table_columns` pour découvrir les schémas de table externes représentés dans SQL Server types de données.

En raison des différences entre les classements dans les données sources Hadoop et les classements pris en charge dans SQL Server 2019, les longueurs de type de données recommandées pour les colonnes de type de données varchar dans les tables externes peuvent être plus volumineuses que prévu. C'est la procédure normale.

## <a name="example"></a>Exemple  

L’exemple suivant retourne les colonnes d’une table externe d’un SQL Server nommé `server` , appartenant à un schéma nommé `schema` .
  
```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @table_location NVARCHAR(400) = N'[database].[schema].[table]';
EXEC sp_data_source_table_columns @data_source, @table_location
```  
  
## <a name="see-also"></a>Voir également

- [Prise en main de PolyBase](../polybase/polybase-guide.md)
- [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)