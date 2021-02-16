---
title: Scalabilité | Microsoft Docs
description: Découvrez les améliorations apportées à la scalabilité du stockage sur disque pour les tables à mémoire optimisée dans SQL Server, telles que l’utilisation de plusieurs threads afin de conserver les tables.
ms.custom: ''
ms.date: 08/27/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: a4891c57-56bb-49f4-9bb5-f11b745279e5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 45b69a78843a601bc30f1799901659ba6dd1c13e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353681"
---
# <a name="scalability"></a>Extensibilité
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] comprend plusieurs améliorations concernant la scalabilité au niveau du stockage sur disque pour les tables à mémoire optimisée. 

## <a name="multiple-threads-to-persist-memory-optimized-tables"></a>Conservation des tables mémoire optimisées à l’aide de plusieurs threads  
  
[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] comprenait un seul thread de point de contrôle hors connexion qui recherchait dans le journal des transactions les modifications des tables à mémoire optimisée et qui les conservait dans des fichiers de point de contrôle (tels que des fichiers de données et des fichiers delta). Dans des ordinateurs avec un nombre de cœurs plus élevé, ce thread de point de contrôle hors connexion unique risquait de prendre du retard.  
  
À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], plusieurs threads simultanés sont responsables de la conservation des modifications apportées aux tables à mémoire optimisée.  
  
## <a name="multi-threaded-recovery"></a>Récupération multithread
Dans la version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la demande de journalisation dans le cadre de l’opération de récupération était monothread. À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], cette demande de journalisation est multithread.  
  
## <a name="merge-operation"></a>Opération de fusion  
L’opération de fusion est désormais multithread.  
   
> [!NOTE]
> La fusion manuelle a été désactivée, car une fusion multithread est censée suivre la charge. 

## <a name="dynamic-management-views"></a>Vues de gestion dynamique (DMV)  
Les vues de gestion dynamique [sys.dm_db_xtp_checkpoint_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-stats-transact-sql.md) et [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) ont considérablement évolué.  

## <a name="storage-management"></a>Gestion du stockage
Le moteur OLTP en mémoire continue d’utiliser le groupe de fichiers mémoire optimisé reposant sur FILESTREAM, mais les différents fichiers du groupe de fichiers sont dissociés de FILESTREAM. Ces fichiers sont entièrement gérés (notamment pour les actions de création, de suppression et de garbage collection) par le moteur OLTP en mémoire. 

> [!NOTE]
> [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) n’est pas pris en charge.  
  
## <a name="see-also"></a>Voir aussi   
[Création et gestion du stockage des objets à mémoire optimisée](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)     
[Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)    
[Options de fichiers et de groupes de fichiers ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)    
