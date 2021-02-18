---
description: UPDATE (Transact-SQL)
title: UPDATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATE_TSQL
- UPDATE
dev_langs:
- TSQL
helpviewer_keywords:
- DML [SQL Server], UPDATE statement
- data updates [SQL Server], UPDATE statement
- updating data [SQL Server]
- TOP clause, UPDATE
- OUTPUT clause
- large value data types
- data manipulation language [SQL Server], UPDATE statement
- user-defined types [SQL Server], modifying
- INSTEAD OF triggers
- modifying data [SQL Server], UPDATE statement
- data modifications [SQL Server], UPDATE statement
- large data, modifying
- .WRITE clause
- table modifications [SQL Server], UPDATE statement
- UDTs [SQL Server], modifying
- UPDATE statement [SQL Server], about UPDATE statement
- LOB data [SQL Server], modifying
- modifying LOB data
- UPDATE statement [SQL Server]
- FROM clause, UPDATE statement
- WHERE clause, UPDATE statement
ms.assetid: 40e63302-0c68-4593-af3e-6d190181fee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 82a4adb98429184e316d63df5e94ae077b137f86
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100353599"
---
# <a name="update-transact-sql"></a>UPDATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifie les données existantes d'une table ou d'une vue dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Pour obtenir des exemples, consultez [Exemples](#UpdateExamples).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  

[ WITH <common_table_expression> [...n] ]  
UPDATE   
    [ TOP ( expression ) [ PERCENT ] ]   
    { { table_alias | <object> | rowset_function_limited   
         [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
      }  
      | @table_variable      
    }  
    SET  
        { column_name = { expression | DEFAULT | NULL }  
          | { udt_column_name.{ { property_name = expression  
                                | field_name = expression }  
                                | method_name ( argument [ ,...n ] )  
                              }  
          }  
          | column_name { .WRITE ( expression , @Offset , @Length ) }  
          | @variable = expression  
          | @variable = column = expression  
          | column_name { += | -= | *= | /= | %= | &= | ^= | |= } expression  
          | @variable { += | -= | *= | /= | %= | &= | ^= | |= } expression  
          | @variable = column { += | -= | *= | /= | %= | &= | ^= | |= } expression  
        } [ ,...n ]   
  
    [ <OUTPUT Clause> ]  
    [ FROM{ <table_source> } [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                  { { [ GLOBAL ] cursor_name }   
                      | cursor_variable_name   
                  }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]  
[ ; ]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
    | database_name .[ schema_name ] .   
    | schema_name .  
    ]  
    table_or_view_name}  
```  
  
```syntaxsql 
-- Syntax for Azure Synapse Analysis

[ WITH <common_table_expression> [ ,...n ] ]
UPDATE [ database_name . [ schema_name ] . | schema_name . ] table_name
SET { column_name = { expression | NULL } } [ ,...n ]  
FROM [ database_name . [ schema_name ] . | schema_name . ] table_name   
JOIN {<join_table_source>}[ ,...n ] 
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( LABEL = label_name ) ]  
[;]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

UPDATE [ database_name . [ schema_name ] . | schema_name . ] table_name   
SET { column_name = { expression | NULL } } [ ,...n ]  
[ FROM from_clause ]  
[ WHERE <search_condition> ]   
[ OPTION ( LABEL = label_name ) ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 WITH \<common_table_expression>  
 Spécifie l'ensemble de résultats ou la vue nommés temporaires, également appelés expression de table commune (CTE) et définis dans le cadre de l'instruction UPDATE. Le jeu de résultats CTE est dérivé d'une simple requête et l'instruction UPDATE y fait référence.  
  
 Vous pouvez également utiliser des expressions de table courantes avec les instructions SELECT, INSERT, DELETE et CREATE VIEW. Pour plus d’informations, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP **(** _expression_ **)** [ PERCENT ]  
 Spécifie le nombre ou le pourcentage de lignes à mettre à jour. L'argument *expression* peut être un nombre ou un pourcentage de lignes.  
  
 Les lignes référencées dans l'expression TOP utilisée dans les instructions INSERT, UPDATE ou DELETE ne sont pas triées dans un ordre précis.  
  
 Les parenthèses autour de l’argument *expression* dans TOP sont obligatoires dans les instructions INSERT, UPDATE et DELETE. Pour plus d’informations, consultez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 *table_alias*  
 Alias spécifié dans la clause FROM représentant la table ou la vue à partir de laquelle les lignes doivent être mises à jour.  
  
 *server_name*  
 Nom du serveur (avec un nom de serveur lié ou la fonction [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) comme nom de serveur) contenant la table ou la vue. Si *server_name* est spécifié, *database_name* et *schema_name* sont obligatoires.  
  
 *database_name*  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel la table ou la vue appartient.  
  
 *table_or_view_name*  
 Nom de la table ou de la vue à partir de laquelle les lignes doivent être mises à jour. La vue référencée par *table_or_view_name* doit pouvoir être mise à jour et faire référence à une seule table de base dans la clause FROM de la vue. Pour plus d’informations sur les vues pouvant être mises à jour, consultez [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 Fonction [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) ou [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) , selon les possibilités du fournisseur.  
  
 WITH **(** \<Table_Hint_Limited> **)**  
 Spécifie un ou plusieurs indicateurs de table autorisés pour une table cible. Le mot clé WITH et les parenthèses sont obligatoires. NOLOCK, READUNCOMMITTED et NOEXPAND ne sont pas autorisés. Pour plus d’informations sur les indicateurs de table, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 @*table_variable*  
 Spécifie une variable [table](../../t-sql/data-types/table-transact-sql.md) comme source de table.  
  
 SET  
 Spécifie la liste des noms des colonnes ou des variables à mettre à jour.  
  
 *column_name*  
 Colonne qui contient les données à modifier. *column_name* doit être spécifié dans *table_or view_name*. Il est impossible de mettre à jour les colonnes d'identité.  
  
 *expression*  
 Variable, valeur littérale, expression ou sous-instruction SELECT (entre parenthèses) retournant une valeur unique. La valeur retournée par *expression* remplace la valeur existante dans *column_name* ou @*variable*.  
  
> [!NOTE]  
>  Quand vous faites référence aux types de données caractères Unicode **nchar**, **nvarchar** et **ntext**, 'expression' doit être précédé de la majuscule « N ». Si vous ne spécifiez pas « N », [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit la chaîne dans la page de codes qui correspond au classement par défaut de la base de données ou de la colonne. Tous les caractères absents de cette page de codes sont alors perdus.  
  
 DEFAULT  
 Spécifie que la valeur par défaut définie pour la colonne doit remplacer la valeur actuelle de la colonne. Cet argument peut également être utilisé pour attribuer la valeur NULL à la colonne si celle-ci n'a pas de valeur par défaut et autorise les valeurs NULL.  
  
 { **+=** \| **-=** \| **\*=** \| **/=** \| **%=** \| **&=** \| **^=** \| **|=** }  
 Opérateur d'assignation composé :  
 +=                       Ajouter et assigner  
 -=                        Soustraire et assigner  
 *=                        Multiplier et assigner  
 /=                         Diviser et assigner  
 %=                       Modulo et assigner  
 &=                        AND au niveau du bit et assigner  
 ^=                        XOR au niveau du bit et assigner  
 |=                         OR au niveau du bit et assigner  
  
 *udt_column_name*  
 Colonne définie par l'utilisateur.  
  
 *property_name* | *field_name*  
 Propriété publique ou membre de données public d'un type défini par l'utilisateur.  
  
 *method_name* **(** *argument* [ **,** ... *n*] **)**  
 Méthode de mutateur public non statique de *udt_column_name* qui prend au moins un argument.  
  
 **.** WRITE **(** _expression_ **,** @_Offset_ **,** @_Length_ **)**  
 Indique qu’une section de la valeur de *column_name* doit être modifiée. *expression* remplace les unités @*Length* à partir de @*Offset* de *column_name*. Seules des colonnes de type **varchar(max)** , **nvarchar(max)** ou **varbinary(max)** peuvent être spécifiées avec cette clause. *column_name* ne peut pas être NULL, ni être qualifié avec un nom ou un alias de table.  
  
 *expression* est la valeur qui est copiée dans *column_name*. *expression* doit être du même type que *column_name* ou être implicitement casté dans ce type. Si *expression* a la valeur NULL, @*Length* est ignoré, et la valeur dans *column_name* est tronquée à la position @*Offset* spécifiée.  
  
 @*Offset* est le point de départ dans la valeur stockée dans *column_name* où *expression* est inséré. @*Offset* est une position d’octet ordinale calculée à partir de zéro, de type **bigint** et ne peut pas être un nombre négatif. Si @*Offset* est NULL, la mise à jour ajoute *expression* à la fin de la valeur de *column_name* existante, et @*Length* est ignoré. Si @*Offset* est supérieur à la longueur d’octet de la valeur *column_name*, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] retourne une erreur. Si @*Offset* plus @*Length* dépasse la fin de la valeur sous-jacente dans la colonne, toutes les données sont supprimées jusqu’au dernier caractère de la valeur.  
  
 @*Length* est la longueur de la section dans la colonne, à partir de @*Offset*, qui est remplacée par *expression*. @*Length* est de type **bigint** et ne peut pas être un nombre négatif. Si @*Length* est NULL, la mise à jour supprime toutes les données entre @*Offset* et la fin de la valeur de *column_name*.  
  
 Pour plus d’informations, consultez la section [Mise à jour des données de valeurs élevées](#updating-lobs).  
  
 **@** *variable*  
 Variable déclarée qui est définie à la valeur retournée par *expression*.  
  
 SET **@** _variable_ = *column* = *expression* affecte à la variable la même valeur que la colonne. Ceci est différent de SET **@** _variable_ = _column_, _column_ = _expression_, qui affecte à la variable la valeur de la colonne avant la mise à jour.  
  
 \<OUTPUT_Clause>  
 Retourne des données mises à jour ou des expressions associées dans le cadre de l'opération UPDATE. La clause OUTPUT n'est pas prise en charge dans les instructions DML, qui ciblent les tables ou les vues distantes. Pour plus d’informations sur les arguments et le comportement de cette clause, consultez [Clause OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
 FROM \<table_source>  
 Spécifie qu'une table, une vue ou une source de table dérivée sont utilisées pour fournir les valeurs destinées à servir de critères en vue de la mise à jour. Pour plus d’informations, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
 Si l'objet mis à jour est le même que l'objet de la clause FROM et s'il n'existe qu'une seule référence à cet objet de la clause FROM, un alias d'objet pourra être spécifié ou non. Si l'objet mis à jour apparaît plusieurs fois dans la clause FROM, l'une des références, mais une seule, à cet objet ne doit pas spécifier un alias de la table. Toutes les autres références à l'objet dans la clause FROM doivent inclure un alias d'objet.  
  
 Une vue avec un déclencheur INSTEAD OF UPDATE ne peut pas servir de cible à une instruction UPDATE avec une clause FROM.  
  
> [!NOTE]  
>  Tout appel à OPENDATASOURCE, OPENQUERY ou OPENROWSET dans la clause FROM est évalué séparément et indépendamment de tout appel à ces fonctions utilisé comme cible de la mise à jour, même si des arguments identiques sont fournis aux deux appels. En particulier, les conditions de filtre ou de jointure appliquées sur le résultat de l'un de ces appels n'ont aucun effet sur les résultats de l'autre.  
  
 WHERE  
 Spécifie les conditions de limite des lignes mises à jour. Il existe deux formes de mise à jour en fonction du contenu de la clause WHERE :  
  
-   Les mises à jour avec recherche comportent une condition de recherche pour qualifier les lignes à supprimer.  
  
-   Les mises à jour avec positions utilisent la clause CURRENT OF pour définir un curseur. La mise à jour se produit à l'emplacement actuel du curseur.  
  
\<search_condition>  
 Spécifie la condition à remplir pour mettre à jour les lignes. La condition de recherche peut également être la condition sur laquelle est basée une jointure. Le nombre de prédicats inclus dans une condition de recherche est illimité. Pour plus d’informations sur les prédicats et conditions de recherche, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
CURRENT OF  
 Spécifie que la mise à jour s'effectue à l'emplacement actuel du curseur spécifié.  
  
 Une mise à jour positionnée utilisant une clause WHERE CURRENT OF met à jour uniquement la ligne sur laquelle est positionné le curseur. Cette opération peut s’avérer plus précise qu’une mise à jour avec recherche utilisant une clause WHERE \<search_condition> pour définir les lignes à mettre à jour. Une mise à jour avec recherche modifie plusieurs lignes dès lors que la condition de recherche n'identifie pas de manière unique une seule ligne.  
  
GLOBAL  
 Indique que *cursor_name* fait référence à un curseur global.  
  
*cursor_name*  
 Nom du curseur ouvert grâce auquel s'effectue l'extraction. Si un curseur global et un curseur local portent tous les deux le nom *cursor_name*, cet argument fait référence au curseur global si GLOBAL est spécifié, et au curseur local dans tous les autres cas. Le curseur doit pouvoir gérer les mises à jour.  
  
*cursor_variable_name*  
 Nom d’une variable de curseur. *cursor_variable_name* doit référencer un curseur qui autorise les mises à jour.  
  
OPTION **(** \<query_hint> [ **,** ... *n* ] **)**  
 Spécifie que des indicateurs d’optimiseur sont utilisés pour personnaliser la façon dont le [!INCLUDE[ssDE](../../includes/ssde-md.md)] traite l’instruction. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="best-practices"></a>Meilleures pratiques  
 Utilisez la fonction `@@ROWCOUNT` pour retourner le nombre de lignes insérées dans l’application cliente. Pour plus d’informations, consultez [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).  
  
 Vous pouvez utiliser des noms de variables dans les instructions UPDATE pour présenter les anciennes et les nouvelles valeurs affectées, mais ceci n'est applicable que si l'instruction UPDATE n'affecte qu'un seul enregistrement. Si l’instruction UPDATE affecte plusieurs enregistrements, utilisez la [clause OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) pour retourner les anciennes et nouvelles valeurs de chaque enregistrement.  
  
 Soyez vigilant lors de la spécification de la clause FROM pour fournir les valeurs destinées à servir de critères en vue de la mise à jour. Les résultats d'une instruction UPDATE ne sont pas définis si celle-ci comprend une clause FROM qui ne spécifie pas qu'une seule valeur doit être disponible pour chaque occurrence de colonne mise à jour ; à savoir, si l'instruction UPDATE n'est pas déterministe. Par exemple, étant donné l'instruction UPDATE dans le script suivant, les deux lignes dans `Table1` correspondent aux qualifications de la clause FROM dans l'instruction UPDATE, mais il n'y a aucune précision quant à savoir quelle ligne de `Table1` est utilisée pour mettre à jour la ligne de `Table2.`  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.Table1', 'U') IS NOT NULL  
    DROP TABLE dbo.Table1;  
GO  
IF OBJECT_ID ('dbo.Table2', 'U') IS NOT NULL  
    DROP TABLE dbo.Table2;  
GO  
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT PRIMARY KEY NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;  
```  
  
 Le même problème peut avoir lieu lors de la combinaison des deux clauses `FROM` et `WHERE CURRENT OF`. Dans cet exemple, les deux lignes de `Table2` correspondent aux qualifications de la clause `FROM` de l'instruction `UPDATE`. Aucune précision n'est fournie quant à savoir quelle ligne de `Table2` est utilisée pour mettre à jour la ligne de `Table1`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.Table1', 'U') IS NOT NULL  
    DROP TABLE dbo.Table1;  
GO  
IF OBJECT_ID ('dbo.Table2', 'U') IS NOT NULL  
    DROP TABLE dbo.Table2;  
GO  
CREATE TABLE dbo.Table1  
    (c1 INT PRIMARY KEY NOT NULL, c2 INT NOT NULL);  
GO  
CREATE TABLE dbo.Table2  
    (d1 INT PRIMARY KEY NOT NULL, d2 INT NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES (1, 10);  
INSERT INTO dbo.Table2 VALUES (1, 20), (2, 30);  
GO  
DECLARE abc CURSOR LOCAL FOR  
    SELECT c1, c2   
    FROM dbo.Table1;  
OPEN abc;  
FETCH abc;  
UPDATE dbo.Table1   
SET c2 = c2 + d2   
FROM dbo.Table2   
WHERE CURRENT OF abc;  
GO  
SELECT c1, c2 FROM dbo.Table1;  
GO  
```  
  
## <a name="compatibility-support"></a>Prise en charge de la compatibilité  
 La prise en charge des indicateurs READUNCOMMITTED et NOLOCK dans la clause FROM s'appliquant à la table cible d'une instruction UPDATE ou DELETE sera supprimée dans une version future de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces indicateurs dans ce contexte lors de vos nouvelles tâches de développement, et pensez à modifier les applications qui les utilisent actuellement.  
  
## <a name="data-types"></a>Types de données  
 Toutes les colonnes **char** et **nchar** sont remplies par des espaces à droite jusqu’à la longueur définie.  
  
 Si l’option ANSI_PADDING est désactivée (OFF), tous les espaces à droite sont supprimés des données insérées dans les colonnes **varchar** et **nvarchar**, sauf dans les chaînes contenant uniquement des espaces. Ces chaînes sont tronquées en une chaîne vide. Si l'option ANSI_PADDING est activée (ON), des espaces supplémentaires sont insérés. Au moment de la connexion, le pilote ODBC de Microsoft SQL Server et le fournisseur OLE DB pour SQL Server attribuent automatiquement la valeur ON à SET ANSI_PADDING. Ceci peut être configuré dans les sources de données ODBC ou lors de la définition des attributs ou des propriétés de connexion. Pour plus d’informations, consultez [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
### <a name="updating-text-ntext-and-image-columns"></a>Mise à jour des colonnes de type text, ntext et image  
 La modification d’une colonne **text**, **ntext** ou **image** avec une instruction UPDATE initialise la colonne, lui assigne un pointeur de texte valide et lui alloue au moins une page de données, sauf si la colonne est mise à jour avec des valeurs NULL.  
  
 Pour remplacer ou modifier de grands blocs de données **text**, **ntext** ou **image**, utilisez [WRITETEXT](../../t-sql/queries/writetext-transact-sql.md) ou [UPDATETEXT](../../t-sql/queries/updatetext-transact-sql.md) au lieu de l’instruction UPDATE.  
  
 Si l’instruction UPDATE peut modifier plusieurs lignes en mettant à jour la clé de clustering et une ou plusieurs colonnes **text**, **ntext** ou **image**, la mise à jour de ces colonnes est partiellement exécutée en remplaçant toutes ces valeurs.  
  
> [!IMPORTANT]
>  Les types de données **ntext**, **text** et **image** seront supprimés dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces types de données dans un nouveau développement. Prévoyez de modifier les applications qui les utilisent actuellement. Utilisez plutôt les types de données [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)et [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) .  
  
### <a name="updating-large-value-data-types"></a><a name="updating-lobs"></a> Mise à jour des données de valeurs élevées  
 Utilisez la clause **.** WRITE **(** _expression_ **,** @_Offset_ **,** @_Length_ **)** pour effectuer une mise à jour partielle ou complète des types de données **varchar(max)** , **nvarchar(max)** et **varbinary(max)** . 
 
 Par exemple, une mise à jour partielle d’une colonne **varchar(max)** risque de supprimer ou modifier uniquement les 200 premiers octets de la colonne (200 caractères en cas d’utilisation de caractères ASCII), alors qu’une mise à jour complète supprime ou modifie toutes les données de cette colonne. Les mises à jour **.WRITE**, qui insèrent ou ajoutent de nouvelles données, sont journalisées de façon minimale si vous avez spécifié un mode de récupération des bases de données simple ou journalisé en bloc. La journalisation minimale n'est pas utilisée lors de la mise à jour de valeurs existantes. Pour plus d'informations, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] change une mise à jour partielle en mise à jour complète quand l’instruction UPDATE provoque l’une des actions suivantes :  
-   Modification d'une colonne clé de la vue ou table partitionnée.  
-   Modification de plusieurs lignes et mise à jour de la clé d'un index cluster non unique sur une valeur non constante.  
  
Vous ne pouvez pas utiliser la clause **.WRITE** pour mettre à jour une colonne NULL ou définir *column_name* avec la valeur NULL.  
  
@*Offset* et @*Length* sont spécifiés en octets pour les types de données **varbinary** et **varchar**, et en paires d’octets pour le type de données **nvarchar**. Pour plus d’informations sur les longueurs de type de données String, consultez [char et varchar (Transact-SQL)](../../t-sql/data-types/char-and-varchar-transact-sql.md) et [nchar et nvarchar (Transact-SQL)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md).
  
Pour optimiser les performances, nous recommandons l'insertion ou la mise à jour de données en blocs multiples de 8 040 octets.  
  
Si une clause OUTPUT référence la colonne modifiée par la clause **\.WRITE**, la valeur complète de cette colonne (l’image avant dans **deleted.** _column\_name_ ou l’image après dans **inserted.** _column\_name_) est retournée dans la colonne spécifiée dans la variable de table. Consultez l’exemple R ci-dessous.  
  
Pour utiliser les mêmes fonctions de la clause **\.WRITE** avec des données de type caractère ou binaire, utilisez [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md).  
  
### <a name="updating-user-defined-type-columns"></a>Mise à jour des colonnes définies par l’utilisateur  
 La mise à jour des valeurs dans des colonnes de type défini par l'utilisateur peut s'effectuer de l'une des façons suivantes :  
  
-   Fournir une valeur dans un type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à condition que le type défini par l'utilisateur prenne en charge la conversion de façon implicite ou explicite. L'exemple suivant indique comment mettre à jour une valeur dans une colonne définie par l'utilisateur `Point`, en la convertissant explicitement à partir d'une chaîne.  
  
    ```sql  
    UPDATE Cities  
    SET Location = CONVERT(Point, '12.3:46.2')  
    WHERE Name = 'Anchorage';  
    ```  
  
-   Appeler une méthode, marquée comme mutateur, d'un type défini par l'utilisateur, pour procéder à la mise à jour. L'exemple suivant appelle une méthode de mutateur du type `Point` appelé `SetXY`. L'état de l'instance de ce type est mis à jour.  
  
    ```sql  
    UPDATE Cities  
    SET Location.SetXY(23.5, 23.5)  
    WHERE Name = 'Anchorage';  
    ```  
  
    > [!NOTE]  
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur si une méthode mutateur est appelée sur une valeur NULL [!INCLUDE[tsql](../../includes/tsql-md.md)] ou si une nouvelle valeur produite par une méthode mutateur est NULL.  
  
-   Modifier la valeur d'une propriété enregistrée ou d'un membre de données public, défini par l'utilisateur. L'expression qui fournit la valeur doit être implicitement convertible au type de propriété. L'exemple suivant modifie la valeur de propriété `X` du type défini par l'utilisateur `Point`.  
  
    ```sql  
    UPDATE Cities  
    SET Location.X = 23.5  
    WHERE Name = 'Anchorage';  
    ```  
  
     Pour modifier différentes propriétés d'une colonne du même type défini par l'utilisateur, vous devez émettre plusieurs instructions UPDATE ou appeler la méthode du mutateur correspondant à ce type.  
  
### <a name="updating-filestream-data"></a>Mise à jour de données FILESTREAM  
 Vous pouvez utiliser l'instruction UPDATE pour mettre à jour un champ FILESTREAM avec une valeur Null, une valeur vide ou une quantité relativement faible de données incluses. Toutefois, une grande quantité de données est diffusée en continu plus efficacement dans un fichier à l'aide des interfaces Win32. Lorsque vous mettez à jour un champ FILESTREAM, vous modifiez les données d'objet blob sous-jacentes dans le système de fichiers. Lorsqu'un champ FILESTREAM a la valeur NULL, les données d'objet blob associées au champ sont supprimées. Vous ne pouvez pas utiliser .WRITE() pour appliquer des mises à jour partielles aux données FILESTREAM. Pour plus d’informations, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Si la mise à jour d'une ligne enfreint une contrainte ou une règle, qu'elle viole le paramètre NULL de la colonne ou que la nouvelle valeur est d'un type de données incompatible, l'instruction est annulée, une erreur est retournée et aucun enregistrement n'est mis à jour.  
  
 Lorsqu'une instruction UPDATE rencontre une erreur arithmétique (erreur de dépassement de capacité, de division par zéro ou de domaine) lors de l'évaluation de l'expression, la mise à jour n'est pas effectuée. Le reste du traitement n'est pas exécuté et un message d'erreur est retourné.  
  
 Si la mise à jour d'une ou de plusieurs colonnes participant à un index cluster conduit à une taille d'index et de ligne supérieure à 8 060 octets, la mise à jour n'est pas effectuée et un message d'erreur est retourné.  
  
## <a name="interoperability"></a>Interopérabilité  
 Vous pouvez insérer des instructions UPDATE dans le corps des fonctions définies par l'utilisateur uniquement si la table en cours de modification est une variable de table.  
  
 Quand un déclencheur `INSTEAD OF` est défini sur des actions UPDATE appliquées à une table, il est exécuté à la place de l’instruction UPDATE. Les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prennent uniquement en charge les déclencheurs AFTER définis sur UPDATE et autres instructions de modification de données. La clause FROM ne peut pas être spécifiée dans une instruction UPDATE qui référence, directement ou indirectement, à une vue sur laquelle est défini un déclencheur `INSTEAD OF`. Pour plus d’informations sur les déclencheurs INSTEAD OF, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 La clause FROM ne peut pas être spécifiée dans une instruction UPDATE qui référence, directement ou indirectement, une vue sur laquelle est défini un déclencheur `INSTEAD OF`. Pour plus d’informations sur les déclencheurs `INSTEAD OF`, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 Lorsqu'une expression de table commune (CTE) est la cible d'une instruction UPDATE, toutes les références cette expression de table commune dans l'instruction doivent correspondre. Par exemple, si un alias est affecté à l'expression de table commune dans la clause FROM, cet alias doit être utilisé pour toutes les autres références à l'expression de table commune. Des références non ambiguës à l'expression de table commune sont requises, car une expression de table commune n'a pas d'ID d'objet, lequel est utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour identifier la relation implicite entre un objet et son alias. Sans cette relation, le plan de requête peut avoir un comportement de jointure et des résultats de requête inattendus. Les exemples suivants illustrent l'utilisation de méthodes correctes et incorrectes en matière de spécification d'une expression de table commune lorsque celle-ci est l'objet cible de l'opération de mise à jour.  
  
```sql  
USE tempdb;  
GO  
-- UPDATE statement with CTE references that are correctly matched.  
DECLARE @x TABLE (ID INT, Value INT);  
DECLARE @y TABLE (ID INT, Value INT);  
INSERT @x VALUES (1, 10), (2, 20);  
INSERT @y VALUES (1, 100),(2, 200);  
  
WITH cte AS (SELECT * FROM @x)  
UPDATE x -- cte is referenced by the alias.  
SET Value = y.Value  
FROM cte AS x  -- cte is assigned an alias.  
INNER JOIN @y AS y ON y.ID = x.ID;  
SELECT * FROM @x;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 ID     Value  
 ------ -----  
 1      100  
 2      200  
 (2 row(s) affected)  
```  

Instruction UPDATE avec des références d’expression de table commune qui ne sont pas mappées correctement.  

```sql  
USE tempdb;  
GO  
DECLARE @x TABLE (ID INT, Value INT);  
DECLARE @y TABLE (ID INT, Value INT);  
INSERT @x VALUES (1, 10), (2, 20);  
INSERT @y VALUES (1, 100),(2, 200);  
  
WITH cte AS (SELECT * FROM @x)  
UPDATE cte   -- cte is not referenced by the alias.  
SET Value = y.Value  
FROM cte AS x  -- cte is assigned an alias.  
INNER JOIN @y AS y ON y.ID = x.ID;   
SELECT * FROM @x;   
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ID     Value  
------ -----  
1      100  
2      100  
(2 row(s) affected)  
```  

## <a name="locking-behavior"></a>Comportement du verrouillage  
 Une instruction UPDATE acquiert un verrou exclusif (X) sur les lignes qu’elle modifie et maintient ces verrous jusqu’à la fin de la transaction. Selon le plan de requête de l’instruction UPDATE, le nombre de lignes modifiées et le niveau d’isolation de la transaction, les verrous peuvent être acquis au niveau de PAGE ou de TABLE plutôt qu’au niveau de ROW. Pour éviter ces verrous de niveau supérieur, scindez les instructions UPDATE qui portent sur des milliers de lignes voire plus en lots et vérifiez que les conditions de jointure et de filtre sont prises en charge par les index. Pour plus d’informations sur les mécanismes de verrouillage dans SQL Server, consultez l’article traitant du [verrouillage dans le moteur de base de données](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine).  
  
## <a name="logging-behavior"></a>Comportement de journalisation  
 L’instruction UPDATE est journalisée. Cependant, les mises à jour partielles de données de valeurs élevées avec la clause **\.WRITE** sont journalisées de façon minimale. Pour plus d’informations, consultez « Mise à jour des données de valeurs élevées » de la section précédente « Types de données ».  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Les autorisations `UPDATE` sont obligatoires sur la table cible. Les autorisations `SELECT` sont également obligatoires pour la table mise à jour si l’instruction UPDATE contient une clause WHERE ou si *expression* dans la clause SET utilise une colonne de la table.  
  
 Les autorisations UPDATE sont attribuées par défaut aux membres du rôle serveur fixe `sysadmin`, aux membres des rôles de base de données fixes `db_owner` et `db_datawriter` ainsi qu’au propriétaire de la table. Les membres des rôles `sysadmin`, `db_owner`, et `db_securityadmin` ainsi que le propriétaire de la table peuvent transférer des autorisations à d’autres utilisateurs.  
  
##  <a name="examples"></a><a name="UpdateExamples"></a> Exemples  
  
|Category|Éléments syntaxiques proposés|  
|--------------|------------------------------|  
|[Syntaxe de base](#BasicSyntax)|UPDATE|  
|[Limitation des lignes mises à jour](#LimitingValues)|WHERE • TOP • WITH expression de table commune • WHERE CURRENT OF|  
|[Définition des valeurs de colonne](#ColumnValues)|valeurs calculées • opérateurs composés • valeurs par défaut • sous-requêtes|  
|[Spécification d’objets cibles autres que les tables standard](#TargetObjects)|vues • variables de table • alias de table|  
|[Mise à jour de données en fonction des données d’autres tables](#OtherTables)|FROM|  
|[Mise à jour de lignes dans une table distante](#RemoteTables)|serveur lié • OPENQUERY • OPENDATASOURCE|  
|[Mise à jour de types de données Large Object (LOB)](#LOBValues)|.WRITE • OPENROWSET|  
|[Mise à jour de types définis par l’utilisateur](#UDTs)|types définis par l'utilisateur|  
|[Substitution du comportement par défaut de l’optimiseur de requête à l’aide d’indicateurs](#TableHints)|indicateurs de table • indicateurs de requête|  
|[Capture des résultats de l’instruction UPDATE](#CaptureResults)|Clause OUTPUT|  
|[Utilisation de l’instruction UPDATE dans d’autres instructions](#Other)|Procédures stockées • TRY...CATCH|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Syntaxe de base  
 Les exemples fournis dans cette section présentent les fonctionnalités de base de l'instruction UPDATE en utilisant la syntaxe minimale requise.  
  
#### <a name="a-using-a-simple-update-statement"></a>R. Utilisation d'une instruction UPDATE simple  
 L'exemple suivant met à jour une colonne unique pour toutes les lignes de la table `Person.Address`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Person.Address  
SET ModifiedDate = GETDATE();  
```  
  
#### <a name="b-updating-multiple-columns"></a>B. Mise à jour de plusieurs colonnes  
 L'exemple suivant met à jour les valeurs dans les colonnes `Bonus`, `CommissionPct` et `SalesQuota` pour toutes les lignes de la table `SalesPerson`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET Bonus = 6000, CommissionPct = .10, SalesQuota = NULL;  
GO  
```  
  
###  <a name="limiting-the-rows-that-are-updated"></a><a name="LimitingValues"></a> Limitation des lignes mises à jour  
 Les exemples de cette section montrent les différentes méthodes que vous pouvez utiliser pour limiter le nombre de lignes affectées par l'instruction UPDATE.  
  
#### <a name="c-using-the-where-clause"></a>C. Utilisation de la clause WHERE  
 L'exemple suivant utilise la clause WHERE pour spécifier quelles lignes mettre à jour. L'instruction met à jour la valeur dans la colonne `Color` de la table `Production.Product` pour toutes les lignes qui ont une valeur existante de « Red » (rouge) dans la colonne `Color` et ont une valeur dans la colonne `Name` qui commence par « Road-250 ».  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Product  
SET Color = N'Metallic Red'  
WHERE Name LIKE N'Road-250%' AND Color = N'Red';  
GO  
```  
  
#### <a name="d-using-the-top-clause"></a>D. Utilisation de la clause TOP  
 Les exemples suivants utilisent la clause TOP pour limiter le nombre de lignes modifiées dans une instruction UPDATE. Quand une clause TOP (*n*) est utilisée avec UPDATE, la mise à jour est effectuée sur une sélection aléatoire de *n* lignes. L'exemple suivant met à jour la colonne `VacationHours` à hauteur de 25 % pour 10 lignes aléatoires dans la table `Employee`.  
  
```sql  
USE AdventureWorks2012;
GO
UPDATE TOP (10) HumanResources.Employee
SET VacationHours = VacationHours * 1.25 ;
GO  
```  
  
 Si vous devez utiliser la clause TOP pour appliquer des mises à jour en respectant une certaine chronologie, vous devez combiner les clauses TOP et ORDER BY dans une instruction de sous-sélection. L'exemple ci-dessous met à jour les heures de congé des 10 employés dont la date d'embauche est la plus ancienne.  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
#### <a name="e-using-the-with-common_table_expression-clause"></a>E. Utilisation de la clause WITH common_table_expression  
 L'exemple suivant met à jour la valeur `PerAssemblyQty` pour l'ensemble des parties et des composants utilisés directement ou indirectement pour créer `ProductAssemblyID 800`. L’expression de table commune retourne une liste hiérarchique des parties utilisées directement pour générer `ProductAssemblyID 800` et des parties utilisées pour générer ces composants, et ainsi de suite. Seules les lignes renvoyées par l'expression de table commune récursive sont modifiées.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
UPDATE Production.BillOfMaterials  
SET PerAssemblyQty = c.PerAssemblyQty * 2  
FROM Production.BillOfMaterials AS c  
JOIN Parts AS d ON c.ProductAssemblyID = d.AssemblyID  
WHERE d.ComponentLevel = 0;  
```  
  
#### <a name="f-using-the-where-current-of-clause"></a>F. Utilisation de la clause WHERE CURRENT OF  
 L'exemple suivant utilise la clause WHERE CURRENT OF pour mettre à jour uniquement la ligne sur laquelle le curseur est positionné. Dans le cas d'un curseur basé sur une jointure, seul le paramètre `table_name` spécifié dans l'instruction UPDATE est modifié. Les autres tables participant au curseur ne sont pas affectées.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
UPDATE HumanResources.EmployeePayHistory  
SET PayFrequency = 2   
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
###  <a name="setting-column-values"></a><a name="ColumnValues"></a> Définition des valeurs de colonne  
 Les exemples de cette section illustrent la mise à jour de colonnes à l'aide de valeurs calculées, de sous-requêtes et des valeurs DEFAULT.  
  
#### <a name="g-specifying-a-computed-value"></a>G. Spécification d'une valeur calculée  
 Les exemples suivants utilisent des valeurs calculées dans une instruction UPDATE. Cet exemple double la valeur dans la colonne `ListPrice` pour toutes les lignes de la table `Product`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
UPDATE Production.Product  
SET ListPrice = ListPrice * 2;  
GO  
```  
  
#### <a name="h-specifying-a-compound-operator"></a>H. Spécification d'un opérateur composé  
 L'exemple suivant utilise la variable `@NewPrice` pour incrémenter le prix de toutes les bicyclettes rouges en prenant le prix actuel et en lui ajoutant 10.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NewPrice INT = 10;  
UPDATE Production.Product  
SET ListPrice += @NewPrice  
WHERE Color = N'Red';  
GO  
```  
  
 L'exemple suivant utilise l'opérateur composé += pour ajouter les données `' - tool malfunction'` à la valeur existante dans la colonne `Name` pour les lignes qui ont un `ScrapReasonID` compris entre 10 et 12.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.ScrapReason   
SET Name += ' - tool malfunction'  
WHERE ScrapReasonID BETWEEN 10 and 12;  
```  
  
#### <a name="i-specifying-a-subquery-in-the-set-clause"></a>I. Spécification d'une sous-requête dans la clause SET  
 L'exemple suivant utilise une sous-requête dans la clause SET pour déterminer la valeur utilisée pour mettre à jour la colonne. La sous-requête doit retourner uniquement une valeur scalaire (autrement dit, une valeur unique par ligne). Cet exemple modifie la colonne `SalesYTD` dans la table `SalesPerson` pour illustrer les dernières ventes enregistrées dans la table `SalesOrderHeader`. La sous-requête agrège les ventes de chaque commercial dans l'instruction `UPDATE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD +   
    (SELECT SUM(so.SubTotal)   
     FROM Sales.SalesOrderHeader AS so  
     WHERE so.OrderDate = (SELECT MAX(OrderDate)  
                           FROM Sales.SalesOrderHeader AS so2  
                           WHERE so2.SalesPersonID = so.SalesPersonID)  
     AND Sales.SalesPerson.BusinessEntityID = so.SalesPersonID  
     GROUP BY so.SalesPersonID);  
GO  
```  
  
#### <a name="j-updating-rows-using-default-values"></a>J. Mise à jour de lignes à l'aide des valeurs DEFAULT  
 L'exemple suivant définit la colonne `CostRate` sur sa valeur par défaut (0.00) pour toutes les lignes qui ont une valeur `CostRate` supérieure à `20.00`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Location  
SET CostRate = DEFAULT  
WHERE CostRate > 20.00;  
```  
  
###  <a name="specifying-target-objects-other-than-standard-tables"></a><a name="TargetObjects"></a> Spécification d’objets cibles autres que les tables standard  
 Les exemples présentés dans cette section montrent comment mettre à jour des lignes en spécifiant une variable de table, un alias de table ou une vue.  
  
#### <a name="k-specifying-a-view-as-the-target-object"></a>K. Spécification d'une vue comme objet cible  
 L'exemple suivant met à jour des lignes dans une table en spécifiant une vue comme objet cible. La définition de la vue référence plusieurs tables. Toutefois, l'instruction UPDATE réussit car elle référence des colonnes d'une seule des tables sous-jacentes. L'instruction UPDATE échouerait si des colonnes des deux tables étaient spécifiées. Pour plus d’informations, consultez [Modifier les données par l’intermédiaire d’une vue](../../relational-databases/views/modify-data-through-a-view.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Person.vStateProvinceCountryRegion  
SET CountryRegionName = 'United States of America'  
WHERE CountryRegionName = 'United States';  
```  
  
#### <a name="l-specifying-a-table-alias-as-the-target-object"></a>L. Spécification d'un alias de table comme objet cible  
 L'exemple suivant met à jour des lignes dans la table `Production.ScrapReason`. L'alias de table affecté à `ScrapReason` dans la clause FROM est spécifié comme objet cible dans la clause UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE sr  
SET sr.Name += ' - tool malfunction'  
FROM Production.ScrapReason AS sr  
JOIN Production.WorkOrder AS wo   
     ON sr.ScrapReasonID = wo.ScrapReasonID  
     AND wo.ScrappedQty > 300;  
```  
  
#### <a name="m-specifying-a-table-variable-as-the-target-object"></a>M. Spécification d'une variable de table comme objet cible  
 L'exemple suivant met à jour des lignes dans une variable de table.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create the table variable.  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
  
-- Populate the table variable with employee ID values from HumanResources.Employee.  
INSERT INTO @MyTableVar (EmpID)  
    SELECT BusinessEntityID FROM HumanResources.Employee;  
  
-- Update columns in the table variable.  
UPDATE @MyTableVar  
SET NewVacationHours = e.VacationHours + 20,  
    ModifiedDate = GETDATE()  
FROM HumanResources.Employee AS e   
WHERE e.BusinessEntityID = EmpID;  
  
-- Display the results of the UPDATE statement.  
SELECT EmpID, NewVacationHours, ModifiedDate FROM @MyTableVar  
ORDER BY EmpID;  
GO  
```  
  
###  <a name="updating-data-based-on-data-from-other-tables"></a><a name="OtherTables"></a> Mise à jour de données en fonction des données d’autres tables  
 Les exemples fournis dans cette section présentent des méthodes de mise à jour de lignes d'une table en fonction d'informations contenues dans une autre table.  
  
#### <a name="n-using-the-update-statement-with-information-from-another-table"></a>N. Utilisation de l'instruction UPDATE avec des informations provenant d'une autre table  
 L'exemple suivant modifie la colonne `SalesYTD` dans la table `SalesPerson` pour illustrer les dernières ventes enregistrées dans la table `SalesOrderHeader`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD + SubTotal  
FROM Sales.SalesPerson AS sp  
JOIN Sales.SalesOrderHeader AS so  
    ON sp.BusinessEntityID = so.SalesPersonID  
    AND so.OrderDate = (SELECT MAX(OrderDate)  
                        FROM Sales.SalesOrderHeader  
                        WHERE SalesPersonID = sp.BusinessEntityID);  
GO  
```  
  
 L'exemple précédent suppose qu'une seule vente est enregistrée pour un vendeur particulier sur une date donnée et que les mises à jour sont actuelles. Cet exemple ne convient pas si plusieurs ventes peuvent être enregistrées pour un vendeur donné au cours d'une même journée. Il s’exécutera sans erreur, mais chaque valeur `SalesYTD` ne sera mise à jour que pour une seule vente, en dépit du nombre de ventes ayant réellement eu lieu ce jour-là. En effet, une instruction UPDATE ne met jamais une même ligne à jour à deux reprises.  
  
 Au cas où plusieurs ventes pourraient avoir lieu le même jour pour un commercial donné, toutes les ventes de chaque commercial doivent être agrégées dans l'instruction `UPDATE`, comme le montre l'exemple suivant :  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD +   
    (SELECT SUM(so.SubTotal)   
     FROM Sales.SalesOrderHeader AS so  
     WHERE so.OrderDate = (SELECT MAX(OrderDate)  
                           FROM Sales.SalesOrderHeader AS so2  
                           WHERE so2.SalesPersonID = so.SalesPersonID)  
     AND Sales.SalesPerson.BusinessEntityID = so.SalesPersonID  
     GROUP BY so.SalesPersonID);  
GO  
```  
  
###  <a name="updating-rows-in-a-remote-table"></a><a name="RemoteTables"></a> Mise à jour de lignes dans une table distante  
 Les exemples présentés dans cette section montrent comment mettre à jour des lignes dans une table cible distante en utilisant un [serveur lié](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) ou une [fonction d’ensemble de lignes](../functions/opendatasource-transact-sql.md) pour référencer la table distante.  
  
#### <a name="o-updating-data-in-a-remote-table-by-using-a-linked-server"></a>O. Mise à jour de données dans une table distante en utilisant un serveur lié  
 L'exemple suivant met à jour une table sur un serveur distant. L’exemple commence par créer un lien vers la source de données distante en utilisant [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Le nom du serveur lié, `MyLinkedServer`, est ensuite spécifié dans un nom d'objet en quatre parties qui se présente sous la forme server.catalog.schema.object. Notez que vous devez spécifier un nom de serveur valide pour `@datasrc`.  
  
```sql  
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_nameinstance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkedServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI10',   
    @datasrc = N'<server name>',  
    @catalog = N'AdventureWorks2012';  
GO  
USE AdventureWorks2012;  
GO  
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
UPDATE MyLinkedServer.AdventureWorks2012.HumanResources.Department  
SET GroupName = N'Public Relations'  
WHERE DepartmentID = 4;  
```  
  
#### <a name="p-updating-data-in-a-remote-table-by-using-the-openquery-function"></a>P. Mise à jour de données dans une table distante en utilisant la fonction OPENQUERY  
 L’exemple suivant met à jour une ligne dans une table distante en spécifiant la fonction d’ensemble de lignes [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md). Le nom de serveur lié créé dans l'exemple précédent est utilisé dans cet exemple.  
  
```sql  
UPDATE OPENQUERY (MyLinkedServer, 'SELECT GroupName FROM HumanResources.Department WHERE DepartmentID = 4')   
SET GroupName = 'Sales and Marketing';  
```  
  
#### <a name="q-updating-data-in-a-remote-table-by-using-the-opendatasource-function"></a>Q. Mise à jour de données dans une table distante en utilisant la fonction OPENDATASOURCE  
 L’exemple suivant met à jour une ligne dans une table distante en spécifiant la fonction d’ensemble de lignes [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md). Spécifiez un nom de serveur valide pour la source de données en utilisant le format *server_name* ou *server_name\instance_name*. Vous devrez peut-être configurer l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les requêtes distribuées appropriées. Pour plus d’informations, consultez la page [Option de configuration du serveur : requêtes distribuées ad hoc](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md).  

```sql
UPDATE OPENDATASOURCE('SQLNCLI', 'Data Source=<server name>;Integrated Security=SSPI').AdventureWorks2012.HumanResources.Department
SET GroupName = 'Sales and Marketing' WHERE DepartmentID = 4;  
```

###  <a name="updating-large-object-data-types"></a><a name="LOBValues"></a> Mise à jour de types de données Large Object (LOB)  
 Les exemples de cette section illustrent des méthodes de mise à jour de valeurs dans les colonnes définies avec les types de données LOB.  
  
#### <a name="r-using-update-with-write-to-modify-data-in-an-nvarcharmax-column"></a>R. Utilisation de l'instruction UPDATE avec la clause .WRITE pour modifier les données dans une colonne nvarchar(max)  
 L’exemple suivant utilise la clause .WRITE pour mettre à jour une valeur partielle dans `DocumentSummary`, une colonne **nvarchar(max)** dans la table `Production.Document`. Le terme `components` est remplacé par le terme `features`, en spécifiant le terme de remplacement, l'emplacement de départ (décalage) du terme à remplacer dans les données existantes et le nombre de caractères à remplacer (longueur). L’exemple utilise également la clause OUTPUT pour retourner les images avant et après de la colonne `DocumentSummary` à la variable de table `@MyTableVar`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE (  
    SummaryBefore NVARCHAR(max),  
    SummaryAfter NVARCHAR(max));  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
OUTPUT deleted.DocumentSummary,   
       inserted.DocumentSummary   
    INTO @MyTableVar  
WHERE Title = N'Front Reflector Bracket Installation';  
SELECT SummaryBefore, SummaryAfter   
FROM @MyTableVar;  
GO  
```  
  
#### <a name="s-using-update-with-write-to-add-and-remove-data-in-an-nvarcharmax-column"></a>S. Utilisation de l'instruction UPDATE avec la clause .WRITE pour ajouter et supprimer des données dans une colonne nvarchar(max)  
 Les exemples suivants ajoutent et suppriment les données d’une colonne **nvarchar(max)** dont la valeur est définie à NULL. Étant donné que la clause .WRITE ne peut pas être utilisée pour modifier une colonne NULL, celle-ci est d’abord remplie avec des données provisoires. Ces données sont ensuite remplacées par les données appropriées à l'aide de la clause .WRITE. Les exemples supplémentaires ajoutent des données à la fin de la valeur de colonne, suppriment (tronquent) les données de la colonne et, pour finir, suppriment les données partielles de la colonne. Les instructions SELECT affichent la modification de données générée par chaque instruction UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Replacing NULL value with temporary data.  
UPDATE Production.Document  
SET DocumentSummary = N'Replacing NULL value'  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Replacing temporary data with the correct data. Setting @Length to NULL   
-- truncates all existing data from the @Offset position.  
UPDATE Production.Document  
SET DocumentSummary .WRITE(N'Carefully inspect and maintain the tires and crank arms.',0,NULL)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Appending additional data to the end of the column by setting   
-- @Offset to NULL.  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N' Appending data to the end of the column.', NULL, 0)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Removing all data from @Offset to the end of the existing value by   
-- setting expression to NULL.   
UPDATE Production.Document  
SET DocumentSummary .WRITE (NULL, 56, 0)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Removing partial data beginning at position 9 and ending at   
-- position 21.  
UPDATE Production.Document  
SET DocumentSummary .WRITE ('',9, 12)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
```  
  
#### <a name="t-using-update-with-openrowset-to-modify-a-varbinarymax-column"></a>T. Utilisation de l'instruction UPDATE avec OPENROWSET pour modifier une colonne varbinary(max)  
 L’exemple suivant remplace une image existante stockée dans une colonne **varbinary(max)** par une nouvelle image. La fonction [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) est utilisée en conjonction avec l’option BULK pour charger l’image dans la colonne. Cet exemple suppose qu'un fichier nommé `Tires.jpg` existe dans le chemin d'accès spécifié.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.ProductPhoto  
SET ThumbNailPhoto = (  
    SELECT *  
    FROM OPENROWSET(BULK 'c:Tires.jpg', SINGLE_BLOB) AS x )  
WHERE ProductPhotoID = 1;  
GO  
```  
  
#### <a name="u-using-update-to-modify-filestream-data"></a>U. Utilisation de l'instruction UPDATE pour modifier des données FILESTREAM  
 L'exemple suivant utilise l'instruction UPDATE pour modifier les données dans le fichier de système de fichiers. Nous déconseillons cette méthode pour transmettre en continu de grandes quantités de données à un fichier. Utilisez les interfaces Win32 appropriées. L'exemple suivant remplace tout texte dans l'enregistrement de fichier par le texte `Xray 1`. Pour plus d’informations, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
```sql  
UPDATE Archive.dbo.Records  
SET [Chart] = CAST('Xray 1' as VARBINARY(max))  
WHERE [SerialNumber] = 2;  
```  
  
###  <a name="updating-user-defined-types"></a><a name="UDTs"></a> Mise à jour de types définis par l’utilisateur  
 Les exemples suivants modifient des valeurs dans les colonnes de type clr défini par l'utilisateur (UDT). Trois méthodes sont illustrées. Pour plus d’informations sur les colonnes définies par l’utilisateur, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).  
  
#### <a name="v-using-a-system-data-type"></a>V. Utilisation d'un type de données système  
 Vous pouvez mettre à jour un UDT en fournissant une valeur dans un type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à condition que le type défini par l'utilisateur prenne en charge la conversion de façon implicite ou explicite. L'exemple suivant indique comment mettre à jour une valeur dans une colonne définie par l'utilisateur `Point`, en la convertissant explicitement à partir d'une chaîne.  
  
```sql  
UPDATE dbo.Cities  
SET Location = CONVERT(Point, '12.3:46.2')  
WHERE Name = 'Anchorage';  
```  
  
#### <a name="w-invoking-a-method"></a>Heure standard Appel d’une méthode  
 Vous pouvez mettre à jour un UDT en appelant une méthode, marquée en tant que mutateur, du type défini par l'utilisateur afin d'effectuer la mise à jour. L'exemple suivant appelle une méthode de mutateur du type `Point` appelé `SetXY`. L'état de l'instance de ce type est mis à jour.  
  
```sql  
UPDATE dbo.Cities  
SET Location.SetXY(23.5, 23.5)  
WHERE Name = 'Anchorage';  
```  
  
#### <a name="x-modifying-the-value-of-a-property-or-data-member"></a>X. Modification de la valeur d'une propriété ou d'un membre de données  
 Vous pouvez mettre à jour un UDT en modifiant la valeur d'une propriété inscrite ou d'un membre de données publiques du type défini par l'utilisateur. L'expression qui fournit la valeur doit être implicitement convertible au type de propriété. L'exemple suivant modifie la valeur de propriété `X` du type défini par l'utilisateur `Point`.  
  
```sql  
UPDATE dbo.Cities  
SET Location.X = 23.5  
WHERE Name = 'Anchorage';  
```  
  
###  <a name="overriding-the-default-behavior-of-the-query-optimizer-by-using-hints"></a><a name="TableHints"></a> Remplacement du comportement par défaut de l’optimiseur de requête à l’aide d’indicateurs  
 Les exemples présentés dans cette section montrent comment utiliser des indicateurs de table et de requête pour substituer temporairement le comportement par défaut de l'optimiseur de requête lors du traitement de l'instruction UPDATE.  
  
> [!CAUTION]  
>  Étant donné que l'optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionne généralement le meilleur plan d'exécution pour une requête, nous vous recommandons de ne recourir à ces conseils qu'en dernier ressort et seulement si vous êtes un développeur ou un administrateur de base de données expérimenté.  
  
#### <a name="y-specifying-a-table-hint"></a>Y. Spécification d'un indicateur de table  
 L’exemple suivant spécifie [l’indicateur de table](../../t-sql/queries/hints-transact-sql-table.md) TABLOCK. Cet indicateur spécifie qu'un verrou partagé est établi sur la table `Production.Product` et maintenu jusqu'à la fin de l'instruction UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Product  
WITH (TABLOCK)  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE 'BK-%';  
GO  
```  
  
#### <a name="z-specifying-a-query-hint"></a>Z. Spécification d'un indicateur de requête  
 L’exemple suivant spécifie [l’indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md)`OPTIMIZE FOR (@variable)` dans l’instruction UPDATE. Cet indicateur spécifie à l'optimiseur de requête d'attribuer à une variable locale une valeur déterminée lors de la compilation et de l'optimisation de la requête. Cette valeur n'est utilisée que pendant l'optimisation de la requête, et non pas lors de son exécution.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE Production.uspProductUpdate  
@Product NVARCHAR(25)  
AS  
SET NOCOUNT ON;  
UPDATE Production.Product  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE @Product  
OPTION (OPTIMIZE FOR (@Product = 'BK-%') );  
GO  
-- Execute the stored procedure   
EXEC Production.uspProductUpdate 'BK-%';  
```  
  
###  <a name="capturing-the-results-of-the-update-statement"></a><a name="CaptureResults"></a> Capture des résultats de l’instruction UPDATE  
 Les exemples présentés dans cette section montrent comment utiliser la [clause OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) pour retourner des informations de chaque ligne affectée par une instruction UPDATE, ou des expressions basées sur ces lignes. Ces résultats peuvent être retournés à l'application en cours de traitement afin d'être utilisés notamment avec des messages de confirmation, des opérations d'archivage et d'autres spécifications d'application similaires.  
  
#### <a name="aa-using-update-with-the-output-clause"></a>AA. Utilisation de l'instruction UPDATE avec la clause OUTPUT  
 L'exemple suivant met à jour la colonne `VacationHours` dans la table `Employee` par 25 pour cent pour les 10 premières lignes et définit également la valeur dans la colonne `ModifiedDate` sur la date actuelle. La clause `OUTPUT` retourne la valeur de `VacationHours` qui existe avant l'application de l'instruction `UPDATE` dans la colonne `deleted.VacationHours` et la valeur mise à jour dans la colonne `inserted.VacationHours` sur la variable de table `@MyTableVar`.  
  
 Deux instructions `SELECT` suivent ; elles retournent les valeurs dans `@MyTableVar`, ainsi que les résultats de la mise à jour dans la table `Employee`. Pour plus d’exemples d’utilisation de la clause OUTPUT, consultez [OUTPUT, clause &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate datetime);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()   
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
###  <a name="using-update-in-other-statements"></a><a name="Other"></a> Utilisation de l’instruction UPDATE dans d’autres instructions  
 Les exemples de cette section montrent comment utiliser UPDATE dans d'autres instructions.  
  
#### <a name="ab-using-update-in-a-stored-procedure"></a>AB. Utilisation de l'instruction UPDATE dans une procédure stockée  
 L'exemple ci-dessous utilise une instruction UPDATE dans une procédure stockée. La procédure accepte un paramètre d'entrée, `@NewHours` et un paramètre de sortie `@RowCount`. La valeur du paramètre `@NewHours` est utilisée dans l’instruction UPDATE pour mettre à jour la colonne `VacationHours` de la table `HumanResources.Employee`. Le paramètre de sortie `@RowCount` est utilisé pour retourner le nombre de lignes affectées à une variable locale. L'expression CASE est utilisée dans la clause SET pour déterminer de manière conditionnelle la valeur qui est définie pour `VacationHours`. Lorsque l'employé est payé à l'heure (`SalariedFlag` = 0), `VacationHours` est défini avec le nombre actuel d'heures plus la valeur spécifiée dans `@NewHours` ; sinon, `VacationHours` est défini avec la valeur spécifiée dans `@NewHours`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE HumanResources.Update_VacationHours  
@NewHours SMALLINT  
AS   
SET NOCOUNT ON;  
UPDATE HumanResources.Employee  
SET VacationHours =   
    ( CASE  
         WHEN SalariedFlag = 0 THEN VacationHours + @NewHours  
         ELSE @NewHours  
       END  
    )  
WHERE CurrentFlag = 1;  
GO  
  
EXEC HumanResources.Update_VacationHours 40;  
```  
  
#### <a name="ac-using-update-in-a-trycatch-block"></a>AC. Utilisation de l’instruction UPDATE dans un bloc TRY...CATCH  
 L’exemple suivant utilise une instruction UPDATE dans un bloc TRY...CATCH pour gérer les erreurs d’exécution qui peuvent se produire pendant l’opération de mise à jour.  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Intentionally generate a constraint violation error.  
    UPDATE HumanResources.Department  
    SET Name = N'MyNewName'  
    WHERE DepartmentID BETWEEN 1 AND 2;  
END TRY  
BEGIN CATCH  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="ad-using-a-simple-update-statement"></a>AD. Utilisation d'une instruction UPDATE simple  
 Les exemples suivants montrent comment une mise à jour peut affecter toutes les lignes quand la clause WHERE n’est pas utilisée pour spécifier la ou les lignes à mettre à jour.  
  
 Cet exemple met à jour les valeurs dans les colonnes `EndDate` et `CurrentFlag` pour toutes les lignes de la table `DimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET EndDate = '2010-12-31', CurrentFlag='False';  
```  
  
 Vous pouvez également utiliser des valeurs calculées dans une instruction UPDATE. L'exemple suivant double la valeur dans la colonne `ListPrice` pour toutes les lignes de la table `Product`.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET BaseRate = BaseRate * 2;  
```  
  
### <a name="ae-using-the-update-statement-with-a-where-clause"></a>AE. Utilisation de l’instruction UPDATE avec une clause WHERE  
 L'exemple suivant utilise la clause WHERE pour spécifier quelles lignes mettre à jour.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET FirstName = 'Gail'  
WHERE EmployeeKey = 500;  
```  
  
### <a name="af-using-the-update-statement-with-label"></a>AF. Utilisation de l’instruction UPDATE avec une étiquette  
 L’exemple suivant montre comment ajouter une étiquette (LABEL) dans l’instruction UPDATE.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimProduct  
SET ProductSubcategoryKey = 2   
WHERE ProductKey = 313  
OPTION (LABEL = N'label1');  
```  
  
### <a name="ag-using-the-update-statement-with-information-from-another-table"></a>AG. Utilisation de l'instruction UPDATE avec des informations provenant d'une autre table  
 Cet exemple crée une table qui stocke le total des ventes annuelles. Il met à jour le total des ventes durant l’année 2004 en exécutant une instruction SELECT sur la table FactInternetSales.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE TABLE YearlyTotalSales (  
    YearlySalesAmount MONEY NOT NULL,  
    Year SMALLINT NOT NULL )  
WITH ( DISTRIBUTION = REPLICATE );  
  
INSERT INTO YearlyTotalSales VALUES (0, 2004);  
INSERT INTO YearlyTotalSales VALUES (0, 2005);  
INSERT INTO YearlyTotalSales VALUES (0, 2006);  
  
UPDATE YearlyTotalSales  
SET YearlySalesAmount=  
(SELECT SUM(SalesAmount) FROM FactInternetSales WHERE OrderDateKey >=20040000 AND OrderDateKey < 20050000)  
WHERE Year=2004;  
  
SELECT * FROM YearlyTotalSales;   
```  

### <a name="ah-ansi-join-for-update-statements"></a>AH. Jointure ANSI pour les instructions de mise à jour
Cet exemple montre comment mettre à jour des données en fonction du résultat de la jointure d’une autre table.

```sql
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;
GO
```

## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Curseurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Fonctions texte et image &#40;Transact-SQL&#41;](../functions/text-and-image-functions-textptr-transact-sql.md)   
 [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md)  
 [Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)    
 [Jeux de caractères codés sur un octet et multioctets](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)  
