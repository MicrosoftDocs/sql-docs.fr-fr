---
description: Classe d'événements Lock:Escalation
title: Lock:Escalation, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Escalation event class
- lock escalation [SQL Server], event class
ms.assetid: d253b44c-7600-4afa-a3a7-03cc937c6a4b
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4b873770c5c8c7ac79d98a29706382615c254bf1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195667"
---
# <a name="lockescalation-event-class"></a>Classe d'événements Lock:Escalation
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La classe d’événements **Lock:Escalation** indique qu’un verrouillage spécifique s’est transformé en verrouillage de plus grande ampleur, comme par exemple un verrou de ligne transformé en verrou d’objet. La classe d'événements Escalation est l'ID d'événement 60.  
  
## <a name="lockescalation-event-class-data-columns"></a>Colonnes de données de la classe d'événements Lock:Escalation  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|**ClientProcessID**|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. La colonne de données est remplie si le client fournit l'ID du processus client.|9|Oui|  
|**DatabaseID**|**int**|ID de la base de données dans laquelle le verrou a été obtenu. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**DatabaseName**|**nvarchar**|Nom de la base de données dans laquelle a eu lieu la promotion.|35|Oui|  
|**EventClass**|**int**|Type d’événement = 60.|27|Non|  
|**EventSubClass**|**int**|Cause de l'escalade de verrous :<br /><br /> **0 - LOCK_THRESHOLD** indique que l’instruction a dépassé le seuil de verrou.<br /><br /> **1 - MEMORY_THRESHOLD** indique que l’instruction a dépassé le seuil de la mémoire.|21|Oui|  
|**EventSequence**|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|**GroupID**|**int**|ID du groupe de charges de travail où l'événement Trace SQL se déclenche.|66|Oui|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel le client est exécuté. La colonne de données est remplie si le client fournit le nom de l'hôte. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|**IntegerData**|**int**|Nombre de verrous HoBT. Nombre de verrous pour HoBT au moment de l'escalade de verrous.|25|Oui|  
|**IntegerData2**|**int**|Nombre de verrous escaladés. Nombre total des verrous convertis. Ces structures de verrou sont libérées parce qu'elles sont déjà couvertes par le verrou escaladé.|55|Oui|  
|**IsSystem**|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|**LineNumber**|**int**|Numéro de ligne de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .|5|Oui|  
|**LoginName**|**nvarchar**|Nom de la connexion de l'utilisateur (soit la connexion de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit les informations d'identification de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows au format DOMAINE\nom_utilisateur).|11|Oui|  
|**LoginSid**|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Vous pouvez trouver ces informations dans l’affichage catalogue **sys.server_principals** . Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**Mode**|**int**|Mode de verrouillage obtenu après l'escalade :<br /><br /> 0=NULL - Compatible avec tous les autres modes de verrouillage (LCK_M_NL)<br /><br /> 1=Verrou de stabilité de schéma (LCK_M_SCH_S)<br /><br /> 2 = verrou de modification de schéma (LCK_M_SCH_M)<br /><br /> 3 = verrou partagé (LCK_M_S)<br /><br /> 4 = verrou de mise à jour (LCK_M_U)<br /><br /> 5 = verrou exclusif (LCK_M_X)<br /><br /> 6 = verrou Intent partagé (LCK_M_IS)<br /><br /> 7 = verrou Intent de mise à jour (LCK_M_IU)<br /><br /> 8 = verrou Intent exclusif (LCK_M_IX)<br /><br /> 9 = verrou partagé avec mise à jour Intent (LCK_M_SIU)<br /><br /> 10 = verrou partagé avec Intent exclusif (LCK_M_SIX)<br /><br /> 11 = verrou mise à jour avec Intent exclusif (LCK_M_UIX)<br /><br /> 12 = verrou de mise à jour en bloc (LCK_M_BU)<br /><br /> 13 = verrou d'étendue de clés partagé/de ressources partagé (LCK_M_RS_S)<br /><br /> 14 = verrou d'étendue de clés partagé/de ressources partagé (LCK_M_RS_U)<br /><br /> 15 = verrou d'étendue de clés d'insertion de valeurs NULL (LCK_M_RI_NL)<br /><br /> 16 = verrou d'étendue de clés d'insertion partagé (LCK_M_RI_S)<br /><br /> 17 = verrou d'étendue de clés d'insertion de mise à jour (LCK_M_RI_U)<br /><br /> 18 = verrou d'étendue de clés d'insertion exclusif (LCK_M_RI_X)<br /><br /> 19 = verrou d'étendue de clés exclusif partagé (LCK_M_RX_S)<br /><br /> 20 = verrou d'étendue de clés exclusif de mise à jour (LCK_M_RX_U)<br /><br /> 21 = verrou d'étendue de clés exclusif/de ressources exclusif (LCK_M_RX_X)|32|Oui|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom d'utilisateur Windows.|6|Oui|  
|**ObjectID**|**int**|ID assigné par système de la table pour laquelle l'escalade de verrous a été déclenchée.|22|Oui|  
|**ObjectID2**|**bigint**|ID de l'objet ou de l'entité associé. (ID HoBT pour lequel l'escalade de verrous a été déclenchée.)|56|Oui|  
|**Offset**|**int**|Décalage de départ de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .|61|Oui|  
|**OwnerID**|**int**|1=TRANSACTION<br /><br /> 2=CURSOR<br /><br /> 3=SESSION<br /><br /> 4=SHARED_TRANSACTION_WORKSPACE<br /><br /> 5=EXCLUSIVE_TRANSACTION_WORKSPACE<br /><br /> 6=WAITFOR_QUERY|58|Oui|  
|**RequestID**|**int**|ID de la demande contenant l'instruction.|49|Oui|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|**SessionLoginName**|**nvarchar**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen de Login1 et que vous exécutez une commande en tant que Login2, **SessionLoginName** affiche Login1 et **LoginName** affiche Login2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|**SPID**|**int**|ID de la session au cours de laquelle l'événement s'est produit.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle a débuté l'événement, si elle est connue.|14|Oui|  
|**TextData**|**ntext**|Texte de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui a provoqué l'escalade de verrous.|1|Oui|  
|**TransactionID**|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
|**Type**|**int**|Granularité de l'escalade de verrous :<br /><br /> 1=NULL_RESOURCE<br /><br /> 2=DATABASE<br /><br /> 3=FILE<br /><br /> 5=OBJECT (niveau table)<br /><br /> 6=PAGE<br /><br /> 7=KEY<br /><br /> 8=EXTENT<br /><br /> 9=RID<br /><br /> 10=APPLICATION<br /><br /> 11=METADATA<br /><br /> 12=HOBT<br /><br /> 13=ALLOCATION_UNIT|57|Oui|  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise la procédure `sp_trace_create` pour créer une trace, utilise `sp_trace_setevent` pour ajouter des colonnes d'escalade de verrous à la trace, puis utilise `sp_trace_setstatus` pour démarrer la trace. Dans les instructions telles que `EXEC sp_trace_setevent @TraceID, 60, 22, 1`, le nombre `60` désigne la classe d’événements d’escalade, `22` désigne la colonne **ObjectID** et `1` affecte à l’événement de trace la valeur ON.  
  
```  
DECLARE @RC int, @TraceID int;  
EXEC @rc = sp_trace_create @TraceID output, 0, N'C:\TraceResults';  
-- Set the events and data columns you need to capture.  
EXEC sp_trace_setevent @TraceID, 60,  1, 1; --  1 = TextData  
EXEC sp_trace_setevent @TraceID, 60, 12, 1; -- 12 = SPID  
EXEC sp_trace_setevent @TraceID, 60, 21, 1; -- 21 = EventSubClass  
EXEC sp_trace_setevent @TraceID, 60, 22, 1; -- 22 = ObjectID  
EXEC sp_trace_setevent @TraceID, 60, 25, 1; -- 25 = IntegerData  
EXEC sp_trace_setevent @TraceID, 60, 55, 1; -- 25 = IntegerData2  
EXEC sp_trace_setevent @TraceID, 60, 57, 1; -- 57 = Type  
-- Set any filter  by using sp_trace_setfilter.  
-- Start the trace.  
EXEC sp_trace_setstatus @TraceID, 1;  
GO  
```  
  
 Maintenant que la trace s'exécute, exécutez les instructions que vous souhaitez tracer. Une fois ces instructions terminées, exécutez le code suivant pour arrêter et puis fermer la trace. Cet exemple utilise la fonction `fn_trace_getinfo` pour utiliser `traceid` dans les instructions `sp_trace_setstatus` .  
  
```  
-- After the trace is complete.  
DECLARE @TraceID int;  
-- Find the traceid of the current trace.  
SELECT @TraceID = traceid   
FROM ::fn_trace_getinfo(default)   
WHERE value = N'C:\TraceResults.trc';  
  
-- First stop the trace.   
EXEC sp_trace_setstatus @TraceID, 0;  
  
-- Close and then delete its definition from SQL Server.   
EXEC sp_trace_setstatus @TraceID, 2;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
