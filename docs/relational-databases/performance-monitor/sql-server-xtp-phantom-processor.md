---
title: Processeur fantôme XTP de SQL Server | Microsoft Docs
description: Découvrez l’objet de performance Processeur fantôme XTP de SQL Server, qui contient des compteurs pour le sous-système de traitement fantôme du moteur OLTP en mémoire.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 0f691b3d-a8fd-4459-ad21-2cfc8574a8c0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c0547f8f93b548e7dcf51271e7c796bad52f46ad
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505470"
---
# <a name="sql-server-xtp-phantom-processor"></a>Processeur fantôme XTP de SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  L’objet de performance Processeur fantôme XTP de SQL Server contient des compteurs liés au sous-système de traitement fantôme du moteur OLTP en mémoire. Ce composant est chargé de la détection des lignes fantômes dans les transactions exécutées dans le niveau d’isolation SERIALIZABLE, et de la validation de la contrainte dans les scénarios de concurrence  
  
 Ce tableau décrit les compteurs **Processeur fantôme XTP SQL Server** .  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Nouvelles tentatives d'analyse d'angles inutilisés par seconde (sortie fantôme)**|Nombre de tentatives d'analyse dues à des conflits d'écriture lors des nettoyages d'angles inutilisés indiqué par le processeur fantôme (en moyenne), par seconde. Il s'agit d'un compteur de très bas niveau, non destiné au client.|  
|**Lignes fantômes expirées, supprimées par seconde**|Nombre de lignes expirées supprimées par les analyses fantômes (en moyenne), par seconde.|  
|**Lignes expirées fantômes, touchées par seconde**|Nombre de lignes expirées touchées par les analyses fantômes (en moyenne), par seconde.|  
|**Lignes expirant fantômes, touchées par seconde**|Nombre de lignes expirant touchées par les analyses fantômes (en moyenne), par seconde.|  
|**Lignes fantômes touchées par seconde**|Nombre de lignes touchées par les analyses fantômes (en moyenne), par seconde.|  
|**Analyses fantômes démarrées par seconde**|Nombre d'analyses fantômes démarrées (en moyenne), par seconde.|  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server XTP &#40;OLTP en mémoire&#41;, compteurs de performances](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
