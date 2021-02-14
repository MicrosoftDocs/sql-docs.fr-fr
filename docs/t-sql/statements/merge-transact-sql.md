---
description: MERGE (Transact-SQL)
title: MERGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MERGE
- MERGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- updating data [SQL Server]
- modifying data [SQL Server], MERGE statement
- MERGE statement [SQL Server]
- adding data
- DML [SQL Server], MERGE statement
- table modifications [SQL Server], MERGE statement
- data manipulation language [SQL Server], MERGE statement
- inserting data
ms.assetid: c17996d6-56a6-482f-80d8-086a3423eecc
author: XiaoyuMSFT
ms.author: XiaoyuL
ms.openlocfilehash: 6b6610293bb78ef34ae5ca8b88f762c9ab4948e9
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978861"
---
# <a name="merge-transact-sql"></a>MERGE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asa.md)]

Exécute des opérations d'insertion, de mise à jour ou de suppression sur une table cible à partir des résultats d'une jointure avec une table source. Par exemple, synchronisez deux tables en insérant, mettant à jour ou supprimant des lignes dans une seule table selon les différences trouvées dans l'autre table. 

> [!NOTE]
> MERGE est actuellement en préversion pour Azure Synapse Analytics.
  
**Conseil relatif aux performances :** Le comportement conditionnel décrit pour l'instruction MERGE fonctionne mieux lorsque les deux tables ont un mélange complexe de caractéristiques correspondantes. Par exemple, l'insertion d'une ligne si elle n'existe pas ou la mise à jour d’une ligne si elle correspond. Vous pouvez améliorer les performances et l'extensibilité lors d'une simple mise à jour d'une table basée sur les lignes d'une autre table en utilisant les instructions INSERT, UPDATE et DELETE. Par exemple :  
  
```sql
INSERT tbl_A (col, col2)  
SELECT col, col2
FROM tbl_B
WHERE NOT EXISTS (SELECT col FROM tbl_A A2 WHERE A2.col = tbl_B.col);  
```  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql

-- SQL Server and Azure SQL Database
[ WITH <common_table_expression> [,...n] ]  
MERGE
    [ TOP ( expression ) [ PERCENT ] ]
    [ INTO ] <target_table> [ WITH ( <merge_hint> ) ] [ [ AS ] table_alias ]  
    USING <table_source> [ [ AS ] table_alias ]
    ON <merge_search_condition>  
    [ WHEN MATCHED [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]  
        THEN <merge_not_matched> ]  
    [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ <output_clause> ]  
    [ OPTION ( <query_hint> [ ,...n ] ) ]
;  
  
<target_table> ::=  
{
    [ database_name . schema_name . | schema_name . ]  
  target_table  
}  
  
<merge_hint>::=  
{  
    { [ <table_hint_limited> [ ,...n ] ]  
    [ [ , ] INDEX ( index_val [ ,...n ] ) ] }  
}  

<merge_search_condition> ::=  
    <search_condition>  
  
<merge_matched>::=  
    { UPDATE SET <set_clause> | DELETE }  
  
<merge_not_matched>::=  
{  
    INSERT [ ( column_list ) ]
        { VALUES ( values_list )  
        | DEFAULT VALUES }  
}  
  
<clause_search_condition> ::=  
    <search_condition> 
```  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

```syntaxsql
-- MERGE (Preview) for Azure Synapse Analytics 
[ WITH <common_table_expression> [,...n] ]  
MERGE
    [ INTO ] <target_table> [ [ AS ] table_alias ]  
    USING <table_source> [ [ AS ] table_alias ]
    ON <merge_search_condition>  
    [ WHEN MATCHED [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]  
        THEN <merge_not_matched> ]  
    [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]
    [ OPTION ( <query_hint> [ ,...n ] ) ]
;  -- The semi-colon is required, or the query will return syntax  error. 
```
 
## <a name="arguments"></a>Arguments

WITH \<common_table_expression>  
Spécifie le jeu de résultats ou la vue nommés temporaires, également appelés expression de table commune et définis dans le cadre de l'instruction MERGE. Le jeu de résultats est dérivé d'une simple requête et est référencé par l'instruction MERGE. Pour plus d’informations, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
TOP ( *expression* ) [ PERCENT ]  
Spécifie le nombre ou le pourcentage de lignes affectées. L’argument *expression* peut être un nombre ou un pourcentage de lignes. Les lignes référencées dans l'expression TOP ne sont pas triées dans un ordre donné. Pour plus d’informations, consultez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
La clause TOP est appliquée après la jointure de l'intégralité de la table source et de la table cible, et après la suppression des lignes jointes qui ne sont pas éligibles pour une opération de type INSERT, UPDATE ou DELETE. La clause TOP réduit le nombre de lignes jointes à la valeur spécifiée. Les actions d’insertion, de mise à jour ou de suppression s’appliquent aux lignes jointes restantes de manière non ordonnée. Les lignes ne sont donc pas réparties selon un ordre particulier dans le cadre des actions définies dans les clauses WHEN. Par exemple, la spécification de la clause TOP (10) affecte 10 lignes. Parmi ces lignes, 7 peuvent être mises à jour et 3 insérées, ou alors 1 ligne peut être supprimée, 5 mises à jour et 4 insérées, et ainsi de suite.  
  
Étant donné que l'instruction MERGE effectue une analyse complète des tables source et cible, les performances d'E/S peuvent être affectées lorsque la clause TOP est utilisée pour modifier une table volumineuse en créant plusieurs lots. Dans ce scénario, il est important de s’assurer que tous les lots consécutifs ciblent les nouvelles lignes.  
  
*database_name*  
Nom de la base de données contenant *target_table*.  
  
*schema_name*  
Nom du schéma auquel *target_table* appartient.  
  
*target_table*  
Table ou vue à laquelle les lignes de données de \<table_source> sont comparées sur la base de \<clause_search_condition>. *target_table* est la cible de toute opération de type INSERT, UPDATE ou DELETE spécifiée par les clauses WHEN de l’instruction MERGE.  
  
Si *target_table* est une vue, toutes les opérations dont elle fait l’objet doivent satisfaire aux conditions requises pour la mise à jour des vues. Pour plus d’informations, consultez [Modifier les données par l’intermédiaire d’une vue](../../relational-databases/views/modify-data-through-a-view.md).  
  
*target_table* ne peut pas être une table distante. Aucune règle ne peut être définie sur *target_table*.  
  
[ AS ] *table_alias*  
Autre nom utilisé pour faire référence à une table pour *target_table*.  
  
USING \<table_source>  
Spécifie la source de données correspondant aux lignes de données dans *target_table* en fonction de \<merge_search condition>. Le résultat de cette correspondance dicte les actions à effectuer par les clauses WHEN de l'instruction MERGE. \<table_source> peut être une table distante ou une table dérivée qui accède à des tables distantes.
  
\<table_source> peut être une table dérivée qui utilise le [!INCLUDE[tsql](../../includes/tsql-md.md)][constructeur de valeurs de table](../../t-sql/queries/table-value-constructor-transact-sql.md) pour construire une table en spécifiant plusieurs lignes.  
  
 [ AS ] *table_alias*  
Autre nom utilisé pour faire référence à une table pour table_source.   
  
Pour plus d’informations sur la syntaxe et les arguments de cette clause, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
ON \<merge_search_condition>  
Spécifie les conditions de jointure de \<table_source> avec *target_table* afin de déterminer où la correspondance a lieu.
  
> [!CAUTION]  
> Il est important de spécifier uniquement les colonnes de la table cible utilisées à des fins de correspondance. Autrement dit, spécifiez les colonnes de la table cible qui seront comparées à la colonne correspondante de la table source. N'essayez pas d'améliorer les performances des requêtes en éliminant par filtrage des lignes de la table cible dans la clause ON, via la spécification de `AND NOT target_table.column_x = value`, par exemple. Cette approche peut retourner des résultats inattendus et incorrects.  
  
WHEN MATCHED THEN \<merge_matched>  
Spécifie que toutes les lignes de *target_table qui correspondent aux lignes retournées par \<table_source> ON \<merge_search_condition>, et qui répondent aux critères de recherche supplémentaires, sont mises à jour ou supprimées en fonction de la clause \<merge_matched>.  
  
L'instruction MERGE peut avoir au plus deux clauses WHEN MATCHED. Si deux clauses sont spécifiées, la première clause doit être accompagnée d’une clause AND \<search_condition>. Pour toute ligne donnée, la deuxième clause WHEN MATCHED est appliquée uniquement si la première ne l'est pas. En présence de deux clauses WHEN MATCHED, l'une d'elles doit spécifier une action UPDATE et l'autre une action DELETE. Lorsque l’action UPDATE est spécifiée dans la clause \<merge_matched> et que plusieurs lignes de \<table_source> correspondent à une ligne dans *target_table* en fonction de \<merge_search_condition>, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur. L'instruction MERGE ne peut pas mettre à jour la même ligne plus d'une fois, ou mettre à jour et supprimer la même ligne.  
  
WHEN NOT MATCHED [ BY TARGET ] THEN \<merge_not_matched>  
Spécifie qu’une ligne est insérée dans *target_table* pour chaque ligne retournée par \<table_source>ON \<merge_search_condition>qui ne correspond pas à une ligne dans *target_table*, mais satisfait à un critère de recherche supplémentaire, le cas échéant. Les valeurs à insérer sont spécifiées par la clause \<merge_not_matched>. L’instruction MERGE peut avoir une seule clause WHEN NOT MATCHED [ BY TARGET ].

WHEN NOT MATCHED BY SOURCE THEN \<merge_matched>  
Spécifie que toutes les lignes de *target_table qui correspondent aux lignes renvoyées par \<table_source> ON \<merge_search_condition>, et qui répondent aux conditions de recherche supplémentaires, sont mises à jour ou supprimées en fonction de la clause \<merge_matched>.  
  
L'instruction MERGE peut avoir au plus deux clauses WHEN NOT MATCHED BY SOURCE. Si deux clauses sont spécifiées, la première clause doit être accompagnée d’une clause AND \<clause_search_condition>. Pour toute ligne donnée, la deuxième clause WHEN NOT MATCHED BY SOURCE est appliquée uniquement si la première ne l'est pas. En présence de deux clauses WHEN NOT MATCHED BY SOURCE, l'une d'elles doit spécifier une action UPDATE et l'autre une action DELETE. Seules les colonnes de la table cible peuvent être référencées dans \<clause_search_condition>.  
  
Quand aucune ligne n’est retournée par \<table_source>, les colonnes de la table source ne sont pas accessibles. Si l’opération de mise à jour ou de suppression spécifiée dans la clause \<merge_matched> des colonnes dans la table source, l’erreur 207 (nom de colonne non valide) est retournée. Par exemple, la clause `WHEN NOT MATCHED BY SOURCE THEN UPDATE SET TargetTable.Col1 = SourceTable.Col1` peut faire en sorte que l'instruction échoue dans la mesure où `Col1` dans la table source est inaccessible.  
  
AND \<clause_search_condition>  
Spécifie toute condition de recherche valide. Pour plus d’informations, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
\<table_hint_limited>  
Spécifie un ou plusieurs indicateurs de table qui sont appliqués à la table cible pour chaque opération INSERT, UPDATE ou DELETE exécutée par l'instruction MERGE. Le mot clé WITH et les parenthèses sont obligatoires.  
  
NOLOCK et READUNCOMMITTED ne sont pas autorisés. Pour plus d’informations sur les indicateurs de table, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
La spécification de l'indicateur TABLOCK sur une table qui est la cible d'une instruction INSERT a le même effet que la spécification de l'indicateur TABLOCKX. Un verrou exclusif est appliqué à la table. Lorsque FORCESEEK est spécifié, il s'applique à l'instance implicite de la table cible jointe à la table source.  
  
> [!CAUTION]  
> Le fait de spécifier READPAST avec WHEN NOT MATCHED [ BY TARGET ] THEN INSERT peut se traduire par des opérations INSERT qui violent des contraintes UNIQUE.  
  
INDEX ( index_val [ ,...n ] )  
Spécifie le nom ou l'ID d'un ou de plusieurs index sur la table cible pour effectuer une jointure implicite avec la table source. Pour plus d’informations, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
\<output_clause>  
Retourne une ligne pour chaque ligne dans *target_table* qui est mise à jour, insérée ou supprimée, peu importe l’ordre. **$action** peut être spécifié dans la clause de sortie. **$action** est une colonne de type **nvarchar(10)** qui retourne l’une des trois valeurs possibles pour chaque ligne : « INSERT », « UPDATE » ou « DELETE », en fonction de l'action effectuée sur cette ligne. Pour plus d’informations sur les arguments et le comportement de cette clause, consultez [Clause OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
OPTION ( \<query_hint> [ ,...n ] )  
Spécifie que des indicateurs de l'optimiseur sont utilisés pour personnaliser la façon dont le moteur de base de données traite l'instruction. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
\<merge_matched>  
Spécifie l’opération de mise à jour ou suppression qui s’applique à toutes les lignes de *target_table* qui ne correspondent pas aux lignes retournées par \<table_source> ON \<merge_search_condition>, et qui satisfont à toute condition de recherche supplémentaire.  
  
UPDATE SET \<set_clause>  
Spécifie la liste de noms de colonne ou de variable à mettre à jour dans la table cible et les valeurs avec lesquelles les mettre à jour.  
  
Pour plus d’informations sur les arguments de cette clause, consultez [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md). La définition d'une variable à la même valeur qu'une colonne n'est pas prise en charge.  
  
Suppression  
Spécifie que les lignes qui correspondent aux lignes dans *target_table* sont supprimées.  
  
\<merge_not_matched>  
Spécifie les valeurs à insérer dans la table cible.  
  
(*column_list*)  
Liste d'une ou de plusieurs colonnes de la table cible dans lesquelles insérer des données. Les colonnes doivent être spécifiées comme un nom en une seule partie sinon l'instruction MERGE échouera. *column_list* doit être placé entre parenthèses et délimité par des virgules.  
  
VALUES ( *values_list*)  
Liste séparée par des virgules et contenant des constantes, variables ou expressions qui retourne les valeurs à insérer dans la table cible. Les expressions ne peuvent pas contenir d'instruction EXECUTE.  
  
DEFAULT VALUES  
Force la ligne insérée à prendre les valeurs par défaut définies pour chaque colonne.  
  
Pour plus d’informations sur cette clause, consultez [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
\<search_condition>  
Spécifie les conditions de recherche pour spécifier \<merge_search_condition> ou \<clause_search_condition>. Pour plus d’informations sur les arguments pour cette clause, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  

\<graph search pattern>  
Spécifie le modèle de correspondance de graphe. Pour plus d’informations sur les arguments de cette clause, consultez [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md).
  
## <a name="remarks"></a>Notes
>[!NOTE]
> Dans Azure Synapse Analytics, la commande MERGE (préversion) présente les différences suivantes par rapport à SQL Server et à Azure SQL Database.  
> - Une mise à jour MERGE correspond à une paire suppression-insertion. Le nombre de lignes affectées par une mise à jour MERGE comprend les lignes supprimées et insérées. 
> - Dans préversion, MERGE…WHEN NOT MATCHED INSERT n’est pas pris en charge pour les tables avec colonnes IDENTITY.  
> - La prise en charge de tables présentant des types de distribution différents est décrite dans le tableau suivant :

>|Clause MERGE dans Azure Synapse Analytics|Table de distribution CIBLE prise en charge| Table de distribution SOURCE prise en charge|Comment|  
>|-----------------|---------------|-----------------|-----------|  
>|**WHEN MATCHED**| Tous les points de distribution |Tous les points de distribution||  
>|**NOT MATCHED BY TARGET**|HASH |Tous les points de distribution|Utilisez UPDATE/DELETE FROM… JOIN pour synchroniser deux tables. |
>|**NOT MATCHED BY SOURCE**|Tous les points de distribution|Tous les points de distribution|||  

Au moins l'une des trois clauses MATCHED doit être spécifiée, mais cela peut être dans n'importe quel ordre. Une variable ne peut pas être mise à jour plus d'une fois dans la même clause MATCHED.  
  
Toute opération d'insertion, de mise à jour ou de suppression spécifiée sur la table cible par l'instruction MERGE est limitée par toutes contraintes qui s'appliquent à elle, notamment les contraintes d'intégrité référentielle en cascade. Si IGNORE_DUP_KEY a la valeur ON pour un index unique sur la table cible, MERGE ignore ce paramètre.  
  
L'instruction MERGE requiert un point-virgule (;) comme terminateur d'instruction. L'erreur 10713 est générée lorsqu'une instruction MERGE est exécutée sans le terminateur.  
  
En cas d’utilisation après MERGE, [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md) retourne au client le nombre total de lignes insérées, mises à jour et supprimées.  
  
MERGE est un mot clé entièrement réservé lorsque le niveau de compatibilité de la base de données a la valeur 100 ou une valeur supérieure. L'instruction MERGE est disponible sous les niveaux de compatibilité de base de données 90 et 100 ; cependant, le mot clé n'est pas entièrement réservé lorsque le niveau de compatibilité de la base de données a la valeur 90.  
  
N’utilisez pas l’instruction **MERGE** avec la réplication de mise à jour en attente. **MERGE** et le déclencheur de mise à jour en attente ne sont pas compatibles. Remplacez l’instruction **MERGE** par une instruction d’insertion (INSERT) ou de mise à jour (UPDATE).  


## <a name="trigger-implementation"></a>Implémentation de déclencheur

Pour chaque opération INSERT, UPDATE ou DELETE spécifiée dans l'instruction MERGE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lance tous les déclencheurs AFTER correspondants définis sur la table cible, mais ne garantit pas l'opération sur laquelle les déclencheurs seront lancés en premier ou en dernier. Les déclencheurs définis pour la même opération respectent l'ordre que vous spécifiez. Pour plus d’informations sur le paramétrage de l’ordre de lancement des déclencheurs, consultez [Spécifier les premier et dernier déclencheurs](../../relational-databases/triggers/specify-first-and-last-triggers.md).  
  
Si la table cible a un déclencheur INSTEAD OF actif défini pour une opération INSERT, UPDATE ou DELETE effectuée par une instruction MERGE, elle doit avoir un déclencheur INSTEAD OF actif pour toutes les opérations spécifiées dans l'instruction MERGE.  
  
Si des déclencheurs INSTEAD OF UPDATE ou INSTEAD OF DELETE sont définis sur *target_table*, les opérations UPDATE ou DELETE ne sont pas exécutées. À la place, les déclencheurs sont lancés et les tables **insérées** et **supprimées** sont remplies en conséquence.  
  
Si des déclencheurs INSTEAD OF INSERT sont définis sur *target_table*, l’opération INSERT n’est pas effectuée. Au lieu de cela, la table est remplie en conséquence.  
  
## <a name="permissions"></a>Autorisations

Nécessite l'autorisation SELECT sur la table source et les autorisations INSERT, UPDATE ou DELETE sur la table cible. Pour plus d’informations, consultez la section Autorisations dans les articles [SELECT](../../t-sql/queries/select-transact-sql.md), [INSERT](../../t-sql/statements/insert-transact-sql.md), [UPDATE](../../t-sql/queries/update-transact-sql.md) et [DELETE](../../t-sql/statements/delete-transact-sql.md).  
  
## <a name="optimizing-merge-statement-performance"></a>Optimisation des performances de l'instruction MERGE

En utilisant l'instruction MERGE, vous pouvez remplacer les instructions individuelles DML par une instruction unique. Vous pouvez ainsi améliorer les performances des requêtes car les opérations sont effectuées dans une seule instruction, cela permettant de réduire le nombre de traitements des données des tables sources et cibles. Toutefois, vous ne pouvez obtenir des gains de performance que si vous disposez d'index et de jointures corrects, d'autres considérations entrant également en jeu.

### <a name="index-best-practices"></a>Meilleures pratiques pour les index

Pour améliorer les performances de l'instruction MERGE, nous vous recommandons de respecter les règles suivantes relatives aux index :

- Créez un index sur les colonnes de jointure de la table source qui sont uniques et qui sont des colonnes de couverture.
- Créez un index cluster unique sur les colonnes de jointure dans la table cible.

Ces index garantissent que les clés de jointure sont uniques et que les données des tables sont triées. Les performances des requêtes sont meilleures car l'optimiseur de requête n'a pas besoin d'effectuer de validation supplémentaire pour localiser et mettre à jour les lignes en double, et aucune opération de tri supplémentaire n'est nécessaire.

### <a name="join-best-practices"></a>Meilleures pratiques pour les jointures

Pour améliorer les performances de l'instruction MERGE et garantir l'obtention de résultats corrects, nous vous recommandons de respecter les règles suivantes relatives aux jointures :

- Spécifiez uniquement dans la clause ON <merge_search_condition> des conditions de recherche qui déterminent les critères de correspondance des données dans les tables sources et cibles. Autrement dit, spécifiez uniquement les colonnes de la table cible qui seront comparées aux colonnes correspondantes de la table source. 
- N'incluez pas de comparaisons avec d'autres valeurs (par exemple, une constante).

Pour filtrer des lignes des tables sources ou cibles, utilisez l’une des méthodes suivantes.

- Spécifiez la condition de recherche destinée au filtrage des lignes dans la clause WHEN appropriée. Par exemple, WHEN NOT MATCHED AND S.EmployeeName LIKE 'S%' THEN INSERT....
- Définissez une vue sur la source ou la cible qui retourne les lignes filtrées et référencez la vue en tant que table source ou cible. Si la vue est définie sur la table cible, toutes les opérations dont elle fait l'objet doivent satisfaire aux conditions requises pour la mise à jour des vues. Pour plus d’informations sur la mise à jour de données à l’aide d’un affichage, consultez Modification des données via un affichage.
- Utilisez la clause `WITH <common table expression>` pour filtrer les lignes des tables sources ou cibles. Cette méthode est semblable à la spécification de critères de recherche supplémentaires dans la clause ON et peut produire des résultats incorrects. Nous vous recommandons d'éviter l'utilisation de cette méthode ou de la tester soigneusement avant de l'implémenter.

L'opération de jointure dans l'instruction MERGE peut être optimisée de la même façon qu'une jointure dans une instruction SELECT. Lorsque SQL Server procède au traitement des jointures, l’optimiseur de requête choisit parmi plusieurs possibilités la méthode de traitement la plus efficace. Lorsque la source et la cible sont de taille semblable et que vous avez appliqué aux tables sources et cibles les lignes directrices relatives aux index décrites précédemment, un opérateur de jointure de fusion constitue le plan de requête le plus efficace. En effet, les deux tables ne sont analysées qu'une fois et il n'est pas nécessaire de trier les données. Lorsque la source est plus petite que la table cible, un opérateur de boucles imbriquées est préférable.

Vous pouvez forcer l'utilisation d'une jointure spécifique en spécifiant la clause `OPTION (<query_hint>)` dans l'instruction MERGE. Nous vous recommandons de ne pas utiliser de jointure de hachage comme indicateur de requête pour les instructions MERGE car ce type de jointure n'utilise pas d'index.

### <a name="parameterization-best-practices"></a>Meilleures pratiques pour le paramétrage

Si une instruction SELECT, INSERT, UPDATE ou DELETE est exécutée sans paramètres, l’optimiseur de requête SQL Server peut choisir de paramétrer l’instruction en interne. Toute valeur littérale contenue dans la requête est alors remplacée par des paramètres. Par exemple, l’instruction insère dbo. Les valeurs MyTable (col1, col2) (1, 10) peuvent être implémentées en interne sous la forme INSERT dbo. VALEURS MyTable (col1, col2) (@p1, @p2). Ce processus, appelé paramétrage simple, augmente la capacité du moteur relationnel à faire correspondre les nouvelles instructions SQL aux plans d’exécution existants compilés précédemment. On peut ainsi améliorer les performances des requêtes grâce à la diminution de la fréquence des compilations et recompilations des requêtes. L'optimiseur de requête n'applique pas le processus de paramétrage simple aux instructions MERGE. Par conséquent, les instructions MERGE qui contiennent des valeurs littérales peuvent ne pas s'exécuter de façon aussi fluide que les instructions individuelles INSERT, UPDATE ou DELETE car un nouveau plan est compilé à chaque exécution de l'instruction MERGE.

Pour améliorer les performances des requêtes, nous vous recommandons de respecter les règles suivantes relatives au paramétrage :

- Paramétrez toutes les valeurs littérales de la clause `ON <merge_search_condition>` et des clauses `WHEN` de l’instruction MERGE. Par exemple, vous pouvez incorporer l'instruction MERGE dans une procédure stockée pour remplacer les valeurs littérales par les paramètres d'entrée appropriés.
- Si vous ne pouvez pas paramétrer l’instruction, créez un repère de plan de type `TEMPLATE` et spécifiez l’indicateur de requête `PARAMETERIZATION FORCED` dans le repère de plan.
- Si les instructions MERGE sont fréquemment exécutées sur la base de données, vous devez envisager de définir l'option PARAMETERIZATION sur FORCED. Configurez cette option avec précaution. L’option `PARAMETERIZATION` est un paramètre de niveau base de données qui affecte le traitement de toutes les requêtes effectuées sur la base de données.

### <a name="top-clause-best-practices"></a>Meilleures pratiques pour la clause TOP

Dans l'instruction MERGE, la clause TOP définit le nombre ou le pourcentage de lignes affectées après la jointure de la table source et de la table cible, et après la suppression des lignes qui ne sont pas éligibles pour une opération de type INSERT, UPDATE ou DELETE. La clause TOP réduit le nombre de lignes jointes à la valeur spécifiée et les actions INSERT, UPDATE ou DELETE sont appliquées aux lignes jointes restantes sans respecter un ordre particulier. Les lignes ne sont donc pas réparties selon un ordre particulier dans le cadre des actions définies dans les clauses WHEN. Par exemple, la spécification de la clause TOP (10) affecte 10 lignes, dont 7 peuvent être mises à jour et 3 insérées, ou alors 1 ligne peut être supprimée, 5 mises à jour et 4 insérées, et ainsi de suite.

On utilise couramment la clause TOP pour effectuer des opérations en langage DML sur une grande table dans les lots. Lorsque l'on utilise dans ce but la clause TOP dans l'instruction MERGE, il est important de tenir compte des conséquences suivantes.

- Les performances d'E/S peuvent être affectées.

  L'instruction MERGE effectue une analyse complète des tables sources et cibles. La division de l'opération en lots permet de réduire le nombre d'opérations d'écriture effectuées par lot. Toutefois, chaque lot effectuera une analyse complète des tables sources et cibles. L'activité de lecture résultante peut affecter les performances de la requête.

- Des résultats incorrects peuvent se produire.

  Il est important de s’assurer que tous les lots consécutifs traitent les nouvelles lignes. Dans le cas contraire, des comportements indésirables tels que l’insertion incorrecte de lignes en double dans la table cible peuvent se produire. Cela peut se produire lorsque la table source inclut une ligne que ne figurait pas dans un lot cible mais figurait dans la table cible globale.

- Pour garantir l'obtention de résultats corrects :

  - Utilisez la clause ON pour déterminer quelles lignes sources affectent les lignes cibles existantes et quelles lignes sont vraiment nouvelles.
  - Utilisez une condition supplémentaire dans la clause WHEN MATCHED pour déterminer si la ligne cible a déjà été mise à jour par un lot précédent.

Étant donné que la clause TOP n'est appliquée qu'une fois ces clauses appliquées, chaque exécution insère une ligne réellement sans correspondance ou met à jour une ligne existante.

### <a name="bulk-load-best-practices"></a>Meilleures pratiques pour le chargement en masse

L’instruction MERGE peut être utilisée pour effectuer un chargement en masse efficace des données à partir d’un fichier de données sources vers une table cible en spécifiant la clause `OPENROWSET(BULK…)` comme source de table. La totalité du fichier est ainsi traitée dans un lot unique.

Pour améliorer les performances du processus de fusion en masse, nous vous recommandons de respecter les règles suivantes :

- Créez un index cluster sur les colonnes de jointure de la table cible.
- Utilisez les conseils ORDER et UNIQUE dans la clause `OPENROWSET(BULK…)` pour spécifier la façon dont le fichier de données sources a été trié.

  Par défaut, le processus de chargement en masse considère que le fichier de données n'est pas trié. Par conséquent, il est important que les données sources soient triées d'après l'index cluster sur la table cible et que l'indicateur ORDER soit utilisé pour indiquer l'ordre de tri afin que l'optimiseur de requête puisse générer un plan de requête plus efficace. Les indicateurs sont validés pendant l'exécution. Si le flux de données ne se conforme pas aux indicateurs spécifiés, une erreur est générée.

Le respect de ces règles permet d'être sûr que les clés de jointure sont uniques et que l'ordre de tri des données dans le fichier source correspond à celui de la table cible. Les performances des requêtes sont améliorées car aucune opération de tri supplémentaire n'est nécessaire et aucune copie des données n'est requise.

### <a name="measuring-and-diagnosing-merge-performance"></a>Mesure et diagnostic des performances de l'instruction MERGE

Les fonctionnalités suivantes sont à votre disposition pour vous aider à mesurer et diagnostiquer les performances des instructions MERGE.

- Utilisez le compteur merge stmt dans la vue de gestion dynamique [sys.dm_exec_query_optimizer_info](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-optimizer-info-transact-sql.md) pour retourner le nombre d’optimisations de requêtes concernant les instructions MERGE.
- Utilisez l’attribut merge_action_type dans la vue de gestion dynamique [sys.dm_exec_plan_attributes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md) pour retourner le type de plan d’exécution de déclencheur utilisé comme résultat d’une instruction MERGE.
- Utilisez Trace SQL pour rassembler des données de résolution des problèmes pour l'instruction MERGE comme vous le feriez pour d'autres instructions en langage DML. Pour en savoir plus, voir [SQL Trace](../../relational-databases/sql-trace/sql-trace.md).

## <a name="examples"></a>Exemples  

### <a name="a-using-merge-to-do-insert-and-update-operations-on-a-table-in-a-single-statement"></a>R. Utilisation de MERGE pour effectuer des opérations INSERT et UPDATE sur une table dans une instruction unique

Un scénario courant est la mise à jour d’une ou de plusieurs colonnes dans une table si une ligne correspondante existe. Ou l’insertion des données en tant que nouvelle ligne si une ligne correspondante n’existe pas. En général, vous exécutez l’un des scénarios en passant des paramètres à une procédure stockée qui contient les instructions UPDATE et INSERT appropriées. Avec l'instruction MERGE, vous pouvez effectuer les deux tâches dans une instruction unique. L'exemple suivant illustre une procédure stockée qui contient à la fois une instruction INSERT et une instruction UPDATE dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La procédure est ensuite modifiée pour exécuter les opérations équivalentes à l'aide d'une seule instruction MERGE.  
  
```sql  
CREATE PROCEDURE dbo.InsertUnitMeasure  
    @UnitMeasureCode nchar(3),  
    @Name nvarchar(25)  
AS
BEGIN  
    SET NOCOUNT ON;  
-- Update the row if it exists.
    UPDATE Production.UnitMeasure  
SET Name = @Name  
WHERE UnitMeasureCode = @UnitMeasureCode  
-- Insert the row if the UPDATE statement failed.  
IF (@@ROWCOUNT = 0 )  
BEGIN  
    INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name)  
    VALUES (@UnitMeasureCode, @Name)  
END  
END;  
GO  
-- Test the procedure and return the results.  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'Test Value';  
SELECT UnitMeasureCode, Name FROM Production.UnitMeasure  
WHERE UnitMeasureCode = 'ABC';  
GO  
  
-- Rewrite the procedure to perform the same operations using the
-- MERGE statement.  
-- Create a temporary table to hold the updated or inserted values
-- from the OUTPUT clause.  
CREATE TABLE #MyTempTable  
    (ExistingCode nchar(3),  
     ExistingName nvarchar(50),  
     ExistingDate datetime,  
     ActionTaken nvarchar(10),  
     NewCode nchar(3),  
     NewName nvarchar(50),  
     NewDate datetime  
    );  
GO  
ALTER PROCEDURE dbo.InsertUnitMeasure  
    @UnitMeasureCode nchar(3),  
    @Name nvarchar(25)  
AS
BEGIN  
    SET NOCOUNT ON;  
  
    MERGE Production.UnitMeasure AS target  
    USING (SELECT @UnitMeasureCode, @Name) AS source (UnitMeasureCode, Name)  
    ON (target.UnitMeasureCode = source.UnitMeasureCode)  
    WHEN MATCHED THEN
        UPDATE SET Name = source.Name  
    WHEN NOT MATCHED THEN  
        INSERT (UnitMeasureCode, Name)  
        VALUES (source.UnitMeasureCode, source.Name)  
    OUTPUT deleted.*, $action, inserted.* INTO #MyTempTable;  
END;  
GO  
-- Test the procedure and return the results.  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'New Test Value';  
EXEC InsertUnitMeasure @UnitMeasureCode = 'XYZ', @Name = 'Test Value';  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'Another Test Value';  
  
SELECT * FROM #MyTempTable;  
-- Cleanup
DELETE FROM Production.UnitMeasure WHERE UnitMeasureCode IN ('ABC','XYZ');  
DROP TABLE #MyTempTable;  
GO  
```  
  
### <a name="b-using-merge-to-do-update-and-delete-operations-on-a-table-in-a-single-statement"></a>B. Utilisation de MERGE pour effectuer des opérations UPDATE et DELETE sur une table dans une instruction unique

L'exemple suivant utilise la clause MERGE pour mettre quotidiennement à jour la table `ProductInventory` dans l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], selon les commandes traitées dans la table `SalesOrderDetail`. La colonne `Quantity` de la table `ProductInventory` est mise à jour en soustrayant le nombre de commandes passées chaque jour pour chaque produit dans la table `SalesOrderDetail`. Si le nombre de commandes concernant un produit est tel que le stock de ce produit tombe à 0 ou en dessous de cette valeur, la ligne correspondant à ce produit est supprimée de la table `ProductInventory`.  
  
```sql  
CREATE PROCEDURE Production.usp_UpdateInventory  
    @OrderDate datetime  
AS  
MERGE Production.ProductInventory AS target  
USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
    JOIN Sales.SalesOrderHeader AS soh  
    ON sod.SalesOrderID = soh.SalesOrderID  
    AND soh.OrderDate = @OrderDate  
    GROUP BY ProductID) AS source (ProductID, OrderQty)  
ON (target.ProductID = source.ProductID)  
WHEN MATCHED AND target.Quantity - source.OrderQty <= 0  
    THEN DELETE  
WHEN MATCHED
    THEN UPDATE SET target.Quantity = target.Quantity - source.OrderQty,
                    target.ModifiedDate = GETDATE()  
OUTPUT $action, Inserted.ProductID, Inserted.Quantity,
    Inserted.ModifiedDate, Deleted.ProductID,  
    Deleted.Quantity, Deleted.ModifiedDate;  
GO  
  
EXECUTE Production.usp_UpdateInventory '20030501'  
```  
  
### <a name="c-using-merge-to-do-update-and-insert-operations-on-a-target-table-by-using-a-derived-source-table"></a>C. Utilisation de l'instruction MERGE pour effectuer des opérations UPDATE et INSERT sur une table cible à l'aide d'une table source dérivée

L'exemple suivant utilise l'instruction MERGE pour modifier la table `SalesReason` en mettant à jour ou en insérant des lignes dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Lorsque la valeur de `NewName` dans la table source correspond à une valeur de la colonne `Name` dans la table cible, (`SalesReason`), la colonne `ReasonType` est mise à jour dans la table cible. Lorsque la valeur de `NewName` ne correspond à aucune autre valeur, la ligne source est insérée dans la table cible. La table source est une table dérivée qui utilise le constructeur de valeurs de table [!INCLUDE[tsql](../../includes/tsql-md.md)] afin de spécifier plusieurs lignes pour la table source. Pour plus d’informations sur l’utilisation du constructeur de valeurs de table dans une table dérivée, consultez [Constructeur de valeurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md). L’exemple montre également comment stocker les résultats de la clause OUTPUT dans une variable de table. Puis, vous résumez les résultats de l’instruction MERGE en exécutant une opération de sélection simple qui retourne le nombre de lignes insérées et mises à jour.  
  
```sql  
-- Create a temporary table variable to hold the output actions.  
DECLARE @SummaryOfChanges TABLE(Change VARCHAR(20));  
  
MERGE INTO Sales.SalesReason AS Target  
USING (VALUES ('Recommendation','Other'), ('Review', 'Marketing'),
              ('Internet', 'Promotion'))  
       AS Source (NewName, NewReasonType)  
ON Target.Name = Source.NewName  
WHEN MATCHED THEN  
UPDATE SET ReasonType = Source.NewReasonType  
WHEN NOT MATCHED BY TARGET THEN  
INSERT (Name, ReasonType) VALUES (NewName, NewReasonType)  
OUTPUT $action INTO @SummaryOfChanges;  
  
-- Query the results of the table variable.  
SELECT Change, COUNT(*) AS CountPerChange  
FROM @SummaryOfChanges  
GROUP BY Change;  
```  
  
### <a name="d-inserting-the-results-of-the-merge-statement-into-another-table"></a>D. Insertion des résultats de l'instruction MERGE dans une autre table

L'exemple suivant capture les données retournées par la clause OUTPUT d'une instruction MERGE et insère ces données dans une autre table. L’instruction MERGE met à jour la colonne `Quantity` de la table `ProductInventory` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] selon les commandes traitées dans la table `SalesOrderDetail`. L'exemple capture les lignes mises à jour et les insère dans une autre table utilisée pour suivre les modifications de stock.  
  
```sql  
CREATE TABLE Production.UpdatedInventory  
    (ProductID INT NOT NULL, LocationID int, NewQty int, PreviousQty int,  
     CONSTRAINT PK_Inventory PRIMARY KEY CLUSTERED (ProductID, LocationID));  
GO  
INSERT INTO Production.UpdatedInventory  
SELECT ProductID, LocationID, NewQty, PreviousQty
FROM  
(    MERGE Production.ProductInventory AS pi  
     USING (SELECT ProductID, SUM(OrderQty)
            FROM Sales.SalesOrderDetail AS sod  
            JOIN Sales.SalesOrderHeader AS soh  
            ON sod.SalesOrderID = soh.SalesOrderID  
            AND soh.OrderDate BETWEEN '20030701' AND '20030731'  
            GROUP BY ProductID) AS src (ProductID, OrderQty)  
     ON pi.ProductID = src.ProductID  
    WHEN MATCHED AND pi.Quantity - src.OrderQty >= 0
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0
        THEN DELETE  
    OUTPUT $action, Inserted.ProductID, Inserted.LocationID,
        Inserted.Quantity AS NewQty, Deleted.Quantity AS PreviousQty)  
 AS Changes (Action, ProductID, LocationID, NewQty, PreviousQty)
 WHERE Action = 'UPDATE';  
GO  
```  

### <a name="e-using-merge-to-do-insert-or-update-on-a-target-edge-table-in-a-graph-database"></a>E. Utilisation de MERGE pour effectuer une opération INSERT ou UPDATE sur une table d’arêtes cible dans une base de données de graphes

Dans cet exemple, vous créez des tables de nœuds `Person` et `City` et une table d’arêtes `livesIn`. Vous utilisez l’instruction MERGE sur l’arête `livesIn` et insérez une nouvelle ligne si l’arête n’existe pas encore entre une `Person` et `City`. Si l’arête existe déjà, vous mettez simplement à jour l’attribut StreetAddress sur l’arête `livesIn`.

```sql
-- CREATE node and edge tables
CREATE TABLE Person
    (
        ID INTEGER PRIMARY KEY,
        PersonName VARCHAR(100)
    )
AS NODE
GO

CREATE TABLE City
    (
        ID INTEGER PRIMARY KEY,
        CityName VARCHAR(100),
        StateName VARCHAR(100)
    )
AS NODE
GO

CREATE TABLE livesIn
    (
        StreetAddress VARCHAR(100)
    )
AS EDGE
GO

-- INSERT some test data into node and edge tables
INSERT INTO Person VALUES (1, 'Ron'), (2, 'David'), (3, 'Nancy')
GO

INSERT INTO City VALUES (1, 'Redmond', 'Washington'), (2, 'Seattle', 'Washington')
GO

INSERT livesIn SELECT P.$node_id, C.$node_id, c
FROM Person P, City C, (values (1,1, '123 Avenue'), (2,2,'Main Street')) v(a,b,c)
WHERE P.id = a AND C.id = b
GO

-- Use MERGE to update/insert edge data
CREATE OR ALTER PROCEDURE mergeEdge
    @PersonId integer,
    @CityId integer,
    @StreetAddress varchar(100)
AS
BEGIN
    MERGE livesIn
        USING ((SELECT @PersonId, @CityId, @StreetAddress) AS T (PersonId, CityId, StreetAddress)
                JOIN Person ON T.PersonId = Person.ID
                JOIN City ON T.CityId = City.ID)
        ON MATCH (Person-(livesIn)->City)
    WHEN MATCHED THEN
        UPDATE SET StreetAddress = @StreetAddress
    WHEN NOT MATCHED THEN
        INSERT ($from_id, $to_id, StreetAddress)
        VALUES (Person.$node_id, City.$node_id, @StreetAddress) ;
END
GO

-- Following will insert a new edge in the livesIn edge table
EXEC mergeEdge 3, 2, '4444th Avenue'
GO

-- Following will update the StreetAddress on the edge that connects Ron to Redmond
EXEC mergeEdge 1, 1, '321 Avenue'
GO

-- Verify that all the address were added/updated correctly
SELECT PersonName, CityName, StreetAddress
FROM Person , City , livesIn
WHERE MATCH(Person-(livesIn)->city)
GO
```
  
## <a name="see-also"></a>Voir aussi

- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
- [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)
- [OUTPUT, clause &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)
- [MERGE dans les packages Integration Services](../../integration-services/control-flow/merge-in-integration-services-packages.md)
- [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)
- [Constructeur de valeurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md)  
