---
description: CREATE VIEW (Transact-SQL)
title: CREATE VIEW (Transact-SQL)
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1aeb0ce44bc48dbe208bad7ed58a94641c6e0ce9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237026"
---
# <a name="create-view-transact-sql"></a>CREATE VIEW (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crée une table virtuelle dont le contenu (colonnes et lignes) est défini par une requête. Utilisez cette instruction pour créer une vue des données dans une ou plusieurs tables de la base de données. Par exemple, une vue peut être utilisée aux fins suivantes :  
  
-   pour affiner, simplifier et personnaliser la perception de la base de données par chaque utilisateur ;  
  
-   comme mécanisme de sécurité en permettant aux utilisateurs d'accéder aux données par le biais de la vue, sans leur accorder d'autorisations qui leur permettraient d'accéder directement aux tables de base sous-jacentes de la vue ;  
  
-   pour fournir une interface à compatibilité descendante pour émuler une table dont le schéma a été modifié.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE [ OR ALTER ] VIEW [ schema_name . ] view_name [ (column [ ,...n ] ) ]
[ WITH <view_attribute> [ ,...n ] ]
AS select_statement
[ WITH CHECK OPTION ]
[ ; ]  
  
<view_attribute> ::=
{  
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]
}
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

OR ALTER  
 **S’applique à** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1).   
  
 Modifie, de manière conditionnelle, la vue uniquement si elle existe déjà. 
 
 *schema_name*  
 Nom du schéma auquel appartient la vue.  
  
 *view_name*  
 Nom de la vue. Les noms des vues doivent se conformer aux règles applicables aux identificateurs. Vous n'êtes pas tenu de spécifier le nom du propriétaire de la vue.  
  
 *column*  
 Nom à utiliser pour une colonne dans une vue. L'attribution d'un nom à une colonne n'est obligatoire que lorsqu'une colonne est dérivée d'une expression arithmétique, d'une fonction ou d'une constante, lorsque plusieurs colonnes risquent de porter le même nom (généralement à cause d'une jointure), ou encore lorsqu'une colonne d'une vue reçoit un nom différent de la colonne de laquelle elle est dérivée. Les noms des colonnes peuvent également être attribués dans l'instruction SELECT.  
  
 Si vous ne spécifiez pas le paramètre *column*, les colonnes de la vue prennent les mêmes noms que les colonnes de l’instruction SELECT.  
  
> [!NOTE]  
>  Dans les colonnes de la vue, les autorisations pour un nom de colonne s'appliquent d'un bout à l'autre d'une instruction CREATE VIEW ou ALTER VIEW, quelle que soit la source des données sous-jacentes. Par exemple, si des autorisations sont octroyées sur la colonne **SalesOrderID** dans une instruction CREATE VIEW, une instruction ALTER VIEW peut donner à la colonne **SalesOrderID** un autre nom, comme **OrderRef**, et disposer néanmoins des autorisations associées à la vue qui utilise **SalesOrderID**.  
  
 AS  
 Actions que la vue doit réaliser.  
  
 *select_statement*  
 Instruction SELECT qui définit la vue. Elle peut utiliser plusieurs tables et d'autres vues. Les autorisations nécessaires à la sélection à partir d'objets référencés dans la clause SELECT de la vue créée doivent être définies.  
  
 Une vue ne doit pas être un simple sous-ensemble de lignes et de colonnes d'une table particulière. Une vue peut être créée à l'aide de plusieurs tables ou d'autres vues avec une clause SELECT complexe.  
  
 Dans la définition d'une vue indexée, l'instruction SELECT doit être une instruction de table unique ou une jointure multitable avec une clause d'agrégation.  
  
 Les clauses SELECT faisant partie d'une définition de vue ne peuvent inclure les paramètres suivants :  
  
-   une clause ORDER BY, sauf si une clause TOP figure également dans la liste de sélection de l'instruction SELECT ;  
  
    > [!IMPORTANT]  
    >  La clause ORDER BY est utilisée uniquement pour déterminer les lignes retournées par la clause TOP ou OFFSET dans la définition de la vue. La clause ORDER BY ne garantit pas des résultats classés lorsque la vue est interrogée, sauf si ORDER BY est également spécifiée dans la requête proprement dite.  
  
-   le mot clé INTO ;  
  
-   la clause OPTION ;  
  
-   une référence à une table temporaire ou à une variable de type table.  
  
 Étant donné que *select_statement* utilise l’instruction SELECT, vous pouvez utiliser les indicateurs \<join_hint> et \<table_hint> comme spécifiés dans la clause FROM. Pour plus d’informations, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md) et [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md). 
  
 Il est possible d’utiliser des fonctions et plusieurs instructions SELECT séparées par UNION ou UNION ALL dans *select_statement*.  
  
 WITH CHECK OPTION.  
 Oblige toutes les instructions de modification de données exécutées sur la vue à respecter les critères définis dans *select_statement*. Lorsqu'une ligne est modifiée par l'intermédiaire d'une vue, WITH CHECK OPTION vérifie que les données resteront visibles dans la vue après validation de la modification.  
  
> [!NOTE]
>  CHECK OPTION s’applique uniquement aux mises à jour effectuées par l’intermédiaire de la vue. Elle ne s’applique pas aux mises à jour effectuées directement dans les tables sous-jacentes d’une vue.  
  
 ENCRYPTION  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Chiffre les entrées de [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md) qui contiennent le texte de l’instruction CREATE VIEW. L'utilisation de l'argument WITH ENCRYPTION évite la publication de la vue dans le cadre de la réplication SQL Server.  
  
 SCHEMABINDING  
 Lie la vue au schéma des tables sous-jacentes ou des autres tables. Si SCHEMABINDING est précisé, la table de base ou les tables ne peuvent alors pas être modifiées de façon à ne pas en affecter la définition de la vue. Cette dernière doit d'ailleurs être modifiée ou supprimée au préalable pour supprimer les dépendances par rapport à la table qui doit être modifiée. Quand vous utilisez l’argument SCHEMABINDING, *select_statement* doit comprendre les noms en deux parties (_schema_ **.** _object_) des tables, des vues ou des fonctions définies par l’utilisateur référencées. Tous ces objets référencés doivent se trouver dans la même base de données.  
  
 Les vues ou les tables impliquées dans une vue créée avec la clause SCHEMABINDING ne peuvent pas être supprimées, sauf si cette vue perd, à la suite de sa suppression ou de sa modification, la liaison au schéma. Dans le cas contraire, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur. De plus, l’exécution d’instructions ALTER TABLE sur des tables impliquées dans des vues qui sont liées au schéma échoue quand ces instructions affectent la définition des vues.  
  
 VIEW_METADATA  
 Indique que l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne aux interfaces de programmation d'applications (API) DB-Library, ODBC et OLE DB les informations de métadonnées sur la vue, plutôt que sur la ou les tables de base, lorsque des métadonnées en mode lecture sont sollicitées pour une requête qui fait référence à la vue. Les métadonnées en mode Parcourir correspondent à des métadonnées supplémentaires que l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne à ces API clientes. Ces métadonnées permettent aux API clientes d'implémenter des curseurs clients pouvant être mis à jour. Les métadonnées en mode Parcourir comprennent des informations sur la table de base à laquelle appartiennent les colonnes du jeu de résultats.  
  
 Dans le cas d'une vue créée par le biais de VIEW_METADATA, les métadonnées en mode Parcourir retournent le nom de la vue, et non celui de la table de base, lors de la description des colonnes de la vue comprise dans le jeu de résultats.  
  
 Quand une vue est créée à l’aide de WITH VIEW_METADATA, toutes ses colonnes, à l’exception d’une colonne **timestamp**, peuvent être mises à jour si la vue contient des déclencheurs INSTEAD OF INSERT ou INSTEAD OF UPDATE. Pour plus d'informations sur les vues pouvant être mises à jour, consultez les Notes ci-dessous.  
  
## <a name="remarks"></a>Notes  
 Vous ne pouvez créer des vues que dans la base de données actuelle. CREATE VIEW doit être la première instruction d'un traitement de requêtes. Une vue ne peut faire référence qu'à un maximum de 1 024 colonnes.  
  
 Lorsque vous effectuez une requête par l'intermédiaire d'une vue, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie que tous les objets de base de données référencés dans l'instruction existent, qu'ils sont bien valides dans le contexte de l'instruction et que les instructions de modification de données ne violent pas les règles d'intégrité des données. Si une vérification échoue, le système retourne un message d'erreur. Si la vérification réussit, l'action est transformée en une action applicable dans la ou les tables sous-jacentes.  
  
 Si une vue dépend d'une table ou d'une vue qui a été supprimée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] envoie un message d'erreur lors de toute tentative d'utilisation de la vue. Si une table ou une vue est créée pour remplacer celle qui a été supprimée et que la structure de la table n'est pas modifiée par rapport à la table de base précédente, la vue est de nouveau utilisable. Si la structure de la nouvelle table ou vue change, la vue doit être supprimée puis recréée.  
  
 Si aucune vue n’est créée avec la clause SCHEMABINDING, exécutez [sp_refreshview](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md) quand des modifications sont apportées aux objets sous-jacents de la vue qui affectent sa définition. Autrement, la vue risque de produire des résultats imprévisibles en cas d'interrogation.  
  
 Quand une vue est créée, les informations la concernant sont stockées dans les vues de catalogue suivantes : [sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md), [sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) et [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md). Le texte de l’instruction CREATE VIEW est stocké dans la vue de catalogue [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
 Le résultat d’une requête utilisant un index sur une vue définie à partir d’expressions **numeric** ou **float** peut être différent de celui d’une requête similaire n’utilisant pas l’index sur la vue. Cette différence peut être le résultat d'erreurs d'arrondi survenues au cours d'opérations INSERT, DELETE ou UPDATE sur des tables sous-jacentes.  
  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] enregistre les paramètres de SET QUOTED_IDENTIFIER et SET ANSI_NULLS lors de la création d'une vue. Ces paramètres d'origine servent à analyser la vue lorsque celle-ci est utilisée. Par conséquent, tout paramétrage d'une session client pour SET QUOTED_IDENTIFIER et SET ANSI_NULLS n'influe pas sur la définition de la vue lors de l'accès à la vue.  
  
## <a name="updatable-views"></a>Vues pouvant être mises à jour  
 Pour modifier les données d'une table sous-jacente, vous pouvez utiliser une vue sous réserve que les conditions suivantes soient vraies :  
  
-   Toute modification, y compris celles via les instructions UPDATE, INSERT et DELETE, doivent faire référence aux colonnes d'une seule et même table de base.  
  
-   Les colonnes étant modifiées dans la vue doivent faire référence directement aux données sous-jacentes se trouvant dans les colonnes des tables. Les colonnes ne peuvent être dérivées de quelque autre façon, telle que par :  
  
    -   Une fonction d’agrégation : AVG, COUNT, SUM, MIN, MAX, GROUPING, STDEV, STDEVP, VAR et VARP.  
  
    -   un calcul, car la colonne ne peut être calculée à partir d'une expression utilisant d'autres colonnes ; de même, les colonnes formées par le biais des opérateurs UNION, UNION ALL, CROSSJOIN, EXCEPT et INTERSECT équivalent à une somme de calculs et ne peuvent donc pas être mises à jour.  
  
-   Les colonnes en cours de modification ne sont pas affectées par l'utilisation des clauses GROUP BY, HAVING ou DISTINCT.  
  
-   La clause TOP n’est utilisée nulle part dans le paramètre *select_statement* de la vue avec la clause WITH CHECK OPTION.  
  
 Les restrictions précédentes s'appliquent à toutes les sous-requêtes de la clause FROM participant à créer la vue, tout comme elles s'appliquent aussi à la vue même. De façon générale, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit pouvoir suivre les modifications de façon claire, à partir de la définition de la vue vers une table de base. Pour plus d’informations, consultez [Modifier les données par l’intermédiaire d’une vue](../../relational-databases/views/modify-data-through-a-view.md).  
  
 Si les restrictions précédentes vous empêchent de modifier des données directement à travers une vue, voici quelques options à considérer pour vous aider :  
  
-   **Déclencheurs INSTEAD OF**  
  
     Les déclencheurs INSTEAD OF peuvent être créés sur une vue pour que celle-ci puisse être mise à jour. Le déclencheur INSTEAD OF est exécuté à la place de l'instruction de modification de données sur laquelle il est défini. Ce déclencheur permet à l'utilisateur de spécifier l'ensemble d'actions à exécuter pour traiter l'instruction de modification de données. Par conséquent, si une instruction précise de modification de données (INSERT, UPDATE ou DELETE) détient un déclencheur INSTEAD OF associé à une vue, celle-ci peut être mise à jour par le biais de cette instruction. Pour plus d’informations sur les déclencheurs INSTEAD OF, consultez [Déclencheurs DML](../../relational-databases/triggers/dml-triggers.md).  
  
-   **Vues partitionnées**  
  
     Si la vue est dite « partitionnée », elle peut être mise à jour sous certaines conditions. Au besoin, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fait la distinction entre une vue partitionnée locale d'une part, car cette vue ainsi que toutes les tables impliquées à sa création figurent sur le même serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et une vue partitionnée distribuée d'autre part, car au moins l'une des tables de la vue se trouve sur un serveur différent, voire distant.  
  
## <a name="partitioned-views"></a>Vues partitionnées  
 Une vue partitionnée est définie par une opération UNION ALL portant sur des tables membres structurées de façon identique, mais elle est stockée sous forme de plusieurs tables séparées que ce soit sur une seule instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans un groupe d'instances autonomes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], appelé « serveurs de bases de données fédérés ».  
  
> [!NOTE]  
>  Le partitionnement des données en local s'effectue de préférence par le biais de tables partitionnées. Pour plus d’informations, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 Lorsque vous concevez un schéma de partitionnement, il est indispensable d'identifier clairement les données appartenant à chaque partition. Par exemple, les données de la table `Customers` sont réparties entre trois tables membres, chacune sur un emplacement serveur distinct : `Customers_33` sur `Server1`, `Customers_66` sur `Server2` et `Customers_99` sur `Server3`.  
  
 Une vue partitionnée sur `Server1` est définie de la façon suivante :  
  
```sql
--Partitioned view as defined on Server1  
CREATE VIEW Customers  
AS  
--Select from local member table.  
SELECT *  
FROM CompanyData.dbo.Customers_33  
UNION ALL  
--Select from member table on Server2.  
SELECT *  
FROM Server2.CompanyData.dbo.Customers_66  
UNION ALL  
--Select from member table on Server3.  
SELECT *  
FROM Server3.CompanyData.dbo.Customers_99;  
```  
  
 En général, une vue est dite partitionnée si elle se présente sous la forme suivante :  
  
```syntaxsql
SELECT <select_list1>  
FROM T1  
UNION ALL  
SELECT <select_list2>  
FROM T2  
UNION ALL  
...  
SELECT <select_listn>  
FROM Tn;  
```  
  
## <a name="conditions-for-creating-partitioned-views"></a>Conditions de création des vues partitionnées  
  
1.  La `list` de sélection  
  
    -   Dans la liste des colonnes de la définition de la vue, sélectionnez toutes les colonnes dans les tables membres.  
  
    -   Vérifiez que les colonnes de position ordinale identique de chaque `select list` sont de même type, notamment en ce qui concerne les classements. Il n'est pas suffisant que les colonnes soient de type convertible de façon implicite, comme cela est généralement le cas pour UNION.  
  
         De plus, au moins une colonne (par exemple `<col>`) doit apparaître dans toutes les listes SELECT à la même position ordinale. Définissez `<col>` de façon à ce que les tables membres `T1, ..., Tn` aient des contraintes CHECK `C1, ..., Cn` définies pour `<col>`, respectivement.  
  
         La contrainte `C1` définie sur la table `T1` doit se présenter sous la forme suivante :  
  
        ```syntaxsql
        C1 ::= < simple_interval > [ OR < simple_interval > OR ...]  
        < simple_interval > :: =   
        < col > { < | > | \<= | >= | = < value >}   
        | < col > BETWEEN < value1 > AND < value2 >  
        | < col > IN ( value_list )  
        | < col > { > | >= } < value1 > AND  
        < col > { < | <= } < value2 >  
        ```
  
    -   La définition des contraintes doit permettre à toute valeur spécifiée de `<col>` de satisfaire au plus une des contraintes `C1, ..., Cn`, de telle sorte que les contraintes forment un ensemble d’intervalles disjoints ou sans chevauchement. La colonne `<col>` sur laquelle les contraintes disjointes sont définies est appelée colonne de partitionnement. Notez que la colonne de partitionnement peut porter différents noms dans les tables sous-jacentes. Les contraintes doivent être dans un état activé et approuvé pour répondre aux conditions précédemment mentionnées de la colonne de partitionnement. Si les contraintes sont désactivées, réactivez la vérification des contraintes à l’aide de l’option CHECK CONSTRAINT *constraint_name* d’ALTER TABLE, puis utilisez l’option WITH CHECK pour valider ces contraintes.  
  
         Voici des exemples d'ensembles valides de contraintes :  
  
        ```syntaxsql
        { [col < 10], [col between 11 and 20] , [col > 20] }  
        { [col between 11 and 20], [col between 21 and 30], [col between 31 and 100] }  
        ```  
  
    -   La même colonne ne peut être utilisée qu'une seule fois dans la liste de sélection.  
  
2.  Colonne de partitionnement  
  
    -   La colonne de partitionnement fait partie de la clé primaire de la table.  
  
    -   Il ne peut pas s’agir d’une colonne calculée, d’une colonne d’identité, d’une colonne par défaut ou d’une colonne de type **timestamp**.  
  
    -   Si une colonne d'une table membre comporte plusieurs contraintes, le moteur de bases de données ignore toutes les contraintes et n'en tient pas compte pour déterminer si la vue est ou non une vue partitionnée. Pour que les conditions associées à la vue partitionnée soient remplies, vérifiez qu’il n’y a qu’une seule contrainte de partitionnement sur la colonne de partitionnement.  
  
    -   Aucune restriction ne s'applique sur la possibilité de mettre à jour la colonne de partitionnement.  
  
3.  Tables membres ou tables sous-jacentes `T1, ..., Tn`  
  
    -   Les tables peuvent être des tables locales ou des tables provenant d'autres serveurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] référencées par le biais d'un nom à quatre composantes ou d'un nom basé sur OPENDATASOURCE ou OPENROWSET. La syntaxe OPENDATASOURCE et OPENROWSET permet de spécifier le nom d'une table, mais pas une requête directe. Pour plus d’informations, consultez [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md) et [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
         Si une ou plusieurs tables membres sont distantes, la vue est appelée vue partitionnée de données distribuées, et des conditions supplémentaires s'appliquent. Ces conditions sont présentées plus loin dans cette section.  
  
    -   Une table ne peut apparaître qu'une fois dans l'ensemble de tables combinées avec l'instruction UNION ALL.  
  
    -   Dans les tables membres, il est impossible de créer des index sur les colonnes calculées de la table.  
  
    -   Toutes les contraintes PRIMARY KEY des tables membres doivent être appliquées sur un nombre identique de colonnes.  
  
    -   Toutes les tables membres de la vue doivent avoir le même paramètre de remplissage ANSI. Ce dernier peut être défini à l’aide de l’option **user options** qui se trouve dans **sp_configure** ou de l’instruction SET.  
  
## <a name="conditions-for-modifying-data-in-partitioned-views"></a>Conditions de modification des données dans les vues partitionnées  
 Les restrictions suivantes s'appliquent aux instructions qui modifient les données dans les vues partitionnées :  
  
-   L’instruction INSERT fournit des valeurs pour toutes les colonnes de la vue, même si les tables membres sous-jacentes ont une contrainte DEFAULT pour ces colonnes ou si elles acceptent des valeurs NULL. Pour les colonnes de tables membres dont la contrainte porte sur la valeur DEFAULT, les instructions ne peuvent pas spécifier la valeur NULL ou utiliser le mot clé DEFAULT de manière explicite.  
  
-   La valeur insérée dans la colonne de partitionnement respecte au moins une des contraintes sous-jacentes ; sinon, l’action d’insertion échoue à la suite d’une violation de contrainte.  
  
-   Les instructions UPDATE ne peuvent pas définir le mot clé DEFAULT comme valeur dans la clause SET même si une valeur DEFAULT est définie pour la colonne dans la table membre correspondante.  
  
-   Les colonnes de la vue qui sont des colonnes d'identité dans une ou plusieurs tables membres ne peuvent pas être mises à jour par le biais d'une instruction INSERT ou UPDATE.  
  
-   Si l’une des tables membres contient une colonne **timestamp**, les données ne peuvent pas être modifiées par le biais d’une instruction INSERT ou UPDATE.  
  
-   Si l'une des tables membres contient un déclencheur ou une contrainte ON UPDATE CASCADE/SET NULL/SET DEFAULT ou encore une contrainte ON DELETE CASCADE/SET NULL/SET DEFAULT, la vue ne peut alors pas être modifiée.  
  
-   Les actions INSERT UPDATE et DELETE sur une vue partitionnée ne sont pas autorisées s'il existe une jointure réflexive sur la vue ou sur une des tables membres indiquées dans l'instruction.  
  
-   L’importation en bloc de données dans une vue partitionnée n’est pas prise en charge par **bcp** ou par les instructions BULK INSERT et INSERT ... SELECT * FROM OPENROWSET(BULK...). Toutefois, vous pouvez insérer plusieurs lignes dans une vue partitionnée en utilisant l’instruction [INSERT](../../t-sql/statements/insert-transact-sql.md).  
  
    > [!NOTE]  
    >  Pour mettre à jour une vue partitionnée, l'utilisateur doit bénéficier des autorisations nécessaires pour effectuer les instructions INSERT, DELETE et UPDATE sur les tables membres.  
  
## <a name="additional-conditions-for-distributed-partitioned-views"></a>Conditions supplémentaires relatives aux vues partitionnées distribuées  
 Dans le cas des vues partitionnées distribuées, qui impliquent une ou plusieurs tables membres distantes, les conditions supplémentaires suivantes s'appliquent :  
  
-   Une transaction distribuée est démarrée pour garantir l’atomicité sur tous les nœuds affectés par la mise à jour.  
  
-   Définissez l’option XACT_ABORT SET sur ON pour que les instructions INSERT, UPDATE ou DELETE fonctionnent.  
  
-   Toutes les colonnes de tables distantes de type **smallmoney** qui sont référencées dans une vue partitionnée sont mappées en **money**. Par conséquent, les colonnes correspondantes dans les tables locales (de position ordinale identique dans la liste de sélection) doivent également être de type **money**.  
  
-   Quand le niveau de compatibilité de la base de données est 110 et supérieur, toutes les colonnes des tables distantes de type **smalldatetime** qui sont référencées dans une vue partitionnée sont mappées en **smalldatetime**. Les colonnes correspondantes dans les tables locales (de position ordinale identique dans la liste de sélection) doivent être de type **smalldatetime**. Il s’agit d’un changement de comportement par rapport aux versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans lequel toutes les colonnes des tables distantes de type **smalldatetime** référencées dans une vue partitionnée sont mappées en **datetime** et les colonnes correspondantes dans les tables locales doivent être de type **datetime**. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
-   Tout serveur lié dans une vue partitionnée ne peut pas être un serveur dont la liaison constitue un bouclage. En d'autres termes, un serveur lié ne peut pas pointer vers la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La valeur de l'option SET ROWCOUNT est ignorée pour les actions INSERT, UPDATE et DELETE impliquant des vues partitionnées pouvant être mises à jour et des tables distantes.  
  
 Lorsque les tables membres et la définition de la vue partitionnée sont implémentées, l'optimiseur de requête de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des plans intelligents qui utilisent efficacement les requêtes pour accéder aux données des tables membres. Grâce aux définitions de la contrainte CHECK, le processeur de requêtes effectue la répartition des valeurs de clé parmi les tables membres. Lorsqu'un utilisateur émet une requête, le processeur de requêtes compare le plan des références aux valeurs spécifiées dans la clause WHERE et crée un plan d'exécution impliquant un transfert de données minimal entre les serveurs membres. Par conséquent, bien que certaines tables membres puissent se trouver sur des serveurs distants, l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] résout les requêtes distribuées de telle sorte que la quantité de données distribuées à transférer soit minime.  
  
## <a name="considerations-for-replication"></a>À prendre en considération lors des réplications  
 Pour créer des vues partitionnées sur des tables membres entrant en jeu dans une réplication, les points suivants sont à prendre en considération :  
  
-   Si les tables sous-jacentes sont impliquées dans une réplication de fusion ou transactionnelle avec mise à jour des abonnements, vérifiez que la colonne **uniqueidentifier** est également incluse dans la liste de sélection. 
  
     Toute action INSERT dans la vue partitionnée doit fournir une valeur NEWID() pour la colonne **uniqueidentifier**. Toutes les actions UPDATE portant sur la colonne **uniqueidentifier** doivent fournir NEWID() comme valeur, car le mot clé DEFAULT ne peut pas être utilisé.  
  
-   La réplication de mises à jour opérées par le biais de la vue revient à répliquer des tables tirées de deux bases de données différentes : les tables sont servies par différents agents de réplication ; l'ordre des mises à jour ne peut ainsi être garanti.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CREATE VIEW dans la base de données et l'autorisation ALTER sur le schéma dans lequel la vue est créée.  
  
## <a name="examples"></a>Exemples  

Les exemples suivants utilisent la base de données AdventureWorks 2012 ou AdventureWorksDW.  

### <a name="a-using-a-simple-create-view"></a>R. Utilisation d'une instruction CREATE VIEW simple  
 L'exemple suivant crée une vue par le biais d'une instruction `SELECT` simple. Une vue simple est utile lorsque vous interrogez régulièrement une combinaison de colonnes. Les données de cette vue sont tirées des tables `HumanResources.Employee` et `Person.Person` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Ces données fournissent les noms et les informations relatives à la date d'embauche des employés de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)]. La vue doit pouvoir être créée par la personne chargée de suivre les dates anniversaires d'embauche mais sans pour autant l'autoriser à accéder à toutes les données de ces tables.  
  
```sql
CREATE VIEW hiredate_view  
AS   
SELECT p.FirstName, p.LastName, e.BusinessEntityID, e.HireDate  
FROM HumanResources.Employee e   
JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
  
```  
  
### <a name="b-using-with-encryption"></a>B. Utilisation de WITH ENCRYPTION  
 Cet exemple utilise l'option `WITH ENCRYPTION` et montre les colonnes calculées, les colonnes renommées et les colonnes multiples.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql
CREATE VIEW Purchasing.PurchaseOrderReject  
WITH ENCRYPTION  
AS  
SELECT PurchaseOrderID, ReceivedQty, RejectedQty,   
    RejectedQty / ReceivedQty AS RejectRatio, DueDate  
FROM Purchasing.PurchaseOrderDetail  
WHERE RejectedQty / ReceivedQty > 0  
AND DueDate > CONVERT(DATETIME,'20010630',101) ;  
GO  
  
```  
  
### <a name="c-using-with-check-option"></a>C. Utilisation de WITH CHECK OPTION  
 Cet exemple montre une vue appelée `SeattleOnly` se reportant à cinq tables et n'autorisant des modifications de données que pour les employés vivant à Seattle.  
  
```sql
CREATE VIEW dbo.SeattleOnly  
AS  
SELECT p.LastName, p.FirstName, e.JobTitle, a.City, sp.StateProvinceCode  
FROM HumanResources.Employee e  
INNER JOIN Person.Person p  
ON p.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN Person.BusinessEntityAddress bea   
    ON bea.BusinessEntityID = e.BusinessEntityID   
    INNER JOIN Person.Address a   
    ON a.AddressID = bea.AddressID  
    INNER JOIN Person.StateProvince sp   
    ON sp.StateProvinceID = a.StateProvinceID  
WHERE a.City = 'Seattle'  
WITH CHECK OPTION ;  
GO  
```  
  
### <a name="d-using-built-in-functions-within-a-view"></a>D. Utilisation de fonctions intégrées dans une vue  
 L'exemple suivant illustre la définition d'une vue qui inclut une fonction intégrée. Si vous utilisez des fonctions, vous devez attribuer un nom à la colonne dérivée.  
  
```sql
CREATE VIEW Sales.SalesPersonPerform  
AS  
SELECT TOP (100) SalesPersonID, SUM(TotalDue) AS TotalSales  
FROM Sales.SalesOrderHeader  
WHERE OrderDate > CONVERT(DATETIME,'20001231',101)  
GROUP BY SalesPersonID;  
GO  

```  
  
### <a name="e-using-partitioned-data"></a>E. Utilisation de données partitionnées  
 L'exemple suivant s'appuie sur les tables nommées `SUPPLY1`, `SUPPLY2`, `SUPPLY3` et `SUPPLY4`. Ces tables correspondent aux tables des fournisseurs de quatre sièges situées dans des pays/régions distincts.  
  
```sql
--Create the tables and insert the values.  
CREATE TABLE dbo.SUPPLY1 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 1 and 150),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY2 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 151 and 300),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY3 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 301 and 450),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY4 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 451 and 600),  
supplier CHAR(50)  
);  
GO  
--Create the view that combines all supplier tables.  
CREATE VIEW dbo.all_supplier_view  
WITH SCHEMABINDING  
AS  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY1  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY2  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY3  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY4;  
GO
INSERT dbo.all_supplier_view VALUES ('1', 'CaliforniaCorp'), ('5', 'BraziliaLtd')    
, ('231', 'FarEast'), ('280', 'NZ')  
, ('321', 'EuroGroup'), ('442', 'UKArchip')  
, ('475', 'India'), ('521', 'Afrique');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-creating-a-simple-view"></a>F. Création d’un vue simple  
 L’exemple suivant crée une vue en sélectionnant uniquement certaines des colonnes de la table source.  
  
```sql
CREATE VIEW DimEmployeeBirthDates AS  
SELECT FirstName, LastName, BirthDate   
FROM DimEmployee;  
```  
  
### <a name="g-create-a-view-by-joining-two-tables"></a>G. Créer une vue en joignant deux tables  
 L’exemple suivant crée une vue à l’aide d’une instruction `SELECT` avec un `OUTER JOIN`. Les résultats de la requête de jointure s’affichent dans la vue.  
  
```sql
CREATE VIEW view1  
AS 
SELECT fis.CustomerKey, fis.ProductKey, fis.OrderDateKey, 
  fis.SalesTerritoryKey, dst.SalesTerritoryRegion  
FROM FactInternetSales AS fis   
LEFT OUTER JOIN DimSalesTerritory AS dst   
ON (fis.SalesTerritoryKey=dst.SalesTerritoryKey);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_refreshview &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sys.views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

