---
title: Surveiller les scripts avec des événements étendus
description: Découvrez comment utiliser des événements étendus pour surveiller et résoudre les problèmes liés à SQL Server Machine Learning Services, à SQL Server Launchpad et aux scripts externes des travaux R ou Python.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/04/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: d8324e49ce4de61096880275a07df5a20e288a93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337154"
---
# <a name="monitor-python-and-r-scripts-with-extended-events-in-sql-server-machine-learning-services"></a>Surveiller les scripts Python et R avec des événements étendus dans SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Découvrez comment utiliser des événements étendus pour surveiller et résoudre les problèmes liés à SQL Server Machine Learning Services, à SQL Server Launchpad et aux scripts externes des travaux R ou Python.

## <a name="extended-events-for-sql-server-machine-learning-services"></a>Événements étendus pour SQL Server Machine Learning Services

Pour afficher la liste des événements liés à SQL Server Machine Learning Services, exécutez la requête suivante depuis Azure Data Studio ou SQL Server Management Studio.

```sql
SELECT o.name AS event_name, o.description
FROM sys.dm_xe_objects o
JOIN sys.dm_xe_packages p
ON o.package_guid = p.guid
WHERE o.object_type = 'event'
AND p.name = 'SQLSatellite';
```

Pour plus d’informations sur l’utilisation des événements étendus, consultez [Outils associés aux événements étendus](../../relational-databases/extended-events/extended-events-tools.md).

## <a name="additional-events-specific-to-machine-learning-services"></a>Événements supplémentaires spécifiques à Machine Learning Services

Des événements étendus supplémentaires sont disponibles pour les composants liés à SQL Server Machine Learning Services et utilisés par ces derniers, tels que [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]et BXLServer, et le processus satellite qui démarre le runtime Python ou R. Ces événements étendus supplémentaires sont déclenchés à partir des processus externes. Par conséquent, ils doivent être capturés à l’aide d’un utilitaire externe.

Pour plus d’informations sur cette opération, consultez la section [Collecte d’événements à partir de processus externes](#bkmk_externalevents).

<a name="bkmk_xeventtable"></a> 

## <a name="table-of-extended-events"></a>Tableau des événements étendus

|Événement|Description|Notes|  
|-----------|-----------------|---------|  
|connection_accept|Se produit lorsqu’une nouvelle connexion est acceptée. Cet événement permet de journaliser toutes les tentatives de connexion.||  
|failed_launching|Le lancement a échoué.|Indique une erreur.|  
|satellite_abort_connection|Enregistrement d’abandon de connexion||  
|satellite_abort_received|Se déclenche lorsqu’un message d’abandon est reçu sur une connexion satellite.||  
|satellite_abort_sent|Se déclenche lorsqu’un message d’abandon est envoyé sur une connexion satellite.||  
|satellite_authentication_completion|Se déclenche lorsque l'authentification est terminée pour une connexion sur TCP ou canal nommé.||  
|satellite_authorization_completion|Se déclenche lorsque l'autorisation est effectuée pour une connexion sur TCP ou canal nommé.||  
|satellite_cleanup|Se déclenche lorsque le satellite appelle la fonction de nettoyage.|Déclenché uniquement à partir de processus externes. Consultez les instructions sur la collecte d'événements à partir de processus externes.|  
|satellite_data_chunk_sent|Se déclenche lorsque la connexion satellite termine l'envoi d'un bloc de données unique.|L’événement indique le nombre de lignes envoyées, le nombre de colonnes, le nombre de paquets SNI utilisés et le temps écoulé en millisecondes pendant l’envoi du bloc. Ces informations peuvent vous aider à comprendre le temps nécessaire à la transmission des différents types de données, ainsi que le nombre de paquets utilisés.|  
|satellite_data_receive_completion|Se déclenche lorsque toutes les données requises par une requête sont reçues sur la connexion satellite.|Déclenché uniquement à partir de processus externes. Consultez les instructions sur la collecte d'événements à partir de processus externes.|  
|satellite_data_send_completion|Se déclenche lorsque toutes les données requises pour une session sont envoyées sur la connexion satellite.||  
|satellite_data_send_start|Se déclenche lorsque la transmission des données démarre.| La transmission des données démarre juste avant l’envoi du premier bloc de données.|  
|satellite_error|Utilisé pour le traçage des erreurs de satellite sql||  
|satellite_invalid_sized_message|La taille du message n’est pas valide||  
|satellite_message_coalesced|Utilisé pour fusionner les messages de traçage sur la couche réseau||  
|satellite_message_ring_buffer_record|Enregistrement de la mémoire tampon en anneau du message||  
|satellite_message_summary|Informations récapitulatives de la messagerie||  
|satellite_message_version_mismatch|Le champ de version du message ne correspond pas||  
|satellite_messaging|Utilisé pour le traçage des événements de messagerie (lier, dissocier, etc.)||  
|satellite_partial_message|Utilisé pour le traçage des messages partiels sur la couche réseau||  
|satellite_schema_received|Se déclenche lorsque le message de schéma est reçu et lu par SQL.||  
|satellite_schema_sent|Se déclenche lorsque le message de schéma est envoyé par le satellite.|Déclenché uniquement à partir de processus externes. Consultez les instructions sur la collecte d'événements à partir de processus externes.|  
|satellite_service_start_posted|Se déclenche lorsque le message de démarrage de service est envoyé à Launchpad.|Ce message indique à Launchpad de démarrer le processus externe, et contient un ID pour la nouvelle session.|  
|satellite_unexpected_message_received|Se déclenche lors de la réception d'un message inattendu.|Indique une erreur.|  
|stack_trace|Se produit lors d'une demande de vidage mémoire pour le processus.|Indique une erreur.|  
|trace_event|Utilisé à des fins de traçage|Ces événements peuvent contenir des messages de trace de SQL Server, de Launchpad et de processus externes. Cela comprend la sortie vers stdout et stderr à partir de R.|  
|launchpad_launch_start|Se déclenche lorsque Launchpad démarre le lancement d’un satellite.|Déclenché uniquement à partir de Launchpad. Consultez les instructions sur la collecte d'événements à partir de launchpad.exe.|  
|launchpad_resume_sent|Se déclenche lorsque launchpad a lancé le satellite et envoyé un message de reprise à SQL Server.|Déclenché uniquement à partir de Launchpad. Consultez les instructions sur la collecte d'événements à partir de launchpad.exe.|  
|satellite_data_chunk_sent|Se déclenche lorsque la connexion satellite termine l'envoi d'un bloc de données unique.|Contient des informations sur le nombre de colonnes, le nombre de lignes, le nombre de paquets et le temps écoulé pour l’envoi du bloc.|  
|satellite_sessionId_mismatch|L’ID de session du message n’est pas attendu||  

<a name="bkmk_externalevents"></a>

### <a name="collecting-events-from-external-processes"></a>Collecte d’événements à partir de processus externes

SQL Server Machine Learning Services démarre des services s’exécutant à l’extérieur du processus SQL Server. Pour capturer les événements liés à ces processus externes, vous devez créer un fichier de configuration de traçage des événements et placer le fichier dans le même répertoire que l’exécutable du processus.  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!IMPORTANT]
> À partir de SQL Server 2019, le mécanisme d’isolation a changé. Il est par conséquent nécessaire d’accorder les autorisations appropriées au répertoire dans lequel est stocké le fichier de configuration du suivi d’événements. Pour savoir comment définir ces autorisations, consultez la [section Autorisations de fichiers dans SQL Server 2019 sur Windows : Modifications de l’isolation dans Machine Learning Services](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

+ **[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]**   
  
    Pour capturer les événements liés à Launchpad, placez le fichier *.xml* dans le répertoire Binn de l’instance SQL Server. Dans une installation par défaut, il s’agit de :

    `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\MSSQL\Binn`.  
  
+ **BXLServer** est le processus satellite qui prend en charge l’extensibilité SQL avec des scripts de langage externes tels que R ou Python. Une instance distincte de BxlServer est lancée pour chaque instance de langage externe.
  
    Pour capturer les événements liés à BXLServer, placez le fichier *.xml* dans le répertoire d’installation de R ou de Python. Dans une installation par défaut, il s’agit de :
     
    **R :** `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64`.  

    **Python :** `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\PYTHON_SERVICES\Lib\site-packages\revoscalepy\rxLibs`.

Le fichier de configuration doit porter le même nom que le fichier exécutable, avec le format « [nom].xevents.xml ». En d'autres termes, les fichiers doivent être nommés comme suit :

+ `Launchpad.xevents.xml`
+ `bxlserver.xevents.xml`

Le fichier de configuration lui-même a le format suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
<event_session name="[session name]" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="you">Xevent for launchpad or bxl server.</description>  
    <event package="SQLSatellite" name="[XEvent Name 1]" />  
    <event package="SQLSatellite" name="[XEvent Name 2]" />  
    <target package="package0" name="event_file">  
      <parameter name="filename" value="[SessionName].xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Pour configurer la trace, modifiez l’espace réservé *nom de session*, l’espace réservé pour le nom de fichier (`[SessionName].xel`) et les noms des événements que vous voulez capturer (par exemple `[XEvent Name 1]`, `[XEvent Name 1]`).  
+ Un nombre quelconque de balises de packages d’événement peut apparaître. Elles seront collectées tant que l’attribut de nom est correct.

### <a name="example-capturing-launchpad-events"></a>Exemple : Capture des événements de Launchpad

L’exemple suivant montre la définition d’une trace d’événements pour le service Launchpad :

```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
<event_session name="sqlsatelliteut" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="hay">Xevent for sql tdd runner.</description>  
    <event package="SQLSatellite" name="launchpad_launch_start" />  
    <event package="SQLSatellite" name="launchpad_resume_sent" />  
    <target package="package0" name="event_file">  
      <parameter name="filename" value="launchpad_session.xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Placez le fichier *.xml* dans le répertoire Binn de l’instance SQL Server.
+ Ce fichier doit être nommé `Launchpad.xevents.xml`.

### <a name="example-capturing-bxlserver-events"></a>Exemple : Capture des événements de BXLServer  

L'exemple suivant montre la définition d'une trace d'événements pour le service BXLServer.
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
 <event_session name="sqlsatelliteut" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="hay">Xevent for sql tdd runner.</description>  
    <event package="SQLSatellite" name="satellite_abort_received" />  
    <event package="SQLSatellite" name="satellite_authentication_completion" />  
    <event package="SQLSatellite" name="satellite_cleanup" />  
    <event package="SQLSatellite" name="satellite_data_receive_completion" />  
    <event package="SQLSatellite" name="satellite_data_send_completion" />  
    <event package="SQLSatellite" name="satellite_data_send_start" />  
    <event package="SQLSatellite" name="satellite_schema_sent" />   
    <event package="SQLSatellite" name="satellite_unexpected_message_received" />    
    <event package="SQLSatellite" name="satellite_data_chunk_sent" />   
    <target package="package0" name="event_file">  
      <parameter name="filename" value="satellite_session.xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Placez le fichier *.xml* dans le même répertoire que l’exécutable de BXLServer.
+ Ce fichier doit être nommé `bxlserver.xevents.xml`.

## <a name="next-steps"></a>Étapes suivantes

- [Surveiller l’exécution de scripts Python et R à l’aide de rapports personnalisés dans SQL Server Management Studio](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)
- [Surveiller SQL Server Machine Learning Services à l’aide de vues de gestion dynamique (DMV)](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)