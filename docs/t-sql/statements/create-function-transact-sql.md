---
description: CREATE FUNCTION (Transact-SQL)
title: CREATE FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FUNCTION
- CREATE FUNCTION
- CREATE_FUNCTION_TSQL
- FUNCTION_TSQL
- TVF
- MSTVF
dev_langs:
- TSQL
helpviewer_keywords:
- invoking functions
- extended stored procedures [SQL Server], functions
- table-valued parameters
- user-defined functions [SQL Server], creating
- CLR functions [SQL Server]
- CREATE FUNCTION statement
- nondeterministic functions
- user-defined data types
- functions [SQL Server], creating
- inline table-valued functions [SQL Server]
- multi-statement table-valued functions [SQL Server]
- table-valued functions [SQL Server], CREATE FUNCTION
- parameters [SQL Server], functions
- nesting user-defined functions
- deterministic functions
- scalar-valued functions
- scalar UDF
- MSTVF
- TVF
- functions [SQL Server], invoking
ms.assetid: 864b393f-225f-4895-8c8d-4db59ea60032
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 717ffe936bef35c9794e160e56c7ea7b0658058f
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170701"
---
# <a name="create-function-transact-sql"></a>CREATE FUNCTION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crée une fonction définie par l'utilisateur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Une fonction définie par l'utilisateur est une routine [!INCLUDE[tsql](../../includes/tsql-md.md)] ou CLR (Common Language Runtime) qui accepte des paramètres, exécute une action, par exemple un calcul complexe, et retourne le résultat de cette action sous forme de valeur. La valeur retournée peut être une valeur scalaire (unique) ou une table. Utilisez cette instruction pour créer une routine réutilisable, exploitable :

- dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] telles que SELECT ;

- dans des applications appelant la fonction ;

- dans la définition d'une autre fonction définie par l'utilisateur ;

- pour paramétrer une vue ou améliorer la fonctionnalité d'une vue indexée ;

- pour définir une colonne dans une table ;

- pour définir une contrainte CHECK sur une colonne ;

- pour remplacer une procédure stockée.

- Utiliser une fonction inline comme prédicat de filtre pour une stratégie de sécurité

> [!NOTE]
>
> - L’intégration du CLR .NET Framework à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est décrite dans cette rubrique. L’intégration du CLR ne s’applique pas à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
> - Pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], consultez [CREATE FUNCTION (Azure Synapse Analytics)](../../t-sql/statements/create-function-sql-data-warehouse.md).

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Transact-SQL Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
 [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    BEGIN
        function_body
        RETURN scalar_expression
    END
[ ; ]
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

```syntaxsql
-- Transact-SQL Multi-Statement Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [READONLY] }
    [ ,...n ]
  ]
)
RETURNS @return_variable TABLE <table_type_definition>
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    BEGIN
        function_body
        RETURN
    END
[ ; ]

```

```syntaxsql
-- Transact-SQL Function Clauses
<function_option>::=
{
    [ ENCRYPTION ]
  | [ SCHEMABINDING ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
  | [ EXECUTE_AS_Clause ]
  | [ INLINE = { ON | OFF }]
}

<table_type_definition>:: =
( { <column_definition> <column_constraint>
  | <computed_column_definition> }
    [ <table_constraint> ] [ ,...n ]
)
<column_definition>::=
{
    { column_name data_type }
    [ [ DEFAULT constant_expression ]
      [ COLLATE collation_name ] | [ ROWGUIDCOL ]
    ]
    | [ IDENTITY [ (seed , increment ) ] ]
    [ <column_constraint> [ ...n ] ]
}

<column_constraint>::=
{
    [ NULL | NOT NULL ]
    { PRIMARY KEY | UNIQUE }
      [ CLUSTERED | NONCLUSTERED ]
      [ WITH FILLFACTOR = fillfactor
        | WITH ( < index_option > [ , ...n ] )
      [ ON { filegroup | "default" } ]
  | [ CHECK ( logical_expression ) ] [ ,...n ]
}

<computed_column_definition>::=
column_name AS computed_column_expression

<table_constraint>::=
{
    { PRIMARY KEY | UNIQUE }
      [ CLUSTERED | NONCLUSTERED ]
      ( column_name [ ASC | DESC ] [ ,...n ]
        [ WITH FILLFACTOR = fillfactor
        | WITH ( <index_option> [ , ...n ] )
  | [ CHECK ( logical_expression ) ] [ ,...n ]
}

<index_option>::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS ={ ON | OFF }
}
```

```syntaxsql
-- CLR Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS { return_data_type }
    [ WITH <clr_function_option> [ ,...n ] ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS TABLE <clr_table_type_definition>
    [ WITH <clr_function_option> [ ,...n ] ]
    [ ORDER ( <order_clause> ) ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Function Clauses
<order_clause> ::=
{
   <column_name_in_clr_table_type_definition>
   [ ASC | DESC ]
} [ ,...n]

<method_specifier>::=
    assembly_name.class_name.method_name

<clr_function_option>::=
}
    [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
  | [ EXECUTE_AS_Clause ]
}

<clr_table_type_definition>::=
( { column_name data_type } [ ,...n ] )
```

```syntaxsql
-- In-Memory OLTP: Syntax for natively compiled, scalar user-defined function
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
 ( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
    [ NULL | NOT NULL ] [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
     WITH <function_option> [ ,...n ]
    [ AS ]
    BEGIN ATOMIC WITH (set_option [ ,... n ])
        function_body
        RETURN scalar_expression
    END

<function_option>::=
{
  |  NATIVE_COMPILATION
  |  SCHEMABINDING
  | [ EXECUTE_AS_Clause ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
}

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*OR ALTER*
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Modifie, de manière conditionnelle, la fonction uniquement si elle existe déjà.

> [!NOTE]
> La syntaxe [OR ALTER] facultative pour le CLR est disponible depuis [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU1.

*schema_name* Nom du schéma auquel appartient la fonction définie par l’utilisateur.

*function_name* Nom de la fonction définie par l’utilisateur. Les noms de fonctions doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md) et doivent être uniques dans la base de données et pour son schéma.

> [!NOTE]
> Les parenthèses sont requises après le nom de fonction même si aucun paramètre n'est spécifié.

@*parameter_name* Paramètre dans la fonction définie par l’utilisateur. Un ou plusieurs paramètres peuvent être déclarés.

Une fonction peut comprendre au maximum 2 100 paramètres. La valeur de chaque paramètre déclaré doit être fournie par l'utilisateur lors de l'exécution de la fonction, sauf si vous définissez une valeur par défaut pour le paramètre.

Spécifiez un nom de paramètre en plaçant le signe @ comme premier caractère. Le nom de paramètre doit suivre les règles applicables aux identificateurs. Un paramètre étant local à une fonction, vous pouvez utiliser le même nom dans d'autres fonctions. Les paramètres ne peuvent que prendre la place de constantes ; ils ne peuvent pas être utilisés à la place de noms de tables, de colonnes ou d'autres objets de base de données.

> [!NOTE]
> ANSI_WARNINGS n'est pas honoré lorsque vous transmettez des paramètres dans une procédure stockée, dans une fonction définie par l'utilisateur ou lorsque vous déclarez et définissez des variables dans une instruction par lot. Par exemple, si une variable est définie comme **char(3)** et qu’une valeur de plus de trois caractères lui est attribuée, les données sont tronquées en fonction de la taille définie, et l’instruction `INSERT` ou `UPDATE` réussit.

[ *type_schema_name*. ] *parameter_data_type* Type de données du paramètre et, éventuellement, schéma auquel il appartient. Dans le cas des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)], tous les types de données, notamment les types CLR définis par l’utilisateur et les types de tables définis par l’utilisateur, sont autorisés à l’exception du type de données **timestamp**. Dans le cas des fonctions CLR, tous les types de données, notamment les types CLR définis par l’utilisateur, sont autorisés à l’exception des types de tables définis par l’utilisateur **text**, **ntext** et **image**, et des types de données **timestamp**. Les types non scalaires **cursor** et **table** ne peuvent pas être spécifiés comme type de données de paramètre dans les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou CLR.

Si *type_schema_name* n’est pas spécifié, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] recherche *scalar_parameter_data_type* dans l’ordre suivant :

- Schéma qui contient les noms des types de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Le schéma par défaut de l'utilisateur actuel dans la base de données active
- Schéma **dbo** dans la base de données active.

[ =*default* ] Valeur par défaut pour le paramètre. Si une valeur *default* est définie, la fonction peut être exécutée sans spécifier de valeur pour ce paramètre.

> [!NOTE]
> Des valeurs de paramètre par défaut peuvent être spécifiées pour les fonctions CLR, à l’exception des types de données **varchar(max)** et **varbinary(max)** .

Lorsque l'un des paramètres de la fonction possède une valeur par défaut, le mot clé DEFAULT doit être spécifié lors de l'appel de la fonction afin de récupérer la valeur par défaut. Ce comportement est différent de l'utilisation de paramètres avec des valeurs par défaut dans des procédures stockées pour lesquelles l'omission du paramètre implique également la prise en compte de la valeur par défaut. Toutefois, le mot clé DEFAULT n'est pas requis lors de l'appel d'une fonction scalaire à l'aide de l'instruction EXECUTE.

READONLY Indique que le paramètre ne peut pas être mis à jour ni modifié dans la définition de la fonction. READONLY est obligatoire pour les paramètres de type table définis par l’utilisateur et il ne peut être utilisé pour aucun autre type de paramètre.

*return_data_type* Valeur de retour d’une fonction scalaire définie par l’utilisateur. Dans le cas des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)], tous les types de données, notamment les types CLR définis par l’utilisateur, sont autorisés, à l’exception du type de données **timestamp**. Dans le cas des fonctions CLR, tous les types de données, notamment les types CLR définis par l’utilisateur, sont autorisés, à l’exception des types de données **text**, **ntext**, **image** et **timestamp**. Les types non scalaires **cursor** et **table** ne peuvent pas être spécifiés comme type de données de retour dans les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou CLR.

*function_body* Spécifie qu’une série d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui ne produisent pas ensemble un effet secondaire, tel que la modification d’une table, définissent la valeur de la fonction. *function_body* est utilisé seulement dans les fonctions scalaires et dans les fonctions table à instructions multiples.

Dans les fonctions scalaires, *function_body* est une série d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui, ensemble, prennent une valeur scalaire.

Dans les fonctions table à instructions multiples, *function_body* est une série d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui remplissent une variable de retour TABLE.

*scalar_expression* Indique la valeur scalaire retournée par la fonction scalaire.

TABLE Indique que la valeur de retour de la fonction table est une table. Seules des constantes et des @*local_variables* peuvent être passés aux fonctions table.

Dans les fonctions table en ligne, la valeur de retour TABLE est définie via une instruction SELECT unique. Aucune variable retournée n'est associée à une fonction en ligne.

<a name="mstvf"></a> Dans les fonctions table à instructions multiples, \@*return_variable* est une variable TABLE servant à stocker et à cumuler les lignes à retourner comme valeur de la fonction. \@*return_variable* peut être spécifié uniquement pour les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)], et pas pour les fonctions CLR.

*select_stmt* Instruction SELECT unique qui définit la valeur de retour d’une fonction table incluse.

ORDER (\<order_clause>) spécifie l’ordre dans lequel les résultats sont retournés à partir de la fonction table. Pour plus d’informations, consultez la section « [Utilisation de l'ordre de tri dans les fonctions table CLR](#using-sort-order-in-clr-table-valued-functions) » plus loin dans cette rubrique.

EXTERNAL NAME \<method_specifier> *nom_assembly*.*nom_classe*.*nom_méthode*
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 et versions ultérieures)

Spécifie l'assembly et la méthode auxquels doit se référer le nom de la fonction créée.

- *assembly_name* : doit correspondre à une valeur de la colonne `name` de `SELECT * FROM sys.assemblies;`.

  Il s'agit du nom qui a été utilisé dans l'instruction `CREATE ASSEMBLY`.

- *class_name* : doit correspondre à une valeur de la colonne `assembly_name` de `SELECT * FROM sys.assembly_modules;`.

  La valeur contient souvent un point incorporé. Dans ce cas, la syntaxe Transact-SQL exige que la valeur soit délimitée par une paire de crochets droits [] ou par une paire de guillemets doubles "".

- *method_name* : doit correspondre à une valeur de la colonne `method_name` de `SELECT * FROM sys.assembly_modules;`.

  La méthode doit être statique.

Dans un exemple classique, pour MyFood.DLL, dans lequel tous les types sont dans l’espace de noms MyFood, la valeur `EXTERNAL NAME` peut être : `MyFood.[MyFood.MyClass].MyStaticMethod`

> [!NOTE]
>
> - Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas exécuter du code CLR. Vous pouvez créer, modifier et supprimer des objets d’une base de données qui font référence à des modules CLR (Common Language Runtime) ; cependant, vous ne pouvez pas exécuter ces références dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tant que vous n’avez pas activé l’[option CLR enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Pour activer cette option, utilisez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).
> - Cette option n'est pas disponible dans une base de données autonome.

*\<*table_type_definition*>* ( { \<column_definition> \<column_constraint>| \<computed_column_definition> } [ \<table_constraint> ] [ ,...*n* ] ) définit le type de données de table pour une fonction [!INCLUDE[tsql](../../includes/tsql-md.md)]. La déclaration de table comprend des définitions de colonne et des contraintes de colonne ou de table. La table est toujours placée dans le groupe de fichiers primaire.

*\< clr_table_type_definition >* ( { *nom_colonne**type_données* } [ ,...*n* ] ) **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ([préversion dans certaines régions](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)).

Définit les types de données de table pour une fonction CLR. La déclaration de table ne comprend que des types de données et des noms de colonne. La table est toujours placée dans le groupe de fichiers primaire.

NULL|NOT NULL Pris en charge uniquement pour les fonctions scalaires définies par l’utilisateur et compilées en mode natif. Pour plus d’informations, consultez [Fonctions scalaires définies par l’utilisateur pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).

NATIVE_COMPILATION Indique si une fonction définie par l’utilisateur est compilée en mode natif. Cet argument est obligatoire pour les fonctions scalaires définies par l’utilisateur et compilées en mode natif.

BEGIN ATOMIC WITH Pris en charge uniquement pour les fonctions scalaires définies par l’utilisateur et compilées en mode natif, et est obligatoire. Pour plus d’informations, consultez [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).

SCHEMABINDING L’argument SCHEMABINDING est obligatoire pour les fonctions scalaires définies par l’utilisateur et compilées en mode natif.

EXECUTE AS EXECUTE AS est obligatoire pour les fonctions scalaires définies par l’utilisateur et compilées en mode natif.

**\<function_option>::= et \<clr_function_option>::=**

Spécifie que la fonction aura une ou plusieurs des options ci-dessous.

ENCRYPTION **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 et versions ultérieures)

Indique que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] se charge de convertir le texte d'origine provenant de l'instruction CREATE FUNCTION dans un format obscurci. La sortie générée par l'obfuscation n'est pas visible directement dans les affichages catalogue. Les utilisateurs n'ayant pas accès aux tables système ou aux fichiers de base de données ne peuvent pas récupérer le texte d'obfuscation. Le texte est cependant à la disposition des utilisateurs disposant de privilèges et qui peuvent accéder aux tables système par le biais du [port DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) ou qui accèdent directement aux fichiers de bases de données. Les utilisateurs qui peuvent associer un débogueur au processus serveur peuvent également récupérer la procédure d'origine de la mémoire au moment de l'exécution. Pour plus d’informations sur l’accès aux métadonnées système, consultez [Configuration de la visibilité des métadonnées](../../relational-databases/security/metadata-visibility-configuration.md).

L'utilisation cette option empêche la publication de la fonction dans le cadre de la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette option ne peut pas être spécifiée pour les fonctions CLR.

SCHEMABINDING Indique que la fonction est liée aux objets de base de données auxquels elle fait référence. Si SCHEMABINDING est précisé, les objets de base ne peuvent pas être modifiés d'une manière susceptible d'affecter la définition de la fonction. Cette dernière doit d'ailleurs être modifiée ou supprimée au préalable pour supprimer les dépendances par rapport à l'objet qui doit être modifié.

La liaison de la fonction aux objets auxquels elle fait référence est supprimée uniquement quand l’une des actions suivantes se produit :

- La fonction est supprimée.
- La fonction est modifiée avec l’instruction `ALTER`, sans spécification de l’option `SCHEMABINDING`.

Une fonction peut être liée au schéma uniquement si les conditions suivantes sont vérifiées :

- La fonction est une fonction [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Les fonctions utilisateur et vues référencées par la fonction sont également liées au schéma.
- La fonction fait référence aux objets à partir d'un nom en deux parties.
- La fonction et les objets auxquels elle fait référence appartiennent à la même base de données.
- L’utilisateur qui a exécuté l’instruction `CREATE FUNCTION` dispose de l’autorisation `REFERENCES` sur les objets de base de données référencés par la fonction.

RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT** Spécifie l’attribut **OnNULLCall** d’une fonction scalaire. S'il n'est pas spécifié, l'argument CALLED ON NULL INPUT est implicite par défaut. Cela signifie que le corps de la fonction est exécuté même si la valeur NULL est transmise comme argument.

Si l'argument RETURNS NULL ON NULL INPUT est spécifié dans une fonction CLR, il indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut retourner la valeur NULL lorsque n'importe lequel des arguments qu'il reçoit a la valeur NULL, sans réellement appeler le corps de la fonction. Si la méthode d’une fonction CLR spécifiée dans \<method_specifier> possède déjà un attribut personnalisé qui indique RETURNS NULL ON NULL INPUT, mais que l’instruction CREATE FUNCTION indique CALLED ON NULL INPUT, l’instruction CREATE FUNCTION est prioritaire. L’attribut **OnNULLCall** ne peut pas être spécifié pour les fonctions table CLR.

Clause EXECUTE AS Spécifie le contexte de sécurité dans lequel la fonction définie par l’utilisateur est exécutée. Par conséquent, vous pouvez contrôler le compte d’utilisateur que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise pour valider les autorisations sur tous les objets de base de données référencés par la fonction.

> [!NOTE]
> `EXECUTE AS` ne peut pas être spécifié pour les fonctions table inline.

Pour plus d’informations, consultez [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).

INLINE = { ON | OFF } **S’APPLIQUE À** : SQL Server 2019 et versions ultérieures.

Spécifie si cette fonction UDF scalaire doit être inline ou non. Cette clause s’applique uniquement aux fonctions scalaires définies par l’utilisateur. La clause `INLINE` n’est pas obligatoire. Si la clause `INLINE` n’est pas spécifiée, elle est automatiquement définie sur ON/OFF selon que la fonction UDF peut ou non être inline. Si `INLINE=ON` est spécifié alors que la fonction UDF ne peut pas être inline, une erreur est levée. Pour plus d’informations, consultez [Incorporation des fonctions UDF scalaires](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).

**\< column_definition >::=**

Définit le type de données de la table. La déclaration de table comprend des contraintes et des définitions de colonne. Pour les fonctions CLR, seuls *column_name* et *data_type* peuvent être spécifiés.

*column_name* Nom d’une colonne de la table. Les noms de colonnes doivent respecter les règles des identificateurs et doivent être uniques au sein de la table. *column_name* peut être composé de 1 à 128 caractères.

*data_type* Indique le type de données de la colonne. Dans le cas des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)], tous les types de données, notamment les types CLR définis par l’utilisateur, sont autorisés, à l’exception du type de données **timestamp**. Dans le cas des fonctions CLR, tous les types de données, notamment les types CLR définis par l’utilisateur, sont autorisés, à l’exception des types de données **text**, **ntext**, **image**, **char**, **varchar**, **varchar(max)** et **timestamp**. Le type non scalaire **cursor** ne peut pas être spécifié comme type de données de colonne dans les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou CLR.

DEFAULT *constant_expression* Spécifie la valeur fournie pour la colonne lorsque vous n’avez pas indiqué explicitement de valeur lors d’une insertion. *constant_expression* est une constante, NULL ou une valeur de fonction système. Les définitions DEFAULT peuvent être appliquées à n'importe quelle colonne, sauf à celles qui possèdent la propriété IDENTITY. L'argument DEFAULT ne peut pas être spécifié pour les fonctions table CLR.

COLLATE *collation_name* spécifie le classement de la colonne. Si l'argument n'est pas spécifié, c'est le classement par défaut de la base de données qui est affecté à la colonne. Le nom du classement peut être un nom de classement Windows ou SQL. Pour obtenir la liste des classements et des informations supplémentaires, consultez [Nom de classement Windows &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md) et [Nom du classement SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Vous pouvez utiliser la clause COLLATE pour changer seulement les classements des colonnes ayant les types de données **char**, **varchar**, **nchar** et **nvarchar**.

> [!NOTE]
> `COLLATE` ne peut pas être spécifié pour les fonctions table CLR.

ROWGUIDCOL Indique que la nouvelle colonne est une colonne d’identificateur unique global de ligne. Une seule colonne **uniqueidentifier** par table peut être désignée comme colonne ROWGUIDCOL. La propriété ROWGUIDCOL ne peut être affectée qu’à une colonne **uniqueidentifier**.

La propriété ROWGUIDCOL n'assure pas l'unicité des valeurs stockées dans la colonne. Elle ne peut de plus générer automatiquement de valeurs pour les nouvelles lignes insérées dans la table. Pour générer des valeurs uniques pour chaque colonne, utilisez la fonction NEWID dans des instructions INSERT. Une valeur par défaut peut être spécifiée, mais ce ne peut pas être NEWID.

IDENTITY indique que la nouvelle colonne est une colonne d’identité. Lorsqu'une ligne est ajoutée à la table, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte une valeur incrémentée unique à la colonne. Les colonnes d'identité sont généralement utilisées avec les contraintes PRIMARY KEY comme identificateur unique de ligne pour la table. La propriété IDENTITY peut être affectée à des colonnes **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** ou **numeric(p,0)** . Une seule colonne d'identité peut être créée par table. Il n'est pas possible d'utiliser des valeurs par défaut liées et des contraintes DEFAULT avec une colonne d'identité. Vous devez spécifier à la fois *seed* et *increment*, ou bien aucun des deux. Si vous n'en spécifiez aucun, la valeur par défaut est (1,1).

L'argument IDENTITY ne peut pas être spécifié pour les fonctions table CLR.

*seed* Valeur entière à affecter à la première ligne de la table.

*increment* Valeur entière à ajouter à la valeur *seed* pour des lignes successives de la table.

 **\< column_constraint >::= et \< table_constraint>::=**

Définit la contrainte d'une colonne ou table spécifiée. Dans le cas des fonctions CLR, le seul type de contrainte autorisé est NULL. L'utilisation de contraintes nommées n'est pas autorisée.

NULL | NOT NULL Détermine si les valeurs Null sont autorisées dans la colonne. NULL n'est pas strictement une contrainte, mais peut être spécifié comme NOT NULL. L'argument NOT NULL ne peut pas être spécifié pour les fonctions table CLR.

PRIMARY KEY Contrainte assurant l’intégrité de l’entité d’une colonne spécifiée au moyen d’un index unique. Dans les fonctions table définies par l'utilisateur, la contrainte PRIMARY KEY ne peut être créée que sur une colonne par table. L'argument PRIMARY KEY ne peut pas être spécifié pour les fonctions table CLR.

UNIQUE est une contrainte assurant l’intégrité de l’entité d’une colonne ou de plusieurs colonnes spécifiées au moyen d’un index unique. Une table peut comprendre plusieurs contraintes UNIQUE. L'argument UNIQUE ne peut pas être spécifié pour les fonctions table CLR.

CLUSTERED | NONCLUSTERED indique qu’un index, cluster ou non cluster, est créé pour la contrainte PRIMARY KEY ou UNIQUE. Les contraintes PRIMARY KEY utilisent CLUSTERED, tandis que les contraintes UNIQUE recourent à NONCLUSTERED.

CLUSTERED peut être spécifié pour une seule contrainte. Si CLUSTERED est spécifié pour une contrainte UNIQUE et qu'une contrainte PRIMARY KEY est également spécifiée, la contrainte PRIMARY KEY utilise NONCLUSTERED.

Les arguments CLUSTERED et NONCLUSTERED ne peuvent pas être spécifiés pour les fonctions table CLR.

CHECK est une contrainte qui assure l’intégrité du domaine en limitant les valeurs possibles pouvant être entrées dans une ou plusieurs colonnes. Les contraintes CHECK ne peuvent pas être spécifiées pour les fonctions table CLR.

*logical_expression* est une expression logique qui retourne TRUE ou FALSE.

**\<computed_column_definition>::=**

Spécifie une colonne calculée. Pour plus d’informations sur les colonnes calculées, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).

*column_name* Nom de la colonne calculée.

*computed_column_expression* Expression définissant la valeur d’une colonne calculée.

**\<index_option>::=**

Spécifie les options de l'index PRIMARY KEY ou UNIQUE. Pour plus d’informations sur les options d’index, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).

PAD_INDEX = { ON | **OFF** } Spécifie le remplissage de l’index. La valeur par défaut est OFF.

FILLFACTOR = *fillfactor* Spécifie un pourcentage qui indique le taux de remplissage appliqué par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au niveau feuille de chaque page d’index lors de la création ou de la modification de l’index. *fillfactor* doit être une valeur entière comprise entre 1 et 100. La valeur par défaut est 0.

IGNORE_DUP_KEY = { ON | **OFF** } spécifie la réponse d’erreur lorsqu’une opération d’insertion essaie d’insérer des valeurs de clés en double dans un index unique. L'option IGNORE_DUP_KEY s'applique uniquement aux opérations d'insertion après la création ou la régénération de l'index. La valeur par défaut est OFF.

STATISTICS_NORECOMPUTE = { ON | **OFF** } Spécifie si les statistiques de distribution sont recalculées. La valeur par défaut est OFF.

ALLOW_ROW_LOCKS = { **ON** | OFF } Indique si les verrous de ligne sont autorisés. La valeur par défaut est ON.

ALLOW_PAGE_LOCKS = { **ON** | OFF } Indique si les verrous de page sont autorisés. La valeur par défaut est ON.

## <a name="best-practices"></a>Bonnes pratiques

Si une fonction définie par l’utilisateur n’est pas créée avec la clause `SCHEMABINDING`, les modifications apportées aux objets sous-jacents peuvent affecter la définition de la fonction et produire des résultats inattendus en cas d’appel. Nous vous recommandons d'implémenter l'une des méthodes suivantes pour vous assurer que la fonction ne devient pas obsolète en raison des modifications apportées à ses objets sous-jacents :

- Spécifiez la clause `WITH SCHEMABINDING` lors de la création de la fonction. Vous vous assurez ainsi que les objets référencés dans la définition de la fonction ne peuvent pas être modifiés sauf si la fonction est également modifiée.
- Exécutez la procédure stockée [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) après avoir modifié tout objet spécifié dans la définition de la fonction.

> [!IMPORTANT]
> Pour plus d’informations et des considérations en matière de performances sur les fonctions table inline et les fonctions table à instructions multiples, consultez [Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

## <a name="data-types"></a>Types de données

Si des paramètres sont spécifiés dans une fonction CLR, ils doivent être de types [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tels que préalablement définis pour *scalar_parameter_data_type*. Pour obtenir des informations sur la comparaison des types de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec les types de données d’intégration CLR ou les types de données CLR (Common Language Runtime) [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], consultez [Mappage des données de paramètres CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

Pour que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] référence la méthode appropriée en cas de surcharge dans une classe, la méthode indiquée dans \<method_specifier> doit présenter les caractéristiques suivantes :

- Recevoir le même nombre de paramètres que ceux spécifiés dans [ ,...*n* ].
- Recevoir tous les paramètres par valeur, non par référence.
- Utiliser des types de paramètre compatibles avec ceux spécifiés dans la fonction [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Si le type de données de retour de la fonction CLR spécifie un type de table (RETURNS TABLE), le type de données de retour de la méthode dans \<method_specifier> doit être de type **IEnumerator** ou **IEnumerable**, et le système considère que l’interface est implémentée par le créateur de la fonction. À la différence des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)], les fonctions CLR ne peuvent pas inclure de contraintes PRIMARY KEY, UNIQUE ou CHECK dans \<table_type_definition>. Les types de données des colonnes spécifiés dans \<table_type_definition> doivent correspondre aux types des colonnes correspondantes du jeu de résultats retourné par la méthode dans \<method_specifier> au moment de l’exécution. Cette vérification de type n'est pas réalisée à la création de la fonction.

Pour plus d’informations sur la façon de programmer des fonctions CLR, consultez [Fonctions CLR définies par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md).

## <a name="general-remarks"></a>Remarques d'ordre général

Les fonctions scalaires peuvent être appelées là où des expressions scalaires sont utilisées. C'est notamment le cas dans les colonnes calculées et les définitions de contraintes CHECK. Les fonctions scalaires peuvent également être exécutées avec l’instruction [EXECUTE](../../t-sql/language-elements/execute-transact-sql.md). Les fonctions scalaires doivent être appelées avec au moins le nom en deux parties de la fonction ( *<schema><function>* ). Pour plus d’informations sur les noms en plusieurs parties, consultez [Conventions de la syntaxe Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md). Les fonctions table peuvent être appelées là où des expressions de table sont autorisées dans la clause `FROM` des instructions `SELECT`, `INSERT`, `UPDATE` ou `DELETE`. Pour plus d’informations, consultez [Exécuter des fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/execute-user-defined-functions.md).

## <a name="interoperability"></a>Interopérabilité

Les instructions suivantes sont valides dans une fonction :

- Instructions d'affectation
- Instructions de contrôle de flux, à l’exception des instructions `TRY...CATCH`.
- Instructions `DECLARE` définissant des variables de données locales et des curseurs locaux.
- Instructions `SELECT` qui contiennent des listes de sélection avec des expressions affectant des valeurs à des variables locales.
- Opérations de curseur faisant référence à des curseurs locaux déclarés, ouverts, fermés et libérés dans la fonction. Seules les instructions `FETCH` affectant des valeurs à des variables locales avec la clause `INTO` sont autorisées ; les instructions `FETCH` qui retournent des données au client ne sont pas autorisées.
- Instructions `INSERT`, `UPDATE` et `DELETE` modifiant des variables de table locales.
- Instructions `EXECUTE` appelant des procédures stockées étendues.

Pour plus d’informations, consultez [Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

### <a name="computed-column-interoperability"></a>Interopérabilité des colonnes calculées

Les fonctions ont les propriétés suivantes. Les valeurs de ces propriétés déterminent si les fonctions sont utilisables dans des colonnes calculées pouvant être indexées ou rendues persistantes.

|Propriété|Description|Notes|
|--------------|-----------------|-----------|
|**IsDeterministic**|La fonction est déterministe ou non déterministe.|L'accès local aux données est autorisé dans les fonctions déterministes. Par exemple, une fonction est dite déterministe si elle retourne toujours le même résultat chaque fois qu'elle est appelée à l'aide d'un ensemble spécifique de valeurs d'entrée et que la base de données présente le même état.|
|**IsPrecise**|La fonction est précise ou imprécise.|Les fonctions imprécises contiennent des opérations telles que les opérations à virgule flottante.|
|**IsSystemVerified**|Les propriétés de précision et de déterminisme de la fonction peuvent être vérifiées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].||
|**SystemDataAccess**|La fonction accède aux données système (catalogues système ou tables système virtuelles) dans l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].||
|**UserDataAccess**|La fonction accède aux données utilisateur dans l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Comprend les tables définies par l'utilisateur et les tables temporaires, mais pas les variables de table.|

Les propriétés de précision et de déterminisme des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont automatiquement déterminées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les propriétés d'accès aux données et de déterminisme des fonctions CLR peuvent être spécifiées par l'utilisateur. Pour plus d’informations, consultez [Vue d’ensemble des attributs personnalisés de l’intégration du CLR](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md).

Pour afficher les valeurs actuelles de ces propriétés, utilisez [OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md).

> [!IMPORTANT]
> Les fonctions doivent être créées avec `SCHEMABINDING` pour être déterministes.

Une colonne calculée qui appelle une fonction définie par l'utilisateur peut être utilisée dans un index lorsque la fonction possède les valeurs de propriété suivantes :

- **IsDeterministic** = true
- **IsSystemVerified** = true (sauf si la colonne calculée est persistante)
- **UserDataAccess** = false
- **SystemDataAccess** = false

Pour plus d'informations, consultez [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

### <a name="calling-extended-stored-procedures-from-functions"></a>Appel de procédures stockées étendues à partir de fonctions

Une procédure stockée étendue appelée à partir d'une fonction ne peut pas retourner de jeux de résultats au client. Les API ODS qui retournent des jeux de résultats au client retournent FAIL. La procédure stockée étendue peut se reconnecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; toutefois, elle ne doit pas joindre la même transaction que la fonction l'ayant appelée.

À l'image des appels à partir d'un lot ou d'une procédure stockée, la procédure stockée étendue est exécutée dans le contexte du compte de sécurité Windows sous lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est exécuté. Le propriétaire de la procédure stockée doit tenir compte de cet élément lors de l'octroi de l'autorisation EXECUTE sur la procédure.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Les fonctions définies par l'utilisateur ne permettent pas d'exécuter des actions qui modifient l'état des bases de données.

Les fonctions définies par l’utilisateur ne peuvent pas contenir de clause `OUTPUT INTO` avec une table comme cible.

Les instructions [!INCLUDE[ssSB](../../includes/sssb-md.md)] suivantes ne peuvent pas être incluses dans la définition d'une fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] définie par l'utilisateur :

- `BEGIN DIALOG CONVERSATION`
- `END CONVERSATION`
- `GET CONVERSATION GROUP`
- `MOVE CONVERSATION`
- `RECEIVE`
- `SEND`

Les fonctions définies par l'utilisateur peuvent être imbriquées ; en d'autres termes, une fonction définie par l'utilisateur peut en appeler une autre. Le niveau d'imbrication est incrémenté lorsque la fonction appelée commence à s'exécuter, et décrémenté lorsque l'exécution est terminée. Les fonctions définies par l'utilisateur peuvent être imbriquées jusqu'à 32 niveaux. Le dépassement des niveaux d'imbrication maximum autorisés, provoque l'échec de la totalité de la chaîne de fonctions appelantes. Toute référence à du code managé depuis une fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] définie par l'utilisateur compte pour un niveau parmi les 32 niveaux d'imbrication possibles. Les méthodes appelées à partir du code managé ne comptent pas par rapport à cette limite.

### <a name="using-sort-order-in-clr-table-valued-functions"></a>Utilisation de l'ordre de tri dans les fonctions table CLR

Quand vous utilisez la clause `ORDER` dans des fonctions table CLR, prenez en compte les indications suivantes :

- Vous devez faire en sorte que les résultats soient toujours triés dans l'ordre spécifié. Si les résultats ne sont pas dans l'ordre spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] générera un message d'erreur lors de l'exécution de la requête.
- Si une clause `ORDER` est spécifiée, la sortie de la fonction table doit être triée en fonction du classement de la colonne (explicite ou implicite). Par exemple, si le classement de la colonne est chinois (spécifié dans le DDL pour la fonction table ou obtenu à partir du classement de la base de données), les résultats retournés doivent être triés conformément aux règles de tri chinoises.
- La clause `ORDER`, si elle est spécifiée, est toujours vérifiée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quand des résultats sont retournés, qu’elle soit utilisée ou non par le processeur de requêtes pour effectuer des optimisations supplémentaires. Utilisez la clause `ORDER` seulement si vous savez que c’est utile pour le processeur de requêtes.
- Le processeur de requêtes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tire automatiquement parti de la clause `ORDER` dans les cas suivants :

  - Requêtes d’insertion dans lesquelles la clause `ORDER` est compatible avec un index.
  - Clauses `ORDER BY` qui sont compatibles avec la clause `ORDER`.
  - Agrégats, où `GROUP BY` est compatible avec la clause `ORDER`.
  - Agrégats `DISTINCT` dans lesquels les colonnes distinctes sont compatibles avec la clause `ORDER`.

La clause `ORDER` ne garantit pas des résultats ordonnés quand une requête SELECT est exécutée, sauf si la clause `ORDER BY` est également spécifiée dans la requête. Pour plus d’informations sur la façon de rechercher par requête les colonnes incluses dans l’ordre de tri pour les fonctions table, consultez [sys.function_order_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-function-order-columns-transact-sql.md).

## <a name="metadata"></a>Métadonnées

Le tableau suivant répertorie les affichages catalogue système que vous pouvez utiliser pour retourner des métadonnées sur les fonctions définies par l'utilisateur.

|Affichage système|Description|
|-----------------|-----------------|
|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|Consultez l’exemple E dans la section Exemples ci-dessous.|
|[sys.assembly_modules](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|Affiche des informations sur les fonctions définies par l'utilisateur CLR.|
|[sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)|Affiche des informations sur les paramètres définis dans les fonctions définies par l'utilisateur.|
|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)|Affiche les objets sous-jacents référencés par une fonction.|

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation `CREATE FUNCTION` dans la base de données et l’autorisation `ALTER` sur le schéma dans lequel la fonction est créée. Si la fonction spécifie un type défini par l’utilisateur, elle nécessite l’autorisation `EXECUTE` sur le type.

## <a name="examples"></a>Exemples

> [!NOTE]
> Pour plus d’exemples et des considérations relatives aux performances sur les fonctions définies par l’utilisateur, consultez [Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

### <a name="a-using-a-scalar-valued-user-defined-function-that-calculates-the-iso-week"></a>R. Utilisation d'une fonction scalaire définie par l'utilisateur calculant la semaine ISO

L'exemple suivant crée la fonction définie par l'utilisateur `ISOweek`. Cette fonction prend un argument date et calcule le numéro de semaine ISO. Pour que ce calcul puisse être correctement réalisé, `SET DATEFIRST 1` doit être appelée avant la fonction.

L’exemple illustre également l’utilisation de la clause [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) pour indiquer le contexte de sécurité dans lequel une procédure stockée peut être exécutée. Dans l'exemple, l'option `CALLER` spécifie que la procédure sera exécutée dans le contexte de l'utilisateur qui l'appelle. Les autres options que vous pouvez spécifier sont `SELF`, `OWNER` et *user_name*.

Voici l'appel de la fonction. Notez que `DATEFIRST` a la valeur `1`.

```sql
CREATE FUNCTION dbo.ISOweek (@DATE datetime)
RETURNS int
WITH EXECUTE AS CALLER
AS
BEGIN
    DECLARE @ISOweek int;
    SET @ISOweek= DATEPART(wk,@DATE)+1
        -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104');
--Special cases: Jan 1-3 may belong to the previous year
    IF (@ISOweek=0)
        SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1
            AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1;
--Special case: Dec 29-31 may belong to the next year
    IF ((DATEPART(mm,@DATE)=12) AND
        ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28))
    SET @ISOweek=1;
    RETURN(@ISOweek);
END;
GO
SET DATEFIRST 1;
SELECT dbo.ISOweek(CONVERT(DATETIME,'12/26/2004',101)) AS 'ISO Week';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
ISO Week
----------------
52
```

### <a name="b-creating-an-inline-table-valued-function"></a>B. Création d'une fonction table incluse

L'exemple suivant retourne une fonction table incluse dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Pour chaque produit vendu au magasin, il retourne trois colonnes : `ProductID`, `Name` et l’agrégat des totaux annuels par magasin sous `YTD Total`.

```sql
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)
RETURNS TABLE
AS
RETURN
(
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'
    FROM Production.Product AS P
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID
    WHERE C.StoreID = @storeid
    GROUP BY P.ProductID, P.Name
);
GO
```

Pour appeler la fonction, exécutez la requête suivante :

```sql
SELECT * FROM Sales.ufn_SalesByStore (602);
```

### <a name="c-creating-a-multi-statement-table-valued-function"></a>C. Création d'une fonction table à instructions multiples

L'exemple suivant crée la fonction table `fn_FindReports(InEmpID)` dans la base de données AdventureWorks2012. Lorsqu'elle est fournie avec un ID d'employé valide, la fonction retourne une table répertoriant tous les employés qui rendent compte à l'employé directement ou indirectement. La fonction utilise une expression de table commune (CTE, Common Table Expression) récursive pour générer la liste hiérarchique des employés. Pour plus d’informations sur les expressions CTE, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).

```sql
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)
RETURNS @retFindReports TABLE
(
    EmployeeID int primary key NOT NULL,
    FirstName nvarchar(255) NOT NULL,
    LastName nvarchar(255) NOT NULL,
    JobTitle nvarchar(50) NOT NULL,
    RecursionLevel int NOT NULL
)
--Returns a result set that lists all the employees who report to the
--specific employee directly or indirectly.*/
AS
BEGIN
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns
    AS (
        -- Get the initial list of Employees for Manager n
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0
        FROM HumanResources.Employee e
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        WHERE e.BusinessEntityID = @InEmpID
        UNION ALL
        -- Join recursive member to anchor
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1
        FROM HumanResources.Employee e
            INNER JOIN EMP_cte
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        )
-- copy the required columns to the result of the function
    INSERT @retFindReports
    SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
    FROM EMP_cte
    RETURN
END;
GO
-- Example invocation
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
FROM dbo.ufn_FindReports(1);

GO
```

### <a name="d-creating-a-clr-function"></a>D. Création d'une fonction CLR

L’exemple crée la fonction CLR`len_s`. Avant que la fonction ne soit créée, l'assembly `SurrogateStringFunction.dll` est inscrit dans la base de données locale.

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 et versions ultérieures)

```sql
DECLARE @SamplesPath nvarchar(1024);
-- You may have to modify the value of this variable if you have
-- installed the sample in a location other than the default location.
SELECT @SamplesPath = REPLACE
    (  physical_name
     , 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf'
     , 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\'
    )
  FROM master.sys.database_files
  WHERE name = 'master';

CREATE ASSEMBLY [SurrogateStringFunction]
FROM @SamplesPath + 'StringManipulate\CS\StringManipulate\bin\debug\SurrogateStringFunction.dll'
WITH PERMISSION_SET = EXTERNAL_ACCESS;
GO

CREATE FUNCTION [dbo].[len_s] (@str nvarchar(4000))
RETURNS bigint
AS EXTERNAL NAME [SurrogateStringFunction].[Microsoft.Samples.SqlServer.SurrogateStringFunction].[LenS];
GO
```

Pour obtenir un exemple de création d’une fonction table CLR, consultez [Fonctions table CLR](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-table-valued-functions.md).

### <a name="e-displaying-the-definition-of-user-defined-functions"></a>E. Affichage de la définition des fonctions définies par l’utilisateur

```sql
SELECT definition, type
FROM sys.sql_modules AS m
JOIN sys.objects AS o ON m.object_id = o.object_id
    AND type IN ('FN', 'IF', 'TF');
GO
```

La définition de fonctions créées avec l’option `ENCRYPTION` ne peut pas être affichée avec sys.sql_modules ; cependant, d’autres informations sur les fonctions chiffrées apparaissent.

## <a name="see-also"></a>Voir aussi

- [Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)
- [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)
- [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)
- [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md)
- [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)
- [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)
- [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)
- [Fonctions CLR définies par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)