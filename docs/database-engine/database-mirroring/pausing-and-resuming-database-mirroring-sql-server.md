---
title: Suspendre et reprendre une mise en miroir de bases de données
description: Découvrez comment suspendre et reprendre ultérieurement une session de mise en miroir de bases de données SQL Server pour conserver l’état de la session tout en suspendant la mise en miroir.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- resuming database mirroring
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: c67802c6-ee8c-4cbd-a6d4-f7b80413a4ab
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4163add4c04b919d0cc99da93a440383879bdb08
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644276"
---
# <a name="pausing-and-resuming-database-mirroring-sql-server"></a>Suspendre et reprendre la mise en miroir de bases de données (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Le propriétaire de la base de données peut suspendre et reprendre ultérieurement une session de mise en miroir de bases de données à tout moment. La suspension conserve l'état de la session tout en suspendant la mise en miroir. En cas de goulots, la suspension peut permettre d'améliorer les performances sur le serveur principal.  
  
 Lorsqu'une session est suspendue, la base de données principale reste disponible. La suspension d'une session de mise en miroir fait passer son état à SUSPENDED, et la base de données miroir ne reflète plus la base de données principale, ce qui rend cette dernière vulnérable lors de son exécution.  
  
 Nous vous recommandons de reprendre une session suspendue rapidement puisque le journal des transactions ne peut pas être tronqué tant que la session de mise en miroir de bases de données est suspendue. Par conséquent, si la session de mise en miroir de bases de données est suspendue trop longtemps, le journal peut être plein, ce qui rend la base de données indisponible. Pour une explication des causes de ce phénomène, consultez « Comment la suspension et la reprise affectent la troncature du journal » plus loin dans cette rubrique.  
  
> [!IMPORTANT]  
>  Après un service forcé, lorsque le serveur principal d'origine se reconnecte, la mise en miroir est suspendue. La reprise de la mise en miroir dans cette situation peut entraîner des pertes de données sur le serveur principal d'origine. Pour plus d'informations sur la gestion des problèmes éventuels de perte de données, consultez [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
 **Dans cette rubrique :**  
  
-   [Comment la suspension et la reprise affectent la troncature du journal](#EffectOnLogTrunc)  
  
-   [Éviter la saturation du journal des transactions](#AvoidFullLog)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="how-pausing-and-resuming-affect-log-truncation"></a><a name="EffectOnLogTrunc"></a> Comment la suspension et la reprise affectent la troncature du journal  
 En général, lorsqu'un point de contrôle automatique est effectué sur une base de données, son journal des transactions est tronqué jusqu'à ce point de contrôle après la sauvegarde du journal suivante. Durant la suspension de la session de la mise en miroir de bases de données, tous les enregistrements du journal en cours restent actifs puisque le serveur principal attend de les envoyer au serveur miroir. Les enregistrements de journal qui n'ont pas été envoyés s'accumulent dans le journal des transactions de la base de données principale en attendant que la session reprenne et que le serveur principal envoie les enregistrements du journal vers le serveur miroir.  
  
 Lors de la reprise de la session, le serveur principal commence immédiatement l'envoi des enregistrements accumulés du journal vers le serveur miroir. Après confirmation par le serveur miroir de la mise en file d'attente de l'enregistrement de journal correspondant jusqu'au point de contrôle automatique le plus ancien, le serveur principal tronque le journal de la base de données principale jusqu'à ce point de contrôle. Le serveur miroir tronque la file d'attente de restauration au même enregistrement de journal. Ce processus est répété pour chaque point de contrôle successif et le journal est tronqué par étape, point de contrôle par point de contrôle.  
  
> [!NOTE]  
>  Pour plus d'informations sur les points de contrôle et la troncature du journal, consultez [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
##  <a name="avoid-a-full-transaction-log"></a><a name="AvoidFullLog"></a> Éviter la saturation du journal des transactions  
 Si le journal est plein (soit parce qu'il a atteint sa taille maximum, soit parce que l'instance du serveur manque de place), la base de données ne peut plus effectuer de mises à jour. Pour éviter ce problème, vous avez deux solutions :  
  
-   Reprendre la session de mise en miroir de base de données avant que le journal ne soit plein, ou ajouter un espace journal supplémentaire. La reprise de la mise en miroir de bases de données permet au serveur principal d'envoyer son journal actif cumulé au serveur miroir et met la base de données miroir en état SYNCHRONIZING. Le serveur miroir peut alors renforcer le journal sur le disque et commencer à le refaire.  
  
-   Arrêter la session de mise en miroir de bases de données en supprimant la mise en miroir.  
  
     Contrairement à la suspension d'une session, la suppression d'une mise en miroir supprime toutes les informations sur la session de mise en miroir. Chaque instance de serveur partenaire conserve sa propre copie de la base de données. Si l'ancienne copie miroir est récupérée, elle divergera de l'ancienne copie principale et accusera un retard par rapport à la durée qui s'est écoulée depuis la suspension de la session. Pour plus d’informations, consultez [Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour suspendre ou reprendre la mise en miroir de bases de données**  
  
-   [Suspendre ou reprendre une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
 **Pour arrêter la mise en miroir de bases de données**  
  
-   [Supprimer une mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
  
  
