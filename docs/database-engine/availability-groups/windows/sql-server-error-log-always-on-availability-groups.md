---
title: Journal des erreurs SQL Server (groupes de disponibilité)
description: Apprenez-en davantage sur les événements du journal des erreurs SQL Server qui affectent les groupes de disponibilité Always On et les symptômes qui doivent conduire à l’examen du journal des erreurs.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
ms.assetid: 39d0c98d-75af-4dd1-b908-30d31af56f2a
author: rothja
ms.author: jroth
ms.openlocfilehash: e88eed3b4b49b15f65ad888725374d953b87806b
ms.sourcegitcommit: 827ad02375793090fa8fee63cc372d130f11393f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89480359"
---
# <a name="sql-server-error-log-always-on-availability-groups"></a>Journal des erreurs SQL Server (groupes de disponibilité Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Le journal des erreurs SQL Server signale les événements qui affectent les groupes de disponibilité Always On, notamment :  
  
-   Communication avec le cluster WSFC (clustering de basculement Windows Server)    
-   Transitions d’état des réplicas de disponibilité    
-   Transitions d’état des bases de données de disponibilité    
-   État de connectivité des bases de données de disponibilité entre les réplicas principal et secondaire    
-   États des points de terminaison de groupe de disponibilité    
-   États des écouteurs de groupe de disponibilité    
-   État du bail entre la DLL de ressource SQL Server (en cours d’exécution dans le cluster WSFC) et l’instance SQL Server (pour plus d’informations, consultez [How It Works: SQL Server Always On lease timeout](https://docs.microsoft.com/archive/blogs/psssql/how-it-works-sql-server-alwayson-lease-timeout) (Fonctionnement : délai d’expiration de bail Always On SQL Server))    
-   Événements d’erreur dans le groupe de disponibilité  

Les symptômes suivants doivent vous inciter à passer en revue le journal des erreurs SQL Server :  

-   Impossibilité d’accéder aux bases de données de disponibilité    
-   Basculement inattendu du groupe de disponibilité    
-   État Résolution affecté de manière inattendue au groupe de disponibilité    
-   Groupe de disponibilité dans un état indéterminé  
  
Pour plus d’informations, consultez [Afficher le journal des erreurs SQL Server &#40;SQL Server Management Studio&#41;](~/relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md).  
  
  
