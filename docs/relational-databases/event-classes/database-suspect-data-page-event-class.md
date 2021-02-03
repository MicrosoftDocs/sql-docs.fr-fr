---
description: classe d'événements Database Suspect Data Page
title: Database Suspect Data Page, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event notifications [SQL Server], database mirroring
- suspect_pages system table
- database mirroring [SQL Server], event notifications
- Database Suspect Data Page event class
ms.assetid: 098e1443-a8a0-425c-9311-0a479b1370ed
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3e64d423cb85a7615cd9eb582a6803e60fa04a31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210909"
---
# <a name="database-suspect-data-page-event-class"></a>classe d'événements Database Suspect Data Page
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d’événements **Database Suspect Data Page** indique qu’une page a été ajoutée à la table [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) dans la base de données [msdb](../../relational-databases/databases/msdb-database.md). Incluez cette classe d'événements dans les traces qui surveillent l'occurrence de pages suspectes.  
  
> [!NOTE]  
>  Cet événement est émis de façon asynchrone à partir de l’insertion d’une ligne correspondante dans la table **suspect_pages** . Par conséquent, un travail à l’écoute de cet événement peut ne pas trouver immédiatement l’entrée **suspect_pages** correspondante.  
  
 Lorsque la classe d'événements **Database Suspect Data Page** est incluse au sein d’une trace, la surcharge relative est peu élevée. La surcharge peut être plus élevée si le nombre de pages suspectes augmente, par exemple, si un lecteur de disque rencontre des problèmes.  
  
## <a name="database-suspect-data-page-event-class-data-columns"></a>Colonnes de données de la classe d'événements Database Suspect Data Page  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|ID de la base de données pour laquelle l'événement de page suspecte a été déclenché. Il correspond à la valeur de la colonne **database_id** de la table **suspect_pages** .|3|Oui|  
|**EventClass**|**int**|Le type de l'événement est 213.|27|Non|  
|**EventSequence**|**int**|Séquence de la classe d'événements dans le lot.|51|Non|  
|**SPID**|**int**|ID de la tâche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a rencontré la page suspecte.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle l'événement s'est produit.|14|Oui|  
|**ObjectID**|**int**|ID du fichier de base de données qui contient la page suspecte. Il correspond à la valeur de la colonne **file_id** de la table **suspect_pages** .|22|Oui|  
|**ObjectID2**|**int**|ID de la page suspecte dans le fichier. Il correspond à la valeur de la colonne **page_id** de la table **suspect_pages** .|56|Oui|  
|**Error**|**int**|Type de l’erreur rencontrée. Il s’agit de la même valeur que celle de l’élément **event_type** de la page dans la table **suspect_pages** .|31|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  
