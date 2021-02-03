---
title: Cibles des Événements étendus SQL Server
description: Cet article décrit les cibles package0 pour les événements étendus dans SQL Server. Découvrez les capacités des cibles en termes de collecte et de génération de rapports sur les données ainsi que les paramètres des cibles.
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
ms.assetid: 47c64144-4432-4778-93b5-00496749665b
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: ee107ed1d1e9fe7ec7aa8b40c87c71750b5cdf99
ms.sourcegitcommit: 3e2421ae45a8e9fa57fb590a5d1a5566721ea74a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98921392"
---
# <a name="targets-for-extended-events-in-sql-server"></a>Cibles des Événements étendus SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


Cet article explique quand et comment utiliser les cibles package0 cible pour les événements étendus dans SQL Server. Pour chaque cible, cet article vous explique :

- Ses capacités en termes de collecte et de génération de rapports sur les données envoyées par les événements.
- Ses paramètres, sauf si le paramètre est explicite.


#### <a name="xquery-example"></a>Exemple de XQuery


La [section ring_buffer section](#h2_target_ring_buffer) inclut un exemple d’utilisation de [XQuery dans Transact-SQL](../../xquery/xquery-language-reference-sql-server.md) pour copier une chaîne XML dans un ensemble de lignes relationnel.


### <a name="prerequisites"></a>Prérequis


- Familiarisez-vous avec les concepts de base des événements étendus, décrits dans [Démarrage rapide : Événements étendus dans SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md).


- Avez installé une version récente de l’utilitaire fréquemment mis à jour SQL Server Management Studio (SSMS.exe). Pour plus d’informations, consultez :
    - [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)


- Dans SSMS.exe, découvrez comment utiliser l’ **Explorateur d’objets** pour cliquer avec le bouton droit sur le nœud cible sous votre session d’événements, pour [faciliter l’affichage des données de sortie](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md).
    - Les données d’événement sont capturées sous forme de chaîne XML. Dans cet article, les données sont affichées dans des lignes relationnelles. SSMS a été utilisé pour afficher les données, puis a été copié et collé dans cet article.
    - L’autre technique de T-SQL pour générer des ensembles de lignes à partir de XML est expliquée dans la [section ring_buffer](#h2_target_ring_buffer). Elle implique XQuery.



## <a name="parameters-actions-and-fields"></a>Paramètres, actions et champs


Dans Transact-SQL, l’instruction [CREATE EVENT SESSION](~/t-sql/statements/create-event-session-transact-sql.md) est au cœur des événements étendus. Pour rédiger l’instruction, vous avez souvent besoin d’une liste et d’une description des éléments suivants :

- Les champs associés à l’événement choisi.
- Les paramètres associés à l’événement choisi.

Les instructions SELECT qui renvoient de telles listes à partir de vues système sont disponibles à la copie depuis l’article suivant dans sa section C:

- [Utilisation de SELECT et JOIN dans les vues système pour les événements étendus dans SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)
    - [C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) Champs SELECT pour un événement.
    - [C.6](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_6_parameters_targets) Paramètres SELECT pour une cible.
    - [C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) Actions SELECT.


Vous pouvez voir les paramètres, les champs et actions utilisées dans le contexte d’une instruction CREATE EVENT SESSION réelle en cliquant sur [ce lien](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_B_2_TSQL_perspective).



<a name="h2_target_etw_classic_sync_target"></a>

## <a name="etw_classic_sync_target-target"></a>Cible etw_classic_sync_target


Les événements SQL Server peuvent interagir avec le suivi d’événements pour Windows (ETW) pour surveiller l’activité système. Pour plus d'informations, consultez les pages suivantes :

- [Suivi d’événements pour cible Windows](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [Surveiller l’activité système à l’aide d’événements étendus](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)


Cette cible ETW traite de manière *synchrone* les données qu’elle reçoit, tandis que la plupart des cibles procèdent de manière *asynchrone*.

> [!NOTE]
> Azure SQL Database ne prend pas en charge `etw_classic_sync_target target`.

<!-- After OPS Versioning is live, the above !NOTE could be converted into a "3colon ZONE".  GeneMi = MightyPen. -->

<a name="h2_target_event_counter"></a>

## <a name="event_counter-target"></a>Cible event_counter


La cible event_counter compte simplement le nombre de fois où chaque événement spécifié se produit.


Contrairement à la plupart des autres cibles :

- La cible event_counter n’a aucun paramètre.


- Contrairement à la plupart des cibles, la cible event_counter traite de manière *synchrone* les données qu’elle reçoit.
    - Le mode synchrone est acceptable pour l’event_counter simple, car il implique très peu de traitement.
    - Le moteur de base de données se déconnecte de toutes les cibles trop lentes, qui risquent, par conséquent de ralentir ses performances. C’est une des raisons pour lesquelles la plupart des cibles procèdent de manière *asynchrone*.


#### <a name="example-output-captured-by-event_counter"></a>Exemple de sortie capturée par event_counter


```
package_name   event_name         count
------------   ----------         -----
sqlserver      checkpoint_begin   4
```


Ensuite, vient l’instruction CREATE EVENT SESSION qui a conduit aux résultats précédents. Pour ce test, sur la clause EVENT...WHERE, le champ **package0.counter** a été utilisé pour faire cesser le décompte une fois que le nombre a atteint 4.


```sql
CREATE EVENT SESSION [event_counter_1]
    ON SERVER 
    ADD EVENT sqlserver.checkpoint_begin   -- Test by issuing CHECKPOINT; statements.
    (
        WHERE ([package0].[counter] <= (4))   -- A predicate filter.
    )
    ADD TARGET package0.event_counter
    WITH
    (
        MAX_MEMORY = 4096 KB,
        MAX_DISPATCH_LATENCY = 3 SECONDS
    );
```



<a name="h2_target_event_file"></a>

## <a name="event_file-target"></a>Cible event_file


La cible **event_file** écrit la sortie de session d’événements à partir de la mémoire tampon sur un fichier de disque :


- Vous spécifiez le paramètre *filename =* dans la clause ADD TARGET.
    - **.xel** doit être l’extension du fichier.


- Le nom de fichier que vous choisissez est utilisé par le système en tant que préfixe auquel un entier long basé sur une date-heure est ajouté suivi de l’extension .xel.

::: moniker range="= azuresqldb-current || = azuresqldb-mi-current "

> [!NOTE]
> Azure SQL Database prend en charge le stockage des fichiers `xel` seulement dans Stockage Blob Azure. 
>
> Pour obtenir un exemple de code **event_file** utilisable avec SQL Database (et avec SQL Managed Instance), consultez [Code cible du fichier d’événements pour les événements étendus dans SQL Database](/azure/sql-database/sql-database-xevent-code-event-file).

::: moniker-end


#### <a name="create-event-session-with-event_file-target"></a>CREATE EVENT SESSION avec cible **event_file**


Ensuite, vient l’instruction CREATE EVENT SESSION utilisée pour le test. L’une des clauses ADD TARGET spécifie une cible event_file.


```sql
CREATE EVENT SESSION [locks_acq_rel_eventfile_22]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION (sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION(sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.event_file
    (
        SET     filename=N'C:\Junk\locks_acq_rel_eventfile_22-.xel'
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=10 SECONDS
    );
```


#### <a name="sysfn_xe_file_target_read_file-function"></a>sys.fn_xe_file_target_read_file (fonction)


La cible event_file stocke les données reçues dans un format binaire qui n’est pas lisible. Transact-SQL peut signaler le contenu du fichier .xel en le sélectionnant à partir de la fonction [**sys.fn_xe_file_target_read_file**](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md) .


Pour SQL Server **2016** et versions ultérieures, l’instruction T-SQL SELECT suivante a inclus les données dans le rapport. Le suffixe *.xel dans le 


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            null, null, null)  AS f;
```


Pour SQL Server **2014**, une instruction SELECT similaire à la suivante inclurait les données dans le rapport. Après SQL Server 2014, les fichiers .xem ne sont plus utilisés.


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            'C:\junk\metafile.xem',
            null, null)  AS f;
```


Bien sûr, vous pouvez utiliser également manuellement l’interface utilisateur SSMS pour voir les données .xel :


#### <a name="data-stored-in-the-event_file-target"></a>Données stockées dans la cible event_file


Ensuite vient le rapport issu de la sélection de **sys.fn_xe_file_target_read_file**, dans SQL Server 2016.


```
module_guid                            package_guid                           object_name     event_data                                                                                                                                                                                                                                                                                          file_name                                                      file_offset
-----------                            ------------                           -----------     ----------                                                                                                                                                                                                                                                                                          ---------                                                      -----------
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_acquired   <event name="lock_acquired" package="sqlserver" timestamp="2016-08-07T20:13:35.827Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_released   <event name="lock_released" package="sqlserver" timestamp="2016-08-07T20:13:35.832Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
```



<a name="h2_target_histogram"></a>

## <a name="histogram-target"></a>Cible d’histogramme


La cible d’ **histogramme** est plus sophistiquée que la cible event_counter. L’histogramme peut effectuer les opérations suivantes :

- Compter les occurrences de plusieurs éléments séparément.
- Compter les occurrences de différents types d’éléments :
    - Champs d’événements.
    - Actions.


Le paramètre **source_type** est la clé permettant de contrôler la cible d’histogramme :

- **source_type = 0** : signifie collecter des données pour les *champs d’événements*).
- **source_type = 1** : signifie collecter des données pour les *actions*).
    - 1 est la valeur par défaut.


La valeur par défaut du paramètre « slots » est 256. Si vous attribuez une autre valeur, la valeur est arrondie à la puissance de 2 la plus proche.

- Par exemple, slots=59 serait arrondie à 64.


### <a name="action-example-for-histogram"></a>Exemple d’*action* pour l’histogramme


Sur sa clause TARGET...SET, l’instruction Transact-SQL CREATE EVENT SESSION suivante spécifie l’affectation du paramètre cible de **source_type = 1**. Le 1 signifie que la cible d’histogramme suit une action.

Dans l’exemple présent, l’offre de la clause EVENT...ACTION se produit pour n’offrir qu’une seule action pour la cible à choisir, à savoir **sqlos.system_thread_id**. Sur la clause TARGET...SET, nous voyons l’affectation **source=N’sqlos.system_thread_id’** de l’affectation.

- Pour effectuer le suivi de plusieurs actions de la source, vous pouvez ajouter une deuxième cible d’histogramme à l’instruction CREATE EVENT SESSION.


```sql
CREATE EVENT SESSION [histogram_lockacquired]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
        (
        ACTION
            (
            sqlos.system_thread_id
            )
        )
    ADD TARGET package0.histogram
        (
        SET
            filtering_event_name=N'sqlserver.lock_acquired',
            slots=(16),
            source=N'sqlos.system_thread_id',
            source_type=1
        )
    WITH
        (
        <.... (For brevity, numerous parameter assignments generated by SSMS.exe are not shown here.) ....>
        );
```


 Les données suivantes ont été capturées. Les valeurs sous la colonne **valeur** étaient des valeurs system_thread_id. Par exemple, un total de 236 verrous ont été effectués sous le thread 6540.


```
value   count
-----   -----
 6540     236
 9308      91
 9668      74
10144      49
 5244      44
 2396      28
```


#### <a name="select-to-discover-available-actions"></a>Utilisez l’instruction SELECT pour découvrir les actions disponibles


L’instruction [C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) SELECT peut rechercher les actions que le système met à votre disposition afin que vous puissiez les spécifier sur votre instruction CREATE EVENT SESSION. Dans la clause WHERE, il vous faudrait tout d’abord modifier le filtre **o.name LIKE** pour refléter les actions qui vous intéressent.


Ensuite, vient un exemple d’ensemble de lignes renvoyé par l’instruction C.3 SELECT. L’action **system_thread_id** s’affiche à la deuxième ligne.


```
Package-Name   Action-Name                 Action-Description
------------   -----------                 ------------------
package0       collect_current_thread_id   Collect the current Windows thread ID
sqlos          system_thread_id            Collect current system thread ID
sqlserver      create_dump_all_threads     Create mini dump including all threads
sqlserver      create_dump_single_thread   Create mini dump for the current thread
```


### <a name="event-field-example-for-histogram"></a>Exemple de *champ* d’événement pour l’histogramme


L’exemple suivant définit **source_type=0**. La valeur attribuée à **source =** est un champ d’événement (et non une action).



```sql
CREATE EVENT SESSION [histogram_checkpoint_dbid]
    ON SERVER 
    ADD EVENT  sqlserver.checkpoint_begin
    ADD TARGET package0.histogram
    (
    SET
        filtering_event_name = N'sqlserver.checkpoint_begin',
        source               = N'database_id',
        source_type          = (0)
    )
    WITH
    ( <....> );
```


Les données suivantes ont été capturées par la cible d’histogramme. Les données montrent que la base de données ID = 5 a connu 7 événements checkpoint_begin.


```
value   count
-----   -----
5       7
7       4
6       3
```


#### <a name="select-to-discover-available-fields-on-your-chosen-event"></a>Utilisez l’instruction SELECT pour découvrir les champs disponibles sur l’événement choisi


L’instruction [C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) SELECT suivante montre les champs d’événement que vous pouvez choisir. Il vous faut tout d’abord modifier le filtre **o.name LIKE** pour qu’il soit le nom d’événement choisi.


L’ensemble de lignes suivant a été renvoyé par l’instruction C.4 SELECT. L’ensemble de lignes montre que database_id est le seul champ de l’événement checkpoint_begin pouvant fournir des valeurs pour la cible d’histogramme.


```
Package-Name   Event-Name         Field-Name   Field-Description
------------   ----------         ----------   -----------------
sqlserver      checkpoint_begin   database_id  NULL
sqlserver      checkpoint_end     database_id  NULL
```


<a name="h2_target_pair_matching"></a>

## <a name="pair_matching-target"></a>Cible pair_matching


La cible pair_matching vous permet de détecter les événements de début qui se produisent sans événement de fin correspondant. Par exemple, cela peut poser problème lorsqu’un événement lock_acquired se produit, mais qu’aucun événement lock_released correspondant ne suit en temps voulu.


Le système ne met pas automatiquement en correspondance les événements de début et de fin. C’est vous qui expliquez la correspondance au système dans l’instruction CREATE EVENT SESSION. Lorsqu’un événement de début et de fin sont mis en correspondance, la paire est ignorée pour que tous les utilisateurs puissent se concentrer sur les événements de début sans correspondance.


#### <a name="finding-matchable-fields-for-the-start-and-end-event-pair"></a>Recherche de champs pouvant être mis en correspondance pour la paire d’événements de début et de fin


À l’aide de l’instruction [C.4 SELECT](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields), nous voyons dans l’ensemble de lignes suivant que 16 champs environ sont associés à l’événement lock_acquired. L’ensemble de lignes affiché ici a été fractionné manuellement pour indiquer les champs mis en correspondance de notre exemple. Il serait insensé de faire correspondre certains champs, comme celui faisant référence à la **durée** de deux événements.


```
Package-Name   Event-Name   Field-Name               Field-Description
------------   ----------   ----------               -----------------
sqlserver   lock_acquired   database_name            NULL
sqlserver   lock_acquired   mode                     NULL
sqlserver   lock_acquired   resource_0               The ID of the locked object, when lock_resource_type is OBJECT.
sqlserver   lock_acquired   resource_1               NULL
sqlserver   lock_acquired   resource_2               The ID of the lock partition, when lock_resource_type is OBJECT, and resource_1 is 0.
sqlserver   lock_acquired   transaction_id           NULL

sqlserver   lock_acquired   associated_object_id     The ID of the object that requested the lock that was acquired.
sqlserver   lock_acquired   database_id              NULL
sqlserver   lock_acquired   duration                 The time (in microseconds) between when the lock was requested and when it was canceled.
sqlserver   lock_acquired   lockspace_nest_id        NULL
sqlserver   lock_acquired   lockspace_sub_id         NULL
sqlserver   lock_acquired   lockspace_workspace_id   NULL
sqlserver   lock_acquired   object_id                The ID of the locked object, when lock_resource_type is OBJECT. For other lock resource types it will be 0
sqlserver   lock_acquired   owner_type               NULL
sqlserver   lock_acquired   resource_description     The description of the lock resource. The description depends on the type of lock. This is the same value as the resource_description column in the sys.dm_tran_locks view.
sqlserver   lock_acquired   resource_type            NULL
```


### <a name="example-of-pair_matching"></a>Exemple de pair_matching


L’instruction CREATE EVENT SESSION suivante spécifie deux événements et deux cibles. La cible pair_matching spécifie deux ensembles de champs afin de faire correspondre les événements par paires. La séquence de champs délimités par des virgules affectées à **begin_matching_columns =** et **end_matching_columns =** doit être identique. Aucune tabulation ou saut de ligne n’est autorisé entre les champs mentionnés dans la valeur délimitée par une virgule, bien que les espaces soient autorisés.

Pour affiner les résultats, nous avons tout d’abord utilisé l’instruction SELECT de sys.objects pour rechercher l’élément object_id de la table test. Nous avons ajouté un filtre pour cet ID à la clause EVENT...WHERE.


```sql
CREATE EVENT SESSION [pair_matching_lock_a_r_33]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.pair_matching
    (
        SET
            begin_event = N'sqlserver.lock_acquired',
            begin_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            end_event = N'sqlserver.lock_released',
            end_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            respond_to_memory_pressure = (1)
    )
    WITH
    (
        MAX_MEMORY = 8192 KB,
        MAX_DISPATCH_LATENCY = 15 SECONDS
    );
```


Pour tester la session d’événements, nous avons empêché volontairement le déverrouillage de verrous acquis. Nous avons effectué cela en réalisant les étapes T-SQL suivantes :

1. BEGIN TRANSACTION.
2. UPDATE MyTable....
3. N’émettez volontairement aucune instruction COMMIT TRANSACTION tant que nous n’avons pas examiné les cibles.
4. Ultérieurement une fois le test, nous avons émis une instruction COMMIT TRANSACTION.

La cible **event_counter** simple a donné les lignes de sortie suivantes. Étant donné que 52-50=2, la sortie nous indique que nous devrions voir 2 événements lock_acquired non jumelés lorsque nous examinons la sortie à partir de la cible pair-matching.


```
package_name   event_name      count
------------   ----------      -----
sqlserver      lock_acquired   52
sqlserver      lock_released   50
```


La cible **pair_matching** a fourni la sortie suivante. Comme suggéré par la sortie event_counter, nous voyons effectivement 2 lignes lock_acquired. Le fait de voir ces lignes signifie que ces deux événements lock_acquired ne sont pas appariés.


```
package_name   event_name      timestamp                     database_name   duration   mode   object_id   owner_type   resource_0   resource_1   resource_2   resource_description   resource_type   transaction_id
------------   ----------      ---------                     -------------   --------   ----   ---------   ----------   ----------   ----------   ----------   --------------------   -------------   --------------
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          S      370100359   Transaction  370100359    3            0            [INDEX_OPERATION]      OBJECT          34126
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          IX     370100359   Transaction  370100359    0            0                                   OBJECT          34126
```


Les lignes des événements lock_acquired non appariés pourraient inclure le texte T-SQL, ou **sqlserver.sql_text**, qui a pris les verrous. Mais nous ne voulions pas gonfler l’affichage.


<a name="h2_target_ring_buffer"></a>

## <a name="ring_buffer-target"></a>Cible ring_buffer


La cible ring_buffer est pratique pour le test d’événements simple et rapide. Lorsque vous arrêtez la session d’événements, la sortie stockée est ignorée.

Dans cette section ring_buffer nous montrons également comment vous pouvez utiliser l’implémentation Transact-SQL de XQuery pour copier le contenu XML de la cible ring_buffer dans un ensemble de lignes relationnel plus lisible.


#### <a name="create-event-session-with-ring_buffer"></a>CREATE EVENT SESSION avec ring_buffer


Aucune information particulière n’est à mentionner au sujet de cette instruction CREATE EVENT SESSION, qui utilise la cible ring_buffer.


```sql
CREATE EVENT SESSION [ring_buffer_lock_acquired_4]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET collect_resource_description=(1)

        ACTION(sqlserver.database_name)

        WHERE
        (
            [object_id]=(370100359)  -- ID of MyTable
            AND
            sqlserver.database_name='InMemTest2'
        )
    )
    ADD TARGET package0.ring_buffer
    (
        SET max_events_limit=(98)
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=3 SECONDS
    );
```


### <a name="xml-output-received-for-lock_acquired-by-ring_buffer"></a>Sortie XML reçue pour lock_acquired par ring_buffer


Le contenu extrait par une instruction SELECT est au format d’une chaîne XML. La chaîne XML stockée par la cible ring_buffer dans nos tests, est présentée ci-après. Toutefois, pour limiter l’affichage XML suivant, tous les éléments d’&#x3c ;événement &#x3e ;, à l’exception de deux éléments ont été supprimés. En outre, au sein de chaque élément &#x3c;event&#x3e;, quelques éléments &#x3c;data&#x3e; superflus ont été supprimés.


```xml
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="6" eventCount="6" droppedCount="0" memoryUsed="1032">
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:53.987Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111030</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:56.012Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111039</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
</RingBufferTarget>
```


Pour afficher le code XML précédent, vous pouvez émettre l’instruction SELECT suivante lorsque la session d’événements est active. Les données XML actives sont récupérées à partir de la vue système **sys.dm_xe_session_targets**.


```sql
SELECT
        CAST(LocksAcquired.TargetXml AS XML)  AS RBufXml
    INTO
        #XmlAsTable
    FROM
        (
        SELECT
                CAST(t.target_data AS XML)  AS TargetXml
            FROM
                     sys.dm_xe_session_targets  AS t
                JOIN sys.dm_xe_sessions         AS s

                    ON s.address = t.event_session_address
            WHERE
                t.target_name = 'ring_buffer'
                AND
                s.name        = 'ring_buffer_lock_acquired_4'
        )
            AS LocksAcquired;


SELECT * FROM #XmlAsTable;
```


### <a name="xquery-to-see-the-xml-as-a-rowset"></a>XQuery pour afficher le code XML sous forme d’un ensemble de lignes


Pour afficher le code XML précédent sous forme d’un ensemble de lignes relationnel, continuez à partir de l’instruction SELECT précédente en émettant l’instruction T-SQL suivante. Les lignes commentées expliquent chaque utilisation de XQuery.


```sql
SELECT
         -- (A)
         ObjectLocks.value('(@timestamp)[1]',
            'datetime'     )  AS [OccurredDtTm]

        -- (B)
        ,ObjectLocks.value('(data[@name="mode"]/text)[1]',
            'nvarchar(32)' )  AS [Mode]

        -- (C)
        ,ObjectLocks.value('(data[@name="transaction_id"]/value)[1]',
            'bigint' )  AS [TxnId]

        -- (D)
        ,ObjectLocks.value('(action[@name="database_name" and @package="sqlserver"]/value)[1]',
            'nvarchar(128)')  AS [DatabaseName]
    FROM
        #TableXmlCell
    CROSS APPLY
        -- (E)
        TargetDateAsXml.nodes('/RingBufferTarget/event[@name="lock_acquired"]')  AS T(ObjectLocks);
```


#### <a name="xquery-notes-from-preceding-select"></a>Notes de XQuery issues de l’instruction SELECT précédente


(A)
- Valeur de timestamp= attribute, sur l’élément &#x3c;event&#x3e;.
- La construction ’ (...) [1]’ garantit le renvoi d’une seule valeur par itération, comme il s’agit d’une limitation requise de la méthode .value() XQuery de la variable et des colonnes de type de données XML.


(B)
- Valeur interne de l’élément &#x3c;text&#x3e; dans un élément &#x3c;data&#x3e; dont son name= attribute est égal à « mode ».


(C)
- Valeur interne des éléments &#x3c;value&#x3e; dans un élément &#x3c;data&#x3e; dont son name= attribute est égal à « transaction_id ».


(D)
- &#x3c;event&#x3e; contient &#x3c;action&#x3e;.
- &#x3c;action&#x3e; dont name= attribute est égal à « database_name » et package= attribute est égal à « sqlserver »" (et non « package0 »), obtient la valeur interne de l’élément &#x3c;value&#x3e;.


(E)
- C.A. entraîne la répétition du traitement de chaque élément &#x3c;event&#x3e; individuel dont name= attribute est égal à « lock_acquired ».
- Cela s’applique au code XML renvoyé par la clause FROM précédente.


#### <a name="output-from-xquery-select"></a>Sortie de XQuery SELECT


Ensuite, vient l’ensemble de lignes généré par l’instruction T-SQL précédente qui inclut XQuery.


```
OccurredDtTm              Mode    DatabaseName
------------              ----    ------------
2016-08-05 23:59:53.987   SCH_S   InMemTest2
2016-08-05 23:59:56.013   SCH_S   InMemTest2
```



## <a name="xevent-net-namespaces-and-cx23"></a>Espaces de noms XEvent .NET et C&#x23;


Package0 a deux cibles supplémentaires, qui ne peuvent toutefois pas être utilisées dans Transact-SQL :

- compressed_history
- event_stream


Une façon de savoir que ces deux cibles ne peuvent pas être utilisées dans T-SQL est que leurs valeurs non null de la colonne *sys.dm_xe_objects.capabilities* n’incluent pas le bit 0x1.


La cible event_stream peut être utilisée dans des programmes .NET écrits dans des langages tels que C#. Les développeurs C# et .NET peuvent accéder à un flux d’événements via une classe .NET Framework, comme dans l’espace de noms Microsoft.SqlServer.XEvents.Linq.

Si l’erreur **25726** se produit, cela signifie que le remplissage de données du flux d’événements a été plus rapide que la consommation des données par le client. Cela a provoqué la déconnexion du moteur de base de données du flux d’événements pour éviter le ralentissement des performances du serveur.


### <a name="xevent-namespaces"></a>Espaces de noms XEvent


- [Espace de noms Microsoft.SqlServer.Management.XEvent](/dotnet/api/microsoft.sqlserver.management.xevent)

- [Espace de noms Microsoft.SqlServer.XEvent.Linq](/dotnet/api/microsoft.sqlserver.xevent.linq)
