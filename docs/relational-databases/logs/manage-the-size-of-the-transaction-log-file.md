---
title: Gérer la taille du fichier journal de transactions | Microsoft Docs
description: Découvrez comment surveiller la taille du journal des transactions SQL Server, réduire le journal, agrandir un journal, optimiser le taux de croissance du journal tempdb et contrôler la croissance du journal des transactions.
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- transaction logs [SQL Server], size management
- manage log size
- log size, manage
ms.assetid: 3a70e606-303f-47a8-96d4-2456a18d4297
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 699052eedf2a32faac9f3bf849f5f4aaa4c43b4c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237702"
---
# <a name="manage-the-size-of-the-transaction-log-file"></a>Gérer la taille du fichier journal des transactions
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Cette rubrique contient des informations sur la façon de surveiller la taille d’un journal des transactions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], de réduire le journal des transactions, d’ajouter ou d’agrandir un fichier journal de transactions, d’optimiser le taux de croissance du journal des transactions de **tempdb**, et de contrôler la croissance d’un fichier journal de transactions.  

##  <a name="monitor-log-space-use"></a><a name="MonitorSpaceUse"></a>Surveiller l’utilisation de l’espace pour le journal  
Surveillez l’utilisation de l’espace pour le journal à l’aide de [sys.dm_db_log_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md). Cette vue de gestion dynamique retourne des informations sur la quantité d’espace journal utilisée et indique à quel moment le journal des transactions a besoin d’être tronqué. 

Pour plus d’informations sur la taille actuelle d’un fichier journal, sa taille maximale et l’option de croissance automatique du fichier, vous pouvez également utiliser les colonnes **size**, **max_size** et **growth** de ce fichier journal dans [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).  
  
> [!IMPORTANT]
> Évitez de surcharger le disque du journal. Assurez-vous que le stockage des journaux peut supporter les exigences [d’IOPS](https://wikipedia.org/wiki/IOPS) et de faible latence inhérentes à votre charge transactionnelle. 
  
##  <a name="shrink-log-file-size"></a><a name="ShrinkSize"></a> Réduire la taille du fichier journal  
 Pour réduire la taille physique d'un fichier journal physique, vous devez réduire le fichier journal. Cela est utile quand vous savez qu’un fichier journal de transactions contient de l’espace inutilisé. Vous pouvez réduire un fichier journal uniquement quand la base de données est en ligne, et qu’au moins un [fichier journal virtuel](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) est libre. Dans certains cas, la réduction du journal peut n'être possible qu'après la troncation de journal suivante.  
  
> [!NOTE]
> Certains facteurs (par exemple, une transaction longue) chargés de maintenir les [fichiers journaux virtuels](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) actifs pendant une période de temps prolongée peuvent limiter, voire empêcher, la réduction du journal. Pour plus d’informations, consultez [Facteurs pouvant retarder la troncation du journal](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation).  
  
La réduction d’un fichier journal supprime un ou plusieurs [fichiers journaux virtuels](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) qui ne contiennent aucune partie du journal logique (autrement dit, des *fichiers journaux virtuels inactifs*). Quand vous réduisez un fichier journal de transactions, les fichiers journaux virtuels inactifs sont supprimés à la fin du fichier journal pour réduire le journal et le ramener à une taille proche de la taille cible. 

> [!IMPORTANT]
> Avant de réduire le journal des transactions, gardez à l’esprit les [facteurs pouvant retarder la troncation du journal](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation). Si l’espace de stockage est à nouveau nécessaire après une réduction de journal, le journal des transactions croît de nouveau, introduisant une surcharge au niveau des performances pendant les opérations d’accroissement du journal. Pour plus d’informations, consultez les [Recommandations](#Recommendations) dans cette rubrique.
  
 **Réduire un fichier journal (sans réduire les fichiers de base de données)**  
  
-   [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)  
  
-   [Réduire un fichier](../../relational-databases/databases/shrink-a-file.md)  
  
 **Surveiller les événements de réduction du fichier journal**  
  
-   [Log File Auto Shrink Event Class](../../relational-databases/event-classes/log-file-auto-shrink-event-class.md).  
  
 **Contrôler l’espace pour le journal**  
  
-   [sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)  
  
-   [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) (consultez les colonnes **size**, **max_size** et **growth** du ou des fichiers journaux.)  
  
##  <a name="add-or-enlarge-a-log-file"></a><a name="AddOrEnlarge"></a> Ajouter ou agrandir un fichier journal  
Vous pouvez gagner de l’espace en agrandissant le fichier journal existant (si l’espace disque le permet) ou en ajoutant un fichier journal à la base de données, généralement sur un autre disque. Un seul fichier journal de transactions est suffisant, sauf si l’espace pour le journal est insuffisant, et que l’espace disque est également insuffisant sur le volume qui contient le fichier journal.   
  
-   Pour ajouter un fichier journal à la base de données, utilisez la clause `ADD LOG FILE` de l’instruction `ALTER DATABASE`. L'ajout d'un fichier journal permet au journal de croître.  
-   Pour agrandir le fichier journal, utilisez la clause `MODIFY FILE` de l’instruction `ALTER DATABASE`, en spécifiant la syntaxe `SIZE` et `MAXSIZE`. Pour plus d’informations, consultez [Options de fichiers et de groupes de fichiers &#40;Transact-SQL&#41; ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  

Pour plus d’informations, consultez les [Recommandations](#Recommendations) dans cette rubrique.
    
##  <a name="optimize-tempdb-transaction-log-size"></a><a name="tempdbOptimize"></a> Optimiser la taille du journal des transactions tempdb  
 Le redémarrage d’une instance de serveur permet de redimensionner le journal des transactions de la base de données **tempdb** conformément à sa taille d’origine avant la croissance automatique. Ceci peut réduire les performances du journal des transactions **tempdb** . 
 
 Vous pouvez éviter cette surcharge en augmentant la taille du journal des transactions **tempdb** après avoir démarré ou redémarré l'instance de serveur. Pour plus d'informations, consultez [tempdb Database](../../relational-databases/databases/tempdb-database.md).  
  
##  <a name="control-transaction-log-file-growth"></a><a name="ControlGrowth"></a> Contrôler la croissance d’un fichier journal de transactions  
 Utilisez l’instruction [ALTER DATABASE - Options de fichiers et de groupes de fichiers &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md) pour gérer la croissance d’un fichier journal de transactions. Notez les points suivants :  
  
-   Pour changer la taille actuelle du fichier selon les unités Ko, Mo, Go et To, utilisez l’option `SIZE`.  
-   Pour changer l’incrément de croissance, utilisez l’option `FILEGROWTH`. Une valeur 0 indique que la croissance automatique est désactivée et qu'aucun espace supplémentaire n'est autorisé.  
-   Pour contrôler la taille maximale d’un fichier journal en Ko, Mo, Go et To ou affecter la valeur UNLIMITED à la croissance, utilisez l’option `MAXSIZE`.  

Pour plus d’informations, consultez les [Recommandations](#Recommendations) dans cette rubrique.

## <a name="recommendations"></a><a name="Recommendations"></a> Recommandations
Voici une série de recommandations générales à suivre pendant l’utilisation de fichiers journaux de transactions :

-   L’incrément de croissance automatique du journal des transactions, tel que défini par l’option `FILEGROWTH`, doit être suffisamment grand pour anticiper les besoins des transactions de la charge de travail. L'incrément de croissance d'un fichier journal doit être suffisamment important pour éviter une expansion fréquente. Une bonne approche pour dimensionner correctement un journal des transactions consiste à contrôler la quantité de journal occupée pendant :
    -  Le temps nécessaire pour exécuter une sauvegarde complète, étant donné que les sauvegardes de fichier journal ne peuvent pas se produire tant qu’elle n’est pas terminée
    -  Le temps nécessaire pour les opérations de maintenance des index les plus volumineux
    -  Le temps nécessaire pour exécuter le lot le plus volumineux dans une base de données

-   Quand vous définissez la **croissance automatique** pour les fichiers journaux et de données à l’aide de l’option `FILEGROWTH`, il peut être préférable de le faire en **taille** plutôt qu’en **pourcentage**, pour permettre un meilleur contrôle du taux de croissance, car le pourcentage exprime une quantité en constante augmentation.
    -  N’oubliez pas que les journaux des transactions ne peuvent pas tirer parti de [l’initialisation instantanée de fichiers](../../relational-databases/databases/database-instant-file-initialization.md) ; les temps de croissance de journal étendus sont donc particulièrement critiques. 
    -  En guise de bonne pratique, ne définissez pas l’option `FILEGROWTH` sur une valeur supérieure à 1 024 Mo pour les journaux des transactions. Les valeurs par défaut pour l’option `FILEGROWTH` sont les suivantes :  
  
      |Version|Valeurs par défaut|  
      |-------------|--------------------|  
      |À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|64 Mo de données. 64 Mo de fichiers journaux.|  
      |À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|1 Mo de données. 10 % de fichiers journaux.|  
      |Avant [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|10 % de données. 10 % de fichiers journaux.|  

-   Un incrément de croissance réduit peut générer un nombre excessif de petits [fichiers journaux virtuels](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) et réduire le niveau de performance. Pour déterminer la distribution optimale des fichiers journaux virtuels pour la taille actuelle du journal des transactions de toutes les bases de données dans une instance donnée, ainsi que les incréments de croissance pour atteindre la taille nécessaire, consultez ce [script](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs).

-   Un incrément de croissance élevé peut générer un nombre insuffisants de [fichiers journaux virtuels](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) volumineux et affecter également le niveau de performance. Pour déterminer la distribution optimale des fichiers journaux virtuels pour la taille actuelle du journal des transactions de toutes les bases de données dans une instance donnée, ainsi que les incréments de croissance pour atteindre la taille nécessaire, consultez ce [script](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs). 

-   Même si la croissance automatique est activée, vous pouvez recevoir un message indiquant que le journal des transactions est complet, s’il ne peut pas croître suffisamment rapidement pour répondre aux besoins de votre requête. Pour plus d’informations sur le changement de l’incrément de croissance, consultez [Options de fichiers et de groupes de fichiers &#40;Transact-SQL&#41; ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).

-   Avoir plusieurs fichiers journaux dans une base de données n’améliore pas du tout le niveau de performance, car les fichiers journaux de transactions n’utilisent pas le [remplissage proportionnel](../../relational-databases/pages-and-extents-architecture-guide.md#ProportionalFill) comme les fichiers de données dans un même groupe de fichiers.  

-   Les fichiers journaux peuvent être définis de manière à se réduire automatiquement. Toutefois, cette configuration étant **déconseillée**, la propriété de base de données **auto_shrink** est définie sur FALSE par défaut. Si **auto_shrink** est définie sur la valeur TRUE, la troncation automatique réduit la taille d’un fichier uniquement quand plus de 25 pour cent de son espace est inutilisé. 
    -   Le fichier est réduit soit à la taille à laquelle seuls 25 % de ce dernier sont inutilisés soit à sa taille d'origine, selon la valeur la plus élevée. 
    -   Pour plus d’informations sur la modification du paramètre de la propriété **auto_shrink**, consultez [Afficher ou modifier les propriétés d’une base de données](../../relational-databases/databases/view-or-change-the-properties-of-a-database.md) et [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). 
  
## <a name="see-also"></a>Voir aussi  
[BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
[Résoudre les problèmes liés à un journal des transactions saturé &#40;erreur SQL Server 9002&#41;](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)    
[Sauvegardes de fichier journal dans le guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Backups)    
[Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)    
[Options de fichiers et de groupes de fichiers &#40;Transact-SQL&#41; ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)
