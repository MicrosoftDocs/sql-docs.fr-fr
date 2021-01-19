---
description: Indicateurs (Transact-SQL) - Table
title: Indicateurs de table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TABLE_HINT_TSQL
- Table Hint
dev_langs:
- TSQL
helpviewer_keywords:
- SERIALIZABLE table hint
- UPDLOCK table hint
- HOLDLOCK table hint
- TABLOCKX table hint
- READUNCOMMITTED table hint
- hints [SQL Server], tables
- READCOMMITTEDLOCK table hint
- FORCESCAN table hint
- ROWLOCK table hint
- XLOCK table hint
- NOLOCK table hint
- READPAST table hint
- NOWAIT table hint
- FORCESEEK table hint
- table hints [SQL Server]
- TABLOCK table hint
- REPEATABLEREAD table hint
- READ COMMITTED table hint
- NOEXPAND table hint
- PAGLOCK table hint
ms.assetid: 8bf1316f-c0ef-49d0-90a7-3946bc8e7a89
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2043bf4c60a1154b719d81b583d055b60b85c6ec
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172331"
---
# <a name="hints-transact-sql---table"></a>Indicateurs (Transact-SQL) - Table
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Les indicateurs de table se substituent au comportement par défaut de l’optimiseur de requête pendant la durée de l’instruction DML (Data Manipulation Language, langage de manipulation de données) en spécifiant une méthode de verrouillage, un ou plusieurs index, une opération de traitement de requête telle qu’une analyse de table ou une recherche d’index, ou d’autres options. Les indicateurs de table sont spécifiés dans la clause FROM de l'instruction DML et n'affectent que la table ou vue référencée dans cette clause.  
  
> [!CAUTION]  
>  Étant donné que l'optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionne généralement le meilleur plan d'exécution pour une requête, nous vous recommandons de ne recourir à ces conseils qu'en dernier ressort et seulement si vous êtes un développeur ou un administrateur de base de données expérimenté.  
  
 **S’applique à :**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
 [SELECT](../../t-sql/queries/select-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 [MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
WITH  ( <table_hint> [ [, ]...n ] )  
  
<table_hint> ::=   
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ] 
  | FORCESCAN  
  | FORCESEEK  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT   
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK   
}   
  
<table_hint_limited> ::=  
{  
    KEEPIDENTITY   
  | KEEPDEFAULTS   
  | HOLDLOCK   
  | IGNORE_CONSTRAINTS   
  | IGNORE_TRIGGERS   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT   
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK   
}   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
WITH **(** \<table_hint> **)** [ [ **,** ]...*n* ]  
À quelques exceptions près, les indicateurs de table sont pris en charge dans la clause FROM uniquement lorsque les indicateurs sont spécifiés à l'aide du mot clé WITH. En outre, les indicateurs de table doivent être spécifiés avec des parenthèses.  
  
> [!IMPORTANT]  
> L'omission du mot clé WITH est une fonctionnalité déconseillée : [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
Les indicateurs de table suivants sont autorisés avec et sans le mot clé `WITH` : `NOLOCK`, `READUNCOMMITTED`, `UPDLOCK`, `REPEATABLEREAD`, `SERIALIZABLE`, `READCOMMITTED`, `TABLOCK`, `TABLOCKX`, `PAGLOCK`, `ROWLOCK`, `NOWAIT`, `READPAST`, `XLOCK`, `SNAPSHOT` et `NOEXPAND`. Lorsque ces indicateurs de table sont spécifiés sans le mot clé WITH, ils doivent être définis seuls. Par exemple :  
  
```sql  
FROM t (TABLOCK)  
```  
  
Lorsque l'indicateur est spécifié avec une autre option, il doit être défini avec le mot clé WITH :  
  
```sql  
FROM t WITH (TABLOCK, INDEX(myindex))  
```  
  
Nous vous recommandons d'utiliser des virgules entre les indicateurs de table.  
  
> [!IMPORTANT]  
> La séparation des indicateurs par des espaces à la place de virgules est une fonctionnalité déconseillée : [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
NOEXPAND  
Spécifie qu'aucune vue indexée n'est étendue pour permettre d'accéder aux tables sous-jacentes lorsque l'optimiseur de requête traite la requête. L'optimiseur de requête traite la vue comme une table avec un index cluster. NOEXPAND s'applique uniquement aux vues indexées. Pour plus d’informations, consultez [Utilisation de NOEXPAND](#using-noexpand).  
  
INDEX  **(** _<index\_value>_ [ **,** ... _n_ ] ) | INDEX =  ( _<index\_value>_ **)**  
La syntaxe INDEX() spécifie les noms ou les ID d'un ou de plusieurs index qui seront utilisés par l'optimiseur de requête lors du traitement de l'instruction. L'autre syntaxe `INDEX =` spécifie une seule valeur d'index. Un seul indicateur d'index par table peut être spécifié.  
  
S'il existe un index cluster, `INDEX(0)` force l'analyse de ce dernier, tandis que `INDEX(1)` en force l'analyse ou la recherche. S'il n'existe pas d'index cluster, `INDEX(0)` force l'analyse d'une table et `INDEX(1)` est interprété comme une erreur.  
  
 Si plusieurs index sont utilisés dans une seule liste d'indicateurs, les doublons sont ignorés et les autres index répertoriés sont utilisés pour récupérer les lignes de la table. L'ordre des index dans l'indicateur d'index est très important. Un indicateur associé à plusieurs index met également en œuvre l'opérateur logique AND et l'optimiseur de requête applique autant de conditions que possible sur chaque index accessible. Si la collection d'index avec indicateur n'inclut pas toutes les colonnes référencées par la requête, une extraction est effectuée pour récupérer les colonnes restantes après que le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] SQL Server a récupéré toutes les colonnes indexées.  
  
> [!NOTE]  
> Lorsqu'une option d'index faisant référence à plusieurs index est utilisée sur la table de faits dans une jointure en étoile, l'optimiseur de requête ignore l'option d'index et retourne un message d'avertissement. De même, la réunion logique d'index n'est pas autorisée pour une table avec une option d'index spécifiée.  
  
 Le nombre maximal d'index dans l'indicateur de table est de 250 index non cluster.  
  
KEEPIDENTITY  
Applicable uniquement dans une instruction INSERT quand l’option BULK est utilisée avec [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  
  
 Indique que la ou les valeurs d'identité figurant dans le fichier de données importé doivent être utilisées dans la colonne d'identité. Si KEEPIDENTITY n'est pas spécifié, les valeurs d'identité de cette colonne sont vérifiées mais pas importées, et l'optimiseur de requête affecte automatiquement des valeurs uniques en fonction d'une valeur initiale et d'un incrément spécifié lors de la création de la table.  
  
> [!IMPORTANT]  
> Si le fichier de données ne contient pas de valeurs pour la colonne d'identité de la table ou de la vue, et que cette colonne n'est pas la dernière colonne de la table, vous devez ignorer cette colonne. Pour plus d’informations, consultez [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md). Si une colonne d’identité est ignorée, l’optimiseur de requête assigne automatiquement des valeurs uniques pour la colonne d’identité dans les lignes de table importées.  
  
Pour obtenir un exemple qui utilise cet indicateur dans une instruction `INSERT ... SELECT * FROM OPENROWSET(BULK...)`, consultez [Conserver des valeurs d’identité lors de l’importation de données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md).  
  
Pour plus d’informations sur la vérification de la valeur d’identité d’une table, consultez [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).  
  
KEEPDEFAULTS  
Applicable uniquement dans une instruction INSERT quand l’option BULK est utilisée avec [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  
  
Spécifie l'insertion d'une valeur par défaut éventuelle de colonne de table, à la place de la valeur NULL, lorsqu'il manque une valeur pour la colonne dans l'enregistrement de données.  
  
Pour un exemple d’utilisation de cet indicateur dans une instruction INSERT ... SELECT * FROM OPENROWSET(BULK...), consultez [Conserver les valeurs NULL ou utiliser les valeurs par défaut lors de l’importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
FORCESEEK [ **(** _<index\_value>_ **(** _<index\_column\_name>_ [ **,** ... _n_ ] **))** ]  
Indique que l'optimiseur de requête doit utiliser uniquement une opération de recherche d'index comme chemin d'accès aux données dans la table ou la vue. 

> [!NOTE]
> À partir de [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP1, il est également possible de spécifier des paramètres d’index. Dans ce cas, l'optimiseur de requête considère uniquement les opérations de recherche d'index par le biais de l'index spécifié utilisant au moins les colonnes d'index spécifiées.  
  
 *index_value*  
 Nom de l'index ou valeur d'ID de l'index. L'ID d'index 0 (segment) ne peut pas être spécifié. Pour retourner le nom ou l’ID de l’index, effectuez une requête sur la vue de catalogue **sys.indexes**.  
  
 *index_column_name*  
 Nom de la colonne d'index à inclure dans l'opération de recherche. La spécification de FORCESEEK avec des paramètres d'index s'apparente à l'utilisation de FORCESEEK avec un indicateur d'index. Toutefois, vous pouvez obtenir un meilleur contrôle du chemin d'accès utilisé par l'optimiseur de requête en spécifiant l'index sur lequel effectuer la recherche et les colonnes d'index à prendre en compte dans l'opération de recherche. L'optimiseur peut prendre en compte des colonnes supplémentaires si nécessaire. Par exemple, si un index non cluster est spécifié, l'optimiseur peut choisir d'utiliser des colonnes clés d'index cluster en plus des colonnes spécifiées.  
  
L'indicateur FORCESEEK peut être spécifié des manières suivantes.  
  
|Syntaxe| Exemple|Description|  
|------------|-------------|-----------------|  
|Sans index ou indicateur INDEX|`FROM dbo.MyTable WITH (FORCESEEK)`|L'optimiseur de requête considère uniquement les opérations de recherche d'index pour accéder à la table ou la vue par le biais de tout index approprié.|  
|Combiné avec un indicateur INDEX|`FROM dbo.MyTable WITH (FORCESEEK, INDEX (MyIndex))`|L'optimiseur de requête considère uniquement les opérations de recherche d'index pour accéder à la table ou la vue par le biais de l'index spécifié.|  
|Paramétrable en spécifiant un index et des colonnes d'index|`FROM dbo.MyTable WITH (FORCESEEK (MyIndex (col1, col2, col3)))`|L'optimiseur de requête considère uniquement les opérations de recherche pour accéder à la table ou la vue par le biais de l'index spécifié utilisant au moins les colonnes d'index spécifiées.|  
  
Si vous utilisez l’indicateur FORCESEEK (avec ou sans paramètres d’index), prenez en compte les recommandations suivantes :  
-   L'indicateur peut être spécifié comme indicateur de table ou indicateur de requête. Pour plus d’informations sur les indicateurs de requête, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
-   Pour appliquer FORCESEEK à une vue indexée, l'indicateur NOEXPAND doit également être spécifié.  
-   L'indicateur peut être appliqué au plus une fois par table ou vue.  
-   L'indicateur ne peut pas être spécifié pour une source de données distante. L'erreur 7377 est retournée lorsque FORCESEEK est spécifié avec un indicateur d'index et l'erreur 8180 est retournée lorsque FORCESEEK est utilisé sans indicateur d'index.  
-   Si FORCESEEK empêche de trouver un plan, l'erreur 8622 est retournée.  
  
Lorsque FORCESEEK est spécifié avec des paramètres d’index, les instructions et les restrictions suivantes s’appliquent :  
-   L'indicateur ne peut pas être spécifié pour une table qui est la cible d'une instruction INSERT, UPDATE ou DELETE.  
-   L'indicateur ne peut pas être spécifié en association avec un indicateur INDEX ou un autre indicateur FORCESEEK.  
-   Au moins une colonne doit être spécifiée et il doit s'agir de la première colonne principale.  
-   Des colonnes supplémentaires d'index peuvent être spécifiées, toutefois les colonnes clés ne peuvent pas être ignorées. Par exemple, si l'index spécifié contient les colonnes clés `a`, `b`, et `c`, la syntaxe valide inclurait `FORCESEEK (MyIndex (a))` et `FORCESEEK (MyIndex (a, b)`. La syntaxe incorrecte inclurait `FORCESEEK (MyIndex (c))` et `FORCESEEK (MyIndex (a, c)`.  
-   L'ordre des noms de colonnes spécifiés dans l'indicateur doit correspondre à l'ordre des colonnes dans l'index référencé.  
-   Les colonnes qui ne sont pas dans la définition de clé d'index ne peuvent pas être spécifiées. Par exemple, dans un index non cluster, seules les colonnes clés d'index définies peuvent être spécifiées. Les colonnes clés cluster qui sont automatiquement incluses dans l'index ne peuvent pas être spécifiées, mais elles peuvent être utilisées par l'optimiseur.  
-   Un index columnstore optimisé en mémoire xVelocity ne peut pas être spécifié comme paramètre d'index. L’erreur 366 est retournée.  
-   La modification de la définition d'index (par exemple en ajoutant ou en supprimant des colonnes) peut imposer des modifications des requêtes qui font référence à cet index.  
-   L'indicateur empêche l'optimiseur de considérer tout index spatial ou XML sur la table.  
-   L'indicateur ne peut pas être spécifié en association avec l'indicateur FORCESCAN.  
-   Pour les index partitionnés, la colonne de partitionnement implicitement ajoutée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être spécifiée dans l'indicateur FORCESEEK.  
  
> [!CAUTION]  
> Le fait de spécifier FORCESEEK avec des paramètres limite davantage le nombre de plans qui peuvent être considérés par l'optimiseur que le fait de spécifier FORCESEEK sans paramètre. Cela peut provoquer une erreur `Plan cannot be generated` dans plusieurs cas. Dans une version ultérieure, il se peut que des modifications internes de l’optimiseur de requête autorisent la prise en considération de davantage de plans.  
  
FORCESCAN **S’applique à** : [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP1 et versions ultérieures.
Indique que l’optimiseur de requête doit utiliser uniquement une opération d’analyse d’index comme chemin d’accès à la table ou la vue référencée. L'indicateur FORCESCAN peut être utile pour les requêtes dans lesquelles l'optimiseur sous-estime le nombre de lignes affectées et choisit une opération de recherche plutôt qu'une opération d'analyse. Dans ce cas, la quantité de mémoire allouée pour l'opération est trop faible, ce qui nuit aux performances des requêtes.  
  
FORCESCAN peut être spécifié avec ou sans indicateur INDEX. Lorsqu'il est associé à un indicateur d'index (`INDEX = index_name, FORCESCAN`), l'optimiseur de requête considère uniquement les chemins d'accès d'analyse dans l'index spécifié lors de l'accès à la table référencée. FORCESCAN peut être spécifié avec l'indicateur d'index INDEX(0) pour forcer une opération d'analyse de table sur la table de base.  
  
Pour les tables et les index partitionnés, FORCESCAN est appliqué après que les partitions ont été éliminées par le biais de l'évaluation de prédicat de requête. Cela signifie que l'analyse est appliquée uniquement aux partitions restantes, et non à la table entière.  
  
L’indicateur FORCESCAN est soumis aux restrictions suivantes :  
-   L'indicateur ne peut pas être spécifié pour une table qui est la cible d'une instruction INSERT, UPDATE ou DELETE.  
-   L'indicateur ne peut pas être utilisé avec plusieurs indicateurs d'index.  
-   L'indicateur empêche l'optimiseur de requête de considérer tout index spatial ou XML sur la table.  
-   L'indicateur ne peut pas être spécifié pour une source de données distante.  
-   L'indicateur ne peut pas être spécifié en association avec l'indicateur FORCESEEK.  
  
HOLDLOCK  
Équivalent à SERIALIZABLE. Pour plus d'informations, consultez SERIALIZABLE plus loin dans cette rubrique. L'option HOLDLOCK s'applique uniquement à la table ou à la vue pour laquelle elle est spécifiée et uniquement pour la durée de la transaction définie par l'instruction dans laquelle elle est utilisée. HOLDLOCK ne peut pas être utilisée dans une instruction SELECT qui comprend l'option FOR BROWSE.  
  
IGNORE_CONSTRAINTS  
Applicable uniquement dans une instruction INSERT quand l’option BULK est utilisée avec [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  
  
Spécifie que toutes les contraintes de la table sont ignorées par l'opération d'importation en bloc. Par défaut, INSERT vérifie les [contraintes uniques et contraintes de validation](../../relational-databases/tables/unique-constraints-and-check-constraints.md), ainsi que les [contraintes de clé primaire et de clé étrangère](../../relational-databases/tables/primary-and-foreign-key-constraints.md). Lorsque IGNORE_CONSTRAINTS est spécifié pour une opération d'importation en bloc, INSERT doit ignorer ces contraintes sur une table cible. Notez que vous ne pouvez pas désactiver les contraintes UNIQUE, PRIMARY KEY ou NOT NULL.  
  
Vous avez la possibilité de désactiver les contraintes CHECK et FOREIGN KEY si les données d'entrée contiennent des lignes qui violent des contraintes. En désactivant ces contraintes, vous pouvez importer les données, puis utiliser des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] pour les nettoyer.  
  
Toutefois, quand les contraintes CHECK et FOREIGN KEY sont ignorées, chaque contrainte ignorée sur la table est marquée comme **is_not_trusted** dans l’affichage [sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) ou [sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md), une fois l’opération terminée. À un certain stade, vous devez vérifier les contraintes sur la table entière. Si la table n'était pas vide avant l'opération d'importation en bloc, il revient plus cher de valider à nouveau la contrainte que d'appliquer des contraintes CHECK et FOREIGN KEY aux données incrémentielles.  
  
IGNORE_TRIGGERS  
Applicable uniquement dans une instruction INSERT quand l’option BULK est utilisée avec [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  
  
Spécifie que tous les déclencheurs définis sur la table sont ignorés par l'opération d'importation en bloc. Par défaut, INSERT applique les déclencheurs.  
  
Utilisez IGNORE_TRIGGERS uniquement si l'application ne dépend d'aucun déclencheur et que l'optimisation des performances est importante.  
  
NOLOCK  
Équivalent à READUNCOMMITTED. Pour plus d'informations, consultez READUNCOMMITTED plus loin dans cette rubrique.  
  
> [!NOTE]  
> Pour les instructions UPDATE ou DELETE : [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
NOWAIT  
Indique au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de retourner un message dès qu'un verrou est rencontré sur la table. L'utilisation de NOWAIT est équivalente à la spécification de `SET LOCK_TIMEOUT 0` pour une table spécifique. L'indicateur NOWAIT ne fonctionne pas lorsque l'indicateur TABLOCK est également inclus. Pour terminer une requête sans délai en cas d'utilisation de l'indicateur TABLOCK, préfacez plutôt la requête avec `SETLOCK_TIMEOUT 0;`.  
  
PAGLOCK  
Établit des verrous de page là où des verrous individuels sont généralement utilisés sur des lignes ou des clés ou là où un verrou de table unique est généralement utilisé. Par défaut, utilise le mode de verrou approprié pour l'opération. Si cet argument est spécifié dans des transactions fonctionnant au niveau d'isolement SNAPSHOT, les verrous de page ne sont établis que si PAGLOCK est combiné avec d'autres indicateurs de table qui requièrent des verrous, tels que UPDLOCK et HOLDLOCK.  
  
READCOMMITTED  
Spécifie que les opérations de lecture doivent respecter les règles du niveau d'isolation READ COMMITTED en utilisant le verrouillage ou le contrôle de version de ligne. Si l'option de base de données READ_COMMITTED_SNAPSHOT a la valeur OFF, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] acquiert des verrous partagés lorsque les données sont lues et libère ces verrous lorsque l'opération de lecture est achevée. Si l'option de base de données READ_COMMITTED_SNAPSHOT a pour valeur ON, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'acquiert pas de verrous et utilise le contrôle de version de ligne. Pour plus d’informations sur les niveaux d’isolement, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
> [!NOTE]  
> Pour les instructions UPDATE ou DELETE : [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
READCOMMITTEDLOCK  
Spécifie que les opérations de lecture doivent respecter les règles du niveau d'isolation READ COMMITTED en utilisant le verrouillage. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] acquiert des verrous partagés lorsque les données sont lues et libère ces verrous lorsque l'opération de lecture est achevée, quelle que soit la valeur de l'option de base de données READ_COMMITTED_SNAPSHOT. Pour plus d’informations sur les niveaux d’isolement, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md). Cet indicateur ne peut pas être spécifié sur la table cible d'une instruction INSERT ; l'erreur 4140 est retournée.  
  
READPAST  
Spécifie que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne doit pas lire les lignes qui sont verrouillées par d'autres transactions. Quand READPAST est spécifié, les verrous de niveau ligne sont ignorés, mais les verrous de niveau page ne le sont pas. Tant que les verrous ne sont pas libérés, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ignore les lignes au lieu de bloquer la transaction actuelle. Par exemple, supposons que la table `T1` contienne une colonne entière unique avec les valeurs 1, 2, 3, 4, 5. Si la transaction A remplace la valeur 3 par 8 mais n'a pas encore validé, une instruction SELECT * FROM T1 (READPAST) obtient les valeurs 1, 2, 4, 5. READPAST permet essentiellement de réduire la contention de verrouillage lors de l’implémentation d’une file d’attente de travail qui utilise une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un lecteur de file d'attente qui utilise READPAST ignore les entrées de file d'attente verrouillées par d'autres transactions et prend en compte l'entrée de file d'attente suivante disponible, sans attendre que les autres transactions libèrent leurs verrous.  
  
READPAST peut être spécifié pour toute table référencée dans une instruction UPDATE ou DELETE et pour toute table référencée dans une clause FROM. Lorsqu'il est spécifié dans une instruction UPDATE et quel que soit l'emplacement auquel il est défini dans celle-ci, l'argument READPAST est uniquement appliqué lorsque l'opération lit des données pour identifier les enregistrements à mettre à jour. READPAST ne peut pas être spécifié pour des tables dans la clause INTO d'une instruction INSERT. Les opérations de mise à jour ou de suppression qui utilisent READPAST peuvent se bloquer lorsqu'elles lisent des clés étrangères ou des vues indexées ou lorsqu'elles modifient des index secondaires.  
  
READPAST ne peut être spécifié que dans les transactions fonctionnant aux niveaux d'isolation READ COMMITTED ou REPEATABLE READ. Si cet argument est spécifié dans des transactions fonctionnant au niveau d'isolement SNAPSHOT, READPAST doit être combiné avec d'autres indicateurs de table qui requièrent des verrous, tels que UPDLOCK et HOLDLOCK.  
  
Il n’est pas possible de spécifier l’indicateur de table READPAST quand l’option de base de données READ_COMMITTED_SNAPSHOT a la valeur ON et que l’une des conditions suivantes est vraie :  
-   Le niveau d'isolation des transactions de la session est READ COMMITTED.  
-   L'indicateur READCOMMITTED de la table est également spécifié dans la requête.  
  
Pour spécifier l'indicateur READPAST dans ces cas, supprimez l'indicateur de table READCOMMITTED s'il est présent, et incorporez l'indicateur de table READCOMMITTEDLOCK dans la requête.  
  
READUNCOMMITTED  
Indique que les lectures erronées sont autorisées. Aucun verrou partagé n'est émis pour empêcher d'autres transactions de modifier les données lues par la transaction actuelle et les verrous exclusifs définis par les autres transactions n'empêchent pas la transaction actuelle de lire les données verrouillées. L'autorisation des lectures erronées peut accroître la concurrence, mais au prix de la lecture de modifications de données qui sont ensuite restaurées par d'autres transactions. Cela peut générer des erreurs pour votre transaction, confronter les utilisateurs à des données qui n'ont jamais été validées ou présenter aux utilisateurs deux fois les mêmes enregistrements (ou pas du tout).  
  
Les indicateurs READUNCOMMITTED et NOLOCK s'appliquent uniquement aux verrous de données. Toutes les requêtes, y compris celles dotées d'indicateurs READUNCOMMITTED et NOLOCK, obtiennent des verrous Sch-S (Stabilité du schéma) durant la compilation et l'exécution. Par conséquent, les requêtes sont bloquées lorsqu'une transaction simultanée détient un verrou de modification du schéma (Sch-M) sur la table. Par exemple, une opération DDL (Data Definition Language) acquiert un verrou Sch-M avant de modifier les informations de schéma de la table. Toutes les requêtes simultanées (y compris celles exécutées avec des indicateurs READUNCOMMITTED ou NOLOCK) sont bloquées lors des tentatives d'obtention d'un verrou Sch-S. Inversement, une requête qui détient un verrou Sch-S bloque une transaction simultanée qui tente d'acquérir un verrou Sch-M.  
  
Il est impossible de spécifier READUNCOMMITTED et NOLOCK pour des tables modifiées par des opérations d'insertion, de mise à jour ou de suppression. L'optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignore les indicateurs READUNCOMMITTED et NOLOCK dans la clause FROM s'appliquant à la table cible d'une instruction UPDATE ou DELETE.  
  
> [!NOTE]  
> La prise en charge des indicateurs READUNCOMMITTED et NOLOCK dans la clause FROM s'appliquant à la table cible d'une instruction UPDATE ou DELETE sera supprimée dans une version future de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces indicateurs dans ce contexte lors de vos nouvelles tâches de développement, et pensez à modifier les applications qui les utilisent actuellement.  
  
Vous pouvez réduire au maximum la contention de verrouillage tout en protégeant les transactions contre les lectures erronées de modifications de données non validées en utilisant l'un des niveaux d'isolation suivants :  
-   le niveau d'isolation READ COMMITTED avec l'option de base de données READ_COMMITTED_SNAPSHOT activée (ON) ;  
-   le niveau d'isolement SNAPSHOT.  
  
Pour plus d’informations sur les niveaux d’isolement, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
> [!NOTE]  
> Si vous recevez le [message d’erreur 601](../../relational-databases/errors-events/database-engine-events-and-errors.md#errors--2-to-999) lorsque READUNCOMMITTED est spécifié, résolvez-le comme une erreur de blocage ([message d’erreur 1205](../../relational-databases/errors-events/mssqlserver-1205-database-engine-error.md)) et relancez votre instruction.  
  
REPEATABLEREAD  
Indique qu'une recherche est effectuée avec la même sémantique de verrouillage qu'une transaction à un niveau d'isolation REPEATABLE READ. Pour plus d’informations sur les niveaux d’isolement, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
ROWLOCK  
Spécifie que les verrous de ligne sont établis lorsque les verrous de page ou de table sont généralement placés. Si cet argument est spécifié dans des transactions fonctionnant au niveau d'isolement SNAPSHOT, les verrous de ligne ne sont établis que si ROWLOCK est combiné avec d'autres indicateurs de table qui requièrent des verrous, tels que UPDLOCK et HOLDLOCK. ROWLOCK ne peut pas être utilisé avec une table qui a un index columnstore cluster. L’exemple suivant retourne une [erreur 651](../../relational-databases/errors-events/database-engine-events-and-errors.md#errors--2-to-999) à l’application.  

```sql 
UPDATE [dbo].[FactResellerSalesXL_CCI] WITH (ROWLOCK)
SET UnitPrice = 50
WHERE ProductKey = 150;
```  
  
SERIALIZABLE  
Équivalent à HOLDLOCK. Étend les restrictions associées aux verrous partagés en les maintenant jusqu'à l'achèvement de la transaction, au lieu de les relâcher dès que la table ou la page de données n'est plus utilisée, que la transaction soit achevée ou non. Effectue une recherche avec la même sémantique de verrouillage qu'une transaction à un niveau d'isolation SERIALIZABLE. Pour plus d’informations sur les niveaux d’isolement, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
SNAPSHOT  
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures. 
  
La table optimisée en mémoire est accédée selon l'isolement SNAPSHOT. SNAPSHOT peut être utilisé uniquement avec les tables optimisées en mémoire (et non avec les tables sur disque), comme le montre l’exemple suivant. Pour plus d’informations, consultez [Introduction aux tables à mémoire optimisée](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md).  
  
```sql 
SELECT * 
FROM dbo.Customers AS c WITH (SNAPSHOT)   
LEFT JOIN dbo.[Order History] AS oh   
    ON c.customer_id=oh.customer_id;  
```  
  
SPATIAL_WINDOW_MAX_CELLS = *<integer\_value>*  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
Spécifie le nombre maximal de cellules à utiliser pour le pavage d'un objet géométrique ou géographique. *<integer\_value>* est une valeur comprise entre 1 et 8 192.  
  
Cette option permet de paramétrer précisément l'heure d'exécution de la requête en ajustant le compromis entre la durée d'exécution du filtre primaire et du filtre secondaire. Un nombre élevé réduit la durée d'exécution du filtre secondaire, mais augmente celle du filtre de l'exécution primaire, tandis qu'un nombre plus petit décroît la durée d'exécution du filtre primaire, mais augmente celle de l'exécution du filtre secondaire. Avec des données spatiales plus denses, un nombre élevé doit aboutir à une durée d'exécution plus rapide en donnant une meilleure approximation avec le filtre primaire et en réduisant la durée d'exécution du filtre secondaire. Avec des données éparses, un nombre inférieur décroît la durée d'exécution du filtre primaire.  
  
 Cette option fonctionne à la fois pour les pavages de grille manuels et automatiques.  
  
TABLOCK  
Spécifie que le verrou acquis est appliqué au niveau de la table. Le type de verrou acquis dépend de l'instruction en cours d'exécution. Par exemple, une instruction SELECT peut acquérir un verrou partagé. En spécifiant TABLOCK, le verrou partagé est appliqué à la table entière plutôt qu'au niveau de la ligne ou de la page. Si l'option HOLDLOCK est également spécifiée, le verrou de table est maintenu jusqu'à la fin de la transaction.  
  
Quand vous importez des données dans un segment à l’aide de l’instruction `INSERT INTO <target_table> SELECT <columns> FROM <source_table>`, vous pouvez activer la journalisation minimale et l’optimisation du verrouillage de l’instruction en spécifiant l’indicateur TABLOCK pour la table cible. En outre, le mode de récupération de la base de données doit correspondre au mode simple ou au mode de récupération utilisant les journaux de transactions. L’indicateur TABLOCK permet également les insertions parallèles dans les segments de mémoire ou les index columnstore en cluster. Pour plus d’informations, consultez [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
Quand l’indicateur TABLOCK est utilisé avec le fournisseur d’ensembles de lignes [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) pour importer des données dans une table, il permet à plusieurs clients de charger en même temps les données dans la table cible avec une optimisation de la journalisation et du verrouillage. Pour plus d’informations, consultez [Conditions requises pour une journalisation minimale dans l’importation en bloc](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
  
TABLOCKX  
Indique qu'un verrou exclusif est établi sur la table.  
  
UPDLOCK  
Spécifie que les verrous de mise à jour doivent être établis et maintenus jusqu'à ce que la transaction s'achève. UPDLOCK prend les verrous de mise à jour pour les opérations de lecture uniquement au niveau de la ligne ou de la page. Si UPDLOCK est combiné avec TABLOCK, ou si un verrou de niveau table est pris pour une raison quelconque, un verrou exclusif (x) est pris à la place.  
  
Lorsque UPDLOCK est spécifié, les indicateurs de niveau d'isolation READCOMMITTED et READCOMMITTEDLOCK sont ignorés. Par exemple, si le niveau d'isolation de la session est SERIALIZABLE et qu'une requête spécifie (UPDLOCK, READCOMMITTED), l'indicateur READCOMMITTED est ignoré et la transaction est exécutée avec le niveau d'isolation SERIALIZABLE.  
  
XLOCK  
Spécifie que les verrous exclusifs doivent être établis et maintenus jusqu'à ce que la transaction s'achève. Si l'option ROWLOCK, PAGLOCK ou TABLOCK est spécifiée, les verrous exclusifs s'appliquent au niveau de granularité approprié.  
  
## <a name="remarks"></a>Notes  
Les indicateurs de table sont ignorés si l'accès à la table ne s'effectue pas par un plan de requête. Ceci peut résulter du choix de l'optimiseur d'empêcher globalement l'accès à la table ou de l'accès à une vue indexée à la place. Dans ce dernier cas, l'accès à une vue indexée peut être proscrit à l'aide de l'indicateur de requête OPTION (EXPAND VIEWS).  
  
Tous les indicateurs de verrou sont diffusés à toutes les vues et tables accessibles par le plan de requête ainsi que les vues et tables référencées dans une vue. En outre, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue les contrôles de cohérence de verrous correspondants.  
  
Les indicateurs de verrou ROWLOCK, UPDLOCK et XLOCK qui acquièrent des verrous de niveau ligne peuvent placer des verrous sur des clés d'index plutôt que sur les lignes de données elles-mêmes. Par exemple, si une table possède un index non cluster et qu'une instruction SELECT utilisant un indicateur de verrou est gérée par un index explicatif, un verrou est acquis sur la clé d'index dans l'index explicatif plutôt que sur la ligne de données dans la table de base.  
  
Si une table contient des colonnes calculées qui sont traitées par des expressions ou des fonctions ayant accès à des colonnes dans d'autres tables, les indicateurs de table ne sont pas utilisés sur ces dernières et ne sont pas propagés. Par exemple, l'indicateur de table NOLOCK est spécifié dans une table de la requête. Cette table possède des colonnes calculées par une combinaison d'expressions et de fonctions qui accèdent à des colonnes dans une autre table. Les tables référencées par les expressions et les fonctions n'utilisent pas l'indicateur de table NOLOCK lors de l'accès à ces dernières.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'autorise pas plus d'un indicateur de table dans chacun des groupes suivants pour chaque table de la clause FROM :  
-   indicateurs de granularité : PAGLOCK, NOLOCK, READCOMMITTEDLOCK, ROWLOCK, TABLOCK ou TABLOCKX.  
-   indicateurs de niveau d’isolation : HOLDLOCK, NOLOCK, READCOMMITTED, REPEATABLEREAD, SERIALIZABLE.  
  
## <a name="filtered-index-hints"></a>Indicateurs d'index filtré  
 Un index filtré peut être utilisé comme indicateur de table mais, en conséquence, l’optimiseur de requête génère l’erreur 8622 si l’index ne couvre pas toutes les lignes que la requête sélectionne. Vous trouverez ci-dessous un exemple d'indicateur d'index filtré non valide. Cet exemple illustre la création de l'index `FIBillOfMaterialsWithComponentID` filtré, puis l'utilisation de cet index comme indicateur d'index pour une instruction SELECT. Le prédicat d'index filtré inclut des lignes de données pour les ComponentID 533, 324 et 753. Le prédicat de la requête inclut également des lignes de données pour les ComponentID 533, 324 et 753 mais étend le jeu de résultats pour inclure les ComponentID 855 et 924, qui ne figurent pas dans l'index filtré. Par conséquent, l'optimiseur de requête ne peut pas utiliser l'indicateur d'index filtré et génère l'erreur 8622. Pour plus d'informations, consultez [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithComponentID'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithComponentID  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED INDEX "FIBillOfMaterialsWithComponentID"  
    ON Production.BillOfMaterials (ComponentID, StartDate, EndDate)  
    WHERE ComponentID IN (533, 324, 753);  
GO  
SELECT StartDate, ComponentID FROM Production.BillOfMaterials  
    WITH( INDEX (FIBillOfMaterialsWithComponentID) )  
    WHERE ComponentID in (533, 324, 753, 855, 924);  
GO  
```  
  
L'optimiseur de requête ne prendra pas en compte un indicateur d'index si les options SET n'ont pas les valeurs requises pour les index filtrés. Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="using-noexpand"></a>Utilisation de NOEXPAND  
NOEXPAND s’applique uniquement aux *vues indexées*. Une vue indexée comporte un index cluster unique, créé sur cette dernière. Si une requête contient des références à des colonnes présentes à la fois dans une vue indexée et dans des tables de base, et que l'optimiseur de requête préconise l'utilisation de la vue indexée comme méthode d'exécution de la requête, il utilise alors l'index sur la vue. Cette fonctionnalité est appelée *correspondance de vue indexée*. Dans les versions antérieures à [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1, l’utilisation automatique d’une vue indexée par l’optimiseur de requête est prise en charge uniquement dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md), [Fonctionnalités prises en charge par les éditions de SQL Server 2017](../../SQL-server/editions-and-components-of-SQL-server-2017.md) et [Fonctionnalités prises en charge par les éditions de SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
Toutefois, pour que l'optimiseur de requête prenne en considération les vues indexées pour la mise en correspondance ou utilise une vue indexée référencée avec l'indicateur NOEXPAND, les options SET suivantes doivent avoir pour valeur ON.  

> [!NOTE]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] prend en charge l’utilisation automatique de vues indexées sans spécifier l’indicateur NOEXPAND.

- ANSI_NULLS
- ANSI_PADDING
- ANSI_WARNINGS
- ARITHABORT<sup>1</sup>
- CONCAT_NULL_YIELDS_NULL
- QUOTED_IDENTIFIER

<sup>1</sup> ARITHABORT a implicitement la valeur ON quand ANSI_WARNINGS a la valeur ON. Par conséquent, vous n'avez pas besoin d'ajuster ce paramètre manuellement.  

En outre, l'option NUMERIC_ROUNDABORT doit être désactivée (OFF).  
  
 Pour contraindre l'optimiseur de requête à utiliser un index pour une vue indexée, spécifiez l'option NOEXPAND. Cet indicateur peut être utilisé uniquement si la vue est également nommée dans la requête. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne fournit pas d'indicateur pour imposer l'utilisation d'une vue indexée particulière dans une requête qui ne nomme pas directement la vue dans la clause FROM ; toutefois, l'optimiseur de requête admet l'utilisation de vues indexées même si elles ne sont pas référencées directement dans la requête. La [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] crée automatiquement des statistiques sur une vue indexée uniquement quand un indicateur de table NOEXPAND est utilisé. L’omission de cet indicateur peut entraîner l’affichage d’avertissements de plan d’exécution concernant des statistiques manquantes, qui ne peuvent pas être résolus en créant manuellement des statistiques. Lors de l’optimisation de requête, la [!INCLUDE[ssde_md](../../includes/ssde_md.md)] utilise les statistiques d’affichage qui ont été créées automatiquement ou manuellement quand la requête référence directement la vue et que l’indicateur NOEXPAND est utilisé.    
  
## <a name="using-a-table-hint-as-a-query-hint"></a>Utilisation d'un indicateur de table comme indicateur de requête  
 Un *indicateur de table* peut également être spécifié comme indicateur de requête avec la clause OPTION (TABLE HINT). Nous vous recommandons d’utiliser un indicateur de table comme indicateur de requête uniquement dans le contexte d’un [repère de plan](../../relational-databases/performance/plan-guides.md). Pour les requêtes ad hoc, spécifiez ces indicateurs uniquement comme indicateurs de table. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="permissions"></a>Autorisations  
 Les indicateurs KEEPIDENTITY, IGNORE_CONSTRAINTS et IGNORE_TRIGGERS requièrent des autorisations `ALTER` sur la table.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-tablock-hint-to-specify-a-locking-method"></a>R. Utilisation de l'indicateur TABLOCK pour spécifier une méthode de verrouillage  
 L'exemple suivant spécifie qu'un verrou partagé est établi sur la table `Production.Product` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] et maintenu jusqu'à la fin de l'instruction UPDATE.  
  
```sql  
UPDATE Production.Product  
WITH (TABLOCK)  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE 'BK-%';  
GO  
```  
  
### <a name="b-using-the-forceseek-hint-to-specify-an-index-seek-operation"></a>B. Utilisation de l'indicateur FORCESEEK pour spécifier une opération de recherche d'index  
 L'exemple ci-dessous utilise l'indicateur FORCESEEK sans spécifier d'index pour forcer l'optimiseur de requête à effectuer une opération de recherche d'index sur la table `Sales.SalesOrderDetail` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
SELECT *  
FROM Sales.SalesOrderHeader AS h  
INNER JOIN Sales.SalesOrderDetail AS d WITH (FORCESEEK)  
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);  
GO  
  
```  
  
 L'exemple ci-dessous utilise l'indicateur FORCESEEK avec un index pour forcer l'optimiseur de requête à effectuer une opération de recherche d'index sur l'index et la colonne d'index spécifiés.  
  
```sql  
SELECT h.SalesOrderID, h.TotalDue, d.OrderQty  
FROM Sales.SalesOrderHeader AS h  
    INNER JOIN Sales.SalesOrderDetail AS d   
    WITH (FORCESEEK (PK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID (SalesOrderID)))   
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);   
GO  
  
```  
  
### <a name="c-using-the-forcescan-hint-to-specify-an-index-scan-operation"></a>C. Utilisation de l'indicateur FORCESCAN pour spécifier une opération d'analyse d'index  
 L'exemple ci-dessous utilise l'indicateur FORCESCAN pour forcer l'optimiseur de requête à effectuer une opération d'analyse sur la table `Sales.SalesOrderDetail` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT h.SalesOrderID, h.TotalDue, d.OrderQty  
FROM Sales.SalesOrderHeader AS h  
    INNER JOIN Sales.SalesOrderDetail AS d   
    WITH (FORCESCAN)   
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Indicateurs &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
 [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)  
  
