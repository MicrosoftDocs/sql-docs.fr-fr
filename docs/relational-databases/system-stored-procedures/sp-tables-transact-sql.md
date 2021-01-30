---
description: sp_tables (Transact-SQL)
title: sp_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tables
- sp_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables
ms.assetid: 787a2fa5-87a1-49bd-938b-6043c245f46b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a176073f8951cd15acd34aa00b92fc268f90c27e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180580"
---
# <a name="sp_tables-transact-sql"></a>sp_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une liste d'objets qui peuvent être interrogés dans l'environnement actuel. Cela signifie toute table ou vue, à l'exception des objets synonymes.  
  
> [!NOTE]  
>  Pour déterminer le nom de l’objet de base d’un synonyme, interrogez l’affichage catalogue [sys. synonymes](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_tables [ [ @table_name = ] 'name' ]   
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @table_type = ] "type" ]   
     [ , [@fUsePattern = ] 'fUsePattern'];  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table_name = ] 'name'` Table utilisée pour retourner les informations de catalogue. *Name* est de type **nvarchar (384)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
`[ @table_owner = ] 'owner'` Propriétaire de la table utilisée pour retourner les informations de catalogue. *owner* est de type **nvarchar (384)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si le propriétaire n'est pas précisé, les règles par défaut définissant la visibilité des tables du SGBD sous-jacent sont utilisées.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si l'utilisateur actuel est propriétaire d'une table portant le nom spécifié, les colonnes de cette table sont renvoyées. Si le propriétaire n'est pas spécifié et que l'utilisateur actuel ne possède pas de table ayant le nom spécifié, la procédure recherche une table portant le nom spécifié, possédée par le propriétaire de la base de données. Si la recherche de la table aboutit, ce sont les colonnes de cette dernière qui sont retournées.  
  
`[ @table_qualifier = ] 'qualifier'` Nom du qualificateur de la table. *qualifier* est de **type sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge les noms de tables en trois parties (_qualificateur_**.** _propriétaire_**.** _nom_). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
``[ , [ @table_type = ] "'type', 'type'" ]`` Liste de valeurs, séparées par des virgules, qui donne des informations sur toutes les tables des types de tables qui sont spécifiés. Il s’agit notamment de **table**, **SYSTEMTABLE** et **View**. *type* **varchar (100)**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Chaque type de table doit être encadré de guillemets simples, l'ensemble du paramètre devant être encadré de guillemets doubles. Les types de table sont en lettres majuscules. Si SET QUOTED_IDENTIFIER est défini à ON, chaque guillemet simple doit devenir double et l'ensemble du paramètre doit être encadré de guillemets simples.  
  
`[ @fUsePattern = ] 'fUsePattern'` Détermine si les caractères de soulignement (_), de pourcentage (%) et de crochets ([ou]) sont interprétés comme des caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est de **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nom du qualificateur de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Ce champ peut contenir la valeur NULL.|  
|**TABLE_OWNER**|**sysname**|Nom du propriétaire de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de l'utilisateur de la base de données qui a créé la table. Ce champ retourne toujours une valeur.|  
|**TABLE_NAME**|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|**TABLE_TYPE**|**varchar(32)**|Table, table système ou vue.|  
|**NOTES**|**varchar (254)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur pour cette colonne.|  
  
## <a name="remarks"></a>Notes  
 Pour assurer une interopérabilité maximale, le client de la passerelle ne doit utiliser que les critères spéciaux SQL standard de SQL-92 (caractères génériques % et _).  
  
 Les informations de privilège concernant l'accès en lecture/écriture de l'utilisateur actuel à une table spécifique ne sont pas toujours vérifiées. Par conséquent, l'accès n'est pas garanti. Ce jeu de résultats ne comprend pas uniquement des tables et des vues, mais également des synonymes et des noms d'alias, dans le cas des passerelles vers les SGBD qui gèrent ces types. Si l’attribut de serveur **ACCESSIBLE_TABLES** est Y dans le jeu de résultats pour **sp_server_info**, seules les tables accessibles par l’utilisateur actuel sont retournées.  
  
 **sp_tables** équivaut à **SQLTables** dans ODBC. Les résultats retournés sont triés par **TABLE_TYPE**, **TABLE_QUALIFIER**, **TABLE_OWNER** et **table_name**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>R. Retour d'une liste d'objets pouvant être sollicités dans l'environnement actuel  
 L’exemple suivant retourne une liste d’objets qui peuvent être des requêtes dans l’environnement actuel.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="b-returning-information-about-the-tables-in-a-specified-schema"></a>B. Retour d'informations sur les tables dans un schéma spécifié  
 L'exemple suivant retourne des informations sur les tables appartenant au schéma `Person` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tables   
   @table_name = '%',  
   @table_owner = 'Person',  
   @table_qualifier = 'AdventureWorks2012';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>C. Retour d'une liste d'objets pouvant être sollicités dans l'environnement actuel  
 L’exemple suivant retourne une liste d’objets qui peuvent être des requêtes dans l’environnement actuel.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="d-returning-information-about-the-tables-in-a-specified-schema"></a>D. Retour d'informations sur les tables dans un schéma spécifié  
 L’exemple suivant retourne des informations sur les tables de dimension de la `AdventureWorksPDW201` base de données.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_tables   
   @table_name = 'Dim%',  
   @table_owner = 'dbo',  
   @table_qualifier = 'AdventureWorksPDW2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys. synonymes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

