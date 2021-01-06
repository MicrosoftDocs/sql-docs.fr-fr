---
title: Basculer vers une base de données secondaire de copie des journaux de transaction
description: Découvrez comment basculer un serveur secondaire d’envoi de journaux SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- primary databases [SQL Server]
- secondary data files [SQL Server], manual fail over
- log shipping [SQL Server], failover
- failover [SQL Server], log shipping
ms.assetid: edfe5d59-4287-49c1-96c9-dd56212027bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 19302cafdd7619107684d39fb7e84f0e5540a30d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643923"
---
# <a name="fail-over-to-a-log-shipping-secondary-sql-server"></a>Basculer vers une base de données secondaire de copie des journaux de transaction (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Le basculement vers une base de données secondaire de copie des journaux de transactions est utile en cas d'échec ou d'un besoin de maintenance de l'instance du serveur principal.  
  
## <a name="preparing-for-a-controlled-failover"></a>Préparation en vue d'un basculement contrôlé  
 En règle générale, les bases de données primaire et secondaire ne sont pas synchronisées, car la base de données primaire est en permanence mise à jour après son dernier travail de sauvegarde. De même, dans certains cas, les sauvegardes récentes du journal des transactions n'ont pas été copiées dans les instances du serveur secondaire ou certaines copies des sauvegardes du journal n'ont peut-être pas encore été appliquées dans la base de données secondaire. Si possible, nous vous recommandons de commencer par synchroniser l'ensemble des bases de données secondaires avec la base de données primaire.  
  
 Pour plus d’informations sur l’opération de copie des journaux de transaction, consultez [À propos de la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
## <a name="failing-over"></a>Basculement  
 Pour basculer vers une base de données secondaire :  
  
1.  Copiez tous les fichiers de sauvegarde non copiés du partage de sauvegarde vers le dossier de destination de copie de chaque serveur secondaire.  
  
2.  Appliquez à la suite toutes les sauvegardes non appliquées du journal des transactions à chaque base de données secondaire. Pour plus d’informations, consultez [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
3.  Si la base de données primaire est accessible, sauvegardez le journal des transactions actif et appliquez la sauvegarde du journal aux bases de données secondaires. Vous devrez peut-être définir la base de données sur le [mode mono-utilisateur](../../relational-databases/databases/set-a-database-to-single-user-mode.md) pour obtenir un accès exclusif avant d’exécuter la commande restore, puis repasser au mode multi-utilisateur une fois la restauration terminée.  
  
     Si l'instance du serveur principal d'origine n'est pas endommagée, sauvegardez la fin du journal des transactions de la base de données primaire à l'aide de l'option WITH NORECOVERY. Cette opération laisse la base de données en état de restauration et donc inaccessible aux utilisateurs. Par la suite, vous pouvez la restaurer par progression en appliquant les sauvegardes du journal des transactions à partir de la base de données primaire de remplacement.  
  
     Pour plus d’informations, consultez [Sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md).   
  
4.  Une fois que les serveurs secondaires sont synchronisés, vous pouvez basculer vers celui de votre choix en récupérant sa base de données secondaire et en redirigeant des clients vers cette instance de serveur. La récupération place la base de données dans un état cohérent et permet sa mise en ligne.  
  
    > [!NOTE]  
    >  Lorsque vous rendez une base de données secondaire disponible, vous devez vous assurer que ses métadonnées sont cohérentes avec celles de la base de données primaire d'origine. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
5.  Une fois que vous avez récupéré une base de données secondaire, vous pouvez la reconfigurer en tant que base de données primaire pour d'autres bases de données secondaires.  
  
     Si aucune autre base de données secondaire n’est disponible, consultez [Configurer la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Changer des rôles entre les serveurs primaire et secondaire de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md)  
  
-   [Gestion des connexions et des travaux après un basculement de rôle &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et procédures stockées liées à la copie des journaux de transaction](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)   
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)  
  
  
