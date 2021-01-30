---
title: MSpeer_conflictdetectionconfigrequest (T-SQL)
description: Décrit la procédure stockée MSPeer_conflictdetectionconfigurerequest utilisée pour suivre les demandes de configuration à l’intérieur de la topologie pour une publication d’égal à égal.
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpeer_conflictdetectionconfigrequest_TSQL
- MSpeer_conflictdetectionconfigrequest
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_conflictdetectionconfigurerequest
ms.assetid: 83afa0ca-707e-4468-a888-228268ed4e10
author: cawrites
ms.author: chadam
ms.openlocfilehash: b31582285145cf4d9f3062b8dac3dd05675d6b9c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205891"
---
# <a name="mspeer_conflictdetectionconfigrequest-transact-sql"></a>MSpeer_conflictdetectionconfigrequest (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de suivre les requêtes de configuration à l'échelle d'une topologie pour une publication dans le cadre d'une réplication d'égal à égal. Cette table est stockée dans la base de données de publication.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|id|**int**|Identifie une demande de configuration en conflit. La colonne request_id dans [MSpeer_conflictdetectionconfigresponse](../../relational-databases/system-tables/mspeer-conflictdetectionconfigresponse-transact-sql.md) utilise cette valeur.|  
|publication|**sysname**|Nom de la publication d'où provient la demande de configuration de conflit.|  
|sent_date|**datetime**|Date et heure d'émission de la demande de configuration de conflit.|  
|délai d'expiration|**int**|Durée d'attente d'une procédure avant le retour des informations de conflit par les homologues.|  
|modified_date|**datetime**|Date et heure d'achèvement d'une phase.|  
|progress_phase|**nvarchar(32)**|Identifie la phase actuelle du traitement en utilisant l'une des valeurs suivantes :<br /><br /> Démarré<br /><br /> Navigation de la topologie<br /><br /> Collecte de l'état<br /><br /> État collecté|  
|phase_timed_out|**bit**|Indique si la phase actuelle a connu un dépassement du délai d'attente.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
