---
title: Surveillance de la mise en miroir de bases de données (SQL Server) | Microsoft Docs
description: Découvrez le Moniteur de mise en miroir de bases de données, les procédures stockées système et le fonctionnement de la surveillance de la mise en miroir de bases de données, y compris le travail Moniteur de mise en miroir de bases de données.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
ms.assetid: a7b1b9b0-7c19-4acc-9de3-3a7c5e70694d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 9b77b54ba48dc2c3820d055227411f61983b1a7c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644283"
---
# <a name="monitoring-database-mirroring-sql-server"></a>Surveillance de la mise en miroir de bases de données (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette section présente le moniteur de mise en miroir de bases de données et les procédures stockées système **sp_dbmmonitor** ; par ailleurs, elle décrit le fonctionnement de la surveillance de la mise en miroir de bases de données (ainsi que du **travail du moniteur de mise en miroir de bases de données)** et récapitule les informations que vous pouvez surveiller en matière de sessions de mise en miroir de bases de données. De plus, cette section explique comment définir des seuils d'avertissement pour un jeu d'événements de mise en miroir de bases de données prédéfinis et comment configurer des alertes pour des événements de mise en miroir de bases de données.  
  
 Vous pouvez surveiller une base de données mise en miroir durant une session de mise en miroir pour vérifier si le flux des données est correct. Pour configurer et gérer la surveillance d’une ou de plusieurs bases de données mises en miroir sur une instance de serveur, vous pouvez utiliser le moniteur de mise en miroir de bases de données ou les procédures stockées système **sp_dbmmonitor** .  
  
 Le travail de surveillance de la mise en miroir de bases de données, **Travail du moniteur de mise en miroir de bases de données**, fonctionne en arrière-plan, indépendamment du moniteur de mise en miroir de bases de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelle le **Travail du moniteur de mise en miroir de bases de données** à intervalles réguliers, une fois par minute, et le travail appelle une procédure stockée pour mettre à jour l'état de mise en miroir. Si vous utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour démarrer une session de mise en miroir, **Travail du moniteur de mise en miroir de bases de données** est automatiquement créé. Toutefois, si vous utilisez uniquement ALTER DATABASE *<nom_base_de_données>* SET PARTNER pour commencer la mise en miroir, vous devez créer le travail en exécutant une procédure stockée.  
  
 **Dans cette rubrique :**  
  
-   [Surveillance de l'état de la mise en miroir](#MonitoringStatus)  
  
-   [Sources d'informations supplémentaires concernant une base de données mise en miroir](#AdditionalSources)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="monitoring-mirroring-status"></a><a name="MonitoringStatus"></a> Surveillance de l'état de la mise en miroir  
 Pour configurer et gérer le contrôle d'une ou de plusieurs bases de données mises en miroir sur une instance de serveur, vous pouvez utiliser le moniteur de mise en miroir de bases de données ou les procédures stockées système **dbmmonitor** . Vous pouvez surveiller une base de données mise en miroir durant une session de mise en miroir pour vérifier si le flux des données est correct.  
  
 Plus précisément, la surveillance d'une base de données mise en miroir vous permet d'effectuer les opérations suivantes :  
  
-   Vérifier que la mise en miroir fonctionne.  
  
     L'état de base permet de savoir si les deux instances de serveur sont opérationnelles, de vérifier que les serveurs sont connectés et que le journal est déplacé du principal vers le serveur miroir.  
  
-   Déterminer si la base de données miroir reste au niveau de la base de données principale.  
  
     En mode hautes performances, un serveur principal peut accumuler un retard de traitement d'enregistrements de journal non envoyés qui devront quand même être envoyés du serveur principal au serveur miroir. Qui plus est, dans n'importe quel mode d'opération, le serveur miroir peut accumuler un retard de traitement des enregistrements de journal non restaurés qui ont été écrits dans le fichier journal mais qui doivent quand même être restaurés sur la base de données miroir.  
  
-   Déterminer la quantité de données perdues lorsque l'instance du serveur principal n'est plus disponible en mode hautes performances.  
  
     Vous pouvez déterminer la perte de données en analysant la proportion d'enregistrements de journal non envoyée (le cas échéant) et le délai de la validation des transactions perdues sur le principal.  
  
-   Comparer les performances actuelles aux performances passées.  
  
     Lorsque des problèmes surviennent, un administrateur de base de données peut consulter l'historique des performances de la mise en miroir pour mieux comprendre l'état actuel. L'examen de l'historique permet à l'utilisateur de détecter des tendances au niveau des performances, d'identifier les causes récurrentes des problèmes de performances (comme les heures de la journée où le réseau est lent ou lorsqu'une importante quantité de commandes est entrée dans le journal).  
  
-   Trouver l'origine du problème de flux de données limité entre les partenaires de la mise en miroir puis résoudre ce problème.  
  
-   Définir des seuils d'avertissement pour les mesures de performances clés.  
  
     Si une nouvelle ligne d'état contient une valeur qui dépasse un seuil, un événement d'informations est envoyé au journal des événements Windows. Un administrateur système peut alors configurer manuellement des alertes reposant sur ces événements. Pour plus d’informations, consultez [Utiliser des seuils d’avertissement et d’alertes sur des métriques de performances de mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
###  <a name="tools-for-monitoring-database-mirroring-status"></a><a name="tools_for_monitoring_dbm_status"></a> Outils pour la surveillance de l'état de la mise en miroir de base de données  
 L’état de la mise en miroir peut être surveillé à l’aide du moniteur de mise en miroir de bases de données ou de la procédure stockée système **sp_dbmmonitorresults** . Ces outils permettent de surveiller la mise en miroir sur toute base de données mise en miroir sur l’instance de serveur locale par les administrateurs système (c’est-à-dire, les membres du rôle serveur fixe **sysadmin** ) et par l’utilisateur qui a été ajouté au rôle de base de données fixe **dbm_monitor** dans la base de données **msdb** par un administrateur système. Lorsqu'un administrateur système utilise ces outils, il peut également actualiser manuellement l'état de la mise en miroir.  
  
> [!NOTE]  
>  Les administrateurs système peuvent également configurer et afficher des seuils d'avertissement pour les mesures de performances clés. Pour plus d’informations, consultez [Utiliser des seuils d’avertissement et d’alertes sur des métriques de performances de mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
-   Moniteur de mise en miroir de bases de données  
  
     Le moniteur de mise en miroir de bases de données est un outil d'interface utilisateur graphique qui permet aux administrateurs système d'afficher ou de mettre à jour l'état et de configurer des seuils d'avertissement pour plusieurs mesures de performances clés. Le moniteur de mise en miroir de bases de données peut également être utilisé par les membres du rôle de base de données fixe **dbm_monitor** pour afficher la ligne la plus récente de la table d’état de la mise en miroir, bien que ces membres ne puissent pas mettre à jour la table d’état.  
  
     Le moniteur affiche l'état, notamment les mesures de performance, pour une base de données sélectionnée dans la page à onglet **État** . Le contenu de cette page provient des instances du principal et du serveur miroir. La page est remplie de manière asynchrone au fur et à mesure que l'état est recueilli par le biais de connexions distinctes aux instances du principal et du serveur miroir. Le moniteur tente de mettre à jour la table d'état toutes les 30 secondes. La mise à jour est effectuée uniquement si la table n'a pas été mise à jour dans les 15 secondes et si l'utilisateur est membre du rôle de serveur fixe **sysadmin** . Pour un résumé des informations indiquées dans la page **État** , consultez la section « [Mesures de performances affichées par le moniteur de mise en miroir de bases de données](#perf_metrics_of_dbm_monitor)» plus loin dans cette rubrique.  
  
     Pour obtenir une présentation de l'interface du moniteur de mise en miroir de bases de données, consultez [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md). Pour plus d’informations sur le lancement du moniteur de mise en miroir de bases de données, consultez [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md).  
  
-   Procédures stockées système  
  
     Vous pouvez aussi récupérer ou mettre à jour l’état actuel en exécutant la procédure stockée système **sp_dbmmonitorresults** . D'autres procédures stockées dbmmonitor vous permettent de configurer la mise en miroir, de modifier les paramètres de la mise en miroir, d'afficher la période de mise à jour actuelle et de supprimer la surveillance sur l'instance du serveur.  
  
     Le tableau suivant introduit les procédures stockées permettant de gérer et d'utiliser la surveillance de la mise en miroir de bases de données indépendamment du Moniteur de mise en miroir de bases de données.  
  
    |Procédure|Description|  
    |---------------|-----------------|  
    |[sp_dbmmonitoraddmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)|Crée un travail qui met régulièrement à jour les informations d'état de chaque base de données mise en miroir sur l'instance du serveur.|  
    |[sp_dbmmonitorchangemonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)|Modifie la valeur d'un paramètre de surveillance de mise en miroir de base de données.|  
    |[sp_dbmmonitorhelpmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)|Retourne la période de mise à jour actuelle.|  
    |[sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)|Retourne les lignes d'état d'une base de données mise en miroir et vous permet de déterminer si la procédure obtient l'état le plus récent à l'avance.|  
    |[sp_dbmmonitordropmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)|Arrête et supprime le travail de surveillance de la mise en miroir pour toutes les bases de données sur l'instance du serveur.|  
  
     La procédure stockée système **dbmmonitor** peut être utilisée en complément du moniteur de mise en miroir de bases de données. Par exemple, même si la surveillance a été configurée à l’aide de **sp_dbmmonitoraddmonitoring**, le moniteur de mise en miroir de bases de données peut être utilisé pour consulter l’état.  
  
### <a name="how-monitoring-works"></a>Fonctionnement de la surveillance  
 Cette section présente la table d'état d'une mise en miroir de base de données, le travail de surveillance de la mise en miroir de bases de données et le moniteur. Elle indique également comment les utilisateurs peuvent surveiller l'état de la mise en miroir de bases de données et comment le travail de surveillance peut être arrêté.  
  
#### <a name="database-mirroring-status-table"></a>Table d'état d'une mise en miroir de base de données  
 L'état de la mise en miroir d'une base de données est stocké dans une table d'état de mise en miroir de base de données interne, non documentée dans la base de données **msdb** . Cette table d'état est automatiquement créée la première fois que l'état de la mise en miroir est mis à jour sur l'instance du serveur.  
  
 La table d'état peut être mise à jour automatiquement ou manuellement par un administrateur système, lequel doit respecter un intervalle de 15 secondes au minimum entre les mises à jour. Cette valeur minimale de 15 secondes permet d'éviter que les instances du serveur ne soient surchargées de demandes d'état.  
  
 La table d'état est mise à jour automatiquement à la fois par le moniteur de mise en miroir de base de données et par le travail de surveillance de la mise en miroir de bases de données, si ce dernier est en cours d'exécution. Par défaut, le **travail du moniteur de mise en miroir de bases de données** met à jour la table une fois par minute (un administrateur système peut spécifier une fréquence de mise à jour comprise entre 1 et 120 minutes). Quant au moniteur de mise en miroir de bases de données, il met à jour la table automatiquement toutes les 30 secondes. Pour ces mises à jour, le **travail du moniteur de mise en miroir de bases de données** et le moniteur de mise en miroir de bases de données appellent **sp_dbmmonitorupdate**.  
  
 À sa première exécution, **sp_dbmmonitorupdate** crée la table **d’état de la mise en miroir de bases de données** et le rôle de base de données fixe **dbm_monitor** dans la base de données **msdb** . **sp_dbmmonitorupdate** met généralement à jour l’état de la mise en miroir en insérant une nouvelle ligne dans la table d’état pour chaque base de données mise en miroir sur l’instance du serveur ; pour plus d’informations, consultez la section « Table d’état d’une mise en miroir de base de données », plus loin dans cette rubrique. Cette procédure évalue également les mesures de performances dans les nouvelles lignes, et elle tronque les lignes antérieures à la période de rétention actuelle (la valeur par défaut est de 7 jours). Pour plus d’informations, consultez [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md).  
  
> [!NOTE]  
>  Sauf si le moniteur de mise en miroir de bases de données est utilisé par un membre du rôle serveur fixe **sysadmin** , la table d’état est automatiquement mise à jour si le **travail du moniteur de mise en miroir de bases de données** existe et que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est en cours d’exécution.  
  
#### <a name="database-mirroring-monitor-job"></a>Travail du moniteur de mise en miroir de bases de données  
 Le travail de surveillance de la mise en miroir de bases de données, **Travail du moniteur de mise en miroir de bases de données**, fonctionne indépendamment du moniteur de mise en miroir de bases de données. Le **Travail de surveillance de la mise en miroir de bases de données** est automatiquement créé uniquement si [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est utilisé pour démarrer une session de mise en miroir. Si les commandes ALTER DATABASE *nom_base_de_données* SET PARTNER sont systématiquement utilisées pour démarrer la mise en miroir, le travail existe uniquement si l’administrateur système exécute la procédure stockée **sp_dbmmonitoraddmonitoring** .  
  
 Une fois que le **Travail de surveillance de la mise en miroir de bases de données** a été créé, en partant du principe que l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution, le travail est appelé une fois par minute par défaut. Le travail appelle ensuite la procédure stockée système **sp_dbmmonitorupdate** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent appelle le **travail du moniteur de mise en miroir de bases de données** une fois par minute par défaut et le travail appelle **sp_dbmmonitorupdate** pour mettre à jour la table d’état. Les administrateurs système peuvent modifier la période de mise à jour à l’aide de la procédure stockée système **sp_dbmmonitorchangemonitoring** et peuvent afficher la période de mise à jour actuelle à l’aide de la procédure stockée système **sp_dbmmonitorchangemonitoring** . Pour plus d’informations, consultez [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md) et [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md).  
  
#### <a name="monitoring-database-mirroring-status-by-system-administrators"></a>État de la mise en miroir de base de données (par les administrateurs système)  
 Les membres du rôle serveur fixe **sysadmin** peuvent afficher et mettre à jour la table d'état.  
  
-   Utilisation du moniteur de mise en miroir de bases de données  
  
     Lorsqu'un administrateur système utilise le moniteur de mise en miroir de bases de données, il peut actualiser manuellement la page **État** , l'arborescence de navigation ou la page **Historique** . La table d'état est également mise à jour, sauf si elle a déjà été mise à jour lors des 15 secondes précédentes.  
  
     Pour afficher l’historique de l’état d’une mise en miroir sur une instance de serveur donnée, l’administrateur système peut également cliquer sur le bouton **Historique** correspondant à une instance de serveur (dans la page **État** ). L'historique est affiché dans la boîte de dialogue **Historique de la mise en miroir de bases de données** . Cette boîte de dialogue permet à l'administrateur système de consulter la totalité des lignes figurant dans la table d'état de l'instance du serveur, ou certaines d'entre elles.  
  
     Pour plus d'informations sur les mesures présentées dans la page **État** , consultez la section « Mesures de performances affichées par le moniteur de mise en miroir de bases de données » plus loin dans cette rubrique.  
  
-   Utilisation de **sp_dbmmonitorresults**  
  
     Les administrateurs système peuvent utiliser la procédure stockée système **sp_dbmmonitorresults** pour afficher (et éventuellement mettre à jour) la table d’état si cette dernière n’a pas été mise à jour au cours des 15 secondes précédentes. Cette procédure appelle la procédure **sp_dbmmonitorupdate** et retourne une ou plusieurs lignes d’historique, en fonction de la quantité demandée dans l’appel de procédure. Pour plus d’informations sur l’état dans son jeu de résultats, consultez [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md).  
  
#### <a name="monitoring-database-mirroring-status-by-dbm_monitor-members"></a>État de la mise en miroir de base de données (par les membres dbm_monitor)  
 Comme nous l’avons mentionné, lors de la première exécution de la procédure **sp_dbmmonitorupdate** , le rôle de base de données fixe **dbm_monitor** est créé dans la base de données **msdb** . Les membres du rôle de base de données fixe **dbm_monitor** peuvent consulter l’état de la mise en miroir existant à l’aide du moniteur de mise en miroir de bases de données ou de la procédure stockée **sp_dbmmonitorresults** . Cependant, ces utilisateurs ne peuvent pas mettre à jour la table d'état. Pour connaître l’ancienneté de l’état affiché, un utilisateur peut observer les heures sur les étiquettes **Journal principal (** _\<time>_ **)** et **Journal miroir** ( _\<time>_ **)** sur la page **État**.  
  
 Les membres du rôle de base de données fixe **dbm_monitor** sont tributaires du **travail du moniteur de mise en miroir de bases de données** pour la mise à jour de la table d’état à des fréquences régulières. Si le travail n'existe pas ou si l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est arrêté, l'état devient de plus en plus obsolète et ne correspondra plus forcément à la configuration de la session de mise en miroir. Par exemple, après un basculement, les partenaires peuvent sembler partager le même rôle (principal ou miroir), ou le serveur principal actuel peut être affiché comme serveur miroir, alors que le serveur miroir actuel est affiché comme principal.  
  
#### <a name="dropping-the-database-mirroring-monitor-job"></a>Suppression du travail de surveillance de la mise en miroir de bases de données  
 Le **Travail de surveillance de la mise en miroir de bases de données** est conservé tant qu'il n'a pas été supprimé. Le travail de surveillance doit être géré par l'administrateur système. Pour supprimer le **travail du moniteur de mise en miroir de bases de données**, utilisez **sp_dbmmonitordropmonitoring**. Pour plus d’informations, consultez [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md).  
  
###  <a name="status-displayed-by-the-database-mirroring-monitor"></a><a name="perf_metrics_of_dbm_monitor"></a> Mesures de performances affichées par le moniteur de mise en miroir de bases de données  
 La page **État** du moniteur de mise en miroir de bases de données décrit les partenaires ainsi que l'état de la session de mise en miroir. L'état inclut les mesures de performance telles que l'état du journal des transactions et d'autres informations destinées à estimer le temps nécessaire à l'exécution d'un basculement automatique et le risque de perte de données, si la session n'est pas synchronisée. De plus, la page **État** affiche l'état ainsi que des informations concernant la session de mise en miroir en général.  
  
> [!NOTE]  
>  Pour une présentation du moniteur de mise en miroir de bases de données et de la page **État** , consultez [Outils pour la surveillance de l’état de la mise en miroir de base de données](#tools_for_monitoring_dbm_status), plus haut dans cette rubrique.  
  
 Les informations fournies pour chaque éléments sont résumées dans les sections suivantes.  
  
#### <a name="partners"></a>Partenaires  
 La page **État** affiche les informations suivantes pour chacun des partenaires :  
  
-   Instance de serveur  
  
     Nom de l'instance de serveur dont l'état est affiché dans la ligne **État** .  
  
-   Rôle actuel  
  
     Rôle actuel de l'instance de serveur. Les états possibles sont :  
  
    -   Principal  
  
    -   Miroir  
  
-   état de la mise en miroir  
  
     Les états possibles sont :  
  
    -   Unknown  
  
    -   Synchronisation  
  
    -   Synchronisé  
  
    -   Interrompu  
  
    -   Déconnecté  
  
-   Connexion témoin  
  
     État de connexion du témoin. Les états possibles sont :  
  
    -   Unknown  
  
    -   Connecté  
  
    -   Déconnecté.  
  
#### <a name="log-on-the-principal-server"></a>Journal sur le serveur principal  
 La page **État** affiche les informations suivantes concernant l'état du journal sur le serveur principal à compter de l'heure indiquée :  
  
-   Journal non envoyé  
  
     Portion de journal en attente dans la file d'attente d'envoi en kilo-octets (Ko).  
  
-   Transaction non envoyée la plus ancienne  
  
     Âge de la transaction non envoyée la plus ancienne dans la file d'attente d'envoi. La durée de vie de cette transaction indique la quantité de transactions en minutes qui n'ont pas encore été envoyées à l'instance de serveur miroir. Cette valeur permet de mesurer la perte de données potentielle en termes de temps.  
  
-   Durée (estimée) d'envoi du journal  
  
     Estimation du nombre de minutes dont l'instance de serveur principal a besoin pour envoyer le journal qui est actuellement dans la file d'attente d'envoi à l'instance du serveur miroir en fonction du débit d'envoi actuel. La durée réelle nécessaire pour l'envoi du journal sera conditionnée par le débit des transactions entrantes, lequel peut varier considérablement. Cependant, la valeur **Durée (estimée) d’envoi du journal** peut être utile pour estimer approximativement la durée requise pour un basculement manuel.  
  
-   Taux d'envoi actuel  
  
     Débit auquel les transactions sont envoyées à l'instance du serveur miroir en Kilo-octets par seconde.  
  
-   Taux actuel de nouvelles transactions  
  
     Débit auquel les transactions entrantes sont consignées dans le journal du principal en Kilo-octets par seconde. Pour déterminer si la mise en miroir prend du retard, reste à jour ou rattrape le retard, comparez cette valeur à la valeur **Durée (estimée) d'envoi du journal** .  
  
#### <a name="log-on-the-mirror-server"></a>Journal sur le serveur miroir  
 La page **État** affiche les informations suivantes concernant l'état du journal sur le serveur miroir à compter de l'heure indiquée :  
  
-   Journal non restauré  
  
     Portion de journal en attente dans la file d'attente de restauration par progression en Ko.  
  
-   Durée (estimée) de restauration du journal  
  
     Nombre approximatif de minutes requises pour que le journal actuellement dans la file d'attente de restauration par progression soit appliqué à la base de données miroir.  
  
-   Taux de restauration actuel  
  
     Débit auquel les transactions sont restaurées dans la base de données miroir (en Kilo-octets par seconde).  
  
#### <a name="mirroring-session"></a>Session de mise en miroir  
 La page **État** affiche également les informations suivantes concernant la session de mise en miroir :  
  
-   Temps de traitement de validation de miroir  
  
     Délai moyen par transaction en millisecondes (pertinent uniquement en mode Haute sécurité). Ce délai correspond au temps de traitement pendant lequel l'instance de serveur principal attend que l'instance de serveur miroir écrive l'enregistrement du journal de transaction dans la file d'attente de restauration par progression.  
  
-   Durée (estimée) d'envoi et de restauration de la totalité du journal en cours  
  
     Estimation de la durée requise pour envoyer l'intégralité du journal non envoyé qui a été validé sur le principal et pour restaurer l'intégralité du journal actuellement dans la file d'attente de restauration par progression. Cette estimation peut être inférieure à la somme des valeurs des champs **Durée (estimée) d’envoi du journal** et **Durée (estimée) de restauration du journal** , car l’envoi et la restauration peuvent fonctionner en parallèle.  
  
-   Adresse témoin  
  
     Adresse réseau de l'instance du serveur témoin. Pour plus d’informations sur le format de cette adresse, consultez [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
-   Mode d'opération  
  
     Mode d'opération de la session de mise en miroir de bases de données :  
  
    -   Haute performance (asynchrone)  
  
    -   Haute sécurité sans basculement automatique (synchrone)  
  
    -   Haute sécurité avec basculement automatique (synchrone)  
  
##  <a name="additional-sources-of-information-about-a-mirrored-database"></a><a name="AdditionalSources"></a> Sources d'informations supplémentaires concernant une base de données mise en miroir  
 En plus d'utiliser le moniteur de mise en miroir de bases de données et les procédures stockées dbmmonitor pour surveiller une base de données mise en miroir et pour configurer des alertes sur des variables de performances analysées, [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] met à disposition des affichages catalogue, des compteurs de performance et des notifications d'événements de mise en miroir de bases de données.  
  
 **Dans cette section :**  
  
-   [Métadonnées de la mise en miroir de bases de données](#DbmMetadata)  
  
-   [Compteurs de performances de la mise en miroir de bases de données](#DbmPerfCounters)  
  
-   [Notifications d'événements de mise en miroir de bases de données](#DbmEventNotif)  
  
###  <a name="database-mirroring-metadata"></a><a name="DbmMetadata"></a> Métadonnées de la mise en miroir de bases de données  
 Chaque session de mise en miroir de bases de données est décrite dans des métadonnées qui sont exposées par le biais des affichages catalogue ou gestion dynamique suivants :  
  
-   **sys.database_mirroring**  
  
     Cet affichage comprend les métadonnées de la mise en miroir de bases de données correspondant à chaque base de données mise en miroir dans une instance de serveur. Pour plus d’informations, consultez [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
-   **sys.database_mirroring_endpoints**  
  
     L’affichage catalogue **sys.database_mirroring_endpoints** contient des informations concernant le point de terminaison de la mise en miroir de bases de données de l’instance de serveur. Pour plus d’informations, consultez [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
-   **sys.database_mirroring_witnesses**  
  
     Cet affichage catalogue contient les métadonnées de la mise en miroir de bases de données correspondant à chaque session dans laquelle une instance de serveur est le témoin. Pour plus d’informations, consultez [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md).  
  
-   **sys.dm_db_mirroring_connections**  
  
     Cet affichage de gestion dynamique retourne une ligne pour chaque connexion réseau de la mise en miroir de bases de données.  
  
     Pour plus d’informations, consultez [sys.dm_db_mirroring_connections &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md).  
  
###  <a name="database-mirroring-performance-counters"></a><a name="DbmPerfCounters"></a> Compteurs de performances de la mise en miroir de bases de données  
 Les compteurs de performances vous permettent d'analyser les performances de la mise en miroir de bases de données. Par exemple, vous pouvez examiner le compteur **Délai de transaction** pour savoir si la mise en miroir de bases de données a une incidence sur les performances du serveur principal, et vous pouvez examiner les compteurs **File d'attente de restauration par progression** et **File d'attente d'envoi du journal** pour savoir si la mise en miroir de la base de données est synchronisée avec la base de données principale. Vous pouvez examiner le compteur **Octets du journal envoyés/s** pour analyser la quantité de journal envoyée par seconde.  
  
 Dans l’Analyseur de performances sur les partenaires, les compteurs de performances sont disponibles dans l’objet de performance de la mise en miroir de bases de données (**SQLServer:Database Mirroring**). Pour plus d’informations, consultez [SQL Server, objet Database Mirroring](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md).  
  
 **Pour démarrer l'Analyseur de performances**  
  
-   [Démarrer le Moniteur système &#40;Windows&#41;](../../relational-databases/performance/start-system-monitor-windows.md)  
  
###  <a name="database-mirroring-event-notifications"></a><a name="DbmEventNotif"></a> Notifications d'événements de mise en miroir de bases de données  
 Les notifications d'événements sont un type spécial d'objet de base de données. Elles sont exécutées en réponse à une variété d'instructions en langage de définition de données (DDL) Transact-SQL et d'événements Trace SQL, et envoient des informations relatives aux événements de serveur et de base de données à un service [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
 Les éléments suivants sont disponibles pour la mise en miroir de bases de données :  
  
-   Classe d'événements **Database Mirroring State Change**  
  
     Indique le changement de l'état de mise en miroir d'une base de données mise en miroir. Pour plus d'informations, voir [Database Mirroring State Change Event Class](../../relational-databases/event-classes/database-mirroring-state-change-event-class.md).  
  
-   Classe d'événements **Audit Database Mirroring Login**  
  
     Retourne les messages d'audit relatifs à la sécurité du transport de la mise en miroir de bases de données. Pour plus d’informations, consultez [Audit Database Mirroring Login Event Class](../../relational-databases/event-classes/audit-database-mirroring-login-event-class.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Utiliser des seuils d’avertissement et d’alertes sur des métriques de performances de mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [Afficher l’état d’une base de données mise en miroir &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/view-the-state-of-a-mirrored-database-sql-server-management-studio.md)  
  
 **procédures stockées**  
  
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
 [Fournisseur WMI pour les concepts des événements de serveur](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)  
  
  
