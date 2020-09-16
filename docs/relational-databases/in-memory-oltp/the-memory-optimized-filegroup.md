---
title: Groupe de fichiers à mémoire optimisée | Microsoft Docs
description: Découvrez comment créer un groupe de fichiers à mémoire optimisée, qui inclut des conteneurs pour les fichiers de données et delta, avant de créer des tables à mémoire optimisée dans SQL Server.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 14106cc9-816b-493a-bcb9-fe66a1cd4630
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed269ebe81cb70feff22fd0d2c3e3bf006ccbc9c
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89537994"
---
# <a name="the-memory-optimized-filegroup"></a>Groupe de fichiers mémoire optimisé
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Pour créer des tables mémoire optimisées, vous devez d'abord créer un groupe de fichiers mémoire optimisé. Le groupe de fichiers mémoire optimisé contient un ou plusieurs conteneurs. Chaque conteneur contient des fichiers de données, des fichiers delta ou les deux.  
  
 Bien que les lignes de données dans les tables `SCHEMA_ONLY` ne soient pas conservées et que les métadonnées des tables à mémoire optimisée et des procédures stockées compilées en mode natif soient stockées dans des catalogues traditionnels, le moteur [!INCLUDE[hek_2](../../includes/hek-2-md.md)] requiert toujours un groupe de fichiers à mémoire optimisée pour les tables à mémoire optimisée `SCHEMA_ONLY` afin de fournir une expérience uniforme pour les bases de données avec des tables à mémoire optimisée.  
  
 Le groupe de fichiers mémoire optimisé est basé sur le groupe de fichiers de flux de fichier, avec les différences suivantes :  
  
-   Vous ne pouvez créer qu'un seul groupe de fichiers mémoire optimisé par base de données. Vous devez marquer explicitement le groupe de fichiers comme contenant memory_optimized_data. Vous pouvez créer le groupe de fichiers lorsque vous créez la base de données, ou vous pouvez l' ajouter ultérieurement :  
  
    ```sql  
    ALTER DATABASE imoltp ADD FILEGROUP imoltp_mod CONTAINS MEMORY_OPTIMIZED_DATA  
    ```  
  
-   Vous devez ajouter un ou plusieurs conteneurs au groupe de fichiers `MEMORY_OPTIMIZED_DATA`. Par exemple :  
  
    ```sql  
    ALTER DATABASE imoltp ADD FILE (name='imoltp_mod1', filename='c:\data\imoltp_mod1') TO FILEGROUP imoltp_mod  
    ```  
  
-   Vous n’avez pas besoin d’activer le flux de fichier ([Activer et configurer FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)) pour créer un groupe de fichiers mémoire optimisé. Le mappage au flux de fichier est effectué par le moteur [!INCLUDE[hek_2](../../includes/hek-2-md.md)] .  
  
-   Vous pouvez ajouter de nouveaux conteneurs à un groupe de fichiers mémoire optimisé. Vous pouvez avoir besoin d'un nouveau conteneur pour développer le stockage requis pour la table durable, mémoire optimisée, et également pour répartir les E/S sur plusieurs conteneurs.  
  
-   Le déplacement des données avec un groupe de fichiers mémoire optimisé est optimisé dans une configuration de groupe de disponibilité Always On. Contrairement aux fichiers de flux de fichier envoyés aux réplicas secondaires, les fichiers de point de contrôle (de données et delta) du groupe de fichiers mémoire optimisé ne sont pas envoyés aux réplicas secondaires. Les fichiers de données et delta sont construits à l'aide du journal des transactions sur le réplica secondaire.  
  
Les limitations suivantes s’appliquent à un groupe de fichiers optimisé en mémoire :  
  
-   Une fois que vous utilisez un groupe de fichiers à mémoire optimisée, vous ne pouvez le supprimer qu’en supprimant la base de données. Dans un environnement de production, il est peu probable que vous deviez supprimer le groupe de fichiers mémoire optimisé.  
  
-   Vous ne pouvez pas supprimer un conteneur non vide ni déplacer des paires de fichiers de données et delta vers un autre conteneur dans le groupe de fichiers mémoire optimisé.    
  
## <a name="configuring-a-memory-optimized-filegroup"></a>Configuration d'un groupe de fichiers mémoire optimisé  
Envisagez de créer plusieurs conteneurs dans le groupe de fichiers à mémoire optimisée et de les répartir sur différents lecteurs afin d’obtenir davantage de bande passante pour transmettre en continu les données en mémoire. 
 
Dans un scénario à plusieurs conteneurs et à plusieurs lecteurs, les fichiers de données et delta sont alloués dans des conteneurs selon le principe du tourniquet. Le premier fichier de données est alloué depuis le premier conteneur, le fichier delta depuis le conteneur suivant, et ainsi de suite. Cette méthode d'allocation répartit les fichiers de données et delta de manière uniforme entre les conteneurs si vous avez un nombre impair de lecteurs, chacun étant mappé à un conteneur. Toutefois, si vous avez un nombre pair de lecteurs, chacun étant mappé à un conteneur, cela peut entraîner un stockage déséquilibré, les fichiers de données étant mappés aux lecteurs impairs et les fichiers delta aux lecteurs pairs. Pour obtenir un flux équilibré d’E/S lors de la récupération, envisagez de placer des paires de fichiers de données et delta sur les mêmes broches/stockages.
  
Lors de la configuration du stockage, vous devez fournir un espace libre correspondant à quatre fois la taille des tables mémoire optimisées durables. Vérifiez également que votre sous-système d’E/S prend en charge les IOPS nécessaires pour votre charge de travail. Si les paires de fichiers de données et delta sont définies à un niveau d’IOPS donné, vous avez besoin de trois fois cette valeur d’IOPS pour tenir compte des opérations de stockage et de fusion. Vous pouvez ajouter une capacité de stockage et des IOPS en ajoutant un ou plusieurs conteneurs au groupe de fichiers mémoire optimisé.  
 
> [!CAUTION]
> Si une valeur `MAXSIZE` est définie pour le groupe de fichiers à mémoire optimisée et que les fichiers de point de contrôle dépassent la taille maximale du conteneur, la base de données présente l’état SUSPECT.   
> Dans ce cas, n’essayez pas de définir les options OFFLINE et ONLINE de base de données, ce qui amène la base de données à rester dans l’état RECOVERY_PENDING.
  
## <a name="see-also"></a>Voir aussi  
[Création et gestion du stockage des objets à mémoire optimisée](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
[Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)    
[Options de fichiers et de groupes de fichiers ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md) 

