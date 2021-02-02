---
description: Classe d'événements CPU Threshold Exceeded
title: CPU Threshold Exceeded, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- CPU Threshold Exceeded Event Class
ms.assetid: eb106f7d-baa3-4a2b-96b2-f9fe0844057d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 35672e1d5a47fd3ed5a253e0c5b7e9a91043bed8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99110644"
---
# <a name="cpu-threshold-exceeded-event-class"></a>Classe d'événements CPU Threshold Exceeded
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d'événements CPU Threshold Exceeded indique que Resource Governor détecte une requête qui dépasse le seuil de l'UC spécifié pour REQUEST_MAX_CPU_TIME_SEC.  
  
> [!NOTE]  
>  L'intervalle de détection pour cet événement est cinq secondes. Il est garanti qu'un événement sera généré si une requête dépasse la limite spécifiée d'au moins cinq secondes. Toutefois, si une requête dépasse le seuil spécifié de moins de cinq secondes, sa détection peut être manquée selon le moment d'exécution de la requête et l'heure de la dernière analyse de détection.  
  
## <a name="cpu-threshold-exceeded-data-columns"></a>Colonnes de données CPU Threshold Exceeded  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|UC|**int**|Utilisation de l'UC en millisecondes.|18|Oui|  
|EventClass|**int**|214|27|Non|  
|EventSubClass|**int**|Violation de la limite de l'UC.|21|Oui|  
|GroupID|**int**|ID de groupe où la violation s'est produite.|66|Oui|  
|OwnerID|**int**|SPID du processus qui a provoqué la violation.|58|Oui|  
|SPID|**int**|ID du processus serveur qui déclenche cet événement.<br /><br /> Remarque : il peut être différent du SPID de l’utilisateur réel si un thread système valide l’utilisation de l’UC comme une tâche en arrière-plan.|12|Oui|  
|StartTime|**datetime**|Heure de déclenchement de cet événement.|14|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
