---
title: SQL Server, HTTP_STORAGE_OBJECT | Microsoft Docs
description: Découvrez l’objet de performance SQLServer:HTTP Storage, constitué de compteurs de performances qui supervisent un compte de stockage Azure.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: ae849f79-c581-42a5-a5cc-0a9ebea171b9
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: a18cb0e74a537a62eb662fdb30ba289e255c0348
ms.sourcegitcommit: 9470c4d1fc8d2d9d08525c4f811282999d765e6e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2020
ms.locfileid: "86457416"
---
# <a name="sql-server-http-storage"></a>SQL Server, stockage HTTP
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L’objet de performance **SQLServer:HTTP Storage** est constitué de compteurs de performances qui surveillent le compte de stockage Microsoft Azure. La fonctionnalité [Fichiers de données SQL Server de Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md) permet d’enregistrer les fichiers de base de données dans les objets blob du Stockage Azure. Cet objet de performance traite chaque compte de stockage Azure en tant que lecteur différent.  
  
|Nom de compteur|Description|  
|------------------|-----------------|  
|**Durée octets/lecture**|Nombre moyen d'octets transférés du stockage HTTP par lecture.|  
|**Durée octets/transfert**|Nombre moyen d'octets transférés du stockage HTTP pendant des opérations de lecture ou d'écriture.|  
|**Durée octets/écriture**|Nombre moyen d'octets transférés du stockage HTTP par écriture.|  
|**Nbre moyen microsecondes/lecture**|Nombre moyen de microsecondes nécessaires pour chaque lecture à partir du stockage HTTP.|  
|**Nbre moyen microsecondes/lecture achevée**|Nombre moyen de microsecondes nécessaires au protocole HTTP pour achever la lecture du stockage.| 
|**Nbre moyen micros./transfert**|Nombre moyen de microsecondes nécessaires pour chaque transfert vers le stockage HTTP.|  
|**Nbre moyen microsecondes/écriture**|Nombre moyen de microsecondes nécessaires pour chaque écriture sur le stockage HTTP.|  
|**Nbre moyen microsecondes/écriture achevée**|Nombre moyen de microsecondes nécessaires au protocole HTTP pour achever l’écriture dans le stockage.|  
|**Échec/s des ES de stockage HTTP**|Nombre de requêtes d’écriture échouées envoyées au stockage HTTP par seconde.| 
|**Nouvelle tentative d'E/S au stockage HTTP/s**|Nombre de demandes de nouvelle tentative envoyées au stockage HTTP par seconde.|  
|**E/S en attente vers le stockage HTTP**|Nombre total d'E/S en attente vers un stockage HTTP.|  
|**Octets lus/s**|Quantité de données transférées du stockage HTTP par seconde pendant des opérations de lecture.|  
|**Lectures/s**|Nombre de lectures par seconde sur le stockage HTTP.|  
|**Octets totaux/s**|Quantité de données transférées du stockage HTTP par seconde pendant des opérations de lecture et d'écriture.|  
|**Transferts/s**|Nombre d'opérations de lecture et d'écriture par seconde sur le stockage HTTP.|  
|**Octets écrits/s**|Quantité de données transférées du stockage HTTP par seconde pendant des opérations d'écriture.|  
|**logiques/s**|Nombre d'écritures par seconde sur le stockage HTTP.|  
  
## <a name="see-also"></a>Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
