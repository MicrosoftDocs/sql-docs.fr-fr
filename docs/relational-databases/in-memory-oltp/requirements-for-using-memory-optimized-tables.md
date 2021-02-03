---
title: Conditions requises pour l’utilisation des tables optimisées en mémoire | Microsoft Docs
description: Découvrez-en plus sur la configuration requise pour utiliser l’OLTP en mémoire, y compris la version SQL Database, la mémoire et les considérations de stockage, ainsi que l’installation.
ms.custom: ''
ms.date: 11/24/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 47d9a7e8-c597-4b95-a58a-dcf66df8e572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5a7c64a949ad4eabfbd02ad78229b04a919ab032
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237192"
---
# <a name="requirements-for-using-memory-optimized-tables"></a>Conditions requises pour l'utilisation des tables optimisées en mémoire
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Pour savoir comment utiliser la fonction OLTP en mémoire dans la base de données Azure, consultez [Prise en main de In-Memory (version préliminaire) dans la base de données SQL](/azure/azure-sql/in-memory-oltp-overview).  
  
 En plus des [configurations matérielle et logicielle exigées pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), les conditions d’utilisation de l’OLTP en mémoire sont les suivantes :  
  
-   [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 (ou ultérieur), n’importe quelle édition. Pour [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM (antérieur à SP1), vous avez besoin de l’édition Enterprise, Developer ou Evaluation.
    
    > [!NOTE]
    > L’OLTP en mémoire nécessite la version 64 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite une quantité de mémoire suffisante pour contenir les données des tables et des index optimisés en mémoire, ainsi qu’une quantité de mémoire supplémentaire pour gérer la charge de travail en ligne. Pour plus d’informations, consultez [Estimer les besoins en mémoire des tables mémoire optimisées](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) .  

-   Lors de l’exécution de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans une machine virtuelle, vérifiez qu’une quantité de mémoire suffisante est allouée à la machine virtuelle pour prendre en charge la mémoire nécessaire pour les tables et index optimisés en mémoire. Selon l’application hôte de la machine virtuelle, l’option de configuration pour garantir l’allocation de mémoire pour la machine virtuelle peut être appelée réserve de mémoire ou, lors de l’utilisation de mémoire dynamique, RAM minimale. Vérifiez que ces paramètres sont suffisants pour les besoins des bases de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
-   Espace disque disponible correspondant au double de la taille de vos tables optimisées en mémoire durables.  
  
-   Un processeur doit prendre en charge l’instruction **cmpxchg16b** pour pouvoir utiliser la fonction OLTP en mémoire. Tous les processeurs 64 bits modernes prennent en charge l’instruction **cmpxchg16b**.  
  
     Si vous utilisez une machine virtuelle et que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche une erreur provoquée par un processeur plus ancien, vérifiez si l’application hôte de la machine virtuelle dispose d’une option de configuration pour autoriser **cmpxchg16b**. Si ce n’est pas le cas, vous pouvez utiliser Hyper-V, qui prend en charge l’instruction **cmpxchg16b** sans qu’il ne soit nécessaire de modifier une option de configuration.  
  
-   OLTP en mémoire est installé dans le cadre des **Services Moteur de base de données**.  
  
     Pour installer la génération de rapports ([Déterminer si une table ou une procédure stockée doit être déplacée vers l’OLTP en mémoire](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)) et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (pour gérer l’OLTP en mémoire par le biais de l’Explorateur d’objets [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]), [téléchargez SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).   
  
## <a name="important-notes-on-using-hek_2"></a>Notes importantes concernant l’utilisation de l’[!INCLUDE[hek_2](../../includes/hek-2-md.md)]  
  
-   À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], il n’existe aucune limite de la taille des tables à mémoire optimisée, autre que la mémoire disponible. 

-   Dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], la taille totale en mémoire de toutes les tables durables dans une base de données ne doit pas dépasser 250 Go. Pour plus d’informations, consultez [Estimer les besoins en mémoire des tables mémoire optimisées](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md).  

> [!NOTE]
> À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, les éditions Standard et Express prennent en charge l’OLTP en mémoire, mais elles imposent des quotas sur la quantité de mémoire que vous pouvez utiliser pour les tables à mémoire optimisée dans une base de données. Dans l’édition Standard, ce quota est de 32 Go par base de données ; dans l’édition Express, il est de 352 Mo par base de données. 
  
-   Si vous créez une ou plusieurs bases de données avec des tables à mémoire optimisée, vous devez activer l’initialisation instantanée de fichiers (IFI) en accordant au compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le droit d’utilisateur *SE_MANAGE_VOLUME_NAME*. Sans IFI, les fichiers de stockage à mémoire optimisée (fichiers de données et delta) sont initialisés à la création, ce qui peut avoir un impact négatif sur les performances de votre charge de travail. Pour plus d’informations sur IFI, consultez [Initialisation instantanée des fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md).
  
## <a name="see-also"></a>Voir aussi  
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
 [Initialisation instantanée des fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md)  
 [Guide d’architecture de la mémoire](../../relational-databases/memory-management-architecture-guide.md)
