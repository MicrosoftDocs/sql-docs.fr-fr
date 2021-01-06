---
title: Configurer des alertes pour les métriques de performances de mise en miroir de bases de données
description: 'Aide pour la configuration des seuils d’avertissement et alertes pour les métriques de performances utilisées par la mise en miroir de bases de données. '
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring database mirroring [SQL Server]
- thresholds [SQL Server]
- database mirroring [SQL Server], managing in SQL Server Management Studio
- alerts [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
- warnings [database mirroring]
ms.assetid: 8cdd1515-0bd7-4f8c-a7fc-a33b575e20f6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c323c5915752e68505e7e1e512843ae03c8197e4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643486"
---
# <a name="use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server"></a>Utiliser des seuils d'avertissement et d'alertes sur des métriques de performances de mise en miroir (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique contient des informations sur les événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour lesquels des seuils d'avertissement peuvent être configurés et gérés pour la mise en miroir de bases de données. Vous pouvez utiliser le moniteur de mise en miroir de bases de données ou les procédures stockées **sp_dbmmonitorchangealert**, **sp_dbmmonitorhelpalert** et **sp_dbmmonitordropalert** . Cette rubrique contient également des informations sur la configuration d'alertes sur des événements de mise en miroir de bases de données.  
  
 Une fois l'analyse établie pour une base de données en miroir, un administrateur système peut configurer des seuils d'avertissements sur plusieurs métriques de performances clés. Il est également possible de configurer des alertes sur ces événements de mise en miroir de bases de données et sur d'autres événements.  
  
  
##  <a name="performance-metrics-and-warning-thresholds"></a><a name="PerfMetricsAndWarningThresholds"></a> Métriques de performances et seuils d'avertissement  
 Le tableau suivant répertorie les métriques de performances pour lesquelles des avertissements peuvent être configurés, décrit les seuils d'avertissement correspondants et répertorie le libellé Moniteur de mise en miroir de bases de données correspondant.  
  
|Mesure de performance|Seuil d'avertissement|Libellé Moniteur de mise en miroir de bases de données|  
|------------------------|-----------------------|--------------------------------------|  
|Journal non envoyé|Spécifie la quantité de kilo-octets (Ko) de journal non envoyé qui génère un avertissement sur l'instance de serveur principal. Cet avertissement aide à mesurer le risque de perte de données en termes de Ko et concerne tout particulièrement le mode hautes performances. Toutefois, l'avertissement est également approprié en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue en raison de la déconnexion des partenaires.|**Avertir si le journal non envoyé dépasse le seuil**|  
|Journal non restauré|Spécifie la quantité de Ko de journal non restauré qui génère un avertissement sur l'instance de serveur miroir. Cet avertissement permet de mesurer le temps de basculement. Le *temps de basculement* est principalement constitué du temps nécessaire à l'ancien serveur miroir pour restaurer par progression tout journal demeuré dans sa file d'attente de restauration par progression et d'un court laps de temps supplémentaire.<br /><br /> Remarque : pour un basculement automatique, le temps nécessaire au système pour remarquer l’erreur ne dépend pas du temps de basculement.<br /><br /> Pour en savoir plus, voir [Estimer l’interruption de service au cours d’un basculement de rôle &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).|**Avertir si le journal non restauré dépasse le seuil**|  
|Transaction non envoyée la plus ancienne|Spécifie le nombre de minutes de transactions pouvant s'accumuler dans la file d'attente d'envoi avant qu'un avertissement ne soit généré sur l'instance de serveur principal. Cet avertissement aide à mesurer le risque de perte de données en termes de temps et concerne tout particulièrement le mode hautes performances. Toutefois, l'avertissement est également approprié en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue en raison de la déconnexion des partenaires.|**Avertir si la durée de vie de la plus ancienne transaction non envoyée dépasse le seuil**|  
|Charge de validation par le serveur miroir|Spécifie le nombre de millisecondes de délai moyen par transaction qui sont tolérés avant qu'un avertissement soit généré sur le serveur principal. Ce délai correspond au temps de traitement pendant lequel l'instance de serveur principal attend que l'instance de serveur miroir écrive l'enregistrement du journal de transaction dans la file d'attente de restauration par progression. Cette valeur est utile uniquement en mode haute sécurité.|**Avertir si le temps de traitement de validation de miroir dépasse le seuil**|  
  
 Pour chacune de ces métriques de performances, un administrateur système peut spécifier un seuil sur une base de données en miroir. Pour plus d'informations, consultez [Définition et gestion de seuils d'avertissement](#SetUpManageWarningThresholds), plus loin dans cette rubrique.  
  
##  <a name="setting-up-and-managing-warning-thresholds"></a><a name="SetUpManageWarningThresholds"></a> Définition et gestion de seuils d'avertissement  
 Un administrateur système peut configurer un ou plusieurs seuils d'avertissement pour les métriques de performances de mise en miroir clés. Nous recommandons de définir un seuil pour un avertissement donné sur les deux partenaires afin de s'assurer que l'avertissement persiste en cas de basculement de la base de données. Le seuil approprié sur chaque partenaire dépend des capacités de performances du système du partenaire.  
  
 Les seuils d'avertissement peuvent être configurés et gérés à l'aide des méthodes suivantes :  
  
-   Moniteur de mise en miroir de bases de données  
  
     Dans le moniteur de mise en miroir de bases de données, l'administrateur peur afficher simultanément la configuration actuelle des avertissements pour une base de données sélectionnée sur les instances du principal et du serveur miroir en sélectionnant la page à onglets **Avertissements** . À partir de là, l'administrateur peut ouvrir la boîte de dialogue **Définir les seuils d'avertissement** pour activer et configurer un ou plusieurs seuils d'avertissement.  
  
     Pour obtenir une présentation de l'interface du moniteur de mise en miroir de bases de données, consultez [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md). Pour plus d’informations sur le lancement du moniteur de mise en miroir de bases de données, consultez [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md).  
  
-   Procédures stockées système  
  
     L'ensemble suivant de procédures stockées système permet à un administrateur de configurer et de gérer des seuils d'avertissement sur des bases de données en miroir d'un partenaire à la fois.  
  
    |Procédure|Description|  
    |---------------|-----------------|  
    |[sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)|Ajoute ou modifie un seuil d'avertissement pour une métrique de performance de mise en miroir spécifiée.|  
    |[sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)|Retourne des informations sur des seuils d'avertissement sur une ou l'ensemble des métriques de performances clés du moniteur de mise en miroir de bases de données.|  
    |[sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)|Supprime l'avertissement pour une métrique de performance spécifiée.|  
  
## <a name="performance-threshold-events-sent-to-the-windows-event-log"></a>Événements de seuil de performance envoyés au journal des événements Windows  
 Si un seuil d’avertissement est défini pour une métrique de performance, la valeur la plus récente est comparée au seuil quand la table d’états est mise à jour. Si le seuil est atteint, la procédure de mise à jour, **sp_dbmmonitorupdate**, génère un événement d’informations (un *événement de seuil de performance*) pour la métrique et elle écrit l’événement dans le journal des événements [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Le tableau suivant répertorie les ID des événements de seuil de performance.  
  
|Mesure de performance|ID de l’événement|  
|------------------------|--------------|  
|Journal non envoyé|32042|  
|Journal non restauré|32043|  
|Transaction non envoyée la plus ancienne|32040|  
|Charge de validation par le serveur miroir|32044|  
  
> [!NOTE]  
>  Un administrateur peut définir des alertes sur un ou plusieurs de ces événements. Pour plus d'informations, consultez [Utilisation d'alertes pour une base de données en miroir](#UseAlerts), plus loin dans cette  
>   
>  rubrique.  
  
##  <a name="using-alerts-for-a-mirrored-database"></a><a name="UseAlerts"></a> Utilisation d'alertes pour une base de données en miroir  
 L'un des aspects importants de la surveillance d'une base de données mise en miroir consiste à configurer des alertes sur des événements de mise en miroir de bases de données importants. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère les types suivants d'événements de mise en miroir de bases de données :  
  
-   Événements de seuil de performance  
  
     Pour plus d'informations, consultez « Événements de seuil de performance envoyés au journal des événements Windows » plus haut dans cette rubrique.  
  
-   Événements de changement d'état  
  
     Il s'agit des événements WMI (Windows Management Instrumentation) générés lorsque des modifications sont apportées à l'état interne d'une session de mise en miroir de bases de données.  
  
    > [!NOTE]  
    >  Pour plus d’informations, consultez [Fournisseur WMI pour les concepts des événements de serveur](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md).  
  
 Un administrateur système peut configurer des alertes sur ces événements à l'aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ou d'autres applications, telles que [!INCLUDE[msCoName](../../includes/msconame-md.md)] Operations Manager.  
  
 Lorsque vous définissez des alertes sur des événements de mise en miroir de bases de données, nous vous recommandons de définir des seuils d'avertissement et des alertes sur les deux instances de serveur partenaires. Les différents événements sont générés sur le serveur principal ou sur le serveur miroir, mais chaque partenaire peut assumer l'un ou l'autre rôle à tout moment. Pour vous assurer qu'une alerte continue de fonctionner après un basculement, vous devez la définir sur les deux partenaires.  
  
> [!IMPORTANT]  
>  Pour toutes les sessions de mise en miroir, nous vous recommandons vivement de configurer la base de données de façon à envoyer une alerte sur tout événement de changement d'état. À moins qu'un changement d'état ne soit attendu suite à une modification de configuration manuelle, un événement risquant de compromettre vos données s'est produit. Pour aider à protéger vos données, identifiez et corrigez la cause d'un changement d'état inattendu.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour créer une alerte à l'aide de SQL Server Management Studio**  
  
-   [Créer une alerte utilisant un numéro d'erreur](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [Créer une alerte d'événement WMI](../../ssms/agent/create-a-wmi-event-alert.md)  
  
 **Pour surveiller une mise en miroir de bases de données**  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)  
  
-   [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)  
  
-   [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
-   [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)  
  
-   [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
-   [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
