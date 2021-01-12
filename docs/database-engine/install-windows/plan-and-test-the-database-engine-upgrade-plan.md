---
title: Planifier et tester le plan de mise à niveau du moteur de base de données | Microsoft Docs
description: Cet article décrit la planification précédant le début de votre mise à niveau de SQL Server, et fournit notamment une liste de vérification pour la planification et explique comment développer et tester un plan de mise à niveau.
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 19c5b725-7400-4881-af8f-fd232ca28234
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 73c70e347de00a18f4c4ec5ce3228d6d000d16f0
ms.sourcegitcommit: 3ec49252e82590de0fe559a8574606ae213f6f3b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97975454"
---
# <a name="plan-and-test-the-database-engine-upgrade-plan"></a>Planifier et tester le plan de mise à niveau du moteur de base de données

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
 Pour effectuer une mise à niveau réussie de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , quelle que soit l’approche, une planification appropriée est nécessaire.  
  
## <a name="release-notes-and-known-upgrade-issues"></a>Notes de publication et problèmes de mise à niveau connus  
 Avant d'effectuer la mise à niveau du [!INCLUDE[ssDE](../../includes/ssde-md.md)], consultez :

- [Notes de publication de SQL Server 2019](../../sql-server/sql-server-version-15-release-notes.md)
- [Notes de publication de SQL Server 2017](../../sql-server/sql-server-2017-release-notes.md) 
- [Notes de publication de SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md)
- [Compatibilité descendante du moteur de base de données SQL Server](../discontinued-database-engine-functionality-in-sql-server.md).  
  
## <a name="pre-upgrade-planning-checklist"></a>Liste de vérification pour la planification d’une mise à niveau  
 Avant la mise à niveau du [!INCLUDE[ssDE](../../includes/ssde-md.md)], consultez la liste de vérification suivante et les articles associés. Ces articles s’appliquent à toutes les mises à niveau, quelle que soit la méthode utilisée, et vous aident à déterminer la méthode de mise à niveau la plus appropriée : mise à niveau propagée, mise à niveau de nouvelle installation ou mise à niveau sur place. Par exemple, il se peut que vous ne soyez pas en mesure d’effectuer une mise à niveau sur place ou une mise à niveau propagée si vous mettez à niveau le système d’exploitation, mettez à niveau à partir de SQL Server 2005 ou mettez à niveau à partir d’une version 32 bits de SQL Server. Pour un arbre de décision, voir [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
-   **Configurations matérielle et logicielle requises :** consultez les configurations matérielle et logicielle requises pour l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous les trouverez à l’adresse suivante : [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). Une partie du cycle de planification de toute mise à niveau consiste à prendre en compte la mise à niveau matérielle (un matériel plus récent est plus rapide et peut réduire le travail de gestion des licences, en raison soit du moindre nombre de processeurs, soit de la consolidation de la base de données et du serveur) et la mise à niveau du système d’exploitation. Ces changements matériels et logiciels affectent le choix de la méthode de mise à niveau.  
  
-   **Environnement actuel :** examinez votre environnement actuel pour déterminer les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisés et les clients qui se connectent à votre environnement.  
  
    -   **Fournisseurs de clients :** bien que la mise à niveau ne nécessite pas de mise à jour du fournisseur pour chacun de vos clients, vous pouvez choisir de la faire. Si vous effectuez la mise à niveau de [!INCLUDE[sql14](../../includes/sssql14-md.md)] ou version antérieure, les fonctionnalités [!INCLUDE[sql15](../../includes/sssql15-md.md)] suivantes nécessitent un fournisseur mis à jour pour chaque client ou qu’un fournisseur mis à jour apporte des fonctionnalités supplémentaires :  
  
       -   [Always Encrypted &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
  
       -   [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
       -   [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
       -   Mise à jour de sécurité vers TLS  

   >[!NOTE]
   >La liste précédente s’applique également à [!INCLUDE[sscurrent](../../includes/sscurrent-md.md)].
  
-   **Composants tiers :** déterminez la compatibilité des composants tiers, comme la sauvegarde intégrée.  
  
-   **Environnement cible :** vérifiez que votre environnement cible présente les configurations matérielle et logicielle requises, et qu’il peut prendre en charge la configuration du système d’origine. Par exemple, la mise à niveau peut impliquer la consolidation de plusieurs instances SQL Server en une seule instance [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] nouvelle, ou la virtualisation de votre environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un cloud privé ou public.  
  
-   **Édition :** déterminez l’édition appropriée de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] pour votre mise à niveau et déterminez les chemins de mise à niveau valides. Pour plus d'informations, consultez [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md). Avant toute mise à niveau d'une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers une autre, vérifiez que la fonctionnalité en cours d'utilisation est prise en charge dans l'édition vers laquelle vous effectuez la mise à niveau.  
  
    > [!NOTE]  
    >  Quand vous procédez à une mise à niveau vers [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] à partir d’une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition, choisissez entre Enterprise Edition : licence par cœur et Enterprise Edition. Ces éditions Enterprise se différencient uniquement par leur mode de licences. Pour plus d’informations, voir [Compute Capacity Limits by Edition of SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
  
-   **Compatibilité descendante**  consultez l’article relatif à la compatibilité descendante du moteur de base de données [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] pour connaître les changements de comportement entre [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] et la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous mettez à niveau. Consultez [SQL Server Database Engine Backward Compatibility](../discontinued-database-engine-functionality-in-sql-server.md).  
  
-   **Assistant Migration de données :** exécutez-le pour diagnostiquer les problèmes susceptibles de bloquer le processus de mise à niveau ou qui nécessitent une modification d’applications ou de scripts existants en raison d’un changement cassant.
    Vous pouvez télécharger l’Assistant Migration de données [ici](https://aka.ms/get-dma).  
  
-   **Outil d’analyse de configuration système :** exécutez l’Outil d’analyse de configuration système (SCC) [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] pour déterminer si le programme d’installation de SQL Server détecte des problèmes bloquants avant la planification de la mise à niveau. Pour plus d'informations, consultez [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md).  
  
-   **Mise à niveau des tables à mémoire optimisée :** quand vous mettez à niveau une instance de base de données SQL Server 2014 contenant des tables à mémoire optimisée vers SQL Server 2016, le processus de mise à niveau nécessite plus de temps pour convertir les tables à mémoire optimisée au nouveau format sur disque (et la base de données est hors connexion pendant ces étapes).   Le temps nécessaire dépend de la taille des tables mémoire optimisées et de la vitesse du sous-système d’E/S. La mise à niveau nécessite trois tailles d’opérations de données pour les mises à niveau sur place et de nouvelle installation (l’étape 1 n’est pas nécessaire pour une mise à niveau propagée, contrairement aux étapes 2 et 3) :  
  
    1.  Exécuter la récupération de base de données à l’aide de l’ancien format sur disque (y compris le chargement dans la mémoire de toutes les données des tables optimisées en mémoire à partir du disque)  
  
    2.  Sérialisez les données sur le disque dans le nouveau format sur disque.  
  
    3.  Exécuter la récupération de base de données à l’aide du nouveau format (y compris le chargement dans la mémoire de toutes les données des tables optimisées en mémoire à partir du disque)  
  
     Par ailleurs, un espace insuffisant sur le disque pendant ce processus entraîne l’échec de la restauration. Vérifiez que l’espace est suffisant sur le disque pour stocker la base de données existante plus le stockage supplémentaire égal à la taille actuelle des conteneurs dans le groupe de fichiers MEMORY_OPTIMIZED_DATA de la base de données, afin d’effectuer une mise à niveau sur place ou quand vous attachez une base de données SQL Server 2014 à une instance de SQL Server 2016. Utilisez la requête suivante pour déterminer l’espace disque actuellement nécessaire pour le groupe de fichiers MEMORY_OPTIMIZED_DATA ainsi que la quantité d’espace disque disponible nécessaire pour la mise à niveau :  
  
    ```  
    select cast(sum(size) as float)*8/1024/1024 'size in GB'   
    from sys.database_files  
    where data_space_id in (select data_space_id from sys.filegroups where type=N'FX')  
    ```  
  
## <a name="develop-and-test-the-upgrade-plan"></a>Développer et tester le plan de mise à niveau  
 La meilleure approche consiste à traiter la mise à niveau comme tout projet informatique. Organisez une équipe de mise à niveau avec les compétences nécessaires pour la mise à niveau : administration de base de données, réseau, extraction, transformation, chargement et autres. L’équipe doit :  
  
-   **Choisir la méthode de mise à niveau :** consultez [Choisir une méthode de mise à niveau du moteur de base de données](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
-   **Développer un plan de restauration :** l’exécution de ce plan vous permet de restaurer votre environnement d’origine en cas de nécessité.  
  
-   **Déterminer les critères d’acceptation :** vérifiez que la mise à niveau a réussi avant de migrer les utilisateurs vers l’environnement mis à niveau.  
  
-   **Tester le plan de mise à niveau :** pour tester les performances à l’aide de votre charge de travail réelle, servez-vous de l’utilitaire Microsoft SQL Server Distributed Replay. Celui-ci peut utiliser plusieurs ordinateurs pour relire les données de trace, en simulant une charge de travail stratégique. En effectuant une nouvelle lecture sur un serveur de test avant et après une mise à niveau de SQL Server, vous pouvez mesurer les écarts de performances et rechercher toute incompatibilité entre votre application et la mise à niveau. Pour plus d’informations, consultez [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md) et [Options de ligne de commande de l’outil d’administration &#40;Distributed Replay Utility&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md).  
  
## <a name="next-steps"></a>Étapes suivantes  
[Mettre à niveau le moteur de base de données](../../database-engine/install-windows/upgrade-database-engine.md) 
  
## <a name="additional-resources"></a>Ressources supplémentaires 
[Guide pour la migration de base de données](https://aka.ms/datamigration)