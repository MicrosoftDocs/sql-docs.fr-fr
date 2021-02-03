---
description: ALTER EVENT SESSION (Transact-SQL)
title: ALTER EVENT SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER EVENT SESSION
- ALTER_EVENT_SESSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- extended events [SQL Server], Transact-SQL
- ALTER EVENT SESSION statement
ms.assetid: da006ac9-f914-4995-a2fb-25b5d971cd90
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a48973fcbd4aa030d41b5bdce3e0c10f37ce8efc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204274"
---
# <a name="alter-event-session-transact-sql"></a>ALTER EVENT SESSION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Démarre ou arrête une session d'événements, ou modifie la configuration d'une session d'événements.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER EVENT SESSION event_session_name  
ON SERVER  
{  
    [ [ {  <add_drop_event> [ ,...n] }     
       | { <add_drop_event_target> [ ,...n ] } ]   
    [ WITH ( <event_session_options> [ ,...n ] ) ]  
    ]  
    | [ STATE = { START | STOP } ]  
}  
  
<add_drop_event>::=  
{  
    [ ADD EVENT <event_specifier>   
         [ ( {   
                 [ SET { event_customizable_attribute = <value> [ ,...n ] } ]  
                 [ ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n ] } ) ]  
                 [ WHERE <predicate_expression> ]  
        } ) ]  
   ]   
   | DROP EVENT <event_specifier> }  
  
<event_specifier> ::=  
{  
[event_module_guid].event_package_name.event_name  
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
  
<add_drop_event_target>::=  
{  
    ADD TARGET <event_target_specifier>  
        [ ( SET { target_parameter_name = <value> [ ,...n] } ) ]  
    | DROP TARGET <event_target_specifier>  
}  
  
<event_target_specifier>::=  
{  
    [event_module_guid].event_package_name.target_name  
}  
  
<event_session_options>::=  
{  
    [    MAX_MEMORY = size [ KB | MB] ]  
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
  
|Terme|Définition|  
|-|-|
|*event_session_name*|Nom d'une session d'événements existante.|  
|STATE = START &#124; STOP|Démarre ou arrête la session d'événements. Cet argument est valide uniquement lorsque ALTER EVENT SESSION est appliqué à un objet de la session d'événements.|  
|ADD EVENT \<event_specifier>|Associe l’événement identifié par \<event_specifier> à la session d’événements.|
|[*event_module_guid*] *.event_package_name.event_name*|Nom d'un événement d'un package d'événement, où :<br /><br /> -   *event_module_guid* est le GUID du module qui contient l’événement.<br />-   *event_package_name* est le package qui contient l’objet d’action.<br />-   *event_name* est l’objet d’événement.<br /><br /> Les événements apparaissent dans la vue sys.dm_xe_objects en tant qu'object_type « événement ».|  
|SET { *event_customizable_attribute*= \<value> [ ,...*n*] }|Spécifie des attributs personnalisables pour l'événement. Les attributs personnalisables s’affichent dans la vue sys.dm_xe_object_columns sous la forme column_type 'customizable' et object_name = *event_name*.|  
|ACTION ( { [*event_module_guid*] *.event_package_name.action_name* [ **,** ...*n*] } )|Action à associer à la session d'événements, où :<br /><br /> -   *event_module_guid* est le GUID du module qui contient l’événement.<br />-   *event_package_name* est le package qui contient l’objet d’action.<br />-   *action_name* est l’objet d’action.<br /><br /> Les actions apparaissent dans la vue sys.dm_xe_objects en tant qu'object_type « action ».|  
|WHERE \<predicate_expression>|Spécifie l'expression de prédicat utilisée pour déterminer si un événement doit être traité. Si \<predicate_expression> est true, le traitement de l’événement par les actions et les cibles pour la session se poursuit. Si \<predicate_expression> est false, l’événement est supprimé par la session avant d’être traité par les actions et les cibles pour la session. Les expressions de prédicat sont limitées à 3 000 caractères, ce qui limite les arguments de chaîne.|
|*event_field_name*|Nom du champ d'événement qui identifie la source de prédicat.|  
|[event_module_guid].event_package_name.predicate_source_name|Nom de la source de prédicat globale, où :<br /><br /> -   *event_module_guid* est le GUID du module qui contient l’événement.<br />-   *event_package_name* est le package qui contient l’objet de prédicat.<br />-   *predicate_source_name* est défini dans la vue sys.dm_xe_objects en tant qu’object_type « pred_source ».|  
|[*event_module_guid*].*event_package_name*.*predicate_compare_name*|Nom de l'objet de prédicat à associer à l'événement, où :<br /><br /> -   *event_module_guid* est le GUID du module qui contient l’événement.<br />-   *event_package_name* est le package qui contient l’objet de prédicat.<br />-   *predicate_compare_name* est une source globale définie dans la vue sys.dm_xe_objects en tant qu’object_type « pred_compare ».|  
|DROP EVENT \<event_specifier>|Supprime l'événement identifié par *\<event_specifier>* . \<event_specifier> doit être valide dans la session d’événements.|  
|ADD TARGET \<event_target_specifier>|Associe l’événement identifié par \<event_target_specifier> à la session d’événements.|
|[*event_module_guid*].*event_package_name*.*target_name*|Nom d'une cible de la session d'événements, où :<br /><br /> -   *event_module_guid* est le GUID du module qui contient l’événement.<br />-   *event_package_name* est le package qui contient l’objet d’action.<br />-   *target_name* est l’action. Les actions apparaissent dans la vue sys.dm_xe_objects en tant qu'object_type « cible ».|  
|SET { *target_parameter_name*= \<value> [, ...*n*] }|Définit un paramètre cible. Les paramètres cibles apparaissent dans la vue sys.dm_xe_object_columns sous la forme column_type 'customizable' et object_name = *target_name*.<br /><br /> **REMARQUE** Si vous utilisez la cible de mémoire tampon en anneau, il est préférable de configurer le paramètre cible max_memory avec 2 048 kilo-octets (Ko) pour éviter les éventuelles données tronquées dans le résultat XML. Pour plus d’informations sur le moment où les différents types de cibles doivent être utilisés, consultez [Cibles des événements étendus SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).|  
|DROP TARGET \<event_target_specifier>|Supprime la cible identifiée par \<event_target_specifier>. \<event_target_specifier> doit être valide dans la session d’événements.|  
|EVENT_RETENTION_MODE = { **ALLOW_SINGLE_EVENT_LOSS** &#124; ALLOW_MULTIPLE_EVENT_LOSS &#124; NO_EVENT_LOSS }|Spécifie le mode de rétention des événements à utiliser pour gérer la perte d'événements.<br /><br /> **ALLOW_SINGLE_EVENT_LOSS**<br /> Il est possible de perdre un événement de la session. Un événement unique est abandonné uniquement lorsque toutes les mémoires tampons d'événements sont saturées. La perte d'un événement unique lorsque les mémoires tampons d'événements sont saturées permet d'obtenir des caractéristiques de performance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acceptables, tout en réduisant la perte de données du flux d'événements traité.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS<br /> Il est possible de perdre des mémoires tampons d'événements saturées contenant plusieurs événements. Le nombre d’événements perdus dépend de la taille de la mémoire allouée à la session, du partitionnement de la mémoire et de la taille des événements dans la mémoire tampon. Cette option atténue l'impact sur les performances du serveur lorsque les mémoires tampons d'événements sont rapidement remplies, mais il est possible de perdre un grand nombre d'événements de la session.<br /><br /> NO_EVENT_LOSS<br /> Aucune perte d'événements n'est autorisée. Cette option garantit que tous les événements déclenchés seront conservés. Cette option force toutes les tâches qui déclenchent des événements à attendre que de l'espace se libère dans une mémoire tampon d'événements. Cela peut entraîner des problèmes de performance détectables pendant que la session d'événements est active. Les connexions utilisateur peuvent se bloquer en attendant que les événements soient supprimés de la mémoire tampon.|  
|MAX_DISPATCH_LATENCY = { *seconds* SECONDS &#124; **INFINITE** }|Spécifie la durée pendant laquelle les événements sont mis en mémoire tampon avant d'être distribués aux cibles de la session d'événements. La valeur de latence minimale est 1 seconde. Toutefois, la valeur 0 peut être utilisée pour spécifier une latence INFINITE. Elle est par défaut de 30 secondes.<br /><br /> *seconds* SECONDS<br /> Durée (en secondes) à attendre avant de commencer à vider les mémoires tampons vers les cibles. *seconds* est un nombre entier.<br /><br /> **INFINITE**<br /> Vide les mémoires tampons vers les cibles uniquement lorsque les mémoires tampons sont saturées ou lors de la fermeture de la session d'événements.<br /><br /> **REMARQUE** MAX_DISPATCH_LATENCY = 0 SECONDS est équivalent à MAX_DISPATCH_LATENCY = INFINITE.|  
|MAX_EVENT_SIZE =*size* [ KB &#124; **MB** ]|Spécifie la taille maximale autorisée pour les événements. MAX_EVENT_SIZE doit être défini uniquement pour autoriser les événements uniques supérieurs à MAX_MEMORY. Si vous lui affectez une valeur inférieure à MAX_MEMORY, une erreur est générée. *size* est un nombre entier qui peut s’exprimer en kilo-octets (Ko) ou en mégaoctets (Mo). Si *size* est spécifié en kilo-octets, la taille minimale autorisée est 64 Ko. Quand MAX_EVENT_SIZE est défini, deux mémoires tampons de *size* sont créées en plus de MAX_MEMORY. Cela signifie que la mémoire totale utilisée pour la mise en mémoire tampon d'événements est MAX_MEMORY + 2 * MAX_EVENT_SIZE.|  
|MEMORY_PARTITION_MODE = { **NONE** &#124; PER_NODE &#124; PER_CPU }|Spécifie l'emplacement où les mémoires tampons d'événements sont créées.<br /><br /> **NONE**<br /> Un jeu unique de mémoires tampons est créé dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> PER NODE - Un jeu de mémoires tampons est créé pour chaque nœud NUMA.<br /><br /> PER CPU - Un jeu de mémoires tampons est créé pour chaque UC.|  
|TRACK_CAUSALITY = { ON &#124; **OFF** }|Spécifie si la causalité est suivie ou non. Si cette option est activée, la causalité permet à des événements associés de différentes connexions au serveur d'être corrélés.|  
|STARTUP_STATE = { ON &#124; **OFF** }|Spécifie si cette session d'événements doit être lancée automatiquement au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Si STARTUP_STATE = ON, la session d’événements est lancée uniquement si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est arrêté, puis redémarré.<br /><br /> ON= La session d’événements est lancée au démarrage.<br /><br /> **OFF** = La session d’événements n’est PAS lancée au démarrage.|  
  
## <a name="remarks"></a>Notes  
 Les arguments `ADD` et `DROP` ne peuvent pas être utilisés dans la même instruction.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation `ALTER ANY EVENT SESSION`.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant démarre une session d'événements, récupère quelques statistiques de session en temps réel, puis ajoute deux événements à la session existante.  
  
```sql  
-- Start the event session  
ALTER EVENT SESSION test_session ON SERVER  
STATE = start;  
GO  

-- Obtain live session statistics   
SELECT * FROM sys.dm_xe_sessions;  
SELECT * FROM sys.dm_xe_session_events;  
GO  
  
-- Add new events to the session  
ALTER EVENT SESSION test_session ON SERVER  
ADD EVENT sqlserver.database_transaction_begin,  
ADD EVENT sqlserver.database_transaction_end;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)   
 [DROP EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-session-transact-sql.md)   
 [Cibles des Événements étendus SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))   
 [sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)   
 [sys.dm_xe_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)   
 [sys.dm_xe_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)  
  
