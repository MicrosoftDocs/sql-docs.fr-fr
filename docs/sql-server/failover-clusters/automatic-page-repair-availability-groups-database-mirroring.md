---
title: Réparation de page automatique pour les groupes de disponibilité et la mise en miroir de bases de données
description: Découvrez comment résoudre automatiquement certains cas de corruption de page lorsqu’une base de données participe à un groupe de disponibilité AlwaysOn ou à une mise en miroir de bases de données.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- automatic page repair
- Availability Groups [SQL Server], automatic page repair
- database mirroring [SQL Server], automatic page repair
- suspect pages [SQL Server]
ms.assetid: cf2e3650-5fac-4f34-b50e-d17765578a8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 111a5ad98210207fcdf55d3656953ddda6ac38c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346182"
---
# <a name="automatic-page-repair-availability-groups-database-mirroring"></a>Réparation de page automatique (Groupes de disponibilité : Mise en miroir de bases de données)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La réparation de page automatique est prise en charge par la mise en miroir de bases de données et par [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. Lorsque certains types d'erreurs endommagent une page et la rendent illisible, un serveur partenaire de mise en miroir de bases de données (principal ou miroir) ou un réplica de disponibilité (principal ou secondaire) tente de récupérer automatiquement la page. Le serveur partenaire/réplica qui ne peut pas lire la page demande une nouvelle copie de la page auprès de son serveur partenaire ou d'un autre réplica. Si cette demande réussit, la page illisible est remplacée par la copie lisible, ce qui permet généralement de résoudre l'erreur.  
  
 En règle générale, la mise en miroir de bases de données et [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] gèrent les erreurs d'E/S de manières équivalentes. Les quelques différences sont présentées ici de manière explicite.  
  
> [!NOTE]  
>  La réparation de page automatique diffère de la réparation DBCC. Lors d'une réparation de page automatique, toutes les données sont conservées. Cependant, la correction des erreurs à l'aide de l'option DBCC REPAIR_ALLOW_DATA_LOSS peut nécessiter la suppression de certaines pages et, par conséquent, de certaines données.  
  
-   [Types d'erreur qui provoquent une tentative de réparation de page automatique](#ErrorTypes)  
  
-   [Types de page qui ne peuvent pas être réparés automatiquement](#UnrepairablePageTypes)  
  
-   [Gestion des erreurs d'E/S sur le principal/la base de données primaire](#PrimaryIOErrors)  
  
-   [Gestion des erreurs d'E/S sur la base de données miroir/secondaire](#SecondaryIOErrors)  
  
-   [Recommandations des développeurs](#DevBP)  
  
-   [Procédure : consulter les tentatives de réparation de page automatique](#ViewAPRattempts)  
  
##  <a name="error-types-that-cause-an-automatic-page-repair-attempt"></a><a name="ErrorTypes"></a> Types d’erreurs qui provoquent une tentative de réparation de Page automatique  
 La réparation de page automatique de la mise en miroir de bases de données tente de réparer uniquement les pages situées dans un fichier de données sur lequel une opération a échoué en raison de l'une des erreurs répertoriées dans le tableau suivant.  
  
|Numéro d'erreur|Description|Instances qui provoquent une tentative de réparation de page automatique|  
|------------------|-----------------|---------------------------------------------------------|  
|823|Cette action est mise en œuvre uniquement si le système d'exploitation a effectué un contrôle de redondance cyclique (CRC) qui a échoué sur les données.|ERROR_CRC. La valeur du système d'exploitation pour cette erreur est 23.|  
|824|Erreurs logiques.|Erreurs de données logiques, telles que des erreurs d'écriture ou une somme de contrôle de page défectueuse.|  
|829|Une page a été marquée comme en attente de restauration.|Toutes|  
  
 Pour consulter les erreurs CRC 823 et les erreurs 824 récentes, consultez la table [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) dans la base de données [msdb](../../relational-databases/databases/msdb-database.md) .  

  
##  <a name="page-types-that-cannot-be-automatically-repaired"></a><a name="UnrepairablePageTypes"></a> Page Types That Cannot Be Automatically Repaired  
 La réparation de page automatique ne peut pas réparer les types de page de contrôle suivants :  
  
-   Page d'en-tête de fichier (ID de page 0).  
  
-   Page 9 (page de démarrage de la base de données).  
  
-   Pages d’allocation : pages Global Allocation Map (GAM), pages Shared Global Allocation Map (SGAM) et pages Page Free Space (PFS).  
  
 
##  <a name="handling-io-errors-on-the-principalprimary-database"></a><a name="PrimaryIOErrors"></a> Gestion des erreurs d’e/s sur la base de données principale/primaire  
 Sur le principal/la base de données primaire, la réparation de page automatique est lancée uniquement lorsque la base de données est dans l'état SYNCHRONIZED et que le principal/la base de données primaire envoie encore des enregistrements de journal au serveur miroir/secondaire pour la base de données. La séquence de base pour les actions relatives à une tentative de réparation de page automatique est la suivante :  
  
1.  Quand une erreur de lecture se produit sur une page de données sur le principal/la base de données primaire, ce dernier ou cette dernière insère une ligne dans la table [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) avec l’état d’erreur approprié. Pour la mise en miroir de bases de données, le principal demande ensuite une copie de la page au serveur miroir. Pour [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], le principal diffuse la demande à tous les serveurs secondaires et obtient la page du premier serveur qui répond. La demande spécifie l'ID de page et le numéro séquentiel dans le journal (Log Sequence Number ou LSN) se trouvant actuellement à la fin du journal vidé. La page est marquée comme *restauration en attente*, elle est par conséquent inaccessible pendant la tentative de réparation de page automatique. Les tentatives d'accès à cette page lors de la tentative de réparation échouent avec une erreur 829 (restauration en attente).  
  
2.  Après avoir reçu la demande de page, le serveur miroir/secondaire attend que le journal soit reconstruit jusqu'au LSN spécifié dans la demande. Ensuite, le serveur miroir/secondaire tente d'accéder à la page dans sa copie de la base de données. Si la page est accessible, le serveur miroir/secondaire envoie la copie de la page au principal/à la base de données primaire. Dans le cas contraire, le serveur miroir/secondaire retourne une erreur au principal/à la base de données primaire et la tentative de réparation de page automatique échoue.  
  
3.  Le principal/la base de données primaire traite la réponse qui contient la copie actualisée de la page.  
  
4.  Si la tentative de réparation automatique d’une page suspecte réussit, la page est marquée dans la table **suspect_pages** comme restaurée (**event_type** = 5).  
  
5.  Si l’erreur d’E/S de la page a provoqué des [transactions différées](../../relational-databases/backup-restore/deferred-transactions-sql-server.md), après la réparation de la page, le principal/la base de données primaire tente de résoudre ces transactions.  
  
 
##  <a name="handling-io-errors-on-the-mirrorsecondary-database"></a><a name="SecondaryIOErrors"></a> Gestion des erreurs d’e/s sur la base de données miroir/secondaire  
 Les erreurs d'E/S sur les pages de données qui se produisent sur la base de données miroir/secondaire sont généralement gérées de la même manière par la mise en miroir de bases de données et par [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
1.  Avec la mise en miroir de bases de données, si le serveur miroir rencontre une ou plusieurs erreurs d'E/S de page lorsqu'il reconstruit un enregistrement de journal, la session de mise en miroir passe à l'état SUSPENDED. Avec [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], si un réplica secondaire rencontre une ou plusieurs erreurs d'E/S de page lorsqu'il reconstruit un enregistrement de journal, la base de données secondaire passe à l'état SUSPENDED. Le serveur miroir/secondaire insère alors une ligne dans la table **suspect_pages** avec l’état d’erreur approprié. Le serveur miroir/secondaire demande ensuite une copie de la page au principal/à la base de données primaire.  
  
2.  Le principal/la base de données primaire tente d'accéder à la page dans sa copie de la base de données. Si la page est accessible, le principal/la base de données primaire envoie la copie de la page au serveur miroir/secondaire.  
  
3.  Si le serveur miroir/secondaire reçoit des copies de chaque page demandée, il tente de rétablir la session de mise en miroir. Si la tentative de réparation automatique d’une page suspecte réussit, la page est marquée dans la table **suspect_pages** comme restaurée (**event_type** = 4).  
  
     Si un serveur miroir/secondaire ne reçoit pas une page qu'il a demandée au principal/à la base de données primaire, la tentative de réparation de page automatique échoue. Avec la mise en miroir de bases de données, la session de mise en miroir reste suspendue. Avec [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], la base de données secondaire reste suspendue. Si la session de mise en miroir ou la base de données secondaire est rétablie manuellement, les pages endommagées sont à nouveau renvoyées lors de la phase de synchronisation.  
  
 
##  <a name="developer-best-practice"></a><a name="DevBP"></a> Developer Best Practice  
 Une réparation de page automatique est un processus asynchrone qui s'exécute en arrière-plan. Par conséquent, une opération de base de données qui demande une page illisible échoue et renvoie le code d'erreur correspondant à la condition ayant provoqué l'échec. Lorsque vous développez une application pour une base de données mise en miroir ou une base de données de disponibilité, vous devez intercepter les exceptions pour les opérations ayant échoué. Si le code d'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est 823, 824 ou 829, vous devez retenter l'opération ultérieurement.  
  

##  <a name="how-to-view-automatic-page-repair-attempts"></a><a name="ViewAPRattempts"></a> Procédure : Consulter les tentatives de réparation de page automatique  
 Les vues de gestion dynamique suivantes retournent les lignes correspondant aux dernières tentatives de réparation de page automatique sur une base de données de disponibilité ou une base de données mise en miroir spécifique, avec un maximum de 100 lignes par base de données.  
  
-   **Groupes de disponibilité Always On :**  
  
     [sys.dm_hadr_auto_page_repair &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
     Retourne une ligne pour chaque tentative de réparation de page automatique sur une base de données de disponibilité sur un réplica de disponibilité hébergé pour un groupe de disponibilité quelconque par l'instance de serveur.  
  
-   **Mise en miroir de bases de données :**  
  
     [sys.dm_db_mirroring_auto_page_repair &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)  
  
     Retourne une ligne pour chaque tentative de réparation de page automatique sur toute base de données mise en miroir sur l'instance de serveur.  
  
 
## <a name="see-also"></a>Voir aussi  
 [Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  


