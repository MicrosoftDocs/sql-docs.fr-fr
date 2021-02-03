---
description: Catégorie d'événements Performance
title: Performance, catégorie d’événement | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL Server event classes, Performance event category
- Performance event category [SQL Server]
- event classes [SQL Server], Performance event category
ms.assetid: 708f3585-d8be-4980-bbff-672d7c59397e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 10e44ce46f927495abadeb03a40607e9d5e679fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165438"
---
# <a name="performance-event-category"></a>Catégorie d'événements Performance
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Utilisez les classes d’événements **Performance** pour surveiller les classes d’événements **Showplan** et celles produites par l’exécution des opérateurs du langage de manipulation des données SQL (DML, Data Manipulation Language).  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Auto Stats, classe d’événements](../../relational-databases/event-classes/auto-stats-event-class.md)|Indique qu'une mise à jour automatique des statistiques d'index et de colonne a eu lieu.|  
|[Classe d’événements Degree of Parallelism &#40;7.0 Insert&#41;](../../relational-databases/event-classes/degree-of-parallelism-7-0-insert-event-class.md)|Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a exécuté une instruction SELECT, INSERT, UPDATE ou DELETE en utilisant un plan série ou parallèle. Le nombre d'unités centrales utilisées pour effectuer l'opération est également indiqué.|  
|[Performance Statistics, classe d’événements](../../relational-databases/event-classes/performance-statistics-event-class.md)|Contrôle les performances des requêtes exécutées.|  
|[Classe d'événements Showplan All](../../relational-databases/event-classes/showplan-all-event-class.md)|Identifie des opérateurs **Showplan** dans une instruction SQL.|  
|[Classe d'événements Showplan All for Query Compile](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md)|Affiche des données temporelles de compilation pour des opérateurs **Showplan** .|  
|[Classe d'événements Showplan Statistics Profile](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md)|Affiche le coût estimé d'une requête.|  
|[Showplan XML, classe d’événements](../../relational-databases/event-classes/showplan-xml-event-class.md)|Identifie les opérateurs **Showplan** dans une instruction SQL. La classe d'événements stocke chaque événement sous la forme d'un document XML bien défini.|  
|[Classe d'événements Showplan XML for Query Compile](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md)|Affiche des données temporelles de compilation pour des opérateurs **Showplan** au format XML.|  
|[Classe d'événements Showplan XML Statistics Profile](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md)|Identifie les opérateurs **Showplan** associés à une instruction SQL. La sortie est un document XML.|  
|[SQL:FullTextQuery, classe d’événements](../../relational-databases/event-classes/sql-fulltextquery-event-class.md)|Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a exécuté une requête de texte intégral.|  
|[Plan Guide Successful, classe d'événements](../../relational-databases/event-classes/plan-guide-successful-event-class.md)|Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a pu produire un plan d'exécution pour une requête ou un lot qui contenait un repère de plan.|  
|[Plan Guide Unsuccessful (classe d'événements)](../../relational-databases/event-classes/plan-guide-unsuccessful-event-class.md)|Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas pu produire un plan d'exécution pour une requête ou un lot qui contenait un repère de plan.|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  
