---
title: Guide de supervision des groupes de disponibilité et de résolution des problèmes liés à ces groupes
description: Ce document offre un index de contenu qui vous aide à bien démarrer avec la supervision des groupes de disponibilité Always On et à résoudre certains problèmes courants liés à ces groupes.
ms.custom: seo-lt-2019
ms.date: 05/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
ms.assetid: 8d6d9954-ff6b-4e58-882e-eff0174f0d07
author: rothja
ms.author: jroth
ms.openlocfilehash: 5594a39b6ab4f8c9c6bd2edaadbe64e99c983229
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643692"
---
# <a name="monitor-and-troubleshoot-availability-groups"></a>Superviser les groupes de disponibilité et résoudre les problèmes liés à ces groupes
 Ce guide explique comment monitorer les groupes de disponibilité Always On et comment résoudre certains problèmes courants liés à ces groupes. Il comprend du contenu original ainsi que des liens vers d’autres informations utiles. Bien que ce guide ne traite pas de tous les problèmes susceptibles de se produire dans le vaste domaine des groupes de disponibilité, il peut vous donner des renseignements utiles qui vous aideront dans l’analyse des causes racines et la résolution des problèmes. 
 
 Les groupes de disponibilité étant une technologie intégrée, de nombreux problèmes que vous rencontrez peuvent être les symptômes d’autres problèmes dans votre système de base de données. Certains problèmes sont causés par des paramètres au sein d’un groupe de disponibilité, par exemple une base de données de disponibilité suspendue. Citons aussi les problèmes ayant trait à d’autres aspects de SQL Server, notamment les paramètres de SQL Server, les déploiements de fichiers de base de données et les dégradations des performances systémiques sans rapport avec la disponibilité. D’autres problèmes peuvent se présenter en dehors de SQL Server, comme ceux affectant les E/S réseau, TCP/IP, Active Directory et WSFC (clustering de basculement Windows Server). Les problèmes qui font leur apparition dans un groupe de disponibilité, un réplica ou une base de données vous obligent souvent à dépanner plusieurs technologies pour identifier leur cause racine.  
  
  
##  <a name="troubleshooting-scenarios"></a><a name="BKMK_SCENARIOS"></a> Scénarios de résolution de problèmes  
 Le tableau suivant contient des liens vers les scénarios courants de résolution de problèmes liés aux groupes de disponibilité. Ils sont classés par type de scénario : configuration, connectivité client, basculement, niveau de performance, etc.  
  
|Scénario|Type de scénario|Description|  
|--------------|-------------------|-----------------|  
|[Résoudre les problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](troubleshoot-always-on-availability-groups-configuration-sql-server.md)|Configuration|Fournit des informations pour vous aider à résoudre les problèmes courants de configuration des instances de serveur pour les groupes de disponibilité. Voici quelques problèmes courants de configuration : les groupes de disponibilité sont désactivés, les comptes sont mal configurés, le point de terminaison de mise en miroir de bases de données n’existe pas, le point de terminaison est inaccessible (erreur SQL Server 1418), l’accès réseau n’existe pas et l’échec d’une commande de jointure de base de données (erreur SQL Server 35250).|  
|[Résoudre une opération d’ajout de fichier ayant échoué &#40;groupes de disponibilité Always On&#41;](troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)|Configuration|Une opération d’ajout de fichier a provoqué la suspension de la base de données secondaire et l’affectation à celle-ci de l’état SANS SYNCHRONISATION.|  
|[Impossible de se connecter à l’écouteur de groupe de disponibilité dans un environnement de sous-réseaux multiples](https://support.microsoft.com/kb/2792139/en-us)|Connectivité client|Après avoir configuré l’écouteur de groupe de disponibilité, vous ne pouvez ni effectuer un test ping sur l’écouteur ni vous y connecter à partir d’une application.|  
|[Résoudre les basculements automatiques ayant échoué](https://support.microsoft.com/kb/2833707)|Basculement|Un basculement automatique ne s’est pas déroulé correctement.|  
|[Dépanner : dépassement de RTO du groupe de disponibilité](troubleshoot-availability-group-exceeded-rto.md)|Performances|Après un basculement automatique ou un basculement manuel planifié sans perte de données, le temps de basculement dépasse votre RTO. Vous pouvez aussi constater que votre estimation du temps de basculement d’un réplica secondaire avec validation synchrone (par exemple, un partenaire de basculement automatique) dépasse votre RTO.|  
|[Dépanner : dépassement de RPO du groupe de disponibilité](troubleshoot-availability-group-exceeded-rpo.md)|Performances|À l’issue d’un basculement manuel forcé, la perte de données est supérieure à votre RPO. Vous pouvez aussi constater que votre calcul de la perte de données potentielle d’un réplica secondaire avec validation asynchrone dépasse votre RPO.|  
|[Dépanner : les changements sur le réplica principal ne sont pas répercutés sur le réplica secondaire](troubleshoot-primary-changes-not-reflected-on-secondary.md)|Performances|L’application cliente mène à bien une mise à jour sur le réplica principal, mais l’exécution d’une requête sur le réplica secondaire montre que le changement n’a pas été répercuté.|  
|[Troubleshooting High HADR_SYNC_COMMIT wait type with Always On Availability Groups](/archive/blogs/sql_server_team/troubleshooting-high-hadr_sync_commit-wait-type-with-always-on-availability-groups)|Performances|Un HADR_SYNC_COMMIT anormalement long indique un problème de performance dans le flux de déplacement des données ou le renforcement du journal du réplica secondaire.|  

##  <a name="useful-tools-for-troubleshooting"></a><a name="BKMK_TOOLS"></a> Outils utiles pour la résolution des problèmes  
 Durant la configuration ou l’exécution de groupes de disponibilité, plusieurs outils peuvent vous aider à diagnostiquer différents types de problèmes. Vous trouverez dans le tableau suivant des liens vers des informations utiles sur ces outils.  
  
|Outil|Description|  
|----------|-----------------|  
|[Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](use-the-always-on-dashboard-sql-server-management-studio.md)|Présente une vue d’ensemble de l’intégrité de votre groupe de disponibilité dans une interface conviviale.|  
|[Stratégies Always On](always-on-policies.md)|Utilisées par le tableau de bord Always On.|  
|[Journal des erreurs SQL Server &#40;groupes de disponibilité Always On&#41;](sql-server-error-log-always-on-availability-groups.md)|Journalise les événements de transition d’état des groupes de disponibilité, réplicas et bases de données, les états d’autres composants Always On et les erreurs Always On.|  
|[CLUSTER.LOG &#40;groupes de disponibilité Always On&#41;](cluster-log-always-on-availability-groups.md)|Journalise les événements du cluster, notamment les transitions d’état de la ressource du groupe de disponibilité, ainsi que les événements et les erreurs de la DLL de ressource SQL Server.|  
|[Journal de diagnostic d’intégrité Always On](always-on-health-diagnostics-log.md)|Journalise les diagnostics d’intégrité SQL Server tels que signalés au cluster WSFC (DLL de ressource SQL Server) par [sp_server_diagnostics &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md).|  
|[Vues de gestion dynamique et vues de catalogue système &#40;groupes de disponibilité Always On&#41;](dynamic-management-views-and-system-catalog-views-always-on-availability-groups.md)|Fournissent des informations sur les groupes de disponibilité, comme la configuration, l’état d’intégrité et les métriques de performances.|  
|[Événements étendus Always On](always-on-extended-events.md)|Fournissent des diagnostics détaillés sur groupes de disponibilité pour l’analyse des causes racines.|  
|[Types d’attentes Always On](always-on-wait-types.md)|Fournissent des statistiques d’attente spécifiques aux groupes de disponibilité pour le réglage des performances.|  
|Compteurs de performances Always On|Présentent le monitoring de l’activité des groupes de disponibilité dans le moniteur système pour le réglage des performances. Pour plus d’informations, consultez [SQL Server, réplica de disponibilité](~/relational-databases/performance-monitor/sql-server-availability-replica.md) et [SQL Server, réplica de base de données](~/relational-databases/performance-monitor/sql-server-database-replica.md).|  
|[Mémoires tampons en anneau Always On](always-on-ring-buffers.md)|Enregistrent les alertes émises dans le système SQL Server pour établir des diagnostics internes et déboguer les problèmes liés aux groupes de disponibilité.|  
  
##  <a name="monitoring-availability-groups"></a><a name="BKMK_MONITOR"></a> Monitoring des groupes de disponibilité  
 Dans l’idéal, un problème lié à un groupe de disponibilité doit être résolu avant qu’il ne nécessite un basculement (automatique ou manuel). Vous devez pour cela superviser les métriques de performances du groupe de disponibilité et envoyer des alertes quand le niveau de performance des réplicas de disponibilité se situe en dehors des limites de votre contrat de niveau de service (SLA). Par exemple, si un réplica secondaire synchrone présente des problèmes de performances qui entraînent l’augmentation du temps de basculement, n’attendez pas qu’un basculement automatique se produise pour découvrir que le temps de basculement dépasse votre RTO (objectif de temps de récupération).  
  
 Les groupes de disponibilité étant une solution de récupération d’urgence haute disponibilité, les métriques de performance les plus importantes à monitorer sont le temps de basculement estimé, qui affecte votre RTO, et le risque de perte de données en cas d’urgence, qui affecte votre RPO (objectif de point de récupération). Vous pouvez à tout moment collecter ces métriques à partir des données exposées par SQL Server et être tenu informé des problèmes liés aux fonctionnalités HADR (haute disponibilité et récupération d’urgence) de votre système avant même l’apparition d’événements d’échec réels. Il est donc important que vous vous familiarisiez avec le processus de synchronisation des données des groupes de disponibilité et que vous collectiez les métriques appropriées.  
  
 Le tableau ci-dessous pointe vers des rubriques qui vous aideront à monitorer l’intégrité de votre solution de groupes de disponibilité.  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Monitorer les performances des groupes de disponibilité Always On](monitor-performance-for-always-on-availability-groups.md)|Décrit le processus de synchronisation de données pour les groupes de disponibilité, les portes de contrôle de flux et les métriques utiles dans le cadre du monitoring d’un groupe de disponibilité. Montre également comment collecter les métriques RTO et RPO.|  
|[Supervision de groupes de disponibilité &#40;SQL Server&#41;](monitoring-of-availability-groups-sql-server.md)|Fournit des informations sur les outils permettant de monitorer un groupe de disponibilité.|  
|[The Always On health model, part 1: Health model architecture](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-1-health-model-architecture)|Fournit une vue d’ensemble du modèle d’intégrité Always On.|  
|[The Always On health model, part 2: Extending the health model](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)|Montre comment personnaliser le modèle d’intégrité Always On et personnaliser le tableau de bord Always On pour que celui-ci affiche des informations supplémentaires.|  
|[Monitoring Always On health with PowerShell, part 1: Basic cmdlet overview](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)|Fournit une vue d’ensemble des applets de commande PowerShell Always On permettant de monitorer l’intégrité d’un groupe de disponibilité.|  
|[Monitoring Always On health with PowerShell, part 2: Advanced cmdlet usage](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)|Fournit des informations sur l’utilisation avancée des applets de commande PowerShell Always On pour monitorer l’intégrité d’un groupe de disponibilité.|  
|[Monitoring Always On health with PowerShell, part 3: A simple monitoring application](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)|Montre comment monitorer automatiquement un groupe de disponibilité avec une application.|  
|[Monitoring Always On health with PowerShell, part 4: Integration with SQL Server Agent](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)|Explique comment intégrer le monitoring des groupes de disponibilité à SQL Server Agent et configurer l’envoi d’une notification aux parties concernées en cas de problème.|  

## <a name="next-steps"></a>Étapes suivantes  
 [Blog de l’équipe SQL Server Always On](/archive/blogs/sqlalwayson/)   
 [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](/archive/blogs/psssql/)  
  
