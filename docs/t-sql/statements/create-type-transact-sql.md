---
description: CREATE TYPE (Transact-SQL)
title: CREATE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql13.swb.sysdatatype.properties.f1
- CREATE TYPE
- TYPE_TSQL
- TYPE
- CREATE_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UDTs [SQL Server], creating
- CLR user-defined types [SQL Server]
- user-defined table types [SQL Server]
- user-defined types [SQL Server], creating
- CREATE TYPE statement
- alias data types [SQL Server], creating
- data types [SQL Server], creating
ms.assetid: 2202236b-e09f-40a1-bbc7-b8cff7488905
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2063543c497070bd8366cae9a8691b3701fc4330
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177903"
---
# <a name="create-type-transact-sql"></a>CREATE TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crée un type de données alias ou un type défini par l'utilisateur dans la base de données active [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. L'implémentation d'un type de données d'alias est basé sur un type de système natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un type défini par l’utilisateur est implémenté à travers une classe d’un assembly dans le CLR [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Pour lier un type défini par l’utilisateur à son implémentation, l’assembly CLR contenant l’implémentation du type doit d’abord être enregistré dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md).  
  
 La possibilité d'exécuter un code CLR est désactivée par défaut dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez créer, modifier ou supprimer des objets de base de données qui référencent des modules de code managé, mais ces références ne seront pas exécutées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à moins que l’option [clr enabled Option](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) ne soit activée à l’aide de [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
 
> [!NOTE]  
>  L’intégration du CLR .NET Framework à SQL Server est décrite dans cette rubrique. L’intégration du CLR ne s’applique pas à Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- User-defined Data Type Syntax    
CREATE TYPE [ schema_name. ] type_name  
{   
    [
      FROM base_type   
      [ ( precision [ , scale ] ) ]  
      [ NULL | NOT NULL ]
    ]
    | EXTERNAL NAME assembly_name [ .class_name ]   
    | AS TABLE ( { <column_definition> | <computed_column_definition> [ ,... n ] }
      [ <table_constraint> ] [ ,... n ]    
      [ <table_index> ] [ ,... n ] } )
 
} [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   
    [ NULL | NOT NULL ]  
    [   
        DEFAULT constant_expression ]   
      | [ IDENTITY [ ( seed ,increment ) ]   
    ]  
    [ ROWGUIDCOL ] [ <column_constraint> [ ...n ] ]   
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
                [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH ( <index_option> [ ,...n ] )   
        ]  
  | CHECK ( logical_expression )   
}   
  
<computed_column_definition> ::=  
  
column_name AS computed_column_expression   
[ PERSISTED [ NOT NULL ] ]  
[   
    { PRIMARY KEY | UNIQUE }  
        [ CLUSTERED | NONCLUSTERED ]  
        [   
            WITH ( <index_option> [ ,...n ] )  
        ]  
    | CHECK ( logical_expression )   
]   
  
<table_constraint> ::=  
{   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
    ( column [ ASC | DESC ] [ ,...n ] )   
        [   
    WITH ( <index_option> [ ,...n ] )   
        ]  
    | CHECK ( logical_expression )   
}   
  
<index_option> ::=  
{  
    IGNORE_DUP_KEY = { ON | OFF }  
}  

< table_index > ::=  
  INDEX constraint_name  
     [ CLUSTERED | NONCLUSTERED ]   (column [ ASC | DESC ] [ ,... n ] )} }  
```  
  
```syntaxsql
-- User-defined Memory Optimized Table Types Syntax  
CREATE TYPE [schema_name. ] type_name  
AS TABLE ( { <column_definition> [ ,... n ] }  
    | [ <table_constraint> ] [ ,... n ]    
    | [ <table_index> ] [ ,... n ] } )
    [ WITH ( <table_option> [ ,... n ] ) ]  
 [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   [ NULL | NOT NULL ]    [  
      [ IDENTITY [ (1 , 1) ]  
    ]  
    [ <column_constraint> [ ... n ] ]    [ <column_index> ]  
  
<data type> ::=  
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]  
  
<column_constraint> ::=  
{ PRIMARY KEY {   NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count) 
                | NONCLUSTERED } }  
  
< table_constraint > ::=  
{ PRIMARY KEY { NONCLUSTERED HASH (column [ ,... n ] ) 
                   WITH (BUCKET_COUNT = bucket_count) 
               | NONCLUSTERED  (column [ ASC | DESC ] [ ,... n ] )  } }  
  
<column_index> ::=  
  INDEX index_name  
{ { [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count) 
     | NONCLUSTERED } }  
  
< table_index > ::=  
  INDEX constraint_name  
{ { [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count) 
 |  [NONCLUSTERED]  (column [ ASC | DESC ] [ ,... n ] )} }  
  
<table_option> ::=  
{  
    [MEMORY_OPTIMIZED = {ON | OFF}]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom du schéma auquel appartient le type de données d'alias ou défini par l'utilisateur.  
  
 *TYPE_NAME*  
 Nom du type de données d'alias ou défini par l'utilisateur. Les noms de type doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 *base_type*  
 Type de données fourni par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur lequel est basé le type de données d’alias. *base_type* est de type **sysname**, sans valeur par défaut, et peut avoir l’une de ces valeurs :  
  
|||||  
|-|-|-|-|  
|**bigint**|**binary(** *n* **)**|**bit**|**char(** *n* **)**|  
|**date**|**datetime**|**datetime2**|**datetimeoffset**|  
|**decimal**|**float**|**image**|**int**|  
|**money**|**nchar(** *n* **)**|**ntext**|**numeric**|  
|**nvarchar(** *n* &#124; **max)**|**real**|**smalldatetime**|**smallint**|  
|**smallmoney**|**sql_variant**|**text**|**time**|  
|**tinyint**|**uniqueidentifier**|**varbinary(** *n* &#124; **max)**|**varchar(** *n* &#124; **max)**|  
  
 *base_type* peut également être n’importe quel synonyme de type de données correspondant à l’un de ces types de données système.  
  
 *precision*  
 Pour **decimal** ou **numeric**, il s’agit d’un entier non négatif indiquant le nombre total maximal de chiffres décimaux pouvant figurer à gauche et à droite de la virgule décimale. Pour plus d’informations, consultez [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 *scale*  
 Pour **decimal** or **numeric**, il s’agit d’un entier non négatif indiquant le nombre maximal de chiffres décimaux qui peuvent figurer à droite d’une virgule décimale ; il doit être inférieur ou égal à la précision. Pour plus d’informations, consultez [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 **NULL** | NOT NULL  
 Précise si le type accepte les valeurs NULL. S'il n'est pas spécifié, la valeur par défaut est NULL.  
  
 *assembly_name*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie l’assembly [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui référence l’implémentation du type défini par l’utilisateur dans le CLR. *assembly_name* doit correspondre à un assembly existant dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans la base de données actuelle.  
  
> [!NOTE]  
>  EXTERNAL_NAME n'est pas disponible dans une base de données autonome.  
  
 **[.** *class_name*  **]**  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie la classe au sein de l'assembly qui implémente le type défini par l'utilisateur. *class_name* doit être un identificateur valide et doit exister en tant que classe dans l’assembly avec une visibilité d’assembly. *class_name* respecte la casse, quel que soit le classement de base de données, et doit correspondre exactement au nom de la classe dans l’assembly correspondant. Le nom de la classe peut être un nom qualifié par un espace de noms entouré de crochets ( **[ ]** ) si le langage de programmation utilisé pour écrire la classe utilise le concept des espaces de noms (par exemple C#). Si *class_name* n’est pas spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suppose qu’il est identique à *type_name*.  
  
 \<column_definition>  
 Définit les colonnes pour un type de table défini par l'utilisateur.  
  
 \<data type>  
 Définit le type de données dans une colonne pour un type de table défini par l'utilisateur. Pour plus d’informations sur les types de données, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md). Pour plus d’informations sur les tables, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<column_constraint>  
 Définit les contraintes de colonnes pour un type de table défini par l'utilisateur. Les contraintes prises en charge incluent PRIMARY KEY, UNIQUE et CHECK. Pour plus d’informations sur les tables, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<computed_column_definition>  
 Définit une expression de colonne calculée en tant que colonne dans un type de table défini par l'utilisateur. Pour plus d’informations sur les tables, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<table_constraint>  
 Définit une contrainte de table sur un type de table défini par l'utilisateur. Les contraintes prises en charge incluent PRIMARY KEY, UNIQUE et CHECK.  
  
 \<index_option>  
 Spécifie la réponse aux erreurs de valeurs de clés dupliquées dans une opération d'insertion de plusieurs lignes dans un index cluster unique ou un index non cluster unique. Pour plus d’informations sur les options d’index, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
 
  `INDEX *index_name* [ CLUSTERED | NONCLUSTERED ] (*column_name* [ ASC | DESC ] [ ,... *n* ] )`  
     
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique qu’il faut créer un index sur la table. Il peut s’agir d’un index cluster ou non-cluster. L’index contiendra les colonnes répertoriées et triera les données dans l’ordre croissant ou décroissant.
  
 INDEX  
 Vous devez spécifier les index de table et de colonne dans le cadre de l'instruction CREATE TABLE. CREATE INDEX et DROP INDEX ne sont pas pris en charge pour les tables optimisées en mémoire.  
  
 MEMORY_OPTIMIZED  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indique si le type de table est optimisé en mémoire. Cette option est désactivée par défaut ; la table (type) n'est pas une table optimisée en mémoire (type). Les types de tables optimisées en mémoire sont des tables utilisateur optimisées en mémoire dont le schéma est rendu persistant sur disque similairement à d'autres tables utilisateur.  
  
 BUCKET_COUNT  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indique le nombre de compartiments qui doivent être créés dans l'index de hachage. La valeur maximale de BUCKET_COUNT dans les index de hachage est de 1 073 741 824. Pour plus d’informations sur le nombre de compartiments, consultez [Index sur des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md). *bucket_count* est un argument obligatoire.  
  
 HASH  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indique qu'un index HASH est créé. Les index de hachage sont pris en charge uniquement sur les tables optimisées en mémoire.  
  
## <a name="remarks"></a>Notes  
 La classe de l’assembly référencé dans *assembly_name*, ainsi que ses méthodes, doit satisfaire tous les besoins d’implémentation d’un type défini par l’utilisateur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur ces exigences, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).  
  
 Les autres considérations portent sur les points suivants :  
  
-   La classe peut avoir des méthodes surchargées, mais ces méthodes ne peuvent être appelées qu'à partir du code managé, et non à partir de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Tous les membres statiques doivent être déclarés en tant que **const** ou **readonly** si *assembly_name* est SAFE ou EXTERNAL_ACCESS.  
  
 Au sein d'une base de données, il ne peut y avoir qu'un seul type défini par l'utilisateur enregistré par rapport à tout type spécifié qui a été téléchargé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir du CLR. Si un type de données défini par l'utilisateur est créé sur un type CLR pour lequel le type défini par l'utilisateur existe déjà dans la base de données, CREATE TYPE échoue avec une erreur. Cette restriction est nécessaire pour éviter toute ambiguïté dans la résolution de type SQL si un type CLR peut être mappé à plusieurs types définis par l'utilisateur.  
  
 Si une méthode mutateur dans le type ne retourne pas *void*, l’instruction CREATE TYPE n’est pas exécutée.  
  
 Pour modifier un type défini par l'utilisateur, vous devez supprimer le type à l'aide d'une instruction DROP TYPE, puis le créer de nouveau.  
  
 Contrairement aux types définis par l’utilisateur créés à l’aide de **sp_addtype**, le rôle de base de données **public** ne dispose pas automatiquement de l’autorisation REFERENCES sur les types créés à l’aide de CREATE TYPE. Cette autorisation doit être accordée séparément.  
  
 Dans les types de tables définis par l’utilisateur, les types définis par l’utilisateur structurés utilisés dans *column_name* \<data type> font partie de l’étendue du schéma de base de données dans lequel le type de table est défini. Pour accéder aux types structurés définis par l'utilisateur dans une étendue différente dans la base de données, utilisez des noms en deux parties.  
  
 Dans les types de tables définis par l'utilisateur, la clé primaire sur les colonnes calculées doit être PERSISTED et NOT NULL.  
  
## <a name="memory-optimized-table-types"></a>Types de tables optimisées en mémoire  
 À partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], le traitement des données dans un type de table peut être effectué dans la mémoire principale, et non sur disque. Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Pour obtenir des exemples de code montrant comment créer des tables à mémoire optimisée, consultez [Création d’une table optimisée en mémoire et d’une procédure stockée compilée en mode natif](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation CREATE TYPE dans la base de données actuelle et l’autorisation ALTER sur *schema_name*. Si *schema_name* n’est pas spécifié, les règles de résolution de noms par défaut pour la détermination du schéma de l’utilisateur actuel s’appliquent. Si *assembly_name* est spécifié, un utilisateur doit être propriétaire de l’assembly ou disposer d’une autorisation REFERENCES sur lui.  

 Si des colonnes de l’instruction CREATE TABLE sont définies avec un type de données défini par l’utilisateur, une autorisation REFERENCES est nécessaire sur ce type.
 
   >[!NOTE]
  > Un utilisateur qui crée une table avec une colonne dont le type est défini par l’utilisateur a besoin de l’autorisation REFERENCES pour ce type défini par l’utilisateur.
  > Si cette table doit être créée dans TempDB, l’autorisation REFERENCES doit être accordée explicitement chaque fois **avant** la création de la table, ou ce type de données et ces autorisations de références doivent être ajoutés à la base de données Model. Lorsque cette opération est effectuée, ce type de données et ces autorisations seront disponibles dans TempDB de façon définitive. Sinon, le type de données défini par l’utilisateur et les autorisations disparaîtront lors du redémarrage de SQL Server. Pour plus d'informations, consultez [CREATE TABLE](./create-table-transact-sql.md#permissions-1)
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-an-alias-type-based-on-the-varchar-data-type"></a>R. Création d'un type d'alias basé sur le type de données varchar  
 L'exemple suivant crée un type d'alias basé sur le type de données `varchar` fourni par le système.  
  
```sql  
CREATE TYPE SSN  
FROM varchar(11) NOT NULL ;  
```  
  
### <a name="b-creating-a-user-defined-type"></a>B. Création d'un type défini par l'utilisateur  
 L’exemple suivant crée un type `Utf8String` qui référence la classe `utf8string` dans l’assembly `utf8string`. Avant de créer le type, l'assembly `utf8string` est enregistré dans la base de données locale. Remplacez la partie binaire de l'instruction CREATE ASSEMBLY par une description valide.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql  
CREATE ASSEMBLY utf8string  
AUTHORIZATION [dbi]   
FROM 0x4D... ;  
GO  
CREATE TYPE Utf8String   
EXTERNAL NAME utf8string.[Microsoft.Samples.SqlServer.utf8string] ;  
GO  
```  
  
### <a name="c-creating-a-user-defined-table-type"></a>C. Création d'un type de table défini par l'utilisateur  
 L'exemple suivant crée un type de table défini par l'utilisateur qui possède deux colonnes. Pour plus d’informations sur la création et l’utilisation des paramètres table, consultez [Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
```sql  
CREATE TYPE LocationTableType AS TABLE   
    ( LocationName VARCHAR(50)  
    , CostRate INT );  
GO  
```  

### <a name="d-creating-a-user-defined-table-type-with-primary-key-and-index"></a>D. Création d’un type de table défini par l’utilisateur avec l’index et la clé primaire
L’exemple suivant crée un type de table défini par l’utilisateur qui a trois colonnes. L’une d’entre elles (`Name`) est la clé primaire et l’autre (`Price`) a un index non-cluster.  Pour plus d’informations sur la création et l’utilisation des paramètres table, consultez [Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).

```sql
CREATE TYPE InventoryItem AS TABLE
(
    [Name] NVARCHAR(50) NOT NULL,
    SupplierId BIGINT NOT NULL,
    Price DECIMAL (18, 4) NULL,
    PRIMARY KEY (
        Name
    ),
    INDEX IX_InventoryItem_Price (
        Price
    )
)
GO
```
  
## <a name="see-also"></a>Voir aussi  
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)    
 [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)     
 [Utilisation de types définis par l’utilisateur dans SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)     
