---
description: CREATE PROCEDURE (Transact-SQL)
title: CREATE PROCEDURE (Transact-SQL)
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- PROC
- PROCEDURE
- CREATE PROCEDURE
- CREATE_PROC_TSQL
- PROCEDURE_TSQL
- CREATE PROC
- PROC_TSQL
- CREATE_PROCEDURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- parameters [SQL Server], stored procedures
- table-valued parameters
- SET statement, stored procedures
- stored procedures [SQL Server], creating
- wildcard parameters [SQL Server]
- maximum size of stored procedures
- WITH RECOMPILE clause
- common language runtime [SQL Server], stored procedures
- CREATE PROCEDURE statement
- local temporary procedures [SQL Server]
- WITH ENCRYPTION clause
- output parameters [SQL Server]
- nesting stored procedures
- user-defined stored procedures [SQL Server]
- system stored procedures [SQL Server], creating
- deferred name resolution, stored procedures
- referenced tables [SQL Server]
- global temporary procedures [SQL Server]
- cursor data type
- temporary stored procedures [SQL Server]
- size [SQL Server], stored procedures
- automatic stored procedure execution
- creating stored procedures
ms.assetid: afe3d86d-c9ab-44e4-b74d-4e3dbd9cc58c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a23a11bf4968179baad9d3cc88e5a64acae68b76
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170161"
---
# <a name="create-procedure-transact-sql"></a>CREATE PROCEDURE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Crée un [!INCLUDE[tsql](../../includes/tsql-md.md)] ou une procédure stockée de common language runtime (CLR) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Les procédures stockées ressemblent aux procédures d'autres langages de programmation, car elles peuvent :

- accepter des paramètres d'entrée et retourner plusieurs valeurs sous la forme de paramètres de sortie à la procédure ou au lot appelant ;
- contenir des instructions de programmation qui exécutent des opérations dans la base de données, y compris l'appel d'autres procédures ;
- retourner une valeur d'état à une procédure ou à un lot appelant pour indiquer une réussite ou un échec (et la raison de l'échec).

Utilisez cette instruction pour créer une procédure permanente dans la base de données actuelle ou une procédure temporaire dans la base de données **tempdb**.

> [!NOTE]
> L’intégration du CLR .NET Framework à SQL Server est décrite dans cette rubrique. L’intégration du CLR ne s’applique pas à Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

Passez à la section [Exemples simples](#Simple) pour ignorer les détails de la syntaxe et obtenir un exemple rapide de procédure stockée de base.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Transact-SQL Syntax for Stored Procedures in SQL Server and Azure SQL Database

CREATE [ OR ALTER ] { PROC | PROCEDURE }
    [schema_name.] procedure_name [ ; number ]
    [ { @parameter [ type_schema_name. ] data_type }
        [ VARYING ] [ = default ] [ OUT | OUTPUT | [READONLY]
    ] [ ,...n ]
[ WITH <procedure_option> [ ,...n ] ]
[ FOR REPLICATION ]
AS { [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }
[;]

<procedure_option> ::=
    [ ENCRYPTION ]
    [ RECOMPILE ]
    [ EXECUTE AS Clause ]
```

```syntaxsql
-- Transact-SQL Syntax for CLR Stored Procedures

CREATE [ OR ALTER ] { PROC | PROCEDURE }
    [schema_name.] procedure_name [ ; number ]
    [ { @parameter [ type_schema_name. ] data_type }
        [ = default ] [ OUT | OUTPUT ] [READONLY]
    ] [ ,...n ]
[ WITH EXECUTE AS Clause ]
AS { EXTERNAL NAME assembly_name.class_name.method_name }
[;]
```

```syntaxsql
-- Transact-SQL Syntax for Natively Compiled Stored Procedures

CREATE [ OR ALTER ] { PROC | PROCEDURE } [schema_name.] procedure_name
    [ { @parameter data_type } [ NULL | NOT NULL ] [ = default ]
        [ OUT | OUTPUT ] [READONLY]
    ] [ ,... n ]
  WITH NATIVE_COMPILATION, SCHEMABINDING [ , EXECUTE AS clause ]
AS
{
  BEGIN ATOMIC WITH (set_option [ ,... n ] )
sql_statement [;] [ ... n ]
 [ END ]
}
 [;]

<set_option> ::=
    LANGUAGE = [ N ] 'language'
  | TRANSACTION ISOLATION LEVEL = { SNAPSHOT | REPEATABLE READ | SERIALIZABLE }
  | [ DATEFIRST = number ]
  | [ DATEFORMAT = format ]
  | [ DELAYED_DURABILITY = { OFF | ON } ]
```

```syntaxsql
-- Transact-SQL Syntax for Stored Procedures in Azure Synapse Analytics
-- and Parallel Data Warehouse

-- Create a stored procedure
CREATE { PROC | PROCEDURE } [ schema_name.] procedure_name
    [ { @parameterdata_type } [ OUT | OUTPUT ] ] [ ,...n ]
AS { [ BEGIN ] sql_statement [;][ ,...n ] [ END ] }
[;]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

OR ALTER

**S’applique à** : Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1).

Modifie la procédure si elle existe déjà.

*schema_name* Le nom du schéma auquel appartient la procédure. Les procédures sont liées à un schéma. Si un nom de schéma n'est pas précisé lors de la création de la procédure, le schéma par défaut de l'utilisateur chargé de créer la procédure est automatiquement utilisé.

*procedure_name* Le nom de la procédure. Les noms de procédures doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md) et doivent être uniques dans le schéma.

Évitez d’utiliser le préfixe **sp_** quand vous affectez un nom à une procédure. En effet, ce préfixe est utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour faire référence aux procédures système. L'utilisation de ce préfixe peut entraîner l'échec du code de l'application s'il existe une procédure système portant le même nom.

Des procédures temporaires locales ou globales peuvent être créées en faisant précéder *procedure_name* d’un seul signe dièse (#) ( *#procedure_name*) pour les procédures temporaires locales et de deux signes dièse ( *##procedure_name*) pour les procédures temporaires globales. Une procédure temporaire locale n'est visible que par la connexion qui l'a créée et est automatiquement supprimée au moment de la déconnexion. Une procédure temporaire globale est disponible pour toutes les connexions et est supprimée à la fin de la dernière session qui l'utilise. Des noms temporaires ne peuvent pas être indiqués pour les procédures CLR.

Le nom complet d'une procédure ou d'une procédure temporaire globale, y compris les signes ##, ne peut dépasser 128 caractères. Le nom complet d'une procédure temporaire locale, y compris le signe #, ne peut dépasser 116 caractères.

**;** *number*

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Entier facultatif utilisé pour regrouper des procédures de même nom. Ces procédures groupées peuvent être supprimées en même temps par le biais d'une seule instruction DROP PROCEDURE.

> [!NOTE]
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]

Les procédures numérotées ne peuvent pas utiliser les types **xml** ou CLR définis par l’utilisateur, et ne peuvent pas être utilisées dans un repère de plan.

**@** *parameter* Un paramètre déclaré dans la procédure. Spécifiez un nom de paramètre en utilisant le signe **@** comme premier caractère. Le nom du paramètre doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Un paramètre étant local à une procédure, vous pouvez utiliser le même nom dans d'autres procédures.

Un ou plusieurs paramètres peuvent être déclarés, dans la limite de 2 100. La valeur de chaque paramètre déclaré doit être fournie par l'utilisateur lors de l'appel à la procédure, sauf si vous définissez une valeur par défaut pour le paramètre ou que sa valeur est définie sur un autre paramètre. Si une procédure contient des [paramètres table](../../relational-databases/tables/use-table-valued-parameters-database-engine.md) et que le paramètre n’est pas indiqué dans l’appel, une table vide est passée. Les paramètres ne peuvent que prendre la place d'expressions constantes ; ils ne peuvent pas être utilisés à la place de noms de tables, de colonnes ou d'autres objets de base de données. Pour plus d’informations, consultez [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).

Il n'est pas possible de déclarer des paramètres si FOR REPLICATION est spécifié.

[ _type\_schema\_name_ **.** ] *data_type* Type de données du paramètre et du schéma auquel le type de données appartient.

**Instructions pour les procédures [!INCLUDE[tsql](../../includes/tsql-md.md)]**  :

- Tous les types de données [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent être utilisés comme paramètres.
- Vous pouvez utiliser le type de table défini par l'utilisateur pour créer des paramètres table. Les paramètres table ne peuvent être spécifiés que comme paramètres INPUT et ils doivent être accompagnés du mot clé READONLY. Pour plus d’informations, consultez [Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).
- Les types de données **cursor** ne peuvent être que des paramètres OUTPUT et doivent être accompagnés du mot clé VARYING.

**Instructions pour les procédures CLR** :

- Tous les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] natifs qui ont un équivalent en code managé peuvent être utilisés en tant que paramètres. Pour plus d’informations sur la correspondance entre les types de données CLR et les types de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Mappage des données de paramètres CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md). Pour plus d’informations sur les types de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et leur syntaxe, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).

- Les types de données table ou **cursor** ne peuvent pas être utilisés comme paramètres.
- Si le type du paramètre correspond à un type CLR défini par l'utilisateur, vous devez dans ce cas bénéficier de l'autorisation EXECUTE sur ce type.

VARYING spécifie le jeu de résultats pris en charge comme paramètre de sortie. Ce paramètre est construit dynamiquement par la procédure ; il se peut donc que son contenu varie. S’applique seulement aux paramètres de type **cursor**. Cette option n'est pas valide pour les procédures CLR.

*default* Une valeur par défaut pour un paramètre. Si une valeur par défaut est définie pour un paramètre, la procédure peut être exécutée sans spécifier de valeur pour ce paramètre. La valeur par défaut doit être une constante ou il peut s'agir de la valeur NULL. La valeur constante peut être exprimée sous la forme d'un caractère générique, rendant ainsi possible l'utilisation du mot clé LIKE lors de la transmission du paramètre à la procédure.

Les valeurs par défaut sont enregistrées dans la colonne **sys.parameters.default** uniquement pour les procédures CLR. Cette colonne est NULL pour les paramètres de procédure [!INCLUDE[tsql](../../includes/tsql-md.md)].

OUT | OUTPUT indique que le paramètre est un paramètre de sortie. Utilisez les paramètres OUTPUT pour retourner les valeurs à la procédure appelante. Les paramètres de type **text**, **ntext** et **image** ne peuvent pas être utilisés comme paramètres OUTPUT à moins que la procédure soit une procédure CLR. Un paramètre de sortie peut être un espace réservé pour curseur, sauf si la procédure correspond à une procédure CLR (Common Language Runtime). Un type de données table ne peut pas être spécifié comme paramètre OUTPUT d'une procédure.

READONLY indique que le paramètre ne peut pas être mis à jour ou modifié dans le corps de la procédure. Si le type de paramètre est un type de table, READONLY doit être spécifié.

RECOMPILE indique que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’utilise pas le cache pour le plan de requête de cette procédure, ce qui oblige celle-ci à être recompilée chaque fois qu’elle est exécutée. Pour plus d’informations sur les raisons d’une recompilation forcée, consultez [Recompiler une procédure stockée](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md). Cette option ne peut pas être utilisée lorsque FOR REPLICATION est spécifié ou pour les procédures CLR (Common Language Runtime).

Pour ordonner au [!INCLUDE[ssDE](../../includes/ssde-md.md)] d’annuler les plans de requête relatifs à des requêtes individuelles au sein d’une procédure, utilisez l’indicateur de requête RECOMPILE dans la définition de la requête. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).

ENCRYPTION

**S’applique à** : SQL Server ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit le texte d’origine de l’instruction CREATE PROCEDURE dans un format d’obfuscation. La sortie générée par l'obfuscation n'est pas visible directement dans les affichages catalogue de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les utilisateurs n'ayant pas accès aux tables système ou aux fichiers de base de données ne peuvent pas récupérer le texte obscurci. Le texte est cependant à la disposition des utilisateurs disposant de privilèges et qui peuvent accéder aux tables système par le biais du [port DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) ou qui accèdent directement aux fichiers de bases de données. Les utilisateurs qui peuvent attacher un débogueur au processus serveur peuvent également récupérer la procédure déchiffrée de la mémoire à l'exécution. Pour plus d’informations sur l’accès aux métadonnées système, consultez [Configuration de la visibilité des métadonnées](../../relational-databases/security/metadata-visibility-configuration.md).

Cette option n'est pas valide pour les procédures CLR.

Les procédures créées à l'aide de cette option ne peuvent pas être publiées dans le cadre d'une réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

La *clause* EXECUTE AS indique le contexte de sécurité dans lequel la procédure doit être exécutée.

Les procédures stockées compilées en mode natif ne présentent aucune limitation sur la clause EXECUTE AS depuis [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], les clauses SELF, OWNER et *’user_name’* sont prises en charge avec des procédures stockées compilées en mode natif.

Pour plus d’informations, consultez [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).

FOR REPLICATION

**S’applique à** : SQL Server ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie que la procédure est créée en vue d'une réplication. Par conséquent, elle ne peut pas être exécutée sur l'Abonné. Une procédure créée avec l'option FOR REPLICATION est utilisée comme filtre de procédure et n'est exécutée que lors de la réplication. Il n'est pas possible de déclarer des paramètres si FOR REPLICATION est spécifié. FOR REPLICATION ne peut pas être précisé pour une utilisation avec des procédures CLR. L'option RECOMPILE est ignorée pour les procédures créées avec l'option FOR REPLICATION.

Une procédure `FOR REPLICATION` a un type d’objet **RF** dans **sys.objects** et **sys.procedures**.

{ [ BEGIN ] *sql_statement* [;] [ ...*n* ] [ END ] } Une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] comprenant le corps de la procédure. Vous pouvez utiliser les mots clés facultatifs BEGIN et END pour délimiter les instructions. Pour plus d'informations, consultez les sections suivantes intitulées Meilleures pratiques, Remarques d'ordre général et Limitations et restrictions.

EXTERNAL NAME _assembly\_name_ **.** _class\_name_ **.** _method\_name_

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

Spécifie la méthode d'un assembly [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour une procédure CLR à référencer. *class_name* doit être un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide et doit exister en tant que classe dans l’assembly. Si la classe possède un nom qualifié par un espace de noms qui utilise un point ( **.** ) pour séparer les parties constituant l’espace de noms, le nom de la classe doit être délimité à l’aide de crochets ( **[]** ) ou de guillemets droits ( **""** ). La méthode spécifiée doit être une méthode statique de la classe.

Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas exécuter du code CLR. Vous pouvez créer, modifier et supprimer des objets d’une base de données qui font référence à des modules CLR (Common Language Runtime) ; cependant, vous ne pouvez pas exécuter ces références dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tant que vous n’avez pas activé l’[option CLR enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Pour activer cette option, utilisez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).

> [!NOTE]
> Les procédures CLR ne sont pas prises en charge dans une base de données autonome.

ATOMIC WITH

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique l'exécution automatique d'une procédure stockée Atomic. Les modifications sont validées ou bien tous les changements sont restaurés en levant une exception. Le bloc ATOMIC WITH est requis pour les procédures stockées compilées en mode natif.

Si la procédure retourne une valeur valide (explicitement via l'instruction RETURN, ou implicitement à la fin de l'exécution), le travail effectué par la procédure est validé. Si la procédure lève une exception, le travail effectué par la procédure est restauré.

XACT_ABORT est activé (ON) par défaut dans un bloc Atomic et ne peut pas être modifié. XACT_ABORT indique si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restaure automatiquement la transaction en cours lorsqu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] déclenche une erreur d'exécution.

 Les options SET suivantes sont toujours activées (ON) dans le bloc ATOMIC ; les options ne peuvent pas être modifiées.

- CONCAT_NULL_YIELDS_NULL
- QUOTED_IDENTIFIER, ARITHABORT
- NOCOUNT
- ANSI_NULLS
- ANSI_WARNINGS

Les options SET ne peuvent pas être modifiées dans les blocs ATOMIC. Les options SET dans la session utilisateur ne sont pas utilisées dans l'étendue des procédures stockées compilées en mode natif. Ces options sont résolues au moment de la compilation.

Les opérations BEGIN, ROLLBACK et COMMIT ne peuvent pas être utilisées dans un bloc Atomic.

Il y a un bloc ATOMIC par procédure stockée compilée en mode natif, au niveau de l'étendue externe de la procédure. Les blocs ne peuvent pas être imbriqués. Pour plus d’informations sur les blocs atomiques, consultez [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).

**NULL** | NOT NULL Détermine si les valeurs Null sont autorisées dans un paramètre. NULL est l'argument par défaut.

NATIVE_COMPILATION

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique que la procédure est compilée en mode natif. NATIVE_COMPILATION, SCHEMABINDING et EXECUTE AS peuvent être spécifiés dans n'importe quel ordre. Pour plus d’informations, consultez [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).

SCHEMABINDING

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Garantit que les tables référencées par une procédure ne peuvent pas être supprimées ou modifiées. SCHEMABINDING est requis dans les procédures stockées compilées en mode natif. (Pour plus d’informations, consultez [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).) Les restrictions SCHEMABINDING sont les mêmes que pour les fonctions définies par l’utilisateur. Pour plus d’informations, consultez la section SCHEMABINDING dans [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md).

LANGUAGE = [N] 'language'

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Équivalent à l’option de session [SET LANGUAGE &#40;Transact-SQL&#41;](../../t-sql/statements/set-language-transact-sql.md). LANGUAGE = [N] 'language' est requis.

TRANSACTION ISOLATION LEVEL

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Requis pour les procédures stockées compilées en mode natif. Spécifie le niveau d'isolation de la transaction pour la procédure stockée. Les options disponibles sont les suivantes :

Pour plus d’informations sur ces options, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).

REPEATABLE READ spécifie que les instructions ne peuvent pas lire les données qui ont été modifiées, mais pas encore validées par d'autres transactions. Si une autre transaction modifie des données qui ont été lues par la transaction actuelle, cette dernière échoue.

SERIALIZABLE spécifie ce qui suit :

- Les instructions ne peuvent pas lire des données qui ont été modifiées mais pas encore validées par d'autres transactions.
- Si une autre transaction modifie des données qui ont été lues par la transaction actuelle, cette dernière échoue.
- Si une autre transaction insère de nouvelles lignes avec des valeurs de clés comprises dans la plage de clés lues par l’une des instructions de la transaction actuelle, cette dernière échoue.

CAPTURE INSTANTANÉE spécifie que les données lues par une instruction dans une transaction sont la version transactionnellement cohérente des données qui existaient au début de la transaction.

DATEFIRST = *number*

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie un nombre compris entre 1 et 7 pour le premier jour de la semaine. DATEFIRST est facultatif. Si cela n'est pas spécifié, le paramètre est déduit en fonction du langage spécifié.

Pour plus d’informations, consultez [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).

DATEFORMAT = *format*

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie le classement des parties de date mois, jour et année pour interpréter les chaînes de caractères date, smalldatetime, datetime, datetime2 et datetimeoffset. DATEFORMAT est facultatif. Si cela n'est pas spécifié, le paramètre est déduit en fonction du langage spécifié.

Pour plus d’informations, consultez [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md).

DELAYED_DURABILITY = { OFF | ON }

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Les validations de transactions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent avoir une durabilité complète, la durabilité par défaut ou une durabilité retardée.

Pour plus d’informations, consultez [Contrôler la durabilité d’une transaction](../../relational-databases/logs/control-transaction-durability.md).

## <a name="simple-examples"></a><a name="Simple"></a> Exemples simples

Pour vous aider à démarrer, voici deux exemples rapides : `SELECT DB_NAME() AS ThisDB;` retourne le nom de la base de données active.
Vous pouvez encapsuler cette instruction dans une procédure stockée, par exemple :

```sql
CREATE PROC What_DB_is_this
AS
SELECT DB_NAME() AS ThisDB;
```

Appelez la procédure stockée avec l’instruction : `EXEC What_DB_is_this;`

Il est légèrement plus complexe de fournir un paramètre d’entrée pour rendre la procédure plus souple. Par exemple :

```sql
CREATE PROC What_DB_is_that @ID INT
AS
SELECT DB_NAME(@ID) AS ThatDB;
```

Fournissez un numéro d’identification de base de données quand vous appelez la procédure. Par exemple, `EXEC What_DB_is_that 2;` retourne `tempdb`.

Consultez [Exemples](#Examples) à la fin de cette rubrique pour obtenir beaucoup plus d’exemples.

## <a name="best-practices"></a>Bonnes pratiques

Les suggestions fournies dans cette section peuvent vous aider à améliorer les performances des procédures, même si cette liste n'est pas exhaustive.

- Utilisez l'instruction SET NOCOUNT ON comme première instruction dans le corps de la procédure. Autrement dit, placez-la juste après le mot clé AS. Cela permet de désactiver les messages renvoyés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au client une fois les instructions SELECT, INSERT, UPDATE, MERGE et DELETE exécutées. Cela permet de raccourcir au maximum la sortie générée pour plus de clarté. Toutefois, il n’existe pas d’avantages mesurables concernant les niveaux de performance du matériel actuel. Pour plus d’informations, consultez [SET NOCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-nocount-transact-sql.md).
- Utilisez des noms de schémas lorsque vous créez ou référencez des objets de base de données dans la procédure. Il faut moins de temps au [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour résoudre les noms d’objets s’il n’a pas à effectuer des recherches dans plusieurs schémas. Ceci évite également les problèmes d’autorisation et d’accès causés par le schéma par défaut d’un utilisateur qui est affecté lors de la création d’objets sans spécification du schéma.
- Évitez les fonctions de renvoi à la ligne pour les fonctions autour des colonnes spécifiées dans les clauses WHERE et JOIN. Les colonnes seront ainsi non déterministes, ce qui empêche le processeur de requêtes d'utiliser des index.
- Évitez d'utiliser des fonctions scalaires dans des instructions SELECT qui retournent un grand nombre de lignes de données. Étant donné que la fonction scalaire doit être appliquée à chaque ligne, le comportement s'apparente à un traitement par ligne et nuit aux performances.
- Évitez d’utiliser `SELECT *`. utilisez les noms de colonnes requis afin d'éviter que des erreurs du [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'interrompent l'exécution de la procédure. Par exemple, l’exécution d’une instruction `SELECT *` qui retourne des données à partir d’une table contenant 12 colonnes, puis insère ces données dans une table temporaire de 12 colonnes s’effectue sans problème jusqu’à ce que l’ordre ou le nombre des colonnes change dans l’une des tables.
- Évitez de traiter ou de retourner un trop grand nombre de données. Restreignez les résultats le plus tôt possible dans le code de la procédure afin que les opérations suivantes effectuées par la procédure impliquent le jeu de données le plus petit possible. Envoyez uniquement les données essentielles à l'application cliente. Cela s'avère plus efficace que l'envoi de données supplémentaires sur le réseau et l'obligation par l'application cliente de traiter inutilement des jeux de résultats volumineux.
- Utilisez des transactions explicites avec BEGIN/COMMIT TRANSACTION, et privilégiez, autant que possible, les transactions courtes. Les transactions plus longues entraînent un verrouillage plus long des enregistrements et un plus grand risque de blocage.
- Utilisez la fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] TRY...CATCH pour la gestion des erreurs au sein d’une procédure. TRY...CATCH peut encapsuler un bloc entier d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cela entraîne non seulement une moindre diminution des performances, mais contribue également à améliorer la création de rapports d'erreurs avec une programmation beaucoup moins lourde.
- Utilisez le mot clé DEFAULT sur toutes les colonnes de table qui sont référencées par des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE TABLE ou ALTER TABLE dans le corps de la procédure. Cela évite de passer une valeur NULL aux colonnes qui n’autorisent pas cette valeur.
- Utilisez la valeur NULL ou NOT NULL pour chaque colonne d'une table temporaire. Les options ANSI_DFLT_ON et ANSI_DFLT_OFF définissent la manière dont le [!INCLUDE[ssDE](../../includes/ssde-md.md)] assigne les attributs NULL ou NOT NULL aux colonnes, s'ils ne sont pas spécifiés dans une instruction CREATE TABLE ou ALTER TABLE. Si une connexion exécute une procédure avec des paramètres différents pour ces options de ceux utilisés pour la connexion à l'origine de la création de la procédure, les colonnes de la table créée par la seconde connexion peuvent avoir des valeurs NULL différentes et présenter ainsi des comportements différents. Si NULL ou NOT NULL est explicitement établi pour chaque colonne, les tables temporaires sont créées avec la même possibilité de valeurs NULL pour toutes les connexions qui exécutent la procédure.
- Utilisez des instructions de modification qui convertissent les valeurs Null et incluez une logique éliminant des requêtes les lignes contenant des valeurs Null. Sachez que dans [!INCLUDE[tsql](../../includes/tsql-md.md)], NULL n’est pas une valeur vide ou « Nothing ». Il s'agit d'un espace réservé à une valeur inconnue et peut être à l'origine d'un comportement inattendu, notamment lors de l'interrogation de jeux de résultats ou de l'utilisation de fonctions AGGREGATE.
- Utilisez l'opérateur UNION ALL au lieu des opérateurs UNION ou OR, sauf si vous avez besoin de valeurs distinctes. L'opérateur UNION ALL requiert moins de charge de traitement étant donné que les doublons ne sont pas filtrés dans le jeu de résultats.

## <a name="general-remarks"></a>Remarques d'ordre général

Il n'existe pas de taille maximale prédéfinie pour une procédure.

Les variables spécifiées dans la procédure peuvent être des variables système ou des variables définies par l’utilisateur, telles que @@SPID.

Lorsque vous exécutez une procédure pour la première fois, elle est compilée afin d'optimiser le plan d'accès pour la récupération des données. Des exécutions de la procédure postérieures peuvent entraîner la réutilisation du plan déjà généré s'il se trouve toujours dans le cache du plan du [!INCLUDE[ssDE](../../includes/ssde-md.md)].

Il est possible de lancer l'exécution automatique d'une ou plusieurs procédures au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les procédures doivent être créées par l’administrateur système dans la base de données **Master** et exécutées sous le rôle serveur fixe **sysadmin** comme processus en arrière-plan. Les procédures ne peuvent pas comprendre de paramètres d'entrée ou de sortie. Pour plus d’informations, consultez [Exécuter une procédure stockée](../../relational-databases/stored-procedures/execute-a-stored-procedure.md).

Les procédures sont imbriquées lorsqu'une procédure en appelle une autre ou exécute du code managé en faisant référence à une routine, un type ou un agrégat CLR. Vous pouvez imbriquer des procédures et des références au code managé jusqu'à 32 niveaux. L'imbrication augmente d'un niveau lorsque la procédure appelée ou la référence au code managé commence à s'exécuter, et diminue d'un niveau lorsque son exécution est terminée. Les méthodes appelées à partir du code managé n'entrent pas en compte dans la limite de niveau d'imbrication. Toutefois, lorsqu'une procédure stockée CLR exécute des opérations d'accès aux données par le biais du fournisseur managé de SQL Server, un niveau d'imbrication supplémentaire est ajouté à la transition du code managé vers SQL.

Au-delà du niveau d'imbrication maximal, toute la chaîne d'appels échoue. Vous pouvez utiliser la fonction @@NESTLEVEL pour retourner le niveau d’imbrication de l’exécution de procédure stockée actuelle.

## <a name="interoperability"></a>Interopérabilité

Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] enregistre les paramètres de SET QUOTED_IDENTIFIER et de SET ANSI_NULLS lors de la création ou de la modification d'une procédure [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ces paramètres d'origine sont utilisés lors de l'exécution de la procédure. Par conséquent, tous les paramètres de la session cliente pour SET QUOTED_IDENTIFIER et SET ANSI_NULLS sont ignorés lors de l'exécution de la procédure.

D'autres options SET, telles que SET ARITHABORT, SET ANSI_WARNINGS ou SET ANSI_PADDINGS ne sont pas sauvegardées lorsqu'une procédure est créée ou modifiée. Si la logique de la procédure dépend d'un paramétrage particulier, insérez une instruction SET au début de la procédure pour assurer un paramétrage adéquat. Lorsqu'une instruction SET est exécutée à partir d'une procédure, les paramètres ne restent effectifs que jusqu'à la fin de l'exécution de la procédure. Les paramètres reprennent ensuite la valeur qu'ils avaient avant l'appel de la procédure. Ceci permet aux clients individuels de définir les options souhaitées sans affecter la logique de la procédure.

Toute instruction SET peut être indiquée dans une procédure, sauf SET SHOWPLAN_TEXT et SET SHOWPLAN_ALL. Elles doivent être les seules instructions d'un lot. L'option SET choisie reste en vigueur durant l'exécution de la procédure, puis retrouve sa valeur d'origine.

> [!NOTE]
> L'option SET ANSI_WARNINGS n'est pas reconnue lors d'une transmission de paramètres dans une procédure ou dans une fonction définie par l'utilisateur, ou bien lors de la déclaration et de la définition de variables dans une instruction par lot. Par exemple, si une variable est définie comme **char**(3), puis réglée sur une valeur supérieure à trois caractères, les données sont tronquées à la taille définie et l’instruction INSERT ou UPDATE réussit.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

L'instruction CREATE PROCEDURE ne peut pas s'utiliser conjointement avec d'autres instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans un même lot.

Les instructions suivantes ne peuvent pas être utilisées dans le corps d'une procédure stockée.

| CREATE | SET | USE |
|--------|-----|-----|
| CREATE AGGREGATE | SET SHOWPLAN_TEXT | USE *database_name*|
| CREATE DEFAULT | SET SHOWPLAN_XML
| CREATE RULE | SET PARSEONLY |
| CREATE SCHEMA | SET SHOWPLAN_ALL |
| CREATE ou ALTER TRIGGER |
| CREATE ou ALTER FUNCTION |
| CREATE ou ALTER PROCEDURE |
| CREATE ou ALTER VIEW |

 Une procédure peut faire référence à des tables qui n'existent pas encore. Au moment de la création, seul le contrôle de la syntaxe est effectué. La procédure n'est compilée qu'à sa première exécution. Ce n'est qu'au moment de la compilation que la procédure résout les références aux objets. Par conséquent, une procédure syntaxiquement correcte faisant référence à des tables qui n’existent pas peut toujours être créée sans problème, mais son exécution échoue car les tables référencées n’existent pas.

 Vous ne pouvez pas spécifier un nom de fonction comme valeur par défaut d'un paramètre ou comme valeur transmise à un paramètre lors de l'exécution d'une procédure. En revanche, vous pouvez passer une fonction comme variable, comme indiqué dans l'exemple suivant.

```sql
-- Passing the function value as a variable.
DECLARE @CheckDate DATETIME = GETDATE();
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;
GO
```

Si la procédure apporte des modifications sur une instance distante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les modifications ne peuvent pas être restaurées. Les procédures distantes ne font pas partie des transactions.

Pour que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée une référence à la méthode appropriée lorsque ses capacités sont dépassées dans le .NET Framework, la méthode indiquée par la clause EXTERNAL NAME doit présenter les caractéristiques suivantes :

- elle doit être déclarée en tant que méthode statique ;
- elle doit compter le même nombre de paramètres que la procédure ;
- les types de paramètres utilisés doivent être compatibles avec ceux des paramètres correspondant de la procédure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur la correspondance des types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec ceux de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], consultez [Mappage des données de paramètres CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

## <a name="metadata"></a>Métadonnées

Le tableau suivant répertorie les affichages catalogue et vues de gestion dynamique que vous pouvez utiliser pour retourner des informations sur les procédures stockées.

|Affichage|Description|
|----------|-----------------|
|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|Retourne la définition d'une procédure [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le texte d’une procédure créée à l’aide de l’option ENCRYPTION ne peut pas être affiché par le biais de la vue de catalogue **sys.sql_modules**.|
|[sys.assembly_modules](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|Retourne des informations sur une procédure CLR.|
|[sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)|Retourne des informations sur les paramètres définis dans une procédure.|
|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)|Retourne les objets référencés par une procédure.|

Pour estimer la taille d'une procédure compilée, utilisez les compteurs de l'Analyseur de performances décrits ci-dessous.

|Nom de l'objet de l'Analyseur de performances|Nom du compteur de l'Analyseur de performances|
|-------------------------------------|--------------------------------------|
|SQLServer : objet Plan Cache|Taux d'accès au cache|
||Pages du cache|
||Nombre d'objets cache*|

\* Ces compteurs sont disponibles pour diverses catégories d'objets du cache, y compris [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, [!INCLUDE[tsql](../../includes/tsql-md.md)] préparé, procédures, déclencheurs, etc. Pour plus d’informations, consultez [SQL Server - Objet Plan Cache](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md).

## <a name="security"></a>Sécurité

### <a name="permissions"></a>Autorisations

Nécessite l’autorisation **CREATE PROCEDURE** dans la base de données et l’autorisation **ALTER** sur le schéma dans lequel la procédure est créée, ou nécessite l’appartenance au rôle de base de données fixe **db_ddladmin**.

Dans le cas de procédures stockées CLR, vous devez être propriétaire de l’assembly référencé dans la clause EXTERNAL NAME ou disposer de l’autorisation **REFERENCES** sur cet assembly.

## <a name="create-procedure-and-memory-optimized-tables"></a><a name="mot"></a> CREATE PROCEDURE et tables optimisées en mémoire

Les tables optimisées en mémoire sont accessibles à l’aide de procédures stockées traditionnelles et compilées en mode natif. Les procédures natives sont, dans la plupart des cas, la manière la plus efficace de procéder. Pour plus d’informations, consultez [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).

L’exemple suivant montre comment créer une procédure stockée compilée en mode natif qui accède à une table optimisée en mémoire `dbo.Departments` :

```sql
CREATE PROCEDURE dbo.usp_add_kitchen @dept_id INT, @kitchen_count INT NOT NULL
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION
AS
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')

UPDATE dbo.Departments
SET kitchen_count = ISNULL(kitchen_count, 0) + @kitchen_count
WHERE ID = @dept_id
END;
GO
```

 Une procédure créée sans NATIVE_COMPILATION ne peut pas être modifiée en une procédure stockée compilée en mode natif.

 Pour obtenir des informations complémentaires sur la programmabilité dans les procédures stockées compilées en mode natif, la surface d’exposition de requête prise en charge et les opérateurs, consultez [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).

## <a name="examples"></a><a name="Examples"></a> Exemples

|Category|Éléments syntaxiques proposés|
|--------------|------------------------------|
|[Syntaxe de base](#BasicSyntax)|CREATE PROCEDURE|
|[Passage de paramètres](#Parameters)|@parameter <br> &nbsp;&nbsp;• = par défaut <br> &nbsp;&nbsp; • OUTPUT <br> &nbsp;&nbsp; • type de paramètre table <br> &nbsp;&nbsp; • CURSOR VARYING|
|[Modification des données à l’aide d’une procédure stockée](#Modify)|UPDATE|
|[Gestion des erreurs](#Error)|TRY...CATCH|
|[Obscurcissement de la définition de procédure](#Encrypt)|WITH ENCRYPTION|
|[Recompilation forcée de la procédure](#Recompile)|WITH RECOMPILE|
|[Définition du contexte de sécurité](#Security)|EXECUTE AS|

### <a name="basic-syntax"></a><a name="BasicSyntax"></a> Syntaxe de base

Les exemples fournis dans cette section présentent les fonctionnalités de base de l'instruction CREATE PROCEDURE en utilisant la syntaxe minimale requise.

#### <a name="a-creating-a-simple-transact-sql-procedure"></a>R. Création d'une procédure Transact-SQL simple

L'exemple suivant crée une procédure stockée qui retourne tous les employés (prénom et nom), leur poste et le nom de leur service à partir d'une vue de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Cette procédure n'utilise aucun paramètre. L'exemple illustre ensuite trois méthodes permettant d'exécuter la procédure.

```sql
CREATE PROCEDURE HumanResources.uspGetAllEmployees
AS
    SET NOCOUNT ON;
    SELECT LastName, FirstName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment;
GO

SELECT * FROM HumanResources.vEmployeeDepartment;
```

La procédure `uspGetEmployees` peut être exécutée des manières suivantes :

```sql
EXECUTE HumanResources.uspGetAllEmployees;
GO
-- Or
EXEC HumanResources.uspGetAllEmployees;
GO
-- Or, if this procedure is the first statement within a batch:
HumanResources.uspGetAllEmployees;
```

#### <a name="b-returning-more-than-one-result-set"></a>B. Renvoi de plusieurs jeux de résultats

La procédure suivante renvoie deux jeux de résultats.

```sql
CREATE PROCEDURE dbo.uspMultipleResults
AS
SELECT TOP(10) BusinessEntityID, Lastname, FirstName FROM Person.Person;
SELECT TOP(10) CustomerID, AccountNumber FROM Sales.Customer;
GO
```

#### <a name="c-creating-a-clr-stored-procedure"></a>C. Création d'une procédure stockée CLR

L’exemple suivant crée la procédure `GetPhotoFromDB` qui fait référence à la méthode `GetPhotoFromDB` de la classe `LargeObjectBinary` se trouvant dans l’assembly `HandlingLOBUsingCLR`. Avant la création de la procédure, l’assembly `HandlingLOBUsingCLR` est inscrit dans la base de données locale.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] (en cas d’utilisation d’un assembly créé à partir d’*assembly_bits*).

```sql
CREATE ASSEMBLY HandlingLOBUsingCLR
FROM '\\MachineName\HandlingLOBUsingCLR\bin\Debug\HandlingLOBUsingCLR.dll';
GO
CREATE PROCEDURE dbo.GetPhotoFromDB
(
    @ProductPhotoID INT
    , @CurrentDirectory NVARCHAR(1024)
    , @FileName NVARCHAR(1024)
)
AS EXTERNAL NAME HandlingLOBUsingCLR.LargeObjectBinary.GetPhotoFromDB;
GO
```

### <a name="passing-parameters"></a><a name="Parameters"></a> Passage de paramètres

Les exemples présentés dans cette section montrent comment utiliser des paramètres d'entrée et de sortie pour transmettre des valeurs vers et à partir d'une procédure stockée.

#### <a name="d-creating-a-procedure-with-input-parameters"></a>D. Création d'une procédure avec des paramètres d'entrée

L'exemple suivant crée une procédure stockée qui retourne des informations pour un employé spécifique en passant des valeurs pour le prénom et le nom de l'employé. Cette procédure accepte uniquement les correspondances exactes pour les paramètres passés.

```sql
IF OBJECT_ID ( 'HumanResources.uspGetEmployees', 'P' ) IS NOT NULL
    DROP PROCEDURE HumanResources.uspGetEmployees;
GO
CREATE PROCEDURE HumanResources.uspGetEmployees
    @LastName NVARCHAR(50),
    @FirstName NVARCHAR(50)
AS

    SET NOCOUNT ON;
    SELECT FirstName, LastName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment
    WHERE FirstName = @FirstName AND LastName = @LastName;
GO

```

La procédure `uspGetEmployees` peut être exécutée des manières suivantes :

```sql
EXECUTE HumanResources.uspGetEmployees N'Ackerman', N'Pilar';
-- Or
EXEC HumanResources.uspGetEmployees @LastName = N'Ackerman', @FirstName = N'Pilar';
GO
-- Or
EXECUTE HumanResources.uspGetEmployees @FirstName = N'Pilar', @LastName = N'Ackerman';
GO
-- Or, if this procedure is the first statement within a batch:
HumanResources.uspGetEmployees N'Ackerman', N'Pilar';

```

#### <a name="e-using-a-procedure-with-wildcard-parameters"></a>E. Utilisation d'une procédure avec des paramètres génériques

L'exemple suivant crée une procédure stockée qui retourne des informations pour des employés en passant des valeurs complètes ou partielles pour le prénom et le nom de l'employé. Ce modèle de procédure fait correspondre les paramètres passés ou, s’ils ne sont pas fournis, utilise les valeurs par défaut prédéfinies (dont le nom commence par la lettre `D`).

```sql
IF OBJECT_ID ( 'HumanResources.uspGetEmployees2', 'P' ) IS NOT NULL
    DROP PROCEDURE HumanResources.uspGetEmployees2;
GO
CREATE PROCEDURE HumanResources.uspGetEmployees2
    @LastName NVARCHAR(50) = N'D%',
    @FirstName NVARCHAR(50) = N'%'
AS
    SET NOCOUNT ON;
    SELECT FirstName, LastName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment
    WHERE FirstName LIKE @FirstName AND LastName LIKE @LastName;
```

L’exécution de la procédure `uspGetEmployees2` peut s’effectuer selon plusieurs combinaisons. Vous trouverez ci-dessous certaines des combinaisons possibles.

```sql
EXECUTE HumanResources.uspGetEmployees2;
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'Wi%';
-- Or
EXECUTE HumanResources.uspGetEmployees2 @FirstName = N'%';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'[CK]ars[OE]n';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'Hesse', N'Stefen';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'H%', N'S%';
```

#### <a name="f-using-output-parameters"></a>F. Utilisation des paramètres OUTPUT

L'exemple suivant crée la procédure `uspGetList`. Cette procédure retourne une liste de produits dont le prix ne dépasse pas un montant précisé. L'exemple illustre l'utilisation de plusieurs instructions `SELECT` et de plusieurs paramètres `OUTPUT`. Les paramètres OUTPUT permettent à une procédure externe, un lot ou à plus d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] d'accéder à un ensemble de valeurs pendant l'exécution de la procédure.

```sql
IF OBJECT_ID ( 'Production.uspGetList', 'P' ) IS NOT NULL
    DROP PROCEDURE Production.uspGetList;
GO  
CREATE PROCEDURE Production.uspGetList @Product VARCHAR(40)
    , @MaxPrice MONEY
    , @ComparePrice MONEY OUTPUT
    , @ListPrice MONEY OUT
AS  
    SET NOCOUNT ON;
    SELECT p.[Name] AS Product, p.ListPrice AS 'List Price'
    FROM Production.Product AS p
    JOIN Production.ProductSubcategory AS s
      ON p.ProductSubcategoryID = s.ProductSubcategoryID
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice;
-- Populate the output variable @ListPprice.
SET @ListPrice = (SELECT MAX(p.ListPrice)
    FROM Production.Product AS p
    JOIN Production.ProductSubcategory AS s
      ON p.ProductSubcategoryID = s.ProductSubcategoryID
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice);
-- Populate the output variable @compareprice.
SET @ComparePrice = @MaxPrice;
GO
```

Exécutez `uspGetList` afin de retourner la liste des produits (vélos) provenant de [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] et coûtant moins de `$700`. Les paramètres `OUTPUT``@Cost` et `@ComparePrices` sont utilisés en conjonction avec un langage de contrôle de flux afin de retourner un message dans la fenêtre **Messages**.

> [!NOTE]
> La variable OUTPUT doit être définie lors de la création de la procédure et de l'utilisation de la variable. Le nom du paramètre et le nom de la variable ne doivent pas nécessairement correspondre, contrairement au type de données et à la position du paramètre (sauf si vous utilisez `@ListPrice` = *variable*).

```sql
DECLARE @ComparePrice MONEY, @Cost MONEY;
EXECUTE Production.uspGetList '%Bikes%', 700,
    @ComparePrice OUT,
    @Cost OUTPUT
IF @Cost <= @ComparePrice
BEGIN
    PRINT 'These products can be purchased for less than
    $'+RTRIM(CAST(@ComparePrice AS VARCHAR(20)))+'.'
END
ELSE
    PRINT 'The prices for all products in this category exceed
    $'+ RTRIM(CAST(@ComparePrice AS VARCHAR(20)))+'.';
```

Voici le jeu de résultats partiel :

```
Product                     List Price
--------------------------  ----------
Road-750 Black, 58          539.99
Mountain-500 Silver, 40     564.99
Mountain-500 Silver, 42     564.99
...
Road-750 Black, 48          539.99
Road-750 Black, 52          539.99

(14 row(s) affected)

These items can be purchased for less than $700.00.
```

#### <a name="g-using-a-table-valued-parameter"></a>G. Utilisation d'un paramètre table

L'exemple suivant utilise un type de paramètre table pour insérer plusieurs lignes dans une table. L'exemple crée le type de paramètre, déclare une variable de table pour y faire référence, remplit la liste de paramètres, puis passe les valeurs à une procédure stockée. La procédure stockée utilise les valeurs pour insérer plusieurs lignes dans une table.

```sql
/* Create a table type. */
CREATE TYPE LocationTableType AS TABLE
( LocationName VARCHAR(50)
, CostRate INT );
GO

/* Create a procedure to receive data for the table-valued parameter. */
CREATE PROCEDURE usp_InsertProductionLocation
    @TVP LocationTableType READONLY
    AS
    SET NOCOUNT ON
    INSERT INTO [AdventureWorks2012].[Production].[Location]
       ([Name]
       , [CostRate]
       , [Availability]
       , [ModifiedDate])
    SELECT *, 0, GETDATE()
    FROM @TVP;
GO

/* Declare a variable that references the type. */
DECLARE @LocationTVP
AS LocationTableType;

/* Add data to the table variable. */
INSERT INTO @LocationTVP (LocationName, CostRate)
    SELECT [Name], 0.00
    FROM
    [AdventureWorks2012].[Person].[StateProvince];

/* Pass the table variable data to a stored procedure. */
EXEC usp_InsertProductionLocation @LocationTVP;
GO
```

##### <a name="h-using-an-output-cursor-parameter"></a>H. Utilisation d'un paramètre OUTPUT de type cursor

L'exemple suivant utilise le paramètres de type cursor OUTPUT pour renvoyer un curseur local à une procédure, au lot appelant, à la procédure ou au déclencheur.

Commencez par créer la procédure qui déclare un curseur puis l'ouvre dans la table `Currency` :

```sql
CREATE PROCEDURE dbo.uspCurrencyCursor
    @CurrencyCursor CURSOR VARYING OUTPUT
AS
    SET NOCOUNT ON;
    SET @CurrencyCursor = CURSOR
    FORWARD_ONLY STATIC FOR
      SELECT CurrencyCode, Name
      FROM Sales.Currency;
    OPEN @CurrencyCursor;
GO
```

Ensuite, exécutez un lot qui déclare une variable locale de type cursor, exécute la procédure pour affecter le curseur à la variable locale et extrait les lignes du curseur.

```sql
DECLARE @MyCursor CURSOR;
EXEC dbo.uspCurrencyCursor @CurrencyCursor = @MyCursor OUTPUT;
WHILE (@@FETCH_STATUS = 0)
BEGIN;
    FETCH NEXT FROM @MyCursor;
END;
CLOSE @MyCursor;
DEALLOCATE @MyCursor;
GO
```

### <a name="modifying-data-by-using-a-stored-procedure"></a><a name="Modify"></a> Modification de données à l’aide d’une procédure stockée

Les exemples présentés dans cette section montrent comment insérer ou modifier des données dans des tables ou des vues en incluant une instruction DML (Data Manipulation Language) dans la définition de la procédure.

#### <a name="i-using-update-in-a-stored-procedure"></a>I. Utilisation de l'instruction UPDATE dans une procédure stockée

L'exemple ci-dessous utilise une instruction UPDATE dans une procédure stockée. La procédure accepte un paramètre d'entrée, `@NewHours` et un paramètre de sortie `@RowCount`. La valeur du paramètre `@NewHours` est utilisée dans l’instruction UPDATE pour mettre à jour la colonne `VacationHours` de la table `HumanResources.Employee`. Le paramètre de sortie `@RowCount` est utilisé pour retourner le nombre de lignes affectées à une variable locale. Une expression CASE est utilisée dans la clause SET pour déterminer de manière conditionnelle la valeur définie pour `VacationHours`. Lorsque l'employé est payé à l'heure (`SalariedFlag` = 0), `VacationHours` est défini avec le nombre actuel d'heures plus la valeur spécifiée dans `@NewHours` ; sinon, `VacationHours` est défini avec la valeur spécifiée dans `@NewHours`.

```sql
CREATE PROCEDURE HumanResources.Update_VacationHours
@NewHours SMALLINT, @Rowcount INT OUTPUT
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
SET @Rowcount = @@rowcount;

GO
DECLARE @Rowcount INT
EXEC HumanResources.Update_VacationHours 40, @Rowcount OUTPUT
PRINT @Rowcount;
```

### <a name="error-handling"></a><a name="Error"></a> Gestion des erreurs

Les exemples de cette section présentent des méthodes pour gérer les erreurs qui peuvent se produire lorsque la procédure stockée est exécutée.

#### <a name="j-using-trycatch"></a>J. Utilisation de TRY...CATCH

L’exemple suivant illustre l’utilisation de la construction TRY...CATCH pour retourner des informations sur les erreurs interceptées pendant l’exécution d’une procédure stockée.

```sql
CREATE PROCEDURE Production.uspDeleteWorkOrder ( @WorkOrderID INT )
AS
SET NOCOUNT ON;
BEGIN TRY
  BEGIN TRANSACTION
  -- Delete rows from the child table, WorkOrderRouting, for the specified work order.
    DELETE FROM Production.WorkOrderRouting
    WHERE WorkOrderID = @WorkOrderID;
  -- Delete the rows from the parent table, WorkOrder, for the specified work order.
    DELETE FROM Production.WorkOrder
    WHERE WorkOrderID = @WorkOrderID;
  COMMIT
END TRY

BEGIN CATCH
  -- Determine if an error occurred.
  IF @@TRANCOUNT > 0
    ROLLBACK

  -- Return the error information.
  DECLARE @ErrorMessage NVARCHAR(4000), @ErrorSeverity INT;
  SELECT @ErrorMessage = ERROR_MESSAGE(),@ErrorSeverity = ERROR_SEVERITY();
  RAISERROR(@ErrorMessage, @ErrorSeverity, 1);
END CATCH;

GO
EXEC Production.uspDeleteWorkOrder 13;

/* Intentionally generate an error by reversing the order in which rows
   are deleted from the parent and child tables. This change does not
   cause an error when the procedure definition is altered, but produces
   an error when the procedure is executed.
*/
ALTER PROCEDURE Production.uspDeleteWorkOrder ( @WorkOrderID INT )
AS

BEGIN TRY
  BEGIN TRANSACTION
  -- Delete the rows from the parent table, WorkOrder, for the specified work order.
    DELETE FROM Production.WorkOrder
    WHERE WorkOrderID = @WorkOrderID;

  -- Delete rows from the child table, WorkOrderRouting, for the specified work order.
    DELETE FROM Production.WorkOrderRouting
    WHERE WorkOrderID = @WorkOrderID;
  COMMIT TRANSACTION
END TRY

BEGIN CATCH
  -- Determine if an error occurred.
  IF @@TRANCOUNT > 0
    ROLLBACK TRANSACTION

  -- Return the error information.
  DECLARE @ErrorMessage NVARCHAR(4000), @ErrorSeverity INT;
  SELECT @ErrorMessage = ERROR_MESSAGE(),@ErrorSeverity = ERROR_SEVERITY();
  RAISERROR(@ErrorMessage, @ErrorSeverity, 1);
END CATCH;
GO
-- Execute the altered procedure.
EXEC Production.uspDeleteWorkOrder 15;

DROP PROCEDURE Production.uspDeleteWorkOrder;
```

### <a name="obfuscating-the-procedure-definition"></a><a name="Encrypt"></a> Obscurcissement de la définition de procédure

Les exemples de cette section illustrent comment obscurcir la définition de la procédure stockée.

#### <a name="k-using-the-with-encryption-option"></a>K. Utilisation de l'option WITH ENCRYPTION

L'exemple suivant crée la procédure `HumanResources.uspEncryptThis`.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, SQL Database.

```sql
CREATE PROCEDURE HumanResources.uspEncryptThis
WITH ENCRYPTION
AS
    SET NOCOUNT ON;
    SELECT BusinessEntityID, JobTitle, NationalIDNumber,
        VacationHours, SickLeaveHours
    FROM HumanResources.Employee;
GO
```

L’option `WITH ENCRYPTION` obfusque la définition de la procédure lors de l’interrogation du catalogue système ou de l’utilisation de fonctions de métadonnées, comme indiqué dans les exemples suivants.

Exécutez `sp_helptext` :

```sql
EXEC sp_helptext 'HumanResources.uspEncryptThis';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

`The text for object 'HumanResources.uspEncryptThis' is encrypted.`

Interrogez directement la vue de catalogue `sys.sql_modules` :

```sql
SELECT definition FROM sys.sql_modules
WHERE object_id = OBJECT_ID('HumanResources.uspEncryptThis');
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
definition
--------------------------------
NULL
```

### <a name="forcing-the-procedure-to-recompile"></a><a name="Recompile"></a> Recompilation forcée de la procédure

Les exemples de cette section utilisent la clause WITH RECOMPILE pour forcer la recompilation de la procédure chaque fois qu'elle est exécutée.

#### <a name="l-using-the-with-recompile-option"></a>L. Utilisation de l'option WITH RECOMPILE

La clause `WITH RECOMPILE` est utile quand les paramètres fournis à la procédure ne sont pas typiques et qu’un nouveau plan d’exécution ne doit pas être mis en cache ou stocké en mémoire.

```sql
IF OBJECT_ID ( 'dbo.uspProductByVendor', 'P' ) IS NOT NULL
    DROP PROCEDURE dbo.uspProductByVendor;
GO
CREATE PROCEDURE dbo.uspProductByVendor @Name VARCHAR(30) = '%'
WITH RECOMPILE
AS
    SET NOCOUNT ON;
    SELECT v.Name AS 'Vendor name', p.Name AS 'Product name'
    FROM Purchasing.Vendor AS v
    JOIN Purchasing.ProductVendor AS pv
      ON v.BusinessEntityID = pv.BusinessEntityID
    JOIN Production.Product AS p
      ON pv.ProductID = p.ProductID
    WHERE v.Name LIKE @Name;
```

### <a name="setting-the-security-context"></a><a name="Security"></a> Définition du contexte de sécurité

Les exemples de cette section utilisent la clause EXECUTE AS pour définir le contexte de sécurité dans lequel la procédure stockée est exécutée.

#### <a name="m-using-the-execute-as-clause"></a>M. Utilisation de la clause EXECUTE AS

L’exemple suivant illustre l’utilisation de la clause [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) afin d’indiquer le contexte de sécurité dans lequel une procédure peut être exécutée. Dans cet exemple, l’option `CALLER` spécifie que la procédure peut être exécutée dans le contexte de l’utilisateur qui l’appelle.

```sql
CREATE PROCEDURE Purchasing.uspVendorAllInfo
WITH EXECUTE AS CALLER
AS
    SET NOCOUNT ON;
    SELECT v.Name AS Vendor, p.Name AS 'Product name',
      v.CreditRating AS 'Rating',
      v.ActiveFlag AS Availability
    FROM Purchasing.Vendor v
    INNER JOIN Purchasing.ProductVendor pv
      ON v.BusinessEntityID = pv.BusinessEntityID
    INNER JOIN Production.Product p
      ON pv.ProductID = p.ProductID
    ORDER BY v.Name ASC;
GO
```

#### <a name="n-creating-custom-permission-sets"></a>N. Création de jeux d'autorisations personnalisés

L'exemple suivant utilise EXECUTE AS pour créer des autorisations personnalisées pour une opération de base de données. Il n'est pas possible d'accorder des autorisations à certaines opérations, telles que TRUNCATE TABLE. En intégrant l'instruction TRUNCATE TABLE dans une procédure stockée et en spécifiant que cette procédure s'exécute en tant qu'utilisateur disposant des autorisations de modifier la table, vous pouvez étendre les autorisations de tronquer la table à l'utilisateur auquel vous accordez les autorisations EXECUTE sur la procédure.

```sql
CREATE PROCEDURE dbo.TruncateMyTable
WITH EXECUTE AS SELF
AS TRUNCATE TABLE MyDB..MyTable;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="o-create-a-stored-procedure-that-runs-a-select-statement"></a>O. Créer une procédure stockée qui exécute une instruction SELECT

Cet exemple montre la syntaxe de base pour créer et exécuter une procédure. Lors de l’exécution d’un traitement, CREATE PROCEDURE doit être la première instruction. Par exemple, pour créer la procédure stockée suivante dans [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)], définissez d’abord le contexte de base de données, puis exécutez l’instruction CREATE PROCEDURE.

```sql
-- Uses AdventureWorksDW database

--Run CREATE PROCEDURE as the first statement in a batch.
CREATE PROCEDURE Get10TopResellers
AS
BEGIN
    SELECT TOP (10) r.ResellerName, r.AnnualSales
    FROM DimReseller AS r
    ORDER BY AnnualSales DESC, ResellerName ASC;
END
;
GO

--Show 10 Top Resellers
EXEC Get10TopResellers;
```

## <a name="see-also"></a>Voir aussi

- [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)
- [Langage de contrôle de flux &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)
- [Curseurs](../../relational-databases/cursors.md)
- [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
- [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)
- [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)
- [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)
- [Procédures stockées &#40;moteur de base de données &#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)
- [sp_procoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md)
- [sp_recompile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md)
- [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)
- [sys.parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)
- [sys.procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)
- [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)
- [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)
- [sys.numbered_procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md)
- [sys.numbered_procedure_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-numbered-procedure-parameters-transact-sql.md)
- [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)
- [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)
- [Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)
- [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)
- [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
