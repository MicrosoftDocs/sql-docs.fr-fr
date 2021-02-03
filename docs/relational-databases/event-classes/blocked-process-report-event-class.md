---
title: Blocked Process Report, classe d’événements | Microsoft Docs
description: La classe d’événements Blocked Process Report indique qu’une tâche a été bloquée plus longtemps que la période spécifiée dans SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Blocked Process Report event class
ms.assetid: e8acb408-938d-4b36-81dd-04f087410cc5
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bfefbd1c87ed3e6da5432a3c45bd1bd99fc28a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187514"
---
# <a name="blocked-process-report-event-class"></a>Blocked Process Report (classe d'événements)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d’événements **Blocked Process Report** indique qu’une tâche a été bloquée plus longtemps que la période spécifiée. Elle ne s'applique pas aux tâches système ou aux tâches en attente de ressources à blocage non détectable.  
  
 Pour définir le seuil et la fréquence de génération des rapports, utilisez la commande **sp_configure** pour configurer l’option **Seuil de processus bloqué** qu’il est possible de définir en secondes. Par défaut, aucun rapport de processus bloqué n'est généré. Pour plus d’informations sur la définition de l’option **Seuil de processus bloqué** , consultez [Seuil de processus bloqué (option de configuration de serveur)](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md).  
  
 Pour plus d’informations sur le filtrage des données retournées par la classe d’événements **Blocked Process Report**, consultez [Filtrer des événements dans une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md), [Définir un filtre de trace &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/set-a-trace-filter-transact-sql.md) ou [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md).  
  
## <a name="blocked-process-report-event-class-data-columns"></a>Colonnes de données de la classe d'événements Blocked Process Report  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|ID de la base de données dans laquelle le verrou a été obtenu. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**Durée**|**bigint**|Durée (en microsecondes) du blocage du processus.|13|Oui|  
|**EndTime**|**datetime**|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme **SQL:BatchStarting** ou **SP:Starting**.|15|Oui|  
|**EventClass**|**int**|Type d’événement = 137.|27|Non|  
|**EventSequence**|**int**|Séquence d'un événement donné au sein de la demande.|51|Non|  
|**IndexID**|**int**|ID de l'index de l'objet affecté par l'événement. Pour déterminer l’ID d’index d’un objet, utilisez la colonne **indid** de la table système **sysindexes** .|24|Oui|  
|**IsSystem**|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur. 1 = système, 0 = utilisateur.|60|Oui|  
|**LoginSid**|**image**|Identificateur de sécurité (SID) de l'utilisateur connecté. Cet événement est toujours signalé à partir du thread système. IsSystem = 1 ; SID = sa.|41|Oui|  
|**Mode**|**int**|État reçu ou demandé par l'événement<br /><br /> 0=NULL<br /><br /> 1=Sch-S<br /><br /> 2=Sch-M<br /><br /> 3=S<br /><br /> 4=U<br /><br /> 5=X<br /><br /> 6=IS<br /><br /> 7=IU<br /><br /> 8=IX<br /><br /> 9=SIU<br /><br /> 10=SIX<br /><br /> 11=UIX<br /><br /> 12=BU<br /><br /> 13=RangeS-S<br /><br /> 14=RangeS-U<br /><br /> 15=RangeI-N<br /><br /> 16=RangeI-S<br /><br /> 17=RangeI-U<br /><br /> 18=RangeI-X<br /><br /> 19=RangeX-S<br /><br /> 20=RangeX-U<br /><br /> 21=RangeX-X|32|Oui|  
|**ObjectID**|**int**|ID affecté par le système à l'objet sur lequel a été acquis le verrou (si disponible et le cas échéant).|22|Oui|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26||  
|**SessionLoginName**|**nvarchar**|Nom d'accès de l'utilisateur qui a créé la session. Par exemple, si vous vous connectez à SQL Server avec le nom Accès1 et que vous exécutez une instruction avec Accès2, **SessionLoginName** affiche Accès1 et **LoginName** affiche Accès2. Cette colonne affiche à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|64|Oui|  
|**TextData**|**ntext**|Valeur texte qui dépend de la classe d'événements capturée dans la trace.|1|Oui|  
|**TransactionID**|**bigint**|ID affecté par le système à la transaction.|4|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
