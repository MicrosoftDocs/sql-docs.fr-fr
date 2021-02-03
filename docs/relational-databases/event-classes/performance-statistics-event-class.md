---
description: Performance Statistics (classe d'événements)
title: Performance Statistics, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Performance Statistics event class
ms.assetid: da9cd2c4-6fdd-4ada-b74f-105e3541393c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5f1ffd07f6573a37005fb7bd20bfa015a0a2102b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196314"
---
# <a name="performance-statistics-event-class"></a>Performance Statistics (classe d'événements)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d'événements Performance Statistics permet de surveiller les performances des requêtes, des procédures stockées et des déclencheurs en cours d'exécution. Chacune des six sous-classes d'événements indique un événement dans la durée de vie des requêtes, des procédures stockées et des déclencheurs au sein du système. En combinant ces sous-classes d'événements aux vues de gestion dynamique sys.dm_exec_query_stats, sys.dm_exec_procedure_statset sys.dm_exec_trigger_stats correspondantes, vous pouvez reconstituer l'historique des performances d'une requête, d'une procédure stockée ou d'un déclencheur donnés.  
  
## <a name="performance-statistics-event-class-data-columns"></a>Colonnes de données de la classe d'événements Performance Statistics  
 Les tableaux suivants décrivent les colonnes des données de la classe d’événements associées à chaque sous-classe des événements suivantes : EventSubClass 0, EventSubClass 1,EventSubClass 2,EventSubClass 3, EventSubClass 4 et EventSubClass 5.  
  
### <a name="eventsubclass-0"></a>EventSubClass 0  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|NULL|52|Oui|  
|BinaryData|**image**|NULL|2|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 0 = Nouveau texte SQL de traitement qui n'est pas actuellement présent dans le cache.<br /><br /> Les types EventSubClass suivants sont générés dans la trace pour les traitements ad hoc.<br /><br /> Pour les lots ad hoc avec un nombre *n* de requêtes :<br /><br /> 1 de type 0|21|Oui|  
|IntegerData2|**int**|NULL|55|Oui|  
|ObjectID|**int**|NULL|22|Oui|  
|Offset|**int**|NULL|61|Oui|  
|PlanHandle|**Image**|NULL|65|Oui|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|SqlHandle|**image**|Handle SQL permettant d'obtenir le texte SQL du lot à l'aide de la vue de gestion dynamique sys.dm_exec_sql_text.|63|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|Texte SQL du traitement.|1|Oui|  
  
### <a name="eventsubclass-1"></a>EventSubClass 1  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Nombre cumulatif de fois où ce plan a été recompilé.|52|Oui|  
|BinaryData|**image**|Code XML binaire du plan compilé.|2|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 1 = Les requêtes d'une procédure stockée ont été compilées.<br /><br /> Les types EventSubClass suivants sont générés dans la trace pour les procédures stockées.<br /><br /> Pour les procédures stockées avec un nombre *n* de requêtes :<br /><br /> Nombre *n* de type 1|21|Oui|  
|IntegerData2|**int**|Fin de l'instruction dans la procédure stockée.<br /><br /> -1 pour la fin de la procédure stockée.|55|Oui|  
|ObjectID|**int**|ID affecté à l'objet par le système.|22|Oui|  
|Offset|**int**|Décalage de départ de l'instruction dans la procédure stockée ou le lot.|61|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|SqlHandle|**image**|Handle SQL permettant d'obtenir le texte SQL de la procédure stockée à l'aide de la vue de gestion dynamique dm_exec_sql_text.|63|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|NULL|1|Oui|  
|PlanHandle|**image**|Handle de plan du plan compilé pour la procédure stockée. Il peut servir à obtenir le plan XML à l'aide de la vue de gestion dynamique sys.dm_exec_query_plan.|65|Oui|  
|ObjectType|**int**|Valeur représentant le type de l'objet qui intervient dans l'événement.<br /><br /> 8272 = procédure stockée|28|Oui|  
|BigintData2|**bigint**|Mémoire totale, en kilo-octets, utilisée au cours de la compilation.|53|Oui|  
|UC|**int**|Durée UC totale, en millièmes de secondes, passée au cours de la compilation.|18|Oui|  
|Duration|**int**|Duré totale, en microsecondes, passée au cours de la compilation.|13|Oui|  
|IntegerData|**int**|Taille, en kilo-octets, du plan compilé.|25|Oui|  
  
### <a name="eventsubclass-2"></a>EventSubClass 2  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Nombre cumulatif de fois où ce plan a été recompilé.|52|Oui|  
|BinaryData|**image**|Code XML binaire du plan compilé.|2|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 2 = Les requêtes d'une instruction SQL ad hoc ont été compilées.<br /><br /> Les types EventSubClass suivants sont générés dans la trace pour les traitements ad hoc.<br /><br /> Pour les lots ad hoc avec un nombre *n* de requêtes :<br /><br /> Nombre *n* de type 2|21|Oui|  
|IntegerData2|**int**|Fin de l'instruction dans le traitement.<br /><br /> -1 pour la fin du traitement.|55|Oui|  
|ObjectID|**int**|N/A|22|Oui|  
|Offset|**int**|Décalage de départ de l'instruction dans le traitement.<br /><br /> 0 pour le début du traitement.|61|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|SqlHandle|**image**|Handle SQL. Peut être utilisé pour obtenir le texte SQL du lot à l'aide de la vue de gestion dynamique dm_exec_sql_text.|63|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|NULL|1|Oui|  
|PlanHandle|**image**|Descripteur de plan du plan compilé pour le traitement. Il peut servir à obtenir le plan XML du lot à l'aide de la vue de gestion dynamique dm_exec_query_plan.|65|Oui|  
|BigintData2|**bigint**|Mémoire totale, en kilo-octets, utilisée au cours de la compilation.|53|Oui|  
|UC|**int**|Durée UC totale, en microsecondes, passée au cours de la compilation.|18|Oui|  
|Duration|**int**|Duré totale, en millisecondes, passée au cours de la compilation.|13|Oui|  
|IntegerData|**int**|Taille, en kilo-octets, du plan compilé.|25|Oui|  
  
### <a name="eventsubclass-3"></a>EventSubClass 3  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Nombre cumulatif de fois où ce plan a été recompilé.|52|Oui|  
|BinaryData|**image**|NULL|2|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 3 = Une requête mise en cache a été détruite et l'historique des données de performances correspondant au plan est sur le point d'être détruit.<br /><br /> Les types EventSubClass suivants sont générés dans la trace.<br /><br /> Pour les lots ad hoc avec un nombre *n* de requêtes :<br /><br /> 1 de type 3 lorsque la requête est vidée du cache<br /><br /> Pour les procédures stockées avec un nombre *n* de requêtes :<br /><br /> 1 de type 3 lorsque la requête est vidée du cache.|21|Oui|  
|IntegerData2|**int**|Fin de l'instruction dans la procédure stockée ou le traitement.<br /><br /> -1 pour la fin de la procédure stockée ou du traitement.|55|Oui|  
|ObjectID|**int**|NULL|22|Oui|  
|Offset|**int**|Décalage de départ de l'instruction dans la procédure stockée ou le lot.<br /><br /> 0 pour le début de la procédure stockée ou du traitement.|61|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|SqlHandle|**image**|Handle SQL permettant d'obtenir le texte SQL de la procédure stockée ou du lot à l'aide de la vue de gestion dynamique dm_exec_sql_text.|63|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|QueryExecutionStats|1|Oui|  
|PlanHandle|**image**|Descripteur de plan du plan compilé pour la procédure stockée ou du traitement. Il peut servir à obtenir le plan XML à l'aide de la vue de gestion dynamique dm_exec_query_plan.|65|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
  
### <a name="eventsubclass-4"></a>EventSubClass 4  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|NULL|52|Oui|  
|BinaryData|**image**|NULL|2|Oui|  
|DatabaseID|**int**|ID de la base de données dans laquelle réside la procédure stockée spécifiée.|3|Oui|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 4 = Une procédure stockée mise en cache a été supprimée du cache et l'historique des données de performances associées est sur le point d'être détruit.|21|Oui|  
|IntegerData2|**int**|NULL|55|Oui|  
|ObjectID|**int**|ID de la procédure stockée. Il est identique à celui de la colonne object_id dans sys.procedures.|22|Oui|  
|Offset|**int**|NULL|61|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|SqlHandle|**image**|Handle SQL permettant d'obtenir le texte SQL de la procédure stockée qui a été exécutée à l'aide de la vue de gestion dynamique dm_exec_sql_text.|63|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|ProcedureExecutionStats|1|Oui|  
|PlanHandle|**image**|Handle de plan du plan compilé pour la procédure stockée. Il peut servir à obtenir le plan XML à l'aide de la vue de gestion dynamique dm_exec_query_plan.|65|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
  
### <a name="eventsubclass-5"></a>EventSubClass 5  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|NULL|52|Oui|  
|BinaryData|**image**|NULL|2|Oui|  
|DatabaseID|**int**|ID de la base de données dans laquelle réside le déclencheur spécifié.|3|Oui|  
|EventSequence|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|SessionLoginName|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le nom Connexion1 et que vous exécutez une instruction en tant que Connexion2, SessionLoginName affiche Connexion1 et LoginName, Connexion2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|EventSubClass|**int**|Type de sous-classe d'événements.<br /><br /> 5 = Un déclencheur mis en cache a été supprimé du cache et l'historique des données de performances associées est sur le point d'être détruit.|21|Oui|  
|IntegerData2|**int**|NULL|55|Oui|  
|ObjectID|**int**|ID du déclencheur. Il est identique à celui de la colonne object_id des affichages catalogue sys.triggers/sys.server_triggers.|22|Oui|  
|Offset|**int**|NULL|61|Oui|  
|SPID|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|SqlHandle|**image**|Handle SQL permettant d'obtenir le texte SQL du déclencheur à l'aide de la vue de gestion dynamique dm_exec_sql_text.|63|Oui|  
|StartTime|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|TextData|**ntext**|TriggerExecutionStats|1|Oui|  
|PlanHandle|**image**|Handle de plan du plan compilé pour le déclencheur. Il peut servir à obtenir le plan XML à l'aide de la vue de gestion dynamique dm_exec_query_plan.|65|Oui|  
|GroupID|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Classe d'événements Showplan XML for Query Compile](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  
