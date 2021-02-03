---
description: PreConnect:Completed, classe d’événements
title: PreConnect:Completed, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- PreConnect:Completed Event Class
ms.assetid: 7ed2f620-6511-4985-9961-d2927c2b1759
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b432f36c72b2118c6fd6a04e7f56a9a1d40ff24b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180842"
---
# <a name="preconnectcompleted-event-class"></a>PreConnect:Completed, classe d’événements
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe d’événements PreConnect:Completed indique quand un déclencheur LOGON ou la fonction classifieur de Resource Governor finit de s’exécuter.  
  
## <a name="preconnectcompleted-event-class-data-columns"></a>Colonnes de données de la classe d'événements PreConnect:Completed  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|EventClass|**int**|216|27|Non|  
|SPID|**int**|ID du processus serveur qui déclenche cet événement.|12|Oui|  
|EventSubClass|**int**|1 pour la fonction classifieur définie par l'utilisateur.|21|Oui|  
|StartTime|**datetime**|Heure de démarrage de la fonction classifieur définie par l'utilisateur.|14|Oui|  
|EndTime|**datetime**|Heure de démarrage de la fonction classifieur définie par l'utilisateur.|15|Oui|  
|Duration|**bigint**|Temps en microsecondes utilisé par la fonction classifieur.|13|Oui|  
|ObjectID|**int**|ID de l'objet classifieur défini par l'utilisateur.|22|Oui|  
|UC|**int**|Utilisation de l'UC en millisecondes.|18|Oui|  
|Lectures|**int**|Nombre de lectures logiques.|16|Oui|  
|Écritures|**int**|Nombre d'écritures logiques.|17|Oui|  
|GroupID|**int**|ID du groupe de charges de travail classifié.|66|Oui|  
|Error|**int**|Dernier numéro d'erreur si la fonction classifieur définie par l'utilisateur ne peut pas s'exécuter.|31|Oui|  
|State|**int**|État de la dernière erreur.|30|Oui|  
|TargetUserName|**sysname**|Valeur de retour (nom de groupe de charges de travail) pour la fonction classifieur définie par l'utilisateur si le système ne peut pas trouver un groupe actif correspondant. Dans le cas contraire, cette colonne a la valeur NULL.|39|Oui|  
|ObjectName|**nvarchar (256)**|Nom en deux parties de la fonction classifieur définie par l'utilisateur. Par exemple, dbo.classifier.|34|Oui|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)   
 [PreConnect:Starting, classe d'événements](../../relational-databases/event-classes/preconnect-starting-event-class.md)   
 [gouverneur de ressources](../../relational-databases/resource-governor/resource-governor.md)  
  
  
