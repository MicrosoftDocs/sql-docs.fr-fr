---
title: Voir le rapport de copie des journaux de transaction (SSMS)
description: Affichez le rapport d’état de la copie des journaux de transaction dans SQL Server Management Studio. Exécutez un rapport d’état sur un serveur moniteur, un serveur principal ou un serveur secondaire.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- viewing log shipping reports
- displaying log shipping reports
- log shipping [SQL Server], monitoring
- log shipping [SQL Server], viewing reports
ms.assetid: 3b549f2f-3683-45e5-b8e8-8095276c41ab
author: cawrites
ms.author: chadam
ms.openlocfilehash: 87292d8c38322069e7bc5fd93802e7e3bffb1978
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643843"
---
# <a name="view-the-log-shipping-report-sql-server-management-studio"></a>Afficher le rapport de la copie des journaux de transaction (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment afficher le rapport d'état sur la copie des journaux de transaction dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez exécuter ce rapport d'état sur un serveur moniteur, un serveur principal ou un serveur secondaire. Pour obtenir les informations les plus complètes sur la configuration de la copie des journaux de transaction, affichez le rapport sur l'instance de serveur moniteur.  
  
 Ce rapport indique l'état de toute activité de copie des journaux de transaction dont l'état est disponible sur l'instance de serveur à laquelle vous êtes connecté. Si l'instance de serveur participe à plusieurs configurations sous différents rôles (tels que serveur moniteur pour une base de données et serveur secondaire pour une autre), les résultats affichés contiennent les informations de chaque configuration du point de vue de chaque rôle. Si la procédure stockée peut se connecter à l'instance de serveur moniteur pour une configuration de copie des journaux de transaction donnée, le rapport affiche des informations d'état supplémentaires sur cette configuration.  
  
 Pour chaque rôle joué par l'instance de serveur actuelle, vous pouvez afficher les informations suivantes :  
  
|Role|Informations affichées|  
|----------|---------------------------|  
|Superviser|Nom et état de chaque serveur principal et serveur secondaire qui utilisent cette instance de serveur comme serveur moniteur.|  
|Principal|Pour chaque base de données primaire, l'état et le nom de l'instance de serveur actuelle (comme serveur principal), ainsi que le nom de la base de données primaire. Le rapport affiche l'état du travail de sauvegarde (stocké localement sur le serveur principal).<br /><br /> Le rapport contient également une ligne pour chacun des serveurs secondaires correspondants. Si la configuration utilise un serveur moniteur et que la procédure stockée peut se connecter au moniteur, ces lignes affichent l'état de copie et l'état de restauration de la sauvegarde de fichier journal la plus récente.|  
|Secondary|Pour chaque base de données secondaire, l'état et le nom de l'instance de serveur actuelle (comme serveur secondaire), ainsi que le nom de la base de données secondaire.<br /><br /> Le rapport affiche l'état des travaux de copie et de restauration sur le serveur secondaire.<br /><br /> Le rapport contient également une ligne pour le serveur principal correspondant. Si la configuration utilise un serveur moniteur et que la procédure stockée peut se connecter au moniteur, cette ligne affiche l'état de la sauvegarde de fichier journal la plus récente.|  
  
 Les informations affichées varient selon que l'instance de serveur est un serveur moniteur, un serveur principal ou un serveur secondaire. Si des informations ne sont pas disponibles, les cellules correspondantes apparaissent estompées.  
  
 Le rapport appelle **sp_help_log_shipping_monitor** pour obtenir les données. Pour plus d’informations sur les autorisations requises, consultez [sp_help_log_shipping_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-transact-sql.md).  
  
### <a name="to-display-the-transaction-log-shipping-status-report-on-a-server-instance"></a>Pour afficher le rapport sur l'état de la copie des journaux de transaction sur une instance de serveur  
  
1.  Connectez-vous à un serveur moniteur, principal ou secondaire.  
  
2.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur l’instance de serveur, pointez sur **Rapports**, puis sur **Rapports standard**.  
  
3.  Cliquez sur **État de l’envoi des journaux**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
  
