---
description: CREATE EVENT SESSION (Transact-SQL)
title: CREATE EVENT SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE EVENT SESSION
- SESSION
- EVENT SESSION
- SESSION_TSQL
- EVENT_SESSION_TSQL
- CREATE_EVENT_SESSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- CREATE EVENT SESSION statement
ms.assetid: 67683027-2b0f-47aa-b223-604731af8b4d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7284b9c3d574a55dc67c99e500c7c7da69400e29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186865"
---
# <a name="create-event-session-transact-sql"></a>CREATE EVENT SESSION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crée une session d'événements étendus qui identifie la source des événements, les cibles de la session d'événements et les options de la session d'événements.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE EVENT SESSION event_session_name
ON { SERVER | DATABASE }
{  
    <event_definition> [ ,...n]
    [ <event_target_definition> [ ,...n] ]
    [ WITH ( <event_session_options> [ ,...n] ) ]
}
;

<event_definition>::=
{
    ADD EVENT [event_module_guid].event_package_name.event_name
         [ ( {
                 [ SET { event_customizable_attribute = <value> [ ,...n] } ]
                 [ ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n] } ) ]
                 [ WHERE <predicate_expression> ]
        } ) ]
}

<predicate_expression> ::=
{
    [ NOT ] <predicate_factor> | {( <predicate_expression> ) }
    [ { AND | OR } [ NOT ] { <predicate_factor> | ( <predicate_expression> ) } ]
    [ ,...n ]
}  
  
<predicate_factor>::=
{
    <predicate_leaf> | ( <predicate_expression> )
}

<predicate_leaf>::=
{
      <predicate_source_declaration> { = | < > | ! = | > | > = | < | < = } <value>
    | [event_module_guid].event_package_name.predicate_compare_name ( <predicate_source_declaration>, <value> )
}

<predicate_source_declaration>::=
{
    event_field_name | ( [event_module_guid].event_package_name.predicate_source_name )
}

<value>::=
{
    number | 'string'
}

<event_target_definition>::=
{
    ADD TARGET [event_module_guid].event_package_name.target_name
        [ ( SET { target_parameter_name = <value> [ ,...n] } ) ]
}

<event_session_options>::=
{  
    [    MAX_MEMORY = size [ KB | MB ] ]
    [ [,] EVENT_RETENTION_MODE = { ALLOW_SINGLE_EVENT_LOSS | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } ]
    [ [,] MAX_DISPATCH_LATENCY = { seconds SECONDS | INFINITE } ]
    [ [,] MAX_EVENT_SIZE = size [ KB | MB ] ]
    [ [,] MEMORY_PARTITION_MODE = { NONE | PER_NODE | PER_CPU } ]
    [ [,] TRACK_CAUSALITY = { ON | OFF } ]
    [ [,] STARTUP_STATE = { ON | OFF } ]
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*event_session_name* Nom défini par l'utilisateur de la session d'événements. *event_session_name* est alphanumérique, peut contenir jusqu’à 128 caractères, doit être unique dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).

ADD EVENT [ *event_module_guid* ].*event_package_name*.*event_name* Événement à associer à la session d'événements, où :

- *event_module_guid* est le GUID du module qui contient l’événement.
- *event_package_name* est le package qui contient l’objet d’action.
- *event_name* est l’objet d’événement.

Les événements apparaissent dans la vue sys.dm_xe_objects en tant qu'object_type « événement ».

SET { *event_customizable_attribute*= \<value> [ ,...*n*] } Autorise les attributs personnalisables pour l'événement à définir. Les attributs personnalisables s’affichent dans la vue sys.dm_xe_object_columns sous la forme column_type 'customizable' et object_name = *event_name*.

ACTION ( { [*event_module_guid*].*event_package_name*.*action_name* [ **,** ...*n*] }) Action à associer à la session d'événements, où :

- *event_module_guid* est le GUID du module qui contient l’événement.
- *event_package_name* est le package qui contient l’objet d’action.
- *action_name* est l’objet d’action.

Les actions apparaissent dans la vue sys.dm_xe_objects en tant qu'object_type « action ».

WHERE \<predicate_expression> Spécifie l'expression de prédicat utilisée pour déterminer si un événement doit être traité. Si \<predicate_expression> est true, le traitement de l’événement par les actions et les cibles pour la session se poursuit. Si \<predicate_expression> est false, l’événement est supprimé par la session avant d’être traité par les actions et les cibles pour la session. Les expressions de prédicat sont limitées à 3 000 caractères, ce qui limite les arguments de chaîne.

*event_field_name* Nom du champ d'événement qui identifie la source de prédicat.

[*event_module_guid*].*event_package_name*.*predicate_source_name* Nom de la source de prédicat globale, où :

- *event_module_guid* est le GUID du module qui contient l’événement.
- *event_package_name* est le package qui contient l’objet de prédicat.
- *predicate_source_name* est défini dans la vue sys.dm_xe_objects comme object_type 'pred_source'.

[*event_module_guid*].*event_package_name*.*predicate_compare_name* Nom de l'objet de prédicat à associer à l'événement, où :

- *event_module_guid* est le GUID du module qui contient l’événement.
- *event_package_name* est le package qui contient l’objet de prédicat.
- *predicate_compare_name* est une source globale définie dans la vue sys.dm_xe_objects comme object_type 'pred_compare'.

*number* Tout type numérique, dont **decimal**. Le manque de mémoire physique ou un nombre trop grand pour être représenté sous forme d'entier 64 bits sont les seules limitations.

'*string*' Chaîne ANSI ou Unicode, comme requis par la comparaison de prédicat. Aucune conversion implicite de type chaîne n'est effectuée pour les fonctions de comparaison de prédicat. La transmission d'un type incorrect provoque une erreur.

ADD TARGET [*event_module_guid*].*event_package_name*.*target_name* Cible à associer à la session d'événements, où :

- *event_module_guid* est le GUID du module qui contient l’événement.
- *event_package_name* est le package qui contient l’objet d’action.
- *target_name* est la cible. Les cibles apparaissent dans la vue sys.dm_xe_en tant qu'object_type « cible ».

SET { *target_parameter_name*= \<value> [, ...*n*] } Définit un paramètre cible. Les paramètres cibles apparaissent dans la vue sys.dm_xe_object_columns sous la forme column_type 'customizable' et object_name = *target_name*.

> [!IMPORTANT]
> Si vous utilisez la cible de mémoire tampon en anneau, il est préférable de configurer le paramètre cible max_memory avec 2 048 kilo-octets (Ko) pour éviter les éventuelles données tronquées dans le résultat XML. Pour plus d’informations sur le moment où les différents types de cibles doivent être utilisés, consultez [Cibles des événements étendus SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).

WITH ( \<event_session_options> [ ,...*n*] ) Spécifie les options à utiliser avec la session d'événements.

MAX_MEMORY =*size* [ KB | **MB** ] Spécifie la quantité de mémoire maximale à allouer à la session pour la mise en mémoire tampon d'événement. La valeur par défaut est 4 Mo. *size* est un nombre entier qui peut s’exprimer en kilo-octets (Ko) ou en mégaoctets (Mo). La valeur maximale ne peut pas dépasser 2 Go (moins de 2 048 Mo). Toutefois, il est déconseillé d’utiliser des plages de valeurs de mémoire exprimées en Go.

EVENT_RETENTION_MODE = { **ALLOW_SINGLE_EVENT_LOSS** | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } Spécifie le mode de rétention des événements à utiliser pour gérer la perte d'événements.

**ALLOW_SINGLE_EVENT_LOSS** Il est possible de perdre un événement de la session. Un événement unique est abandonné uniquement lorsque toutes les mémoires tampons d'événements sont saturées. La perte d'un événement unique lorsque les mémoires tampons d'événements sont saturées permet d'obtenir des caractéristiques de performance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acceptables, tout en réduisant la perte de données du flux d'événements traité.

ALLOW_MULTIPLE_EVENT_LOSS Il est possible de perdre des mémoires tampons d'événements saturées contenant plusieurs événements. Le nombre d’événements perdus dépend de la taille de la mémoire allouée à la session, du partitionnement de la mémoire et de la taille des événements dans la mémoire tampon. Cette option atténue l'impact sur les performances du serveur lorsque les mémoires tampons d'événements sont rapidement remplies, mais il est possible de perdre un grand nombre d'événements de la session.

NO_EVENT_LOSS Aucune perte d'événements n'est autorisée. Cette option garantit que tous les événements déclenchés seront conservés. Cette option force toutes les tâches qui déclenchent des événements à attendre que de l'espace se libère dans une mémoire tampon d'événements. Cela peut entraîner des problèmes de performance détectables pendant que la session d'événements est active. Les connexions utilisateur peuvent se bloquer en attendant que les événements soient supprimés de la mémoire tampon.

MAX_DISPATCH_LATENCY = { *seconds* SECONDS | **INFINITE** } Spécifie la durée pendant laquelle les événements seront mis en mémoire tampon avant d'être distribués aux cibles de la session d'événements. Elle est par défaut de 30 secondes.

*seconds* SECONDS Durée (en secondes) à attendre avant de commencer à vider les mémoires tampons vers les cibles. *seconds* est un nombre entier. La valeur de latence minimale est 1 seconde. Toutefois, la valeur 0 peut être utilisée pour spécifier une latence INFINITE.

**INFINITE** Vide les mémoires tampons vers les cibles uniquement lorsque les mémoires tampons sont saturées ou lors de la fermeture de la session d'événements.

> [!NOTE]
> MAX_DISPATCH_LATENCY = 0 SECONDS est équivalent à MAX_DISPATCH_LATENCY = INFINITE.

MAX_EVENT_SIZE =*size* [ KB | **MB** ] Spécifie la taille maximale autorisée pour les événements. MAX_EVENT_SIZE doit être défini uniquement pour autoriser les événements uniques supérieurs à MAX_MEMORY. Si vous lui affectez une valeur inférieure à MAX_MEMORY, une erreur est générée. *size* est un nombre entier qui peut s’exprimer en kilo-octets (Ko) ou en mégaoctets (Mo). Si *size* est spécifié en kilo-octets, la taille minimale autorisée est 64 Ko. Quand MAX_EVENT_SIZE est défini, deux mémoires tampons de *size* sont créées en plus de MAX_MEMORY. Cela signifie que la mémoire totale utilisée pour la mise en mémoire tampon d'événements est MAX_MEMORY + 2 * MAX_EVENT_SIZE.

MEMORY_PARTITION_MODE = { **NONE** | PER_NODE | PER_CPU } Spécifie l'emplacement où les mémoires tampons d'événements sont créées.

**NONE** Un jeu unique de mémoires tampons est créé dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

PER NODE Un jeu de mémoires tampons est créé pour chaque nœud NUMA.

PER_CPU Un jeu de mémoires tampons est créé pour chaque UC.

TRACK_CAUSALITY = { ON | **OFF** } Spécifie si la causalité est suivie ou non. Si cette option est activée, la causalité permet à des événements associés de différentes connexions au serveur d'être corrélés.

STARTUP_STATE = { ON | **OFF** } Spécifie si cette session d'événements doit être lancée automatiquement au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Si `STARTUP_STATE = ON`, la session d’événements est lancée uniquement si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est arrêté puis redémarré.

ON La session d'événements est lancée au démarrage.

**OFF** La session d'événements n'est pas lancée au démarrage.

## <a name="remarks"></a>Notes

L’ordre de priorité des opérateurs logiques est `NOT`(la plus élevée), puis `AND`, puis `OR`.

## <a name="permissions"></a>Autorisations

Sur SQL Server, requiert l'autorisation `ALTER ANY EVENT SESSION`.
Sur SQL Database, requiert l’autorisation `ALTER ANY DATABASE EVENT SESSION` dans la base de données.

## <a name="examples"></a>Exemples

### <a name="sql-server-example"></a>Exemple SQL Server

L'exemple suivant montre comment créer une session d'événements appelée `test_session`. Cet exemple ajoute deux événements et utilise la cible du suivi d'événements pour Windows.

```sql
IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='test_session')
    DROP EVENT session test_session ON SERVER;
GO
CREATE EVENT SESSION test_session
ON SERVER
    ADD EVENT sqlos.async_io_requested,
    ADD EVENT sqlserver.lock_acquired
    ADD TARGET package0.etw_classic_sync_target
        (SET default_etw_session_logfile_path = N'C:\demo\traces\sqletw.etl' )
    WITH (MAX_MEMORY=4MB, MAX_EVENT_SIZE=4MB);
GO
```
### <a name="sql-database-example"></a>Exemple SQL Database

Pour obtenir un exemple Azure SQL Database, consultez l’exemple dans [Code de la cible de fichier d’événements pour les événements étendus dans SQL Database](/azure/sql-database/sql-database-xevent-code-event-file#transact-sql-code)

### <a name="code-examples-can-differ-for-azure-sql-database"></a>Les exemples de code peuvent être différents pour Azure SQL Database

[!INCLUDE[sql-on-premises-vs-azure-similar-sys-views-include.](../../includes/paragraph-content/sql-on-premises-vs-azure-similar-sys-views-include.md)]

## <a name="see-also"></a>Voir aussi

- [ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)
- [DROP EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-session-transact-sql.md)
- [sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)
- [sys.dm_xe_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)
- [sys.dm_xe_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)