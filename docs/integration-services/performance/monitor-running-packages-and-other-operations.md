---
description: Surveiller les packages en cours d’exécution et autres opérations
title: Surveiller les packages en cours d’exécution et autres opérations | Microsoft Docs
ms.custom: supportability
ms.date: 06/04/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isoperations.executions.f1
- sql13.ssis.ssms.isoperations.general.f1
ms.assetid: cbbcd79f-ab9b-46ec-84cb-4821c1d16b99
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2716b85dde5aaa7a7e2df15e87004c838b8137ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353018"
---
# <a name="monitor-running-packages-and-other-operations"></a>Surveiller les packages en cours d’exécution et autres opérations

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Vous pouvez surveiller les exécutions des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , les validations de projet et d’autres opérations en utilisant un ou plusieurs des outils suivants. Certains outils tels que les drainages de données ne sont disponibles que pour les projets déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Journaux d’activité  
  
     Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
-   Rapports  
  
     Pour plus d'informations, consultez [Reports for the Integration Services Server](#reports).  
  
-   Les vues  
  
     Pour plus d’informations, consultez [Vues &#40;Catalogue Integration Services&#41;](../../integration-services/system-views/views-integration-services-catalog.md).  
  
-   Compteurs de performance  
  
     Pour plus d’informations, consultez [Compteurs de performances](../../integration-services/performance/performance-counters.md).  
  
-   Drainages de données  

> [!NOTE]
> Cet article décrit comment monitorer des packages SSIS en cours d’exécution de manière générale et comment les monitorer au niveau local. Vous pouvez également exécuter et monitorer des packages SSIS dans Azure SQL Database. Pour plus d’informations, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
>
> Bien que vous puissiez également exécuter des packages SSIS sur Linux, aucun outil de monitoring n’est fourni sur Linux. Pour plus d’informations, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../../linux/sql-server-linux-migrate-ssis.md).

## <a name="operation-types"></a>Types d'opération  
 Plusieurs types d’opérations différents sont surveillés dans le catalogue **SSISDB** , sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Plusieurs messages peuvent être associés à chaque opération. Chaque message peut être classifié dans l'un des différents types. Par exemple, un message peut être de type Informations, Warning ou Error. Pour obtenir la liste complète des types de messages, consultez la documentation de la vue Transact-SQL [catalog.operation_messages &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md). Pour obtenir une liste complète des types d’opérations, consultez [catalog.operations &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).  
  
 Neuf types d'état différents sont utilisés pour indiquer l'état d'une opération. Pour obtenir une liste complète des types de statuts, consultez la vue [catalog.operations &#40;Base de données SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).  

## <a name="active-operations-dialog-box"></a><a name="active_ops"></a> Boîte de dialogue Opérations actives
  Utilisez la boîte de dialogue **Opérations actives** pour afficher l'état des opérations [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en cours d'exécution sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , par exemple les opérations de déploiement, de validation et d'exécution de package. Ces données sont stockées dans le catalogue SSISDB.  
  
 Pour plus d’informations sur les vues [!INCLUDE[tsql](../../includes/tsql-md.md)] associées, consultez [catalog.operations &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md), [catalog.validations &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md) et [catalog.executions &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
  
###  <a name="open-the-active-operations-dialog-box"></a><a name="open_dialog"></a> Ouvrir la boîte de dialogue Opérations actives  
  
1.  Ouvrir [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Se connecter au moteur de base de données Microsoft SQL Server  
  
3.  Dans l’Explorateur d’objets, développez le nœud **Integration Services** , cliquez avec le bouton droit sur **SSISDB**, puis cliquez sur **Opérations actives**.  
  
### <a name="configure-the-options"></a>Configurer les options  
  
 **Type**  
 Spécifie le type d'opération. Voici les valeurs possibles pour le champ **Type** et les valeurs correspondantes dans la colonne operations_type de la vue **catalog.operations** de Transact-SQL.  
  
|Description du champ de type|Valeur operations_type|  
|-|-|  
|Initialisation d'Integration Services|1|  
|Nettoyage des opérations (travail de l'Agent SQL)|2|  
|Nettoyage des versions du projet (travail de l'Agent SQL)|3|  
|Déployer le projet|101|  
|Restaurer le projet|106|  
|Créer et démarrer l'exécution du package|200|  
|Arrêter l'opération (arrêt d'une validation ou d'une exécution)|202|  
|Valider le projet|300|  
|Valider le package|301|  
|Configurer le catalogue|1 000|  
  
 **Stop**  
 Cliquez pour arrêter une opération en cours d'exécution.  

## <a name="viewing-and-stopping-packages-running-on-the-integration-services-server"></a>Affichage et arrêt des packages exécutés sur le serveur Integration Services
  La base de données **SSISDB** stocke l'historique de l'exécution dans des tables internes qui ne sont pas visibles par les utilisateurs. Cependant, elle expose les informations dont vous avez besoin via des vues publiques que vous pouvez interroger. Elle fournit également des procédures stockées que vous pouvez appeler pour effectuer des tâches courantes liées aux packages.  
  
 En général, vous gérez des objets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cependant, vous pouvez également interroger les vues de base de données et appeler les procédures stockées directement, ou écrire du code personnalisé qui appelle l'API managé. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et l'API managé interrogent les vues et appellent les procédures stockées pour effectuer de nombreuses tâches. Par exemple, vous pouvez afficher la liste des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui sont en cours d'exécution sur le serveur, et demander l'arrêt de packages si nécessaire.  
  
### <a name="viewing-the-list-of-running-packages"></a>Affichage de la liste de packages en cours d'exécution  
 Vous pouvez afficher la liste des packages en cours d'exécution sur le serveur dans la boîte de dialogue **Opérations actives** . Pour plus d'informations, consultez [Active Operations Dialog Box](#active_ops).  
  
 Pour plus d'informations sur les autres méthodes que vous pouvez utiliser pour afficher la liste de packages en cours d'exécution, consultez les rubriques suivantes.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] Accès Transact-SQL  
 Pour afficher la liste des packages qui s’exécutent sur le serveur, interrogez la vue, [catalog.executions &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md) pour les packages qui ont l’état 2.  
  
 Accès par programmation via l'API managé  
 Consultez l’espace de noms <xref:Microsoft.SqlServer.Management.IntegrationServices> et ses classes.  
  
### <a name="stopping-a-running-package"></a>Arrêt d'un package en cours d'exécution  
 Vous pouvez demander l'arrêt d'un package en cours d’exécution dans la boîte de dialogue **Opérations actives** . Pour plus d'informations, consultez [Active Operations Dialog Box](#active_ops).  
  
 Pour plus d'informations sur les autres méthodes que vous pouvez utiliser pour arrêter l'exécution d'un package, consultez les rubriques suivantes.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] Accès Transact-SQL  
 Pour arrêter un package en cours d’exécution sur le serveur, appelez la procédure stockée, [catalog.stop_operation &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md).  
  
 Accès par programmation via l'API managé  
 Consultez l’espace de noms <xref:Microsoft.SqlServer.Management.IntegrationServices> et ses classes.  
  
### <a name="viewing-the-history-of-packages-that-have-run"></a>Affichage de l'historique des packages qui ont été exécutés  
 Pour afficher l’historique des packages exécutés dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], utilisez le rapport **Toutes les exécutions** . Pour plus d’informations sur le rapport **Toutes les exécutions** et d’autres rapports standard, consultez [Rapports du serveur Integration Services](#reports).  
  
 Pour plus d'informations sur les autres méthodes que vous pouvez utiliser pour afficher l'historique des packages en cours d'exécution, consultez les rubriques suivantes.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] Accès Transact-SQL  
 Pour afficher des informations relatives aux packages qui ont été exécutés, interrogez la vue, [catalog.executions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md).  
  
 Accès par programmation via l'API managé  
 Consultez l’espace de noms <xref:Microsoft.SqlServer.Management.IntegrationServices> et ses classes.  

## <a name="reports-for-the-integration-services-server"></a><a name="reports"></a> Reports for the Integration Services Server
  Dans la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], les rapports standard sont disponibles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour vous aider à surveiller les projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Ces rapports vous aident à consulter l'état et l'historique du package et, si nécessaire, à identifier la cause des erreurs d'exécution du package.  
  
 En haut de chaque page de rapport, l'icône de retour permet de revenir à la page précédente, l'icône de rafraîchissement actualise les informations affichées dans la page et l'icône d'impression vous permet d'imprimer la page en cours.  
  
 Pour plus d’informations sur le déploiement de packages sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
### <a name="integration-services-dashboard"></a>Tableau de bord Integration Services  
 Le rapport **Tableau de bord Integration Services** fournit une vue d’ensemble de toutes les exécutions de package sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour chaque package qui s'est exécuté sur le serveur, le tableau de bord vous permet de « zoomer » pour rechercher des détails spécifiques sur les erreurs d'exécution du package qui se sont produites.  
  
 Le rapport affiche les sections d'informations suivantes.  
  
|Section|Description|  
|-------------|-----------------|  
|**Informations sur l'exécution**|Affiche le nombre d'exécutions à différents états (échec, en cours d'exécution, réussi, autres) au cours des dernières 24 heures.|  
|**Informations sur le package**|Affiche le nombre total de packages exécutés au cours des dernières 24 heures.|  
|**Informations de connexion**|Affiche les connexions utilisées durant les exécutions qui se sont soldées par un échec au cours des dernières 24 heures.|  
|**Informations détaillées sur les packages**|Affiche les détails des exécutions terminées qui se sont produites au cours des dernières 24 heures. Par exemple, cette section présente le nombre d'exécutions qui se sont soldées par un échec par rapport au nombre total d'exécutions, à la durée des exécutions (en secondes) et à la durée moyenne des exécutions au cours des trois derniers mois.<br /><br /> Pour afficher des informations supplémentaires pour un package, cliquez sur **Vue d’ensemble**, **Tous les messages** et **Performances de l’exécution**.<br /><br /> Le rapport **Performances de l’exécution** indique la durée de la dernière instance d’exécution, ainsi que les heures de début et de fin et l’environnement qui a été appliqué.<br /><br /> Le graphique et la table associée inclus dans le rapport **Performances de l’exécution** indiquent la durée des 10 dernières exécutions réussies du package. Le tableau indique également la durée moyenne d'exécution sur une période de trois mois. Différents environnements et valeurs littérales ont pu être appliqués au moment de l'exécution de ces 10 exécutions réussies du package.<br /><br /> Enfin, le rapport **Performances de l’exécution** affiche le temps d’activité et le temps total écoulé des composants de flux de données du package. Le temps d'activité fait référence au temps total d'exécution d'un composant au cours de toutes les phases, et le temps total fait référence au temps total écoulé pour un composant. Le rapport affiche ces informations uniquement pour les composants de package lorsque le niveau de journalisation de la dernière exécution du package est défini sur Performances ou Commentaires.<br /><br /> Le rapport **Vue d’ensemble** indique l’état des tâches du package. Le rapport **Messages** indique les messages d’événements et les messages d’erreur du package et les tâches, par exemple les heures de début et de fin et le nombre de lignes écrites.<br /><br /> Vous pouvez également cliquer sur **Afficher les messages** dans le rapport **Vue d’ensemble** pour accéder au rapport **Messages** . Vous pouvez également cliquer sur **Afficher la vue d’ensemble** dans le rapport **Messages** pour accéder au rapport **Vue d’ensemble** .|  
  
 Vous pouvez filtrer la table affichée dans une page en cliquant sur **Filtrer** et en sélectionnant des critères dans la boîte de dialogue **Paramètres du filtre** . Les critères de filtre disponibles dépendent des données qui sont affichées. Vous pouvez modifier l’ordre de tri du rapport en cliquant sur l’icône de tri dans la boîte de dialogue **Paramètres du filtre** .  
  
### <a name="all-executions-report"></a>Rapport Toutes les exécutions  
 Le rapport **Toutes les exécutions** affiche un résumé de toutes les exécutions d’ [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] effectuées sur le serveur. Il peut y avoir plusieurs exécutions de l'exemple de package. Contrairement au rapport **Tableau de bord Integration Services** , vous pouvez configurer le rapport **Toutes les exécutions** pour afficher les exécutions qui ont démarré au cours d’une plage de dates. Les dates peuvent couvrir plusieurs jours, mois ou années.  
  
 Le rapport affiche les sections d'informations suivantes.  
  
|Section|Description|  
|-------------|-----------------|  
|Filtrer|Indique le filtre actif appliqué au rapport, tel que la plage d'heures de début.|  
|Informations sur l'exécution|Indique l'heure de début, l'heure de fin, la durée d'exécution de chaque package. Vous pouvez afficher la liste des valeurs de paramètre utilisées avec une exécution de package, telles que les valeurs qui ont été passées à un package enfant à l'aide de la tâche d'exécution de package. Pour afficher la liste des paramètres, cliquez sur Vue d'ensemble.|  
  
 Pour plus d’informations sur l’utilisation de la tâche d’exécution de package afin de rendre des valeurs disponibles dans un package enfant, consultez [Tâche d’exécution de package](../../integration-services/control-flow/execute-package-task.md).  
  
 Pour plus d’informations sur les paramètres, consultez [Paramètres des projets et des packages Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-package-and-project-parameters.md).  
  
### <a name="all-connections"></a>Toutes les connexions  
 Le rapport **Toutes les connexions** fournit les informations suivantes pour les connexions qui ont échoué, pour les exécutions qui se sont produites sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Le rapport affiche les sections d'informations suivantes.  
  
|Section|Description|  
|-------------|-----------------|  
|Filtrer|Indique le filtre actif appliqué au rapport, tel que les connexions avec une chaîne spécifiée et la plage **Heure du dernier échec** .<br /><br /> Vous définissez la plage **Heure du dernier échec** pour afficher uniquement les échecs de connexion qui se sont produits pendant une plage de dates. La plage peut couvrir plusieurs jours, mois ou années.|  
|Détails|Affiche la chaîne de connexion, le nombre d'exécutions pendant lesquelles une connexion a échoué et la date du dernier échec de connexion.|  
  
### <a name="all-operations-report"></a>Rapport Toutes les opérations  
 Le rapport **Toutes les opérations** affiche un résumé de toutes les opérations [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] effectuées sur le serveur, notamment les déploiements, validations et exécutions de package, ainsi que d’autres opérations d’administration. Comme avec le Tableau de bord Integration Services, vous pouvez appliquer un filtre à la table afin de réduire les informations affichées.  
  
### <a name="all-validations-report"></a>Rapport Toutes les validations  
 Le rapport **Toutes les validations** affiche un résumé de toutes les validations d’ [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] effectuées sur le serveur. Le résumé affiche les informations relatives à chaque validation telles que l'état, l'heure de début et l'heure de fin. Chaque entrée du résumé inclut un lien vers les messages générés pendant la validation. Comme avec le Tableau de bord Integration Services, vous pouvez appliquer un filtre à la table afin de réduire les informations affichées.  
  
### <a name="custom-reports"></a>Rapports personnalisés  
 Vous pouvez ajouter un rapport personnalisé (fichier .rdl) au nœud de catalogue **SSISDB** sous le nœud **Catalogues Integration Services** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Avant d'ajouter le rapport, vérifiez que vous utilisez une convention d'affectation des noms en trois parties pour qualifier le projet référencé comme table source. Dans le cas contraire, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] affiche une erreur. La convention de nommage est \<database>.\<owner>.\<object>. Par exemple, le nom « SSISDB.interne.exécutions ».  
  
> [!NOTE]  
>  Quand vous ajoutez des rapports personnalisés au nœud **SSISDB** sous le nœud **Bases de données** , le préfixe SSISDB n’est pas nécessaire.  
  
 Pour obtenir des instructions sur la manière de créer et d’ajouter un rapport personnalisé, consultez [Ajouter un rapport personnalisé à Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md).  

## <a name="view-reports-for-the-integration-services-server"></a>Afficher les rapports du serveur Integration Services
  Dans la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], les rapports standard sont disponibles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour vous aider à surveiller les projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  Pour plus d’informations sur les rapports, consultez [Rapports du serveur Integration Services](#reports).  
  
### <a name="to-view-reports-for-the-integration-services-server"></a>Pour afficher les rapports du serveur Integration Services  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud **Catalogues Integration Services** dans l’Explorateur d’objets.  
  
2.  Cliquez avec le bouton droit sur **SSISDB**, cliquez sur **Rapports**, puis cliquez sur **Rapports standard**.  
  
3.  Cliquez sur une ou plusieurs des valeurs suivantes pour afficher un rapport.  
  
    -   **Tableau de bord Integration Services**  
  
    -   **Toutes les exécutions**  
  
    -   **Toutes les validations**  
  
    -   **Toutes les opérations**  
  
    -   **Toutes les connexions**  

## <a name="see-also"></a>Voir aussi  
 [Exécution de projets et de packages](../packages/deploy-integration-services-ssis-projects-and-packages.md)   
 [Rapports de dépannage pour l’exécution des packages](../troubleshooting/troubleshooting-reports-for-package-execution.md)  
