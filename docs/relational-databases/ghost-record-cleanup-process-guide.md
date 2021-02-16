---
title: Guide du processus de nettoyage des enregistrements fantômes | Microsoft Docs
description: Découvrez le processus de nettoyage des enregistrements fantômes, un processus en arrière-plan à thread unique qui supprime les enregistrements marqués pour suppression dans SQL Server.
ms.custom: ''
ms.date: 05/02/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- ghost cleanup
- ghost records
- ghost clean up process
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e228afb793e69ab7b9d36fae31e838f6d1bca15a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059224"
---
# <a name="ghost-cleanup-process-guide"></a>Guide du processus de nettoyage des enregistrements fantômes

Le processus de nettoyage des enregistrements fantômes est un processus en arrière-plan à thread unique qui supprime les enregistrements marqués pour suppression dans des pages. L’article suivant fournit une vue d’ensemble de ce processus.

## <a name="ghost-records"></a>Enregistrements fantômes

Un enregistrement supprimé du niveau feuille d’une page d’index n’est pas physiquement supprimé de la page. Cet enregistrement est en fait marqué pour suppression et devient *fantôme*. La ligne reste donc sur la page, mais un bit est changé dans l’en-tête de la ligne pour indiquer qu’il s’agit vraiment d’un fantôme. Ceci a pour but d’optimiser les performances durant une opération de suppression. Les fantômes sont nécessaires pour le verrouillage au niveau des lignes, mais aussi pour l’isolation d’instantané afin de préserver les anciennes versions des lignes.

## <a name="ghost-record-cleanup-task"></a>Tâche de nettoyage des enregistrements fantômes

Les enregistrements marqués pour suppression, ou *fantômes*, sont nettoyés par le processus en arrière-plan de nettoyage des éléments fantômes. Ce processus en arrière-plan, qui s’exécute une fois la transaction de suppression validée, supprime physiquement les enregistrements fantômes des pages. Le processus de nettoyage des éléments fantômes s’exécute automatiquement à intervalles réguliers (toutes les 5 s pour SQL Server 2012+, toutes les 10 s pour SQL Server 2008/2008 R2) et détermine si des pages contiennent des enregistrements fantômes. S’il en trouve, il supprime ceux qui sont marqués pour suppression (*éléments fantômes*), impactant au plus 10 pages à chaque exécution.

Les bases de données contenant des enregistrements fantômes sont marquées comme telles, et le processus de nettoyage des éléments fantômes n’analyse que ces bases de données. Une fois tous les enregistrements fantômes supprimés, le processus de nettoyage marque la base de données comme « n’ayant plus d’enregistrements fantômes » et l’ignore la prochaine fois qu’il s’exécute. Le processus ignore également les bases de données s’il ne peut pas prendre un verrou partagé. Dans ce cas, il renouvelle l’opération la prochaine fois qu’il s’exécute.

La requête ci-dessous identifie le nombre d’enregistrements fantômes dans une base de données unique. 

 ```sql
 SELECT sum(ghost_record_count) total_ghost_records, db_name(database_id) 
 FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, 'SAMPLED')
 group by database_id
 order by total_ghost_records desc
```

## <a name="disable-the-ghost-cleanup"></a>Désactiver le nettoyage des éléments fantômes

Sur les systèmes à charge élevée avec de nombreuses suppressions, le processus de nettoyage des éléments fantômes peut entraîner des problèmes de performances issus de la conservation des pages dans le pool de mémoires tampons et de la génération d’E/S. Il est donc possible de désactiver ce processus à l’aide de l’[indicateur de trace 661](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Toutefois, la désactivation du processus a des implications au niveau des performances.

La désactivation du processus de nettoyage des éléments fantômes peut se traduire par une augmentation inutile de la taille votre base de données et entraîner des problèmes de performances. Si vous désactivez le processus de nettoyage des éléments fantômes, les enregistrements marqués comme tels dans la page ne sont plus supprimés, empêchant ainsi SQL Server de réutiliser cet espace. SQL Server est alors obligé d’ajouter des données dans de nouvelles pages, d’où des fichiers de base de données ballonnés susceptibles de provoquer des [fractionnements de pages](indexes/specify-fill-factor-for-an-index.md). Ces derniers donnent lieu à des problèmes de performances lors de la création de plans d’exécution et lors des opérations d’analyse. 

Une fois le processus de nettoyage des éléments fantômes désactivé, certaines actions doivent être entreprises pour supprimer les enregistrements fantômes. Une option consiste à exécuter une reconstruction d’index pour déplacer les données dans les pages. Une autre consiste à exécuter manuellement [sp_clean_db_free_space](system-stored-procedures/sp-clean-db-free-space-transact-sql.md) (pour nettoyer tous les fichiers de données de base de données) ou [sp_clean_db_file_free_space](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md) (pour nettoyer un fichier de données de base de données unique), ce qui supprime les enregistrements fantômes.

 >[!warning]
 > Il est généralement déconseillé de désactiver le processus de nettoyage des éléments fantômes. Vous devez tester minutieusement la désactivation du processus de nettoyage des éléments fantômes dans un environnement contrôlé avant de l’implémenter de façon permanente dans un environnement de production.


## <a name="next-steps"></a>Étapes suivantes  
[Désactivation du processus de nettoyage des éléments fantômes](https://support.microsoft.com/help/920093/tuning-options-for-sql-server-when-running-in-high-performance-workloa)
<br>[Supprimer les enregistrements fantômes d’un fichier de base de données unique](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
<br>[Supprimer les enregistrements fantômes de tous les fichiers de données de base de données](system-stored-procedures/sp-clean-db-free-space-transact-sql.md)


