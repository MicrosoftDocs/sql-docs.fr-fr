---
title: Curseurs XTP SQL Server | Microsoft Docs
description: Découvrez l’objet de performance Curseurs XTP SQL Server, qui contient des compteurs liés aux curseurs internes du moteur OLTP en mémoire.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 84bf4654-3ef7-4d7f-a269-c8bb4ed4acad
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 4b140d5efbe33b2c274b50eb34c6ee601a347e09
ms.sourcegitcommit: 9470c4d1fc8d2d9d08525c4f811282999d765e6e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2020
ms.locfileid: "86458094"
---
# <a name="sql-server-xtp-cursors"></a>Curseurs XTP SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  L’objet de performance Curseurs XTP SQL Server contient des compteurs liés aux curseurs internes du moteur OLTP en mémoire. Les curseurs constituent les blocs de construction de bas niveau que le moteur OLTP en mémoire utilise pour traiter les requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] . Par conséquent, vous ne pouvez pas les contrôler directement.  
  
 Ce tableau décrit les compteurs **Curseurs XTP SQL Server** .  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Suppressions des curseurs par seconde**|Nombre de suppressions des curseurs (en moyenne), par seconde.|  
|**Insertions des curseurs par seconde**|Nombre d'insertions des curseurs (en moyenne), par seconde.|  
|**Analyses des curseurs démarrées par seconde**|Nombre d'analyses des curseurs démarrées (en moyenne), par seconde.|  
|**Violations de contrainte unique des curseurs par seconde**|Nombre de violations de contrainte unique (en moyenne), par seconde.|  
|**Mises à jour des curseurs par seconde**|Nombre de mises à jour des curseurs (en moyenne), par seconde.|  
|**Conflits d'écriture des curseurs par seconde**|Nombre de conflits d'écriture-écriture vers la même version de ligne (en moyenne), par seconde.|  
|**Nouvelles tentatives d'analyse d'angles inutilisés par seconde (sortie utilisateur)**|Nombre de tentatives d'analyse dues à des conflits d'écriture lors des nettoyages d'angles inutilisés indiqué par une analyse de table complète de l'utilisateur (en moyenne), par seconde. Il s'agit d'un compteur de très bas niveau, non destiné au client.|  
|**Lignes expirées supprimées par seconde**|Nombre de lignes expirées supprimées par les curseurs (en moyenne), par seconde.|  
|**Lignes expirées touchées par seconde**|Nombre de lignes expirées touchées par les curseurs (en moyenne), par seconde.|  
|**Lignes retournées par seconde**|Nombre de lignes retournées par les curseurs (en moyenne), par seconde.|  
|**Lignes touchées par seconde**|Nombre de lignes touchées par les curseurs (en moyenne), par seconde.|  
|**Lignes en cours de suppression, touchées par seconde**|Nombre de lignes expirant touchées par les curseurs (en moyenne), par seconde. Une ligne expire si la transaction qui l’a supprimée est toujours active (c’est-à-dire qu’elle n’a pas encore été validée ou annulée.)|  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server XTP &#40;OLTP en mémoire&#41;, compteurs de performances](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
