---
title: 'Mise en miroir de bases de données : Prérequis, restrictions et recommandations'
description: Découvrez les prérequis, restrictions et recommandations relatifs à la configuration de la mise en miroir de bases de données avec SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- partners [SQL Server]
- database mirroring [SQL Server], prerequisites
- database mirroring [SQL Server], recommendations
- database mirroring [SQL Server], restrictions
- database mirroring [SQL Server], planning
- database mirroring [SQL Server], about database mirroring
ms.assetid: fdcf2251-9895-44c6-b81e-768fef32e732
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 95dd833cfe2fa558bc03b2aa68b29ac5b13d3272
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343278"
---
# <a name="prerequisites-restrictions-and-recommendations-for-database-mirroring"></a>Conditions préalables, limitations et recommandations relatives à la mise en miroir de bases de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] à la place.  
  
 Cette rubrique décrit les conditions préalables et les recommandations relatives à la configuration de la mise en miroir de bases de données. Pour une présentation de la mise en miroir de bases de données, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
  
##  <a name="support-for-database-mirroring"></a><a name="DbmSupport"></a> Prise en charge de la mise en miroir de bases de données  
 Pour plus d’informations sur la prise en charge de la mise en miroir de bases de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], consultez [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).
  
 Notez que la mise en miroir de bases de données fonctionne avec tous les niveaux de compatibilité de base de données pris en charge. Pour plus d’informations sur les niveaux de compatibilité pris en charge, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Pour établir une session de mise en miroir, les serveurs partenaires et le serveur témoin, s'il existe, doivent être exécutés sur la même version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Les deux serveurs partenaires, c'est-à-dire le principal et le serveur miroir doivent exécuter la même version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le serveur témoin, le cas échéant, peut s'exécuter sur n'importe quelle édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui prend en charge la mise en miroir de bases de données.  
  
    > [!NOTE]  
    >  Vous pouvez mettre à niveau des instances de serveur qui participent à une session de mise en miroir vers une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Mise à niveau des instances en miroir](../../database-engine/database-mirroring/upgrading-mirrored-instances.md).  
  
-   La base de données doit utiliser le mode de récupération complète. Les modes de récupération simples et utilisant les journaux de transactions ne prennent pas en charge la mise en miroir de bases de données. Par conséquent, toutes les opérations en bloc sont entièrement journalisées pour une base de données mise en miroir. Pour plus d’informations sur les modes de récupération, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
-   Vérifiez que le serveur miroir possède suffisamment d'espace disque pour la base de données miroir.  
  
    > [!NOTE]  
    >  Pour plus d’informations sur l’utilisation de la mise en miroir de bases de données sur une base de données répliquée, consultez [Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
-   Lors de la création de la base de données en miroir sur le serveur miroir, assurez-vous que vous restaurez la sauvegarde de la base de données principale en spécifiant le même nom de base de données à l'aide de WITH NORECOVERY. De plus, toutes les sauvegardes du journal créées après la réalisation de cette sauvegarde doivent également être appliquées, à nouveau avec WITH NORECOVERY.  
  
    > [!IMPORTANT]  
    >  Si la mise en miroir de bases de données a été arrêtée, toutes les sauvegardes du journal réalisées ultérieurement sur la base de données principale doivent être appliquées à la base de données miroir avant de pouvoir redémarrer la mise en miroir.  
  
  
##  <a name="restrictions"></a><a name="Restrictions"></a> Restrictions  
  
-   Seules les bases de données utilisateur peuvent être mises en miroir. Vous ne pouvez pas mettre en miroir les bases de données **master**, **msdb**, **tempdb** ou **model** .  
  
-   Une base de données mise en miroir ne peut pas être renommée pendant une session de mise en miroir de bases de données.  
  
-   La mise en miroir de bases de données ne prend pas en charge FILESTREAM. Un groupe de fichiers FILESTREAM ne peut pas être créé sur le serveur principal. La mise en miroir de bases de données ne peut pas être configurée pour une base de données qui contient des groupes de fichiers FILESTREAM.  
  
-   La mise en miroir de bases de données n'est pas prise en charge par les transactions entre bases de données ou les transactions distribuées. Pour plus d’informations, consultez [Transactions entre bases de données et transactions distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
  
##  <a name="recommendations-for-configuring-partner-servers"></a><a name="RecommendationsForPartners"></a> Recommandations relatives à la configuration des serveurs partenaires  
  
-   Il est conseillé d'exécuter les partenaires sur des systèmes comparables pouvant supporter des charges de travail identiques.  
  
    > [!NOTE]  
    >  Si vous prévoyez d'utiliser le mode Haute sécurité avec basculement automatique, la charge normale sur chacun des partenaires de basculement doit utiliser moins de 50 % de l'UC. Si votre charge de travail surcharge l'UC, un partenaire de basculement peut se retrouver dans l'incapacité d'envoyer des pings aux autres instances de serveur de la session de mise en miroir, ce qui provoquera un basculement inutile. Si vous ne parvenez pas à maintenir l'utilisation de l'UC au-dessous de 50 %, nous vous recommandons d'utiliser soit le mode Haute sécurité sans basculement automatique, soit le mode Haute performance.  
  
-   Si possible, le chemin d'accès (y compris la lettre de lecteur) de la base de données miroir doit être identique au chemin d'accès de la base de données principale. Vous devez inclure l'option MOVE dans l'instruction RESTORE si les dispositions de fichiers doivent différer. Par exemple, si la base de données principale se trouve sur le lecteur « F: » mais que le système miroir n'a pas de lecteur F:.  
  
    > [!IMPORTANT]  
    >  Si vous déplacez les fichiers de base de données pendant la création de la base de données miroir, vous risquez par la suite de ne pas pouvoir ajouter de fichiers à la base de données sans suspendre la mise en miroir.  
  
-   Toutes les instances de serveur d'une session de mise en miroir doivent utiliser la même page de codes maître et le même classement. Si ce n'est pas le cas, un problème peut se produire pendant la configuration de la mise en miroir.  
  
-   Éventuellement, évaluez le temps de basculement d'une base de données pour vous assurer que la configuration du système va fournir les performances nécessaires. Pour en savoir plus, voir [Estimer l’interruption de service au cours d’un basculement de rôle &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).  
  
-   Pour des performances optimales, utilisez une carte d'interface réseau dédiée à la mise en miroir.  
  
-   Nous n'avons pas de recommandations sur la fiabilité suffisante d'un réseau WAN (Wide-Area Network) pour la mise en miroir d'une base de données en mode haute sécurité. Si vous optez pour le mode Haute sécurité à la place d'un réseau WAN, soyez prudent quant à la manière dont vous ajoutez un serveur témoin à la session, en raison du risque de basculements automatiques indésirables. Pour plus d’informations, consultez [Recommandations relatives au déploiement de la mise en miroir de bases de données](#RecommendationsForDeploying), plus loin dans cette rubrique.  
  
  
##  <a name="recommendations-for-deploying-database-mirroring"></a><a name="RecommendationsForDeploying"></a> Recommandations relatives au déploiement de la mise en miroir de bases de données  
 Le fonctionnement asynchrone permet d'optimiser les performances de la mise en miroir de bases de données. Une session de mise en miroir qui fonctionne en mode synchrone peut enregistrer une baisse de performance lorsque sa charge de travail génère de grandes quantités de données de journaux de transactions.  
  
 Dans les environnements de test, il convient d'explorer tous les modes de fonctionnement pour évaluer les performances de la mise en miroir de bases de données. Toutefois, avant de déployer la mise en miroir dans un environnement de production, assurez-vous que vous comprenez comment le réseau fonctionne en situation réelle.  
  
 Le mode Haute sécurité avec basculement automatique est conçu pour un réseau de haute capacité équipé d'une connexion dédiée ou d'une configuration réseau relativement simple qui minimise les sources possibles de défaillance du réseau. Ce type d'environnement réseau de haute qualité est nécessaire au mode haute sécurité avec basculement automatique et est recommandé pour toutes les sessions de mise en miroir de bases de données. Toutefois, les modes Haute performance et Haute sécurité sans basculement automatique sont beaucoup moins tributaires de la fiabilité du réseau.  
  
 Par conséquent, pour les environnements de production, nous vous invitons à suivre les instructions de déploiement ci-dessous :  
  
1.  Lancez l'exécution en mode Haute performance, asynchrone. Ce mode est le moins sensible à l'environnement réseau et fournit la meilleure configuration pour explorer le fonctionnement de la mise en miroir. Nous vous recommandons d'exécuter votre système en mode asynchrone avant d'être certain que votre bande passante prend en charge la mise en miroir et pour vous permettre d'approfondir vos connaissances sur la configuration de la mise en miroir et sur les performances du mode asynchrone dans votre environnement. Pour plus d'informations, voir [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
    > [!IMPORTANT]  
    >  Au cours de la phase de test, nous vous recommandons de surveiller vos sessions pour vérifier qu'elles ne contiennent pas d'erreurs réseau susceptibles de faire échouer la mise en miroir de bases de données. Pour plus d'informations sur les sources de défaillance potentielles, consultez [Possible Failures During Database Mirroring](../../database-engine/database-mirroring/possible-failures-during-database-mirroring.md). Pour plus d’informations sur la manière de surveiller la mise en miroir de bases de données, consultez [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md).  
  
2.  Une fois que vous êtes certain que le fonctionnement asynchrone correspond aux besoins de l'entreprise, vous pouvez éventuellement essayer le fonctionnement synchrone pour améliorer la protection de vos données. Si vous décidez de tester la mise en miroir synchrone dans votre environnement, nous vous recommandons de commencer par le mode Haute sécurité sans basculement automatique. Le but principal de cette opération est d'évaluer la manière dont le fonctionnement synchrone affecte les performances des bases de données. Pour plus d'informations, voir [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
3.  N'activez pas le basculement automatique avant de vous être assuré que le mode Haute sécurité sans basculement automatique répond aux besoins de l'entreprise et que les erreurs réseau ne provoquent pas de défaillances. Pour plus d’informations, consultez [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).  
  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Résoudre des problèmes de configuration de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
