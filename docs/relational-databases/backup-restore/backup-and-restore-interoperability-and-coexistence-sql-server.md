---
title: 'Sauvegarde et restauration : interopérabilité des fonctionnalités'
description: Cet article décrit les fonctionnalités de sauvegarde et de restauration de SQL Server, y compris le démarrage de la base de données, la restauration en ligne, les index désactivés et la mise en miroir des bases de données.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server], related features
- restoring [SQL Server], files
- restoring files [SQL Server], related features
- backups [SQL Server], files or filegroups
- file backups [SQL Server], related features
ms.assetid: 69f212b8-edcd-4c5d-8a8a-679ced33c128
author: cawrites
ms.author: chadam
ms.openlocfilehash: b8de8452d7f39b1450df8c280ed27a9ac12603db
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077067"
---
# <a name="backup-and-restore-interoperability-and-coexistence-sql-server"></a>Sauvegarde et restauration : interopérabilité et coexistence (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique comprend des observations sur la sauvegarde et la restauration pour plusieurs fonctionnalités de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Ces fonctionnalités concernent : la restauration de fichiers et le démarrage de bases de données, la restauration et la désactivation en ligne d'index, la mise en miroir de bases de données, la restauration fragmentaire et les index de recherche en texte intégral.  
  
 **Dans cette rubrique :**  
  
-   [Restauration de fichiers et démarrage d'une base de données](#FileRestoreAndDbStartup)  
  
-   [Restauration en ligne et index désactivés](#OnlineRestoreAndDisabledIndexes)  
  
-   [Sauvegarde et restauration et mise en miroir d'une base de données](#DbMandBnR)  
  
-   [Restauration fragmentaire et index de recherche en texte intégral](#PiecemealAndFTIndexes)  
  
-   [Sauvegarde et restauration de fichiers et compression](#FileBnRandCompression)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="file-restore-and-database-startup"></a><a name="FileRestoreAndDbStartup"></a> Restauration de fichiers et démarrage d'une base de données  
 Cette section concerne uniquement les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contiennent plusieurs groupes de fichiers.  
  
> [!NOTE]  
>  Lors du démarrage d'une base de données, seuls les groupes de fichiers dont les fichiers étaient en ligne lorsque la base de données était fermée sont récupérés et mis en ligne.  
  
 Si un problème se produit lors du démarrage de la base de données, la récupération échoue et la base de données est affectée de l'attribut SUSPECT. Si le problème peut être isolé dans un ou des fichiers, l'administrateur de la base de données peut placer les fichiers hors connexion et tenter de la redémarrer. Pour placer un fichier hors connexion, vous pouvez utiliser l'instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) suivante :  
  
 ALTER DATABASE *nom_base_de_données* MODIFY FILE (NAME **='** _nom_fichier_*_'_*, OFFLINE)  
  
 Si le démarrage aboutit, les groupes de fichiers qui contiennent un fichier hors connexion demeurent hors connexion.  
  
##  <a name="online-restore-and-disabled-indexes"></a><a name="OnlineRestoreAndDisabledIndexes"></a> Restauration en ligne et index désactivés  
 Elle concerne seulement les bases de données avec plusieurs groupes de fichiers et, dans le cas du mode de récupération simple, au moins un groupe de fichiers en lecture seule.  
  
 Dans ces cas de figure, lorsqu'une base de données est en ligne, l'index peut être créé, supprimé, activé ou désactivé uniquement si les groupes de fichiers qui contiennent une partie de l'index sont en ligne.  
  
 Pour plus d’informations sur la restauration des groupes de fichiers hors connexion, consultez [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
##  <a name="database-mirroring-and-backup-and-restore"></a><a name="DbMandBnR"></a> Sauvegarde et restauration et mise en miroir d'une base de données  
 Cette section ne concerne que les bases de données en mode de restauration complète et composées de plusieurs groupes de fichiers.  
  
> [!NOTE]  
>  La fonctionnalité de mise en miroir de bases de données sera supprimée dans une prochaine version de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Utilisez [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] à la place.  
  
 La mise en miroir de bases de données est une solution permettant d'accroître la disponibilité d'une base de données. La mise en miroir est implémentée individuellement pour chaque base de données et fonctionne uniquement avec les bases de données qui utilisent le mode de restauration complète. Pour plus d’informations, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!NOTE]  
>  Pour distribuer des copies d'un sous-ensemble des groupes de fichiers d'une base de données, utilisez la réplication : ne répliquez que les objets des groupes de fichiers à copier sur d'autres serveurs. Pour plus d’informations sur la réplication, consultez [Réplication SQL Server](../../relational-databases/replication/sql-server-replication.md).  
  
### <a name="creating-the-mirror-database"></a>Création de la base de données miroir  
 La base de données miroir est créée par la restauration (WITH NORECOVERY) de sauvegardes de la base de données principale sur le serveur miroir. La base de données restaurée doit conserver le nom initial de la base de données. Pour plus d’informations, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Vous pouvez créer la base de données miroir à l'aide d'une séquence de restauration fragmentaire, si celle-ci est prise en charge. Cependant, vous ne pouvez pas démarrer la mise en miroir avant d'avoir restauré tous les groupes de fichiers et, en règle générale, les sauvegardes des journaux pour synchroniser la base de données miroir avec la base de données principale. Pour plus d’informations, consultez [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
### <a name="restrictions-on-backup-and-restore-during-mirroring"></a>Restrictions imposées en matière de sauvegarde et de restauration lors de la mise en miroir  
 Pendant une session de mise en miroir de base de données, les restrictions suivantes s'appliquent :  
  
-   La sauvegarde et la restauration de la base de données ne sont pas autorisées.  
  
-   La sauvegarde de la base de données principale est autorisée, mais pas la sauvegarde du journal des transactions sans récupération (BACKUP LOG WITH NORECOVERY).  
  
-   La restauration de la base de données principale n'est pas autorisée.  
  
##  <a name="piecemeal-restore-and-full-text-indexes"></a><a name="PiecemealAndFTIndexes"></a> Restauration fragmentaire et index de recherche en texte intégral  
 Cette section ne concerne que les bases de données contenant plusieurs groupes de fichiers et, pour les bases de données en mode simple, que les groupes de fichiers en lecture seule.  
  
 Les index de recherche en texte intégral sont stockés dans des groupes de fichiers de base de données et peuvent être affectés par une restauration fragmentaire. Si l'index de recherche en texte intégral réside dans le même groupe de fichiers que des données de table associées, la restauration fragmentaire fonctionne comme prévu.  
  
> [!NOTE]  
>  Pour afficher l’ID du groupe de fichiers qui contient un index de recherche en texte intégral, sélectionnez la colonne data_space_id de [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
### <a name="full-text-indexes-and-tables-in-separate-filegroups"></a>Index de recherche en texte intégral et tables dans des groupes de fichiers distincts  
 Si un index de recherche en texte intégral réside dans un autre groupe de fichiers que les données de table associées, le comportement d'une restauration fragmentaire dépend du groupe de fichiers restauré et mis en ligne en premier :  
  
-   Si le groupe de fichiers contenant l'index de recherche en texte intégral est restauré et mis en ligne avant les groupes de fichiers contenant les données de table associées, la recherche en texte intégral fonctionne comme prévu dès que l'index de recherche en texte intégral est en ligne.  
  
-   Si le groupe de fichiers contenant les données de table est restauré et mis en ligne avant le groupe de fichiers contenant l'index de recherche en texte intégral, le comportement du texte intégral peut être affecté. En effet, les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui déclenchent un remplissage, reconstruisent le catalogue ou réorganisent le catalogue échouent tant que l'index n'est pas mis en ligne. Ces instructions incluent CREATE FULLTEXT INDEX, ALTER FULLTEXT INDEX, DROP FULLTEXT INDEX et ALTER FULLTEXT CATALOG.  
  
     Dans ce cas, les facteurs suivants sont importants :  
  
    -   Si le suivi des modifications est activé pour l'index de recherche en texte intégral, le DML utilisateur échoue tant que le groupe de fichiers de l'index n'est pas mis en ligne. Il en va de même pour une opération de suppression.  
  
    -   Quel que soit le suivi des modifications, les requêtes de texte intégral échouent, car l'index n'est pas disponible. Si une requête de texte intégral est tentée alors que le groupe de fichiers contenant l'index de recherche en texte intégral est hors connexion, une erreur est retournée.  
  
    -   Les fonctions d'état (telles que FULLTEXTCATALOGPROPERTY) réussissent uniquement dans les cas où elles n'ont pas à accéder à l'index de recherche en texte intégral. Par exemple, l’accès à des métadonnées de texte intégral en ligne réussit, alors que **uniquekeycount et itemcount** échouent.  
  
     Une fois le groupe de fichiers de l'index de recherche en texte intégral restauré et mis en ligne, les données de l'index et des tables sont cohérentes.  
  
 Dès que le groupe de fichiers des tables et celui de l'index de recherche en texte intégral sont en ligne, tout remplissage de texte intégral suspendu reprend.  
  
##  <a name="file-backup-and-restore-and-compression"></a><a name="FileBnRandCompression"></a> Sauvegarde et restauration de fichiers et compression  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la compression des données du système de fichiers NTFS pour les groupes de fichiers et les bases de données en lecture seule.  
  
 La restauration des fichiers dans un groupe de fichiers en lecture seule est prise en charge sur les fichiers compressés NTFS. La sauvegarde et la restauration de ces groupes de fichiers fonctionne pour l'essentiel de la même manière que pour n'importe quel groupe de fichiers en lecture seule, à quelques exceptions près :  
  
-   La restauration d'un fichier en lecture-écriture (y compris le fichier primaire et les fichiers journaux d'une base de données en lecture-écriture) vers un volume compressé échoue et génère une erreur.  
  
-   La restauration d'une base de données en lecture seule vers un volume compressé est autorisée.  
  
> [!NOTE]  
>  Les fichiers journaux des bases de données en lecture-écriture ne doivent jamais être placés sur des systèmes de fichiers compressés.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Sauvegarder et restaurer des catalogues et des index de recherche en texte intégral](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Sauvegarder et restaurer des bases de données répliquées](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
[Secondaires actifs : Sauvegarde sur des réplicas secondaires \(groupes de disponibilité AlwaysOn\)](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)  
  
  
