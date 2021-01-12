---
description: INSERT (Transact-SQL)
title: INSERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INSERT_TSQL
- INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- inserting multiple rows
- user-defined types [SQL Server], inserting values
- DML [SQL Server], INSERT statement
- bulk load [SQL Server]
- row additions [SQL Server], INSERT statement
- inserting rows
- table value constructor [SQL Server]
- adding data
- INSERT statement [SQL Server], about INSERT statement
- INSERT statement [SQL Server]
- UDTs [SQL Server], inserting values
- adding rows
- INSERT INTO statement
- data manipulation language [SQL Server], INSERT statement
- inserting data
ms.assetid: 1054c76e-0fd5-4131-8c07-a6c5d024af50
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6cccb11c84b9b5354d418dae19557f7f76e027cf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095729"
---
# <a name="insert-transact-sql"></a>INSERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Ajoute une ou plusieurs lignes à une table ou une vue dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir des exemples, consultez [Exemples](#InsertExamples).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  

[ WITH <common_table_expression> [ ,...n ] ]  
INSERT   
{  
        [ TOP ( expression ) [ PERCENT ] ]   
        [ INTO ]   
        { <object> | rowset_function_limited   
          [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
        }  
    {  
        [ ( column_list ) ]   
        [ <OUTPUT Clause> ]  
        { VALUES ( { DEFAULT | NULL | expression } [ ,...n ] ) [ ,...n     ]   
        | derived_table   
        | execute_statement  
        | <dml_table_source>  
        | DEFAULT VALUES   
        }  
    }  
}  
[;]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
      | database_name .[ schema_name ] .   
      | schema_name .   
    ]  
  table_or_view_name  
}  
  
<dml_table_source> ::=  
    SELECT <select_list>  
    FROM ( <dml_statement_with_output_clause> )   
      [AS] table_alias [ ( column_alias [ ,...n ] ) ]  
    [ WHERE <search_condition> ]  
        [ OPTION ( <query_hint> [ ,...n ] ) ]  
```  
  
```syntaxsql
-- External tool only syntax  

INSERT   
{  
    [BULK]  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
    ( <column_definition> )  
    [ WITH (  
        [ [ , ] CHECK_CONSTRAINTS ]  
        [ [ , ] FIRE_TRIGGERS ]  
        [ [ , ] KEEP_NULLS ]  
        [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]  
        [ [ , ] ROWS_PER_BATCH = rows_per_batch ]  
        [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]  
        [ [ , ] TABLOCK ]  
    ) ]  
}  
  
[; ] <column_definition> ::=  
 column_name <data_type>  
    [ COLLATE collation_name ]  
    [ NULL | NOT NULL ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

INSERT INTO { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    {   
      VALUES ( { NULL | expression } )  
      | SELECT <select_criteria>  
    }  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 WITH \<common_table_expression>  
 Spécifie le jeu de résultats nommé temporaire, également appelé expression de table commune, défini dans l'étendue de l'instruction INSERT. Le jeu de résultats est dérivé d'une instruction SELECT. Pour plus d’informations, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP (*expression*) [ PERCENT ]  
 Spécifie le nombre ou le pourcentage de lignes aléatoires qui seront insérées. L'argument *expression* peut être un nombre ou un pourcentage de lignes. Pour plus d’informations, consultez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 INTO  
 Mot clé facultatif qui peut être inséré entre le mot clé INSERT et la table cible.  
  
 *server_name*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Nom du serveur lié sur lequel se trouve la table ou la vue. *server_name* peut être spécifié comme nom de [serveur lié](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) ou à l’aide de la fonction [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md).  
  
 Quand *server_name* est spécifié comme serveur lié, *database_name* et *schema_name* sont obligatoires. Quand *server_name* est spécifié avec OPENDATASOURCE, *database_name* et *schema_name* peuvent ne pas s’appliquer à toutes les sources de données ; par ailleurs, ils dépendent des fonctionnalités du fournisseur OLE DB qui accède à l’objet distant.  
  
 *database_name*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel la table ou la vue appartient.  
  
 *table_or view_name*  
 Nom de la table ou de la vue qui doit recevoir les données.  
  
 Une variable de [table](../../t-sql/data-types/table-transact-sql.md), dans son étendue, peut être utilisée comme source de table dans une instruction INSERT.  
  
 La vue référencée par *table_or_view_name* doit pouvoir être mise à jour et faire référence à une seule table de base dans la clause FROM de la vue. Par exemple, une instruction INSERT dans une vue contenant plusieurs tables doit utiliser un *column_list* qui référence uniquement les colonnes d’une seule table de base. Pour plus d’informations sur les vues pouvant être mises à jour, consultez [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Fonction [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) ou [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md). L'utilisation de ces fonctions dépend des fonctionnalités du fournisseur OLE DB qui accède à l'objet distant.  
  
 WITH ( \<table_hint_limited> [... *n* ] )  
 Spécifie un ou plusieurs indicateurs de table autorisés pour une table cible. Le mot clé WITH et les parenthèses sont obligatoires.  
  
 READPAST, NOLOCK et READUNCOMMITTED ne sont pas autorisés. Pour plus d’informations sur les indicateurs de table, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
> [!IMPORTANT]  
>  La possibilité de spécifier les indicateurs HOLDLOCK, SERIALIZABLE, READCOMMITTED, REPEATABLEREAD ou UPDLOCK sur les tables qui sont des cibles d'instructions INSERT sera supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces indicateurs n'affectent pas les performances des instructions INSERT. Évitez de les utiliser dans les nouveaux travaux de développement et prévoyez la modification des applications qui les utilisent actuellement.  
  
 La spécification de l'indicateur TABLOCK sur une table qui est la cible d'une instruction INSERT a le même effet que la spécification de l'indicateur TABLOCKX. Un verrou exclusif est appliqué à la table.  
  
 (*column_list*)  
 Liste d'une ou plusieurs colonnes dans lesquelles insérer des données. *column_list* doit être placé entre parenthèses et délimité par des virgules.  
  
 Si une colonne ne se trouve pas dans *column_list*, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit pouvoir fournir une valeur basée sur la définition de la colonne ; sinon, il n’est pas possible de charger la ligne. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fournit automatiquement une valeur pour la colonne si :  
  
-   a une propriété IDENTITY. la valeur d'identité incrémentielle suivante est utilisée ;  
  
-   elle a une valeur par défaut, la valeur par défaut de la colonne est utilisée ;  
  
-   la colonne a un type de données **timestamp** ; la valeur d'horodateur actuelle est utilisée ;  
  
-   Autorise la valeur NULL. Une valeur Null est utilisée.  
  
-   Colonne calculée. la valeur calculée est utilisée.  
  
*column_list* doit être utilisé lors de l’insertion de valeurs explicites dans une colonne d’identité ; par ailleurs, l’option SET IDENTITY_INSERT doit avoir la valeur ON pour la table.  
  
Clause OUTPUT  
 Retourne des lignes insérées dans le cadre de l'opération d'insertion. Les résultats peuvent être retournés à l'application de traitement ou être insérés dans une table ou une variable de table pour un traitement ultérieur.  
  
 La clause OUTPUT n’est pas prise en charge dans les instructions DML qui font référence à des vues partitionnées locales, à des vues partitionnées distribuées, à des tables distantes ou à des instructions INSERT contenant un *execute_statement*. La clause OUTPUT INTO n’est pas prise en charge dans les instructions INSERT qui contiennent une clause \<dml_table_source>. Pour plus d’informations sur les arguments et le comportement de cette clause, consultez [Clause OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).
  
 VALUES  
 Présente la ou les listes de valeurs de données à insérer. Il doit y avoir une valeur de données pour chaque colonne de *column_list* (le cas échéant) ou de la table. La liste de valeurs doit être mise entre parenthèses.  
  
 Si les valeurs de la liste de valeurs ne sont pas dans le même ordre que les colonnes de la table ou n’ont pas de valeur pour chaque colonne de la table, *column_list* doit être utilisé afin de spécifier de manière explicite la colonne qui stocke chaque valeur entrante.  
  
 Vous pouvez utiliser le constructeur ROW [!INCLUDE[tsql](../../includes/tsql-md.md)] (également appelé constructeur de valeurs de table) pour spécifier plusieurs lignes dans une seule instruction INSERT. Le constructeur ROW est constitué d'une seule clause VALUES comportant plusieurs listes de valeurs placées entre parenthèses et séparées par une virgule. Pour plus d’informations, consultez [Constructeur de valeurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md).  
  
 DEFAULT  
 Force le [!INCLUDE[ssDE](../../includes/ssde-md.md)] à charger la valeur par défaut définie pour une colonne. S'il n'existe pas de valeur par défaut pour la colonne et si celle-ci autorise les valeurs NULL, NULL est inséré. Pour une colonne définie avec le type de données **timestamp**, la valeur d’horodateur suivante est insérée. DEFAULT n'est pas valide pour une colonne d'identité.  
  
 *expression*  
 Constante, variable ou expression. L'expression ne peut pas contenir d'instruction EXECUTE.  
  
 Quand vous faites référence aux types de données caractères Unicode **nchar**, **nvarchar** et **ntext**, '*expression*' doit être précédé de la majuscule « N ». Si vous ne spécifiez pas « N », [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit la chaîne dans la page de codes qui correspond au classement par défaut de la base de données ou de la colonne. Tous les caractères absents de cette page de codes sont alors perdus.  
  
 *derived_table*  
 Toute instruction SELECT valide qui retourne des lignes de données à charger dans la table. L'instruction SELECT ne peut pas contenir une expression de table commune.  
  
 *execute_statement*  
 Toute instruction EXECUTE valide qui retourne des données avec les instructions SELECT ou READTEXT. Pour plus d’informations, consultez [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Les options RESULT SETS de l’instruction EXECUTE ne peuvent pas être spécifiées dans une instruction INSERT...EXEC.  
  
 Si *execute_statement* est utilisé avec INSERT, chaque jeu de résultats doit être compatible avec les colonnes de la table ou de *column_list*.  
  
 *execute_statement* peut être utilisé pour exécuter des procédures stockées sur le même serveur ou sur un serveur distant. La procédure du serveur distant est exécutée et les jeux de résultats sont retournés au serveur local où ils sont chargés dans la table. Dans une transaction distribuée, *execute_statement* ne peut pas être émis sur un serveur lié en boucle quand MARS (Multiple Active Result Set) est activé pour la connexion.  
  
 Si *execute_statement* retourne des données avec l’instruction READTEXT, chaque instruction READTEXT peut retourner au maximum 1 Mo (1 024 Ko) de données. *execute_statement* peut également être utilisé avec des procédures étendues. *execute_statement* insère les données retournées par le thread principal de la procédure étendue ; en revanche, les sorties de threads autres que le thread principal ne sont pas insérées.  
  
 Vous ne pouvez pas spécifier de paramètre table en tant que cible d'une instruction INSERT EXEC ; néanmoins, vous pouvez le spécifier en tant que source de la chaîne ou procédure stockée INSERT EXEC. Pour plus d’informations, consultez [Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
 \<dml_table_source>  
 Spécifie que les lignes insérées dans la table cible sont les lignes retournées par la clause OUTPUT d'une instruction INSERT, UPDATE, DELETE ou MERGE, éventuellement filtrées par une clause WHERE. Si \<dml_table_source> est spécifié, la cible de l’instruction INSERT externe doit respecter les restrictions suivantes : 
  
-   La table doit être une table de base et non une vue.  
  
-   La table ne peut pas être une table distante.  
  
-   Aucun déclencheur ne peut être défini sur la table.  
  
-   Elle ne peut participer à aucune relation clé primaire-clé étrangère.  
  
-   Elle ne peut pas participer à la réplication de fusion ou à des abonnements pouvant être mis à jour pour la réplication transactionnelle.  
  
 Le niveau de compatibilité de la base de données doit être 100 ou plus. Pour plus d’informations, consultez [Clause OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
 \<select_list>  
 Liste séparée par des virgules qui spécifie les colonnes retournées par la clause OUTPUT qu'il convient d'insérer. Les colonnes dans \<select_list> doivent être compatibles avec les colonnes dans lesquelles les valeurs sont insérées. \<select_list> ne peut pas faire référence à des fonctions d’agrégation ni à TEXTPTR. 
  
> [!NOTE]  
>  Toutes les variables répertoriées dans la liste SELECT font référence à leurs valeurs d’origine, indépendamment des modifications qui leur ont été apportées dans \<dml_statement_with_output_clause>.  
  
 \<dml_statement_with_output_clause>  
 Instruction INSERT, UPDATE, DELETE ou MERGE valide qui retourne les lignes affectées dans une clause OUTPUT. L'instruction ne peut pas contenir de clause WITH et ne peut pas cibler les tables distantes ni les vues partitionnées. Si l'instruction UPDATE ou DELETE est spécifiée, elle ne peut pas être basée sur un curseur. Les lignes sources ne peuvent pas être référencées comme des instructions DML imbriquées.  
  
 WHERE \<search_condition>  
 Toute clause WHERE contenant une \<search_condition> valide qui filtre les lignes retournées par l’\<dml_statement_with_output_clause>. Pour plus d’informations, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md). Utilisée dans ce contexte, la \<search_condition> ne peut pas contenir de sous-requêtes, de fonctions scalaires définies par l’utilisateur qui accèdent aux données, de fonctions d’agrégation, TEXTPTR ni de prédicats de recherche en texte intégral. 
  
 DEFAULT VALUES  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Force la nouvelle ligne à prendre les valeurs par défaut définies pour chaque colonne.  
  
 BULK  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Utilisé par les outils externes pour télécharger un flux de données binaires. Cette option n’est pas destinée à être utilisée avec des outils tels que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], SQLCMD, OSQL ou des interfaces de programmation d’applications d’accès aux données telles que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 FIRE_TRIGGERS  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie que tous les déclencheurs d'insertion définis sur la table de destination seront exécutés au cours de l'opération de téléchargement de flux de données binaires. Pour plus d’informations, consultez [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 CHECK_CONSTRAINTS  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie que toutes les contraintes sur la table ou la vue cible doivent être vérifiées pendant l'opération de téléchargement de flux de données binaires. Pour plus d’informations, consultez [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 KEEPNULLS  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie que les colonnes vides doivent conserver une valeur NULL pendant l'opération de téléchargement de flux de données binaires. Pour plus d’informations, consultez [Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l’importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
 KILOBYTES_PER_BATCH = kilo-octets par lot  
 Indique le nombre approximatif de kilo-octets (Ko) de données par lot sous la forme *kilobytes_per_batch*. Pour plus d’informations, consultez [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 ROWS_PER_BATCH =*rows_per_batch*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Nombre approximatif de lignes de données que compte le flux de données binaires. Pour plus d’informations, consultez [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
> [!NOTE]
>  Une erreur de syntaxe est générée si aucune liste de colonnes n’est fournie.  

## <a name="remarks"></a>Notes  
Pour obtenir des informations spécifiques à l’insertion de données dans des tables graphiques SQL, consultez [INSERT (graphe SQL)](../../t-sql/statements/insert-sql-graph.md). 

## <a name="best-practices"></a>Bonnes pratiques  
 Utilisez la fonction @@ROWCOUNT pour retourner le nombre de lignes insérées dans l’application cliente. Pour plus d’informations, consultez [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).  
  
### <a name="best-practices-for-bulk-importing-data"></a>Recommandations pour l'importation de données en bloc  
  
#### <a name="using-insert-intoselect-to-bulk-import-data-with-minimal-logging-and-parallelism"></a>Utilisation d’INSERT INTO...SELECT pour importer des données en bloc avec une journalisation et un parallélisme minimaux 
Vous pouvez utiliser `INSERT INTO <target_table> SELECT <columns> FROM <source_table>` pour transférer efficacement un grand nombre de lignes d’une table, par exemple une table intermédiaire, vers une autre table avec une journalisation minimale. La journalisation minimale peut améliorer les performances de l'instruction et réduire le risque de voir l'opération remplir l'espace disponible du journal des transactions au cours de la transaction.  
  
La journalisation minimale pour cette instruction comporte les impératifs suivants :  
-   Le mode de récupération de la base de données doit correspondre au mode simple ou au mode de récupération utilisant les journaux de transactions.  
-   La table cible est un segment de mémoire vide ou non vide.  
-   La table cible n'est pas utilisée dans la réplication.  
-   L'indicateur `TABLOCK` est spécifié pour la table cible.  
  
Les lignes insérées dans un segment de mémoire à la suite d'une action d'insertion dans une instruction MERGE peuvent également être journalisées de façon minimale.  
  
Contrairement à l’instruction `BULK INSERT`, qui maintient un verrou de mise à jour en bloc (BU) moins restrictif, `INSERT INTO … SELECT` avec l’indicateur `TABLOCK` maintient un verrou exclusif (X) sur la table. Cela signifie que vous ne pouvez pas insérer de lignes à l’aide de plusieurs opérations Insert simultanément exécutées. 

Cependant, à partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et du niveau de compatibilité de base de données 130, une instruction `INSERT INTO … SELECT` unique peut être exécutée en parallèle lors de l’insertion dans des segments de mémoire ou des index columnstore en cluster (ICC). Les insertions parallèles sont possibles lors de l’utilisation de l’indicateur `TABLOCK`.  

Le parallélisme de l’instruction ci-dessus présente les exigences suivantes, qui sont similaires aux conditions requises pour la journalisation minimale :  
-   La table cible est un segment de mémoire vide ou non vide.  
-   La table cible a un index columnstore en cluster (ICC), mais aucun index non cluster.  
-   La table cible n’a pas de colonne d’identité avec IDENTITY_INSERT défini sur OFF.  
-   L'indicateur `TABLOCK` est spécifié pour la table cible.

Dans les scénarios où les exigences relatives à la journalisation minimale et à l’insertion en parallèle sont satisfaites, les deux améliorations travaillent ensemble pour garantir un débit maximal de vos opérations de chargement de données.

> [!NOTE]
> Les insertions dans les tables temporaires locales (identifiées par le préfixe #) et les tables temporaires globales (identifiées par les préfixes ##) sont également activées pour le parallélisme à l’aide de l’indicateur TABLOCK.
  
#### <a name="using-openrowset-and-bulk-to-bulk-import-data"></a>Utilisation d'OPENROWSET et de BULK pour les données d'importation en bloc  
 La fonction OPENROWSET peut accepter les indicateurs de table suivants, lesquels offrent des optimisations de chargement en masse avec l'instruction INSERT :  
  
-   L'indicateur `TABLOCK` peut réduire le nombre d'enregistrements de journal pour l'opération d'insertion. Le mode de récupération de la base de données doit être le mode simple ou le mode de récupération utilisant les journaux de transactions ; par ailleurs, la table cible ne peut pas être utilisée dans la réplication. Pour plus d’informations, consultez [Conditions requises pour une journalisation minimale dans l’importation en bloc](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
-   L’indicateur `TABLOCK` peut activer des opérations d’insertion parallèles. La table cible est un segment de mémoire ou un index columnstore en cluster (ICC) sans index non cluster, et la table cible ne peut pas avoir une colonne d’identité spécifiée.  
-   L'indicateur `IGNORE_CONSTRAINTS` peut désactiver temporairement la vérification des contraintes FOREIGN KEY et CHECK.  
-   L'indicateur `IGNORE_TRIGGERS` peut temporairement désactiver l'exécution des déclencheurs.  
-   L'indicateur `KEEPDEFAULTS` permet l'insertion la valeur par défaut éventuelle d'une colonne de table, à la place de la valeur NULL, lorsqu'il manque une valeur pour la colonne dans l'enregistrement de données.  
-   L'indicateur `KEEPIDENTITY` permet que les valeurs d'identité figurant dans le fichier de données importé soient utilisées pour la colonne d'identité dans la table cible.  
  
Ces optimisations sont similaires à celles disponibles avec la commande `BULK INSERT`. Pour plus d’informations, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
## <a name="data-types"></a>Types de données  
 Lorsque vous insérez des lignes, prenez en compte le comportement des types de données suivant :  
  
-   Si une valeur est chargée dans des colonnes ayant le type de données **char**, **varchar** ou **varbinary**, le remplissage ou la troncation des espaces blancs de fin (espaces pour **char** et **varchar**, zéros pour **varbinary**) est déterminé par le paramètre SET ANSI_PADDING défini pour la colonne lors de la création de la table. Pour plus d’informations, consultez [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
     Le tableau suivant illustre l'opération par défaut pour SET ANSI_PADDING OFF.  
  
    |Type de données|Opération par défaut|  
    |---------------|-----------------------|  
    |**char**|Remplit la valeur à l'aide d'espaces jusqu'à la largeur définie pour la colonne.|  
    |**varchar**|Supprime les espaces de fin jusqu’au dernier caractère différent d’un espace ou jusqu’au dernier caractère d’espacement simple pour les chaînes composées uniquement d’espaces.|  
    |**varbinary**|Supprime les zéros à droite.|  
  
-   Si une chaîne vide (' ') est chargée dans une colonne de type **varchar** ou **text**, l’opération par défaut consiste à charger une chaîne de longueur zéro.  
  
-   L’insertion d’une valeur Null dans une colonne de type **text** ou **image** ne crée pas un pointeur de texte valide et ne préalloue pas une page texte de 8 Ko.  
  
-   Les colonnes créées à l’aide du type de données **uniqueidentifier** stockent les valeurs binaires au format spécial 16 octets. Contrairement aux colonnes d’identité, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne génère pas automatiquement de valeurs pour les colonnes comportant le type de données **uniqueidentifier**. Lors d’une opération d’insertion, les variables dont le type de données est **uniqueidentifier** et les constantes de chaîne au format *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* (36 caractères, tirets inclus, où *x* correspond à un chiffre hexadécimal compris entre 0 et 9 ou a et f) peuvent être utilisées pour les colonnes **uniqueidentifier**. Par exemple, 6F9619FF-8B86-D011-B42D-00C04FC964FF est une valeur valide pour une colonne ou une variable **uniqueidentifier**. Utilisez la fonction [NEWID()](../../t-sql/functions/newid-transact-sql.md) pour obtenir un GUID (identificateur global unique).  
  
### <a name="inserting-values-into-user-defined-type-columns"></a>Insertion de valeurs dans des colonnes de type défini par l'utilisateur  
 Vous pouvez insérer des valeurs dans des colonnes de type défini par l'utilisateur en procédant comme suit :  
  
-   En fournissant une valeur du type défini par l'utilisateur.  
  
-   Fournir une valeur dans un type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à condition que le type défini par l'utilisateur prenne en charge la conversion de façon implicite ou explicite. L’exemple suivant montre comment insérer une valeur dans une colonne de type défini par l’utilisateur`Point`, en effectuant la conversion explicite à partir d’une chaîne.  
  
    ```sql
    INSERT INTO Cities (Location)  
    VALUES ( CONVERT(Point, '12.3:46.2') );  
    ```  
  
     Une valeur binaire peut également être fournie sans effectuer de conversion explicite, car tous les types définis par l'utilisateur sont convertis implicitement à partir d'une valeur binaire.  
  
-   En appelant une fonction définie par l'utilisateur qui retourne une valeur du type défini par l'utilisateur. L'exemple suivant utilise une fonction définie par l'utilisateur `CreateNewPoint()` pour créer une valeur de type défini par l'utilisateur `Point` et insérer la valeur dans la table `Cities`.  
  
    ```sql
    INSERT INTO Cities (Location)  
    VALUES ( dbo.CreateNewPoint(x, y) );  
    ```  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Vous pouvez implémenter la gestion des erreurs pour l’instruction INSERT en spécifiant cette dernière dans une construction TRY...CATCH.  
  
 Si une instruction INSERT enfreint une contrainte ou une règle, ou si elle comprend une valeur incompatible avec le type de données de la colonne, l'instruction échoue et un message d'erreur est retourné.  
  
 Si INSERT charge plusieurs lignes à l'aide de SELECT ou EXECUTE, toute violation de règle ou de contrainte à partir des valeurs chargées met fin à l'instruction et aucune ligne n'est chargée.  
  
 Lorsqu'une instruction INSERT rencontre une erreur arithmétique (dépassement de capacité, division par zéro ou erreur de domaine) lors de l'évaluation de l'expression, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] gère ces erreurs comme si SET ARITHABORT avait la valeur ON. Le lot est arrêté et un message d'erreur est retourné. Si, au cours de l'évaluation de l'expression, une instruction INSERT, DELETE ou UPDATE rencontre une erreur arithmétique, un dépassement de capacité, une division par zéro ou une erreur de domaine alors que les options SET ARITHABORT et SET ANSI_WARNINGS ont la valeur OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] insère ou met à jour une valeur NULL. Si la colonne cible ne peut pas prendre la valeur NULL, l'action d'insertion ou de mise à jour échoue et l'utilisateur reçoit une erreur.  
  
## <a name="interoperability"></a>Interopérabilité  
 Lorsqu'un déclencheur `INSTEAD OF` est défini sur des actions INSERT dans une table ou une vue, il est exécuté au lieu de l'instruction INSERT. Pour plus d’informations sur les déclencheurs `INSTEAD OF`, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Lorsque vous insérez des valeurs dans des tables distantes et que toutes les valeurs des colonnes ne sont pas spécifiées, vous devez identifier les colonnes dans lesquelles les valeurs spécifiées doivent être insérées.  
  
 Lorsque TOP est utilisé avec INSERT, les lignes référencées ne sont pas réorganisées dans un ordre particulier et la clause ORDER BY ne peut pas être spécifiée directement dans ces instructions. Si vous devez utiliser une clause TOP pour insérer des lignes dans un ordre chronologique significatif, vous devez associer à cette clause TOP une clause ORDER BY spécifiée dans une instruction de sous-sélection. Consultez la section Exemples plus loin dans cette rubrique.
 
Les requêtes INSERT qui utilisent SELECT avec ORDER BY pour remplir les lignes garantissent la façon dont les valeurs d’identité sont calculées, mais pas l’ordre d’insertion des lignes.

Dans Parallel Data Warehouse, la clause ORDER BY n'est pas valide dans VIEWS, CREATE TABLE AS SELECT, INSERT SELECT, les fonctions inline, les tables dérivées, les sous-requêtes et les expressions de table communes, sauf si TOP est également spécifié.
  
## <a name="logging-behavior"></a>Comportement de journalisation  
 L’instruction INSERT est toujours entièrement journalisée, sauf lors de l’utilisation de la fonction OPENROWSET avec le mot clé BULK ou lors de l’utilisation d’`INSERT INTO <target_table> SELECT <columns> FROM <source_table>`. Ces opérations peuvent faire l'objet d'une journalisation minimale. Pour plus d'informations, consultez la section « Recommandations pour le chargement en masse des données », précédemment dans cette rubrique.  
  
## <a name="security"></a>Sécurité  
 Au cours d'une connexion à un serveur lié, le serveur émetteur fournit un nom et un mot de passe de connexion afin de se connecter au serveur récepteur. Pour que cette connexion fonctionne, vous devez créer un mappage de connexion entre les serveurs liés en utilisant [sp_addlinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md).  
  
 Quand vous utilisez OPENROWSET(BULK…), il est important de bien comprendre comment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère l’emprunt d’identité. Pour plus d’informations, consultez « Considérations relatives à la sécurité » dans [Importer des données en bloc à l’aide de BULK INSERT ou OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).  
  
### <a name="permissions"></a>Autorisations  
 L'autorisation INSERT est obligatoire sur la table cible.  
  
 Les autorisations INSERT sont attribuées par défaut aux membres du rôle serveur fixe `sysadmin`, aux membres des rôles de base de données fixes `db_owner` et `db_datawriter` et au propriétaire de la table. Les membres des rôles `sysadmin`, `db_owner` et `db_securityadmin` ainsi que le propriétaire de la table peuvent transférer des autorisations à d’autres utilisateurs.  
  
 Pour exécuter INSERT avec l'option BULK de la fonction OPENROWSET, vous devez être membre du rôle serveur fixe `sysadmin` ou `bulkadmin`.  
  
##  <a name="examples"></a><a name="InsertExamples"></a> Exemples  
  
|Category|Éléments syntaxiques proposés|  
|--------------|------------------------------|  
|[Syntaxe de base](#BasicSyntax)|INSERT • constructeur de valeurs de table|  
|[Gestion de valeurs de colonnes](#ColumnValues)|IDENTITY • NEWID • valeurs par défaut • types définis par l'utilisateur|  
|[Insertion de données à partir d’autres tables](#OtherTables)|INSERT...SELECT • INSERT...EXECUTE • WITH expression de table commune • TOP • OFFSET FETCH|  
|[Spécification d’objets cibles autres que les tables standard](#TargetObjects)|Vues • variables de table|  
|[Insertion de lignes dans une table distante](#RemoteTables)|Serveur lié • fonction d'ensemble de lignes OPENQUERY • fonction d'ensemble de lignes OPENDATASOURCE|  
|[Chargement en masse de données à partir de tables ou de fichiers de données](#BulkLoad)|INSERT...SELECT • fonction OPENROWSET|  
|[Remplacement du comportement par défaut de l’optimiseur de requête à l’aide d’indicateurs](#TableHints)|Indicateurs de table|  
|[Capture des résultats de l’instruction INSERT](#CaptureResults)|Clause OUTPUT|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Syntaxe de base  
 Les exemples fournis dans cette section présentent les fonctionnalités de base de l'instruction INSERT en utilisant la syntaxe minimale requise.  
  
#### <a name="a-inserting-a-single-row-of-data"></a>R. Insertion d'une seule ligne de données  
 L'exemple suivant insère une ligne dans la table `Production.UnitMeasure` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Les colonnes de cette table sont `UnitMeasureCode`, `Name` et `ModifiedDate`. Étant donné que les valeurs de toutes les colonnes sont fournies et qu’elles sont répertoriées dans le même ordre que les colonnes de la table, il n’est pas nécessaire de spécifier les noms de colonnes dans la liste de colonnes *.*  
  
```sql
INSERT INTO Production.UnitMeasure  
VALUES (N'FT', N'Feet', '20080414');  
```  
  
#### <a name="b-inserting-multiple-rows-of-data"></a>B. Insertion de plusieurs lignes de données  
 L’exemple suivant utilise le [constructeur de valeurs de table](../../t-sql/queries/table-value-constructor-transact-sql.md) pour insérer trois lignes dans la table `Production.UnitMeasure` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] en une seule instruction INSERT. Étant donné que les valeurs de toutes les colonnes sont fournies et qu'elles sont répertoriées dans le même ordre que les colonnes de la table, il n'est pas nécessaire de spécifier les noms de colonnes dans la liste de colonnes.  
  
```sql
INSERT INTO Production.UnitMeasure  
VALUES (N'FT2', N'Square Feet ', '20080923'), (N'Y', N'Yards', '20080923')
    , (N'Y3', N'Cubic Yards', '20080923');  
```  
  
#### <a name="c-inserting-data-that-is-not-in-the-same-order-as-the-table-columns"></a>C. Insertion de données qui ne sont pas dans le même ordre que les colonnes de la table  
 L'exemple suivant utilise une liste de colonnes afin de spécifier de manière explicite les valeurs insérées dans chaque colonne. L’ordre des colonnes de la table `Production.UnitMeasure` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] est `UnitMeasureCode`, `Name`, `ModifiedDate` ; cependant, les colonnes ne sont pas répertoriées dans cet ordre dans *column_list*.  
  
```sql
INSERT INTO Production.UnitMeasure (Name, UnitMeasureCode,  
    ModifiedDate)  
VALUES (N'Square Yards', N'Y2', GETDATE());  
```  
  
###  <a name="handling-column-values"></a><a name="ColumnValues"></a> Gestion de valeurs de colonnes  
 Les exemples fournis dans cette section présentent des méthodes d’insertion de valeurs dans des colonnes qui sont définies avec une propriété IDENTITY ou avec une valeur DEFAULT, ou qui sont définies avec des types de données tels que **uniqueidentifer** ou des colonnes d’un type défini par l’utilisateur.  
  
#### <a name="d-inserting-data-into-a-table-with-columns-that-have-default-values"></a>D. Insertion de données dans une table dont les colonnes ont des valeurs par défaut  
 L'exemple suivant montre l'insertion de lignes dans une table dont les colonnes génèrent automatiquement une valeur ou possèdent une valeur par défaut. `Column_1` est une colonne calculée qui génère automatiquement une valeur en concaténant une chaîne avec la valeur insérée dans `column_2`. `Column_2` est définie avec une contrainte par défaut. Si aucune valeur n'est spécifiée pour cette colonne, la valeur par défaut est utilisée. `Column_3` est défini avec le type de données **rowversion**, qui génère automatiquement un nombre binaire incrémentiel unique. `Column_4` ne génère pas automatiquement de valeur. Lorsqu'aucune valeur n'est spécifiée pour cette colonne, NULL est inséré. Les instructions INSERT insèrent des lignes qui contiennent des valeurs pour certaines colonnes, mais pas pour toutes. Dans la dernière instruction INSERT, aucune colonne n'est spécifiée et seules les valeurs par défaut sont insérées à l'aide de la clause DEFAULT VALUES.  
  
```sql
CREATE TABLE dbo.T1   
(  
    column_1 AS 'Computed column ' + column_2,   
    column_2 varchar(30)   
        CONSTRAINT default_name DEFAULT ('my column default'),  
    column_3 rowversion,  
    column_4 varchar(40) NULL  
);  
GO  
INSERT INTO dbo.T1 (column_4)   
    VALUES ('Explicit value');  
INSERT INTO dbo.T1 (column_2, column_4)   
    VALUES ('Explicit value', 'Explicit value');  
INSERT INTO dbo.T1 (column_2)   
    VALUES ('Explicit value');  
INSERT INTO T1 DEFAULT VALUES;   
GO  
SELECT column_1, column_2, column_3, column_4  
FROM dbo.T1;  
GO  
```  
  
#### <a name="e-inserting-data-into-a-table-with-an-identity-column"></a>E. Insertion de données dans une table qui comprend une colonne d'identité  
 L'exemple suivant illustre différentes méthodes d'insertion de données dans une colonne d'identité. Les deux premières instructions INSERT permettent la génération de valeurs d'identité pour les nouvelles lignes. La troisième instruction INSERT substitue la propriété IDENTITY de la colonne à l'aide de l'instruction SET IDENTITY_INSERT et insère une valeur explicite dans la colonne d'identité.  
  
```sql
CREATE TABLE dbo.T1 ( column_1 int IDENTITY, column_2 VARCHAR(30));  
GO  
INSERT T1 VALUES ('Row #1');  
INSERT T1 (column_2) VALUES ('Row #2');  
GO  
SET IDENTITY_INSERT T1 ON;  
GO  
INSERT INTO T1 (column_1,column_2)   
    VALUES (-99, 'Explicit identity value');  
GO  
SELECT column_1, column_2  
FROM T1;  
GO  
```  
  
#### <a name="f-inserting-data-into-a-uniqueidentifier-column-by-using-newid"></a>F. Insertion de données dans une colonne uniqueidentifier en utilisant NEWID()  
 L’exemple suivant utilise la fonction [NEWID](../../t-sql/functions/newid-transact-sql.md)() pour obtenir un identificateur global unique (GUID) pour `column_2`. Contrairement à ce qui se passe pour les colonnes d’identité, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne génère pas automatiquement de valeurs pour les colonnes ayant le type de données [uniqueidentifier](../../t-sql/data-types/uniqueidentifier-transact-sql.md), comme l’indique la deuxième instruction `INSERT`.  
  
```sql
CREATE TABLE dbo.T1   
(  
    column_1 int IDENTITY,   
    column_2 uniqueidentifier,  
);  
GO  
INSERT INTO dbo.T1 (column_2)   
    VALUES (NEWID());  
INSERT INTO T1 DEFAULT VALUES;   
GO  
SELECT column_1, column_2  
FROM dbo.T1;  
```  
  
#### <a name="g-inserting-data-into-user-defined-type-columns"></a>G. Insertion de données dans des colonnes de type défini par l'utilisateur  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes insèrent trois lignes dans la colonne `PointValue` de table `Points`. Cette colonne utilise un [type CLR défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) (UDT). Le type de données `Point` comprend des valeurs entières X et Y qui sont exposées en tant que propriétés du type UDT. Vous devez utiliser la fonction CAST ou CONVERT pour effectuer une conversion de type (transtypage) des valeurs X et Y délimitées par des virgules en type `Point`. Les deux premières instructions utilisent la fonction CONVERT pour convertir une valeur de chaîne en type `Point`, et la troisième instruction utilise la fonction CAST. Pour plus d’informations, consultez [Manipulation de données UDT](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-manipulating-udt-data.md).  
  
```sql
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
###  <a name="inserting-data-from-other-tables"></a><a name="OtherTables"></a> Insertion de données à partir d’autres tables  
 Les exemples fournis dans cette section présentent des méthodes d'insertion de lignes d'une table dans une autre table.  
  
#### <a name="h-using-the-select-and-execute-options-to-insert-data-from-other-tables"></a>H. Utilisation des options SELECT et EXECUTE pour insérer des données provenant d'autres tables  
 L’exemple suivant montre comment insérer des données d’une table dans une autre table avec INSERT...SELECT ou INSERT...EXECUTE. Chaque méthode s'appuie sur une instruction SELECT basée sur plusieurs tables qui inclut une expression et une valeur littérale dans la liste des colonnes.  
  
 La première instruction INSERT utilise une instruction SELECT pour dériver les données des tables sources (`Employee`, `SalesPerson` et `Person`) dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] et stocker le jeu de résultats dans la table `EmployeeSales`. La deuxième instruction INSERT utilise la clause EXECUTE pour appeler une procédure stockée qui contient l'instruction SELECT, et la troisième instruction INSERT utilise la clause EXECUTE pour référencer l'instruction SELECT en tant que chaîne littérale.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( DataSource   varchar(20) NOT NULL,  
  BusinessEntityID   varchar(11) NOT NULL,  
  LastName     varchar(40) NOT NULL,  
  SalesDollars money NOT NULL  
);  
GO  
CREATE PROCEDURE dbo.uspGetEmployeeSales   
AS   
    SET NOCOUNT ON;  
    SELECT 'PROCEDURE', sp.BusinessEntityID, c.LastName,   
        sp.SalesYTD   
    FROM Sales.SalesPerson AS sp    
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY sp.BusinessEntityID, c.LastName;  
GO  
--INSERT...SELECT example  
INSERT INTO dbo.EmployeeSales  
    SELECT 'SELECT', sp.BusinessEntityID, c.LastName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY sp.BusinessEntityID, c.LastName;  
GO  
--INSERT...EXECUTE procedure example  
INSERT INTO dbo.EmployeeSales   
EXECUTE dbo.uspGetEmployeeSales;  
GO  
--INSERT...EXECUTE('string') example  
INSERT INTO dbo.EmployeeSales   
EXECUTE   
('  
SELECT ''EXEC STRING'', sp.BusinessEntityID, c.LastName,   
    sp.SalesYTD   
    FROM Sales.SalesPerson AS sp   
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE ''2%''  
    ORDER BY sp.BusinessEntityID, c.LastName  
');  
GO  
--Show results.  
SELECT DataSource,BusinessEntityID,LastName,SalesDollars  
FROM dbo.EmployeeSales;  
```  
  
#### <a name="i-using-with-common-table-expression-to-define-the-data-inserted"></a>I. Utilisation de l'expression de table commune WITH pour définir les données insérées  
 L'exemple suivant crée la table `NewEmployee` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Une expression de table commune (`EmployeeTemp`) définit les lignes d'une ou de plusieurs tables à insérer dans la table `NewEmployee`. L'instruction INSERT référence les colonnes dans l'expression de table commune.  
  
```sql
CREATE TABLE HumanResources.NewEmployee  
(  
    EmployeeID int NOT NULL,  
    LastName nvarchar(50) NOT NULL,  
    FirstName nvarchar(50) NOT NULL,  
    PhoneNumber Phone NULL,  
    AddressLine1 nvarchar(60) NOT NULL,  
    City nvarchar(30) NOT NULL,  
    State nchar(3) NOT NULL,   
    PostalCode nvarchar(15) NOT NULL,  
    CurrentFlag Flag  
);  
GO  
WITH EmployeeTemp (EmpID, LastName, FirstName, Phone,   
                   Address, City, StateProvince,   
                   PostalCode, CurrentFlag)  
AS (SELECT   
       e.BusinessEntityID, c.LastName, c.FirstName, pp.PhoneNumber,  
       a.AddressLine1, a.City, sp.StateProvinceCode,   
       a.PostalCode, e.CurrentFlag  
    FROM HumanResources.Employee e  
        INNER JOIN Person.BusinessEntityAddress AS bea  
        ON e.BusinessEntityID = bea.BusinessEntityID  
        INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
        INNER JOIN Person.PersonPhone AS pp  
        ON e.BusinessEntityID = pp.BusinessEntityID  
        INNER JOIN Person.StateProvince AS sp  
        ON a.StateProvinceID = sp.StateProvinceID  
        INNER JOIN Person.Person as c  
        ON e.BusinessEntityID = c.BusinessEntityID  
    )  
INSERT INTO HumanResources.NewEmployee   
    SELECT EmpID, LastName, FirstName, Phone,   
           Address, City, StateProvince, PostalCode, CurrentFlag  
    FROM EmployeeTemp;  
GO  
```  
  
#### <a name="j-using-top-to-limit-the-data-inserted-from-the-source-table"></a>J. Utilisation de TOP pour limiter les données insérées à partir de la table source  
 L'exemple suivant crée la table `EmployeeSales` et insère le nom et les données de ventes de l'année des 5 premiers employés aléatoires de la table `HumanResources.Employee` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. L'instruction INSERT choisit 5 lignes retournées par l'instruction `SELECT`. La clause OUTPUT affiche les lignes insérées dans la table `EmployeeSales`. Notez que la clause ORDER BY dans l'instruction SELECT n'est pas utilisée pour déterminer les 5 premiers employés.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   nvarchar(11) NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  YearlySales  money NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, 
        inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
```  
  
 Si vous devez utiliser une clause TOP pour insérer des lignes dans un ordre chronologique significatif, vous devez associer à cette clause TOP une clause ORDER BY dans une instruction de sous-sélection, comme illustré dans l'exemple suivant. La clause OUTPUT affiche les lignes insérées dans la table `EmployeeSales`. Notez que les 5 premiers employés sont maintenant insérés selon les résultats de la clause ORDER BY au lieu de lignes aléatoires.  
  
```sql
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, 
        inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
```  
  
###  <a name="specifying-target-objects-other-than-standard-tables"></a><a name="TargetObjects"></a> Spécification d’objets cibles autres que les tables standard  
 Les exemples présentés dans cette section montrent comment insérer des lignes en spécifiant une variable de table ou de vue.  
  
#### <a name="k-inserting-data-by-specifying-a-view"></a>K. Insertion de données en spécifiant une vue  
 L'exemple suivant spécifie un nom de vue comme objet cible ; cependant, la nouvelle ligne est insérée dans la table de base sous-jacente. L'ordre des valeurs dans l'instruction `INSERT` doit correspondre à l'ordre des colonnes de la vue. Pour plus d’informations, consultez [Modifier les données par l’intermédiaire d’une vue](../../relational-databases/views/modify-data-through-a-view.md).  
  
```sql
CREATE TABLE T1 ( column_1 int, column_2 varchar(30));  
GO  
CREATE VIEW V1 AS   
SELECT column_2, column_1   
FROM T1;  
GO  
INSERT INTO V1   
    VALUES ('Row 1',1);  
GO  
SELECT column_1, column_2   
FROM T1;  
GO  
SELECT column_1, column_2  
FROM V1;  
GO  
```  
  
#### <a name="l-inserting-data-into-a-table-variable"></a>L. Insertion de données dans une variable de table  
 L'exemple suivant spécifie une variable de table comme objet cible dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
-- Create the table variable.  
DECLARE @MyTableVar table(  
    LocationID int NOT NULL,  
    CostRate smallmoney NOT NULL,  
    NewCostRate AS CostRate * 1.5,  
    ModifiedDate datetime);  
  
-- Insert values into the table variable.  
INSERT INTO @MyTableVar (LocationID, CostRate, ModifiedDate)  
    SELECT LocationID, CostRate, GETDATE() 
    FROM Production.Location  
    WHERE CostRate > 0;  
  
-- View the table variable result set.  
SELECT * FROM @MyTableVar;  
GO  
```  
  
###  <a name="inserting-rows-into-a-remote-table"></a><a name="RemoteTables"></a> Insertion de lignes dans une table distante  
 Les exemples présentés dans cette section montrent comment insérer des lignes dans une table cible distante en utilisant un [serveur lié](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) ou une [fonction d’ensemble de lignes](../functions/opendatasource-transact-sql.md) pour référencer la table distante.  
  
#### <a name="m-inserting-data-into-a-remote-table-by-using-a-linked-server"></a>M. Insertion de données dans une table distante en utilisant un serveur lié  
 L'exemple suivant insère des lignes dans la table distante. L’exemple commence par créer un lien vers la source de données distante en utilisant [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Le nom du serveur lié, `MyLinkServer`, est ensuite spécifié comme partie du nom d’objet en quatre parties qui se présente sous la forme *serveur.catalogue.schéma.objet*.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_nameinstance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
INSERT INTO MyLinkServer.AdventureWorks2012.HumanResources.Department (Name, GroupName)  
VALUES (N'Public Relations', N'Executive General and Administration');  
GO  
```  
  
#### <a name="n-inserting-data-into-a-remote-table-by-using-the-openquery-function"></a>N. Insertion de données dans une table distante en utilisant la fonction OPENQUERY  
 L’exemple suivant insère une ligne dans une table distante en spécifiant la fonction d’ensemble de lignes [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md). Le nom de serveur lié créé dans l'exemple précédent est utilisé dans cet exemple.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql
INSERT OPENQUERY (MyLinkServer, 
    'SELECT Name, GroupName 
     FROM AdventureWorks2012.HumanResources.Department')  
VALUES ('Environmental Impact', 'Engineering');  
GO  
```  
  
#### <a name="o-inserting-data-into-a-remote-table-by-using-the-opendatasource-function"></a>O. Insertion de données dans une table distante en utilisant la fonction OPENDATASOURCE  
 L’exemple suivant insère une ligne dans une table distante en spécifiant la fonction d’ensemble de lignes [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md). Spécifiez un nom de serveur valide pour la source de données en utilisant le format *server_name* ou *server_name\instance_name*.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_nameinstance_name.  
  
INSERT INTO OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department (Name, GroupName)  
    VALUES (N'Standards and Methods', 'Quality Assurance');  
GO  
```  
  
#### <a name="p-inserting-into-an-external-table-created-using-polybase"></a>P. Insertion dans une table externe créée à l’aide de PolyBase  
 Exportez des données de SQL Server vers Hadoop ou le Stockage Azure. Commencez par créer une table externe pointant vers le fichier ou le répertoire de destination. Puis, utilisez INSERT INTO pour exporter les données d’une table SQL Server locale dans une source de données externe. l’instruction INSERT INTO crée le fichier ou le répertoire de destination s’il n’existe pas, et les résultats de l’instruction SELECT sont exportés à l’emplacement et au format spécifiés.  Pour plus d’informations, consultez [Prise en main de PolyBase](../../relational-databases/polybase/polybase-guide.md).  
  
**S'applique à**: [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```sql
-- Create an external table.   
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
        [FirstName] char(25) NOT NULL,   
        [LastName] char(25) NOT NULL,   
        [YearlyIncome] float NULL,   
        [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
        LOCATION='/old_data/2009/customerdata.tbl',  
        DATA_SOURCE = HadoopHDP2,  
        FILE_FORMAT = TextFileFormat,  
        REJECT_TYPE = VALUE,  
        REJECT_VALUE = 0  
);  
  
-- Export data: Move old data to Hadoop while keeping 
-- it query-able via external table.  

INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  
  
###  <a name="bulk-loading-data-from-tables-or-data-files"></a><a name="BulkLoad"></a> Chargement en masse de données à partir de tables ou de fichiers de données  
 Les exemples fournis dans cette section présentent deux méthodes permettant de charger en masse des données dans une table en utilisant l'instruction INSERT.  
  
#### <a name="q-inserting-data-into-a-heap-with-minimal-logging"></a>Q. Insertion de données dans un segment de mémoire avec une journalisation minimale  
 L'exemple suivant crée une nouvelle table (un segment de mémoire) et y insère des données provenant d'une autre table en utilisant une journalisation minimale. L'exemple suppose que le mode de récupération de la base de données `AdventureWorks2012` a la valeur FULL. Pour obtenir une journalisation minimale, le mode de récupération de la base de données `AdventureWorks2012` est défini sur BULK_LOGGED avant l’insertion des lignes ; il reprend ensuite la valeur FULL après l’instruction INSERT INTO...SELECT. En outre, l'indicateur TABLOCK est spécifié pour la table cible `Sales.SalesHistory`. Cela permet de garantir que l'instruction utilise un espace minimal dans le journal des transactions et qu'elle s'exécute de manière efficace.  
  
```sql
-- Create the target heap.  
CREATE TABLE Sales.SalesHistory(  
    SalesOrderID int NOT NULL,  
    SalesOrderDetailID int NOT NULL,  
    CarrierTrackingNumber nvarchar(25) NULL,  
    OrderQty smallint NOT NULL,  
    ProductID int NOT NULL,  
    SpecialOfferID int NOT NULL,  
    UnitPrice money NOT NULL,  
    UnitPriceDiscount money NOT NULL,  
    LineTotal money NOT NULL,  
    rowguid uniqueidentifier ROWGUIDCOL  NOT NULL,  
    ModifiedDate datetime NOT NULL );  
GO  
-- Temporarily set the recovery model to BULK_LOGGED.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY BULK_LOGGED;  
GO  
-- Transfer data from Sales.SalesOrderDetail to Sales.SalesHistory  
INSERT INTO Sales.SalesHistory WITH (TABLOCK)  
    (SalesOrderID,   
     SalesOrderDetailID,  
     CarrierTrackingNumber,   
     OrderQty,   
     ProductID,   
     SpecialOfferID,   
     UnitPrice,   
     UnitPriceDiscount,  
     LineTotal,   
     rowguid,   
     ModifiedDate)  
SELECT * FROM Sales.SalesOrderDetail;  
GO  
-- Reset the recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
#### <a name="r-using-the-openrowset-function-with-bulk-to-bulk-load-data-into-a-table"></a>R. Utilisation de la fonction OPENROWSET avec BULK pour charger en masse des données dans une table  
 L'exemple suivant insère des lignes d'un fichier de données dans une table en spécifiant la fonction OPENROWSET. L'indicateur de table IGNORE_TRIGGERS est spécifié en vue d'optimiser les performances. Pour obtenir d’autres exemples, consultez [Importer des données en bloc à l’aide de BULK INSERT ou OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql
INSERT INTO HumanResources.Department WITH (IGNORE_TRIGGERS) (Name, GroupName)  
SELECT b.Name, b.GroupName   
FROM OPENROWSET (  
    BULK 'C:SQLFilesDepartmentData.txt',  
    FORMATFILE = 'C:SQLFilesBulkloadFormatFile.xml',  
    ROWS_PER_BATCH = 15000)AS b ;  
```  
  
###  <a name="overriding-the-default-behavior-of-the-query-optimizer-by-using-hints"></a><a name="TableHints"></a> Remplacement du comportement par défaut de l’optimiseur de requête à l’aide d’indicateurs  
 Les exemples présentés dans cette section montrent comment utiliser des [indicateurs de table](../../t-sql/queries/hints-transact-sql-table.md) pour remplacer temporairement le comportement par défaut de l’optimiseur de requête lors du traitement de l’instruction INSERT.  
  
> [!CAUTION]  
>  Étant donné que l'optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionne généralement le meilleur plan d'exécution pour une requête, nous vous recommandons de ne recourir à ces conseils qu'en dernier ressort et seulement si vous êtes un développeur ou un administrateur de base de données expérimenté.  
  
#### <a name="s-using-the-tablock-hint-to-specify-a-locking-method"></a>S. Utilisation de l'indicateur TABLOCK pour spécifier une méthode de verrouillage  
 L'exemple suivant spécifie qu'un verrou exclusif (X) est appliqué sur la table Production.Location et est maintenu jusqu'à la fin de l'instruction INSERT.  
  
**S’applique à** : [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].  
  
```sql
INSERT INTO Production.Location WITH (XLOCK)  
(Name, CostRate, Availability)  
VALUES ( N'Final Inventory', 15.00, 80.00);  
```  
  
###  <a name="capturing-the-results-of-the-insert-statement"></a><a name="CaptureResults"></a> Capture des résultats de l’instruction INSERT  
 Les exemples présentés dans cette section montrent comment utiliser la [clause OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) pour retourner des informations à partir de chaque ligne affectée par une instruction INSERT, ou des expressions basées sur ces lignes. Ces résultats peuvent être retournés à l'application en cours de traitement afin d'être utilisés notamment avec des messages de confirmation, des opérations d'archivage et d'autres spécifications d'application similaires.  
  
#### <a name="t-using-output-with-an-insert-statement"></a>T. Utilisation de OUTPUT avec une instruction INSERT  
 L'exemple suivant insère une ligne dans la table `ScrapReason` et utilise la clause `OUTPUT` pour retourner les résultats de l'instruction à la variable de table `@MyTableVar`. Étant donné que la colonne `ScrapReasonID` est définie avec une propriété `IDENTITY`, aucune valeur n'est spécifiée dans l'instruction `INSERT` pour cette colonne. Cependant, notez que la valeur générée par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour cette colonne est retournée dans la clause `OUTPUT` de la colonne `INSERTED.ScrapReasonID`.  
  
```sql
DECLARE @MyTableVar table( NewScrapReasonID smallint,  
                           Name varchar(50),  
                           ModifiedDate datetime);  
INSERT Production.ScrapReason  
    OUTPUT INSERTED.ScrapReasonID, INSERTED.Name, INSERTED.ModifiedDate  
        INTO @MyTableVar  
VALUES (N'Operator error', GETDATE());  
  
--Display the result set of the table variable.  
SELECT NewScrapReasonID, Name, ModifiedDate FROM @MyTableVar;  
--Display the result set of the table.  
SELECT ScrapReasonID, Name, ModifiedDate   
FROM Production.ScrapReason;  
```  
  
#### <a name="u-using-output-with-identity-and-computed-columns"></a>U. Utilisation de OUTPUT avec des colonnes d'identité et des colonnes calculées  
 L'exemple suivant crée la table `EmployeeSales`, puis y insère plusieurs lignes à l'aide d'une instruction INSERT, avec une instruction SELECT pour récupérer les données des tables sources. La table `EmployeeSales` contient une colonne d'identité (`EmployeeID`) et une colonne calculée (`ProjectedSales`). Étant donné que ces valeurs sont générées par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] lors de l'insertion, aucune de ces colonnes ne peut être définie dans `@MyTableVar`.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   int IDENTITY (1,5)NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  CurrentSales money NOT NULL,  
  ProjectedSales AS CurrentSales * 1.10   
);  
GO  
DECLARE @MyTableVar table(  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  CurrentSales money NOT NULL  
  );  
  
INSERT INTO dbo.EmployeeSales (LastName, FirstName, CurrentSales)  
  OUTPUT INSERTED.LastName,   
         INSERTED.FirstName,   
         INSERTED.CurrentSales  
  INTO @MyTableVar  
    SELECT c.LastName, c.FirstName, sp.SalesYTD  
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY c.LastName, c.FirstName;  
  
SELECT LastName, FirstName, CurrentSales  
FROM @MyTableVar;  
GO  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM dbo.EmployeeSales;  
```  
  
#### <a name="v-inserting-data-returned-from-an-output-clause"></a>V. Insertion de données retournées à partir d'une clause OUTPUT  
 L'exemple suivant capture les données retournées par la clause OUTPUT d'une instruction MERGE et insère ces données dans une autre table. L’instruction MERGE met quotidiennement à jour la colonne `Quantity` dans la table `ProductInventory`, selon les commandes traitées dans la table `SalesOrderDetail` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Elle supprime également les lignes correspondant aux produits dont le stock passe à 0. Cet exemple capture les lignes supprimées et les insère dans une autre table, `ZeroInventory`, qui effectue le suivi des produits en rupture de stock.  
  
```sql
--Create ZeroInventory table.  
CREATE TABLE Production.ZeroInventory (DeletedProductID int, RemovedOnDate DateTime);  
GO  
  
INSERT INTO Production.ZeroInventory (DeletedProductID, RemovedOnDate)  
SELECT ProductID, GETDATE()  
FROM  
(   MERGE Production.ProductInventory AS pi  
    USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
           JOIN Sales.SalesOrderHeader AS soh  
           ON sod.SalesOrderID = soh.SalesOrderID  
           AND soh.OrderDate = '20070401'  
           GROUP BY ProductID) AS src (ProductID, OrderQty)  
    ON (pi.ProductID = src.ProductID)  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0  
        THEN DELETE  
    WHEN MATCHED  
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    OUTPUT $action, deleted.ProductID) AS Changes (Action, ProductID)  
WHERE Action = 'DELETE';  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were inserted';  
GO  
SELECT DeletedProductID, RemovedOnDate FROM Production.ZeroInventory;  
```  

#### <a name="w-inserting-data-using-the-select-option"></a>Heure standard Insertion de données à l’aide de l’option SELECT  
 L’exemple suivant montre comment insérer plusieurs lignes de données à l’aide d’une instruction INSERT avec une option SELECT. La première instruction `INSERT` utilise une instruction `SELECT` directement pour récupérer des données à partir de la table source, puis stocke le jeu de résultats dans la table `EmployeeTitles`.  
  
```sql
CREATE TABLE EmployeeTitles  
( EmployeeKey   INT NOT NULL,  
  LastName     varchar(40) NOT NULL,  
  Title      varchar(50) NOT NULL  
);  
INSERT INTO EmployeeTitles  
    SELECT EmployeeKey, LastName, Title   
    FROM ssawPDW.dbo.DimEmployee  
    WHERE EndDate IS NULL;  
```  
  
#### <a name="x-specifying-a-label-with-the-insert-statement"></a>X. Spécification d’une étiquette avec l’instruction INSERT  
 L’exemple suivant illustre l’utilisation d’une étiquette avec une instruction INSERT.  
  
```sql
-- Uses AdventureWorks  
  
INSERT INTO DimCurrency   
VALUES (500, N'C1', N'Currency1')  
OPTION ( LABEL = N'label1' );  
```  
  
#### <a name="y-using-a-label-and-a-query-hint-with-the-insert-statement"></a>Y. Utilisation d’une étiquette et d’un indicateur de requête avec l’instruction INSERT  
 Cette requête présente la syntaxe de base pour utiliser une étiquette et un indicateur de jointure de requête avec l’instruction INSERT. Une fois la requête soumise au nœud de contrôle, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en cours d’exécution sur les nœuds de calcul, applique la stratégie de jointure hachée quand il génère le plan de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur les indicateurs de jointure et sur l’utilisation de la clause OPTION, consultez [OPTION (SQL Server PDW)](../../t-sql/queries/option-clause-transact-sql.md).  
  
```sql
-- Uses AdventureWorks  
  
INSERT INTO DimCustomer (CustomerKey, CustomerAlternateKey, 
    FirstName, MiddleName, LastName )   
SELECT ProspectiveBuyerKey, ProspectAlternateKey, 
    FirstName, MiddleName, LastName  
FROM ProspectiveBuyer p JOIN DimGeography g ON p.PostalCode = g.PostalCode  
WHERE g.CountryRegionCode = 'FR'  
OPTION ( LABEL = 'Add French Prospects', HASH JOIN);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [IDENTITY, propriété &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [OUTPUT, clause &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)   
 [Utiliser les tables inserted et deleted](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md)  
  
