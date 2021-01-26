---
title: Reporting Services avec les groupes de disponibilité
description: Découvrez la configuration de Reporting Services pour utiliser des groupes de disponibilité Always On dans SQL Server. Les fonctionnalités prises en charge diffèrent selon les scénarios.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: edeb5c75-fb13-467e-873a-ab3aad88ab72
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: cf6d3f8e77591e9791fe2e8dc57175f1393716ff
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783260"
---
# <a name="reporting-services-with-always-on-availability-groups-sql-server"></a>Reporting Services avec les groupes de disponibilité Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique contient des informations sur la configuration de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] en vue d'une utilisation avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] (groupes de disponibilité) dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Les trois possibilités d'utilisation de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] et de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sont les bases de données pour les sources de données de rapport, les bases de données de serveur de rapports et la conception de rapports. Les fonctionnalités prises en charge et la configuration requise diffèrent dans les trois cas.  
  
 L'un des principaux avantages d'utiliser [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] avec des sources de données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] réside dans l'exploitation de réplicas secondaires accessibles en lecture en tant que source de données de rapports tandis que, dans le même temps, les réplicas secondaires permettent le basculement vers une base de données principale.  
  
 Pour obtenir des informations générales sur [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez le [Forum aux questions sur Always On pour SQL Server 2012 (../../../sql-server/index.yml)](../../../sql-server/index.yml).  

##  <a name="requirements-for-using-reporting-services-and-always-on-availability-groups"></a><a name="bkmk_requirements"></a> Conditions préalables requises pour l’utilisation de Reporting Services et des groupes de disponibilité Always On  
 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] et Power BI Report Server utilisent le composant .Net Framework 4.0 et prennent en charge les propriétés de chaîne de connexion [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pour les utiliser avec des sources de données.  
  
 Pour utiliser [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] avec  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2014 et les versions antérieures, vous devez télécharger et installer un correctif logiciel pour .Net 3.5 SP1. Ce correctif ajoute une prise en charge au client SQL concernant les fonctionnalités de groupes de disponibilité et la prise en charge des propriétés de chaîne de connexion **ApplicationIntent** et **MultiSubnetFailover**. Si ce correctif n'est pas installé sur chaque ordinateur qui héberge un serveur de rapports, les utilisateurs qui essaient d'afficher un aperçu des rapports recevront un message d'erreur similaire à celui ci-dessous et ce message sera enregistré dans le fichier journal de traces du serveur de rapports :  
  
> **Message d’erreur :** « mot clé non pris en charge : applicationintent. »  
  
 Ce message s'affiche lorsque vous incluez l'une des propriétés [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans la chaîne de connexion [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , mais que le serveur ne reconnaît pas cette propriété. Le message d’erreur indiqué s’affiche lorsque vous cliquez sur le bouton « Tester la connexion » dans les interfaces utilisateur [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] et lorsque vous affichez un aperçu du rapport si des erreurs distantes sont activées sur les serveurs de rapports.  
  
 Pour plus d’informations sur le correctif requis, consultez l’[article 2654347A de la base de connaissance : Le correctif introduit la prise en charge des fonctionnalités Always On de SQL Server 2012 vers .NET Framework 3.5 SP1](https://go.microsoft.com/fwlink/?LinkId=242896).  
  
 Pour des informations sur les autres conditions préalables requises pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
> [!NOTE]  
>  Les fichiers de configuration de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], comme **RSreportserver.config**, ne sont pas pris en charge dans le cadre de la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Si vous apportez manuellement des modifications à un fichier de configuration sur l'un des serveurs de rapports, vous devez mettre à jour manuellement les réplicas.  
  
##  <a name="report-data-sources-and-availability-groups"></a><a name="bkmk_reportdatasources"></a> Sources de données de rapport et groupes de disponibilité  
 Le comportement de sources de données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] basées sur [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] peut varier selon la manière dont votre administrateur a configuré l'environnement de groupes de disponibilité.  
  
 Pour utiliser [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pour les sources de données de rapport que vous devez configurer, la chaîne de connexion de la source de données de rapport doit faire référence au *Nom DNS de l'écouteur* du groupe de disponibilité. Les sources de données prises en charge sont les suivantes :  
  
-   Source de données ODBC utilisant SQL Native Client.  
  
-   Client SQL, avec le correctif .Net appliqué au serveur de rapports.  
  
 La chaîne de connexion peut également contenir de nouvelles propriétés de connexion Always On qui configurent les demandes de requêtes de rapport afin qu’elles utilisent un réplica secondaire pour la création de rapports en lecture seule. L'utilisation d'un réplica secondaire pour les demandes de création de rapports réduit la charge sur un réplica principal en lecture-écriture. L'illustration suivante est un exemple d'une configuration de groupe de disponibilité à trois réplicas dans laquelle les chaînes de connexion de source de données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] ont été configurées avec ApplicationIntent=ReadOnly. Dans cet exemple, les demandes de requêtes de rapports sont envoyées à un réplica secondaire, et non au réplica principal.  
  
 
  
 Voici un exemple de chaîne de connexion, dans laquelle [AvailabilityGroupListenerName] correspond au **Nom DNS de l’écouteur** qui a été configuré au moment de la création des réplicas :  
  
 `Data Source=[AvailabilityGroupListenerName];Initial Catalog = AdventureWorks2016; ApplicationIntent=ReadOnly`  
  
 Le bouton **Tester la connexion** dans les interfaces utilisateur [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] permet de valider s'il est possible d'établir une connexion, mais pas de valider une configuration de groupes de disponibilité. Par exemple, si vous incluez ApplicationIntent dans une chaîne de connexion à un serveur qui ne fait pas partie d'un groupe de disponibilité, le paramètre supplémentaire est ignoré et le bouton **Tester la connexion** ne permet de valider que s'il est possible d'établir une connexion au serveur spécifié.  
  
 La manière dont vos rapports ont été créés et publiés détermine l'endroit où vous allez modifier la chaîne de connexion :  
  
-   **Mode natif :** utilisez le [!INCLUDE[ssRSWebPortal-Non-Markdown](../../../includes/ssrswebportal-non-markdown-md.md)] pour les sources de données partagées et les rapports qui sont déjà publiés sur un serveur de rapports en mode natif.  
  
-   **Mode SharePoint :** Utilisez les pages de configuration de SharePoint dans les bibliothèques de documents pour les rapports qui sont déjà publiés sur un serveur SharePoint.  
  
-   **Conception de rapports :** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] ou [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] lors de la création de rapports. Pour plus d’informations, consultez la section « Conception de rapports », plus loin dans cette rubrique.  
  
 **Ressources supplémentaires :**  
  
-   [Gérer des sources de données de rapports](../../../reporting-services/report-data/manage-report-data-sources.md)  
  
-   Pour plus d'informations sur les propriétés de chaîne de connexion disponibles, consultez [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
-   Pour plus d’informations sur les écouteurs de groupe de disponibilité, consultez [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Considérations :** Les réplicas secondaires subissent généralement un retard lors de la réception de modifications de données à partir du réplica principal. Les facteurs suivants peuvent avoir une influence sur la latence de mise à jour entre les réplicas principal et secondaires :  
  
-   Nombre de réplicas secondaires. Le retard s'accentue avec chaque réplica secondaire ajouté à la configuration.  
  
-   Emplacement géographique et distance entre les réplicas principal et secondaires. Par exemple, le retard est généralement plus important si les réplicas secondaires se trouvent dans un centre de données différent, par comparaison à ceux situés dans le même bâtiment que le réplica principal.  
  
-   Configuration du mode de disponibilité pour chaque réplica. Le mode de disponibilité détermine si le réplica principal attend, pour valider des transactions sur une base de données, qu'un réplica secondaire donné ait écrit la transaction sur le disque. Pour plus d’informations, consultez la section « Modes de disponibilité » de [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 Lors de l'utilisation d'un réplica secondaire en lecture seule en tant que source de données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , il est important de veiller à ce que la latence de mise à jour des données corresponde aux besoins des utilisateurs de rapports.  
  
##  <a name="report-design-and-availability-groups"></a><a name="bkmk_reportdesign"></a> Conception de rapports et groupes de disponibilité  
 Lors de la conception de rapports dans [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] ou d'un projet de rapport dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], un utilisateur peut configurer une chaîne de connexion de source de données de rapport afin qu'elle contienne les nouvelles propriétés de connexion fournies par [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. La prise en charge des nouvelles propriétés de connexion varie selon l'endroit où un utilisateur affiche un aperçu du rapport.  
  
-   **Aperçu local :** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] et [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] utilisent .Net Framework 4.0 et prennent en charge les propriétés de chaîne de connexion [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
-   **Aperçu en mode distant ou serveur :** Si, après avoir publié des rapports sur le serveur de rapports ou utilisé l'aperçu dans [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)], vous voyez une erreur similaire à celle qui suit, cela signifie que vous affichez un aperçu des rapports sur le serveur de rapports et que le correctif .NET Framework 3.5 SP1 pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] n'a pas été installé sur le serveur de rapports.  
  
> **Message d’erreur :** « mot clé non pris en charge : applicationintent. »  
  
##  <a name="report-server-databases-and-availability-groups"></a><a name="bkmk_reportserverdatabases"></a> Bases de données de serveur de rapports et groupes de disponibilité  
 Reporting Services et Power BI Report Server offrent une prise en charge limitée concernant l'utilisation de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] avec des bases de données de serveur de rapports. Les bases de données de serveur de rapports peuvent être configurées dans les groupes de disponibilité afin de les intégrer à un réplica ; toutefois, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] n'utilisera pas automatiquement un réplica différent pour les bases de données de serveur de rapports en cas de basculement. L’utilisation de MultiSubnetFailover avec les bases de données du serveur de rapports n’est pas prise en charge.  
  
 Il est nécessaire de recourir à des actions manuelles ou à des scripts d'automatisation personnalisés pour mener à bien le basculement et la récupération. Tant que ces actions n'ont pas été effectuées, certaines fonctionnalités du serveur de rapports peuvent ne pas fonctionner correctement après le basculement [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
> [!NOTE]  
>  Lors de la planification d'un basculement et d'une récupération d'urgence pour les bases de données de serveur de rapports, il est recommandé de toujours sauvegarder une copie de la clé de chiffrement du serveur de rapports.  
  
###  <a name="differences-between-sharepoint-native-mode"></a><a name="bkmk_differences_in_server_mode"></a> Différences entre le mode natif et le mode SharePoint  
 Cette section résume les différences qu'il existe entre la manière dont les serveurs de rapports en mode SharePoint et en mode natif interagissent avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 Un serveur de rapports SharePoint crée **3** bases de données pour chaque application de service [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] que vous créez. La connexion aux bases de données de serveur de rapports en mode SharePoint est configurée dans l'Administration centrale de SharePoint lorsque vous créez l'application de service. Les noms par défaut des bases de données incluent le GUID associé à l'application de service. Voici des exemples de noms de bases de données pour un serveur de rapports en mode SharePoint :  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6TempDB  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6_Alerting  
  
 Les serveurs de rapports en mode natif utilisent **2** bases de données. Voici des exemples de noms de bases de données pour un serveur de rapports en mode natif :  
  
-   ReportServer  
  
-   ReportServerTempDB  
  
 Le mode natif ne prend pas en charge et n'utilise pas les bases de données d'alerte et les fonctionnalités qui y sont associées. Pour configurer les serveurs de rapports en mode natif, faites appel au Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] . En mode SharePoint, configurez le nom de la base de données de l’application de service pour qu’il corresponde au nom du « point d’accès client » que vous avez créé dans le cadre de la configuration de SharePoint. Pour plus d’informations sur la configuration de SharePoint avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Configurer et gérer des groupes de disponibilité SQL Server pour SharePoint Server (/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14))](/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14)).  
  
> [!NOTE]
>  Les serveurs de rapports en mode SharePoint utilisent un processus de synchronisation entre les bases de données d'application de service [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] et les bases de données de contenu SharePoint. Il est important de conserver les bases de données de serveur de rapports et les bases de données de contenu ensemble. Pensez à les configurer dans les mêmes groupes de disponibilité afin qu'elles soient basculées et récupérées en tant qu'ensemble. Examinez le cas suivant :  
> 
>  -   Vous effectuez la restauration ou le basculement vers une copie des la base de données de contenu qui n'a pas reçu les mêmes mises à jour récentes que celles reçues par la base de données de serveur de rapports.  
> -   Le processus de synchronisation de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] détecte les différences entre la liste des éléments dans la base de données de contenu et celle des bases de données de serveur de rapports.  
> -   Le processus de synchronisation supprime ou met à jour les éléments dans la base de données de contenu.  
  
###  <a name="prepare-report-server-databases-for-availability-groups"></a><a name="bkmk_prepare_databases"></a> Préparer les bases de données de serveur de rapports pour les groupes de disponibilité  
 La section suivante décrit les étapes de base permettant la préparation et l'ajout de bases de données de serveur de rapports à [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
-   Créez votre groupe de disponibilité et configurez un *nom DNS de l'écouteur*.  
  
-   **Réplica principal :** configurez les bases de données de serveur de rapports afin qu'elles fassent partie d'un groupe de disponibilité unique et créez un réplica principal incluant toutes les bases de données de serveur de rapports.  
  
-   **Réplicas secondaires :** Créez un ou plusieurs réplicas secondaires. La méthode la plus courante pour copier les bases de données à partir du réplica principal vers le ou les réplicas secondaires consiste à restaurer les bases de données sur chaque réplica secondaire à l’aide de « RESTORE WITH NORECOVERY ». Pour plus d’informations sur la création de réplicas secondaires et la vérification du bon fonctionnement de la synchronisation des données, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
-   **Informations d’identification de serveur de rapports :** Vous devez créer les informations d'identification appropriées de serveur de rapports sur les réplicas secondaires que vous avez créés sur le réplica principal. La procédure exacte varie selon le type d'authentification que vous utilisez dans votre environnement [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] ; compte de service Windows [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], compte d'utilisateur Windows ou authentification SQL Server. Pour plus d’informations, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
-   Mettez à jour la connexion à la base de données pour utiliser le nom DNS de l'écouteur. Pour les serveurs de rapports en mode natif, remplacez **Nom de la base de données du serveur de rapports** dans le gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] . En mode SharePoint, remplacez le **nom du serveur de base de données** pour les applications de service [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
###  <a name="steps-to-complete-disaster-recovery-of-report-server-databases"></a><a name="bkmk_steps_to_complete_failover"></a> Étapes pour effectuer une récupération d'urgence de bases de données de serveur de rapports  
 La procédure suivante doit être effectuée après un basculement de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] vers un réplica secondaire :  
  
1.  Arrêtez l'instance du service SQL Agent qui était utilisée par le moteur de la base de données principale hébergeant les bases de données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
2.  Démarrez le service SQL Agent sur l'ordinateur qui correspond au nouveau réplica principal.  
  
3.  Arrêtez le service Report Server.  
  
     Si le serveur de rapports est en mode natif, arrêtez le serveur Windows du serveur de rapports à l'aide du gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
     Si le serveur de rapports est configuré pour le mode SharePoint, arrêtez le service partagé [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] dans l'Administration centrale de SharePoint.  
  
4.  Démarrez le service du serveur de rapports ou le service [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] SharePoint.  
  
5.  Vérifiez que les rapports peuvent s'exécuter sur le nouveau réplica principal.  
  
###  <a name="report-server-behavior-when-a-failover-occurs"></a><a name="bkmk_failover_behavior"></a> Comportement d'un serveur de rapports en cas de basculement  
 Lors du basculement de bases de données de serveur de rapports, et si vous avez mis à jour l'environnement du serveur de rapports afin qu'il utilise le nouveau réplica principal, des problèmes opérationnels résultant du processus de basculement et de récupération peuvent survenir. L'impact de ces problèmes varie selon la charge de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] au moment du basculement, ainsi que de la durée nécessaire à [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pour effectuer le basculement vers un réplica secondaire et celle nécessaire à l'administrateur du serveur de rapports pour mettre à jour l'environnement de création de rapports afin qu'il utilise le nouveau réplica principal.  
  
-   L'exécution du traitement en arrière-plan peut avoir lieu plusieurs fois en raison d'une logique de nouvelle tentative et d'une incapacité du serveur de rapports à marquer le travail planifié comme terminé pendant le basculement.  
  
-   L'exécution du traitement en arrière-plan qui aurait normalement été déclenchée pendant le basculement n'a pas lieu car SQL Server Agent n'est pas en mesure d'écrire des données dans la base de données de serveur de rapports et que ces données ne sont pas synchronisées avec le nouveau réplica principal.  
  
-   Une fois que le basculement de la base de données est terminé et que le service du serveur de rapports a été redémarré, les travaux de SQL Server Agent sont recréés automatiquement. Tant que les travaux de SQL Server Agent n'ont pas été recréés, les exécutions en arrière-plan associées aux travaux de SQL Server Agent ne sont pas traitées. Cela inclut les abonnements [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , les planifications et les instantanés.  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en charge des fonctionnalités de récupération d'urgence, haute disponibilité par SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Bien démarrer avec les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)   
 [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)   
 [Prise en charge des fonctionnalités de récupération d'urgence, haute disponibilité par SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
