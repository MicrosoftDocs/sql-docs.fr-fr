---
description: MSSQLSERVER_17884
title: MSSQLSERVER_17884 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17884 (Database Engine error)
ms.assetid: 8d05ba05-3f71-4dc3-bd81-2ea5ac9fe843
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5e81871694060414d0f3fdf24fde05b01d0b13d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196601"
---
# <a name="mssqlserver_17884"></a>MSSQLSERVER_17884
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|17884|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SRV_SCHEDULER_DEADLOCK|  
|Texte du message|Les nouvelles requêtes à traiter sur le nœud %d n'ont pas été sélectionnées par un thread de travail au cours des %d dernières secondes. Les requêtes qui provoquent des blocages ou dont l'exécution est longue peuvent causer cette situation ainsi qu'une dégradation du temps de réponse du client. Utilisez l'option de configuration "max worker threads" pour augmenter le nombre de threads autorisés, ou optimisez les requêtes en cours d'exécution.  Utilisation du processus SQL : %d%%. Système inactif : %d%%.|  
  
## <a name="explanation"></a>Explication  
Il n'y a aucun signe de progression dans chacun des planificateurs, ce qui pourrait être causé par des blocages où aucun thread ne peut avancer et/ou aucun nouveau travail ne peut être sélectionné et traité. Si l'utilisation du processus est faible, d'autres processus sur l'ordinateur entraînent peut-être une insuffisance du processus de serveur au niveau du microprocesseur.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Déterminez la cause du blocage et l'absence de progression, et résolvez la situation en conséquence. Si l'utilisation du processus est faible, vérifiez la charge sur le système générée par d'autres processus.  
  
