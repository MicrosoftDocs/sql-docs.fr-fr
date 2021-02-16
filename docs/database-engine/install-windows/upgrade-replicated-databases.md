---
title: Mettre à niveau ou corriger des bases de données répliquées | Microsoft Docs
description: SQL Server prend en charge la mise à niveau des bases de données répliquées à partir de versions précédentes de SQL Server sans arrêter l’activité sur d’autres nœuds.
ms.custom: ''
ms.date: 07/24/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- merge replication database upgrades [SQL Server replication]
- replication [SQL Server], upgrading
- transactional replication, upgrading databases
- snapshot replication [SQL Server], upgrading databases
- upgrading replicated databases
ms.assetid: 9926a4f7-bcd8-4b9b-9dcf-5426a5857116
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 72b1d22d14cf83d44743d2081ee051bd02faf342
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351027"
---
# <a name="upgrade-or-patch-replicated-databases"></a>Mettre à niveau ou corriger des bases de données répliquées

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] prend en charge la mise à niveau des bases de données répliquées à partir des versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; il n'est pas nécessaire d'interrompre l'activité des autres nœuds lorsqu'un nœud est en cours de mise à niveau. Prenez soin de respecter les règles relatives aux versions qui sont prises en charge dans une topologie :  
  
-   Toute version convient pour le serveur de distribution dès lors qu'elle est égale ou supérieure à celle du serveur de publication (en général, l'instance du serveur de distribution est la même que celle du serveur de publication).    
-   Toute version convient pour le serveur de publication dès lors qu'elle est inférieure ou égale à celle du serveur de distribution.    
-   La version de l'Abonné dépend du type de publication :    
    - La version d'un Abonné à une publication transactionnelle peut être n'importe laquelle des deux versions du serveur de publication. Par exemple : un éditeur SQL Server 2012 (11.x) peut avoir des abonnés SQL Server 2014 (12.x) et SQL Server 2016 (13.x), et un éditeur SQL Server 2016 (13.x) peut avoir des abonnés SQL Server 2014 (12.x) et SQL Server 2012 (11.x).     
    - Un abonné à une publication de fusion peut avoir toute version égale ou inférieure à la version de l’éditeur qui est prise en charge selon le cycle de prise en charge du cycle de vie des versions.  
 
Le chemin de mise à niveau vers SQL Server dépend du modèle de déploiement. SQL Server offre deux chemins de mise à niveau en général :
- Côte à côte Déployez un environnement parallèle et déplacez les bases de données ainsi que les objets de niveau instance associés, tels que les connexions et les travaux, vers le nouvel environnement. 
- Mise à niveau sur place : Autorisez le support d’installation de SQL Server à mettre à niveau l’installation de SQL Server existante en remplaçant les bits de SQL Server et en mettant à niveau les objets de base de données. Pour les environnements exécutant des groupes de disponibilité Always On ou des instances de cluster de basculement, une mise à niveau sur place est combinée avec une [mise à niveau propagée](choose-a-database-engine-upgrade-method.md#rolling-upgrade) pour réduire les temps d’arrêt. 

Une approche courante qui a été adoptée pour les mises à niveau côte à côte de topologies de réplication consiste à déplacer des paires éditeur/abonné par lots vers le nouvel environnement côte à côte, au lieu de procéder à un déplacement de la topologie complète. Cette approche par phases aide à maîtriser les temps d’arrêt et, dans une certaine mesure, à réduire l’impact sur les activités qui dépendent de la réplication.  

La majeure partie de cet article est axée sur la mise à niveau de la version de SQL Server. Toutefois, le processus de mise à niveau sur place doit également être appliqué lors de la mise à jour corrective de SQL Server avec un Service Pack ou une mise à jour cumulative. 

 >[!WARNING]
 > La mise à niveau d’une topologie de réplication est un processus en plusieurs étapes. Nous vous recommandons d’essayer une mise à niveau d’un réplica de votre topologie de réplication dans un environnement de test avant d’exécuter la mise à niveau sur l’environnement de production réel. Cette opération permet d’apporter les dernières touches à toute documentation opérationnelle nécessaire pour gérer la mise à niveau de manière fluide sans entraîner de temps d’arrêt longs et coûteux pendant le processus de mise à niveau réel. Certains de nos clients ont réduit les temps d’arrêt considérablement avec l’utilisation de groupes de disponibilité Always On et/ou d’instances de cluster de basculement SQL Server pour leurs environnements de production pendant la mise à niveau de leur topologie de réplication. En outre, nous vous recommandons, avant de tenter la mise à niveau, d’effectuer des sauvegardes de toutes les bases de données, notamment des bases de données MSDB, Master et de distribution ainsi que des bases de données utilisateur participant à la réplication.


## <a name="replication-matrix"></a>Matrice de réplication

[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]
  
## <a name="run-the-log-reader-agent-for-transactional-replication-before-upgrade"></a>Exécuter l'Agent de lecture du journal pour la réplication transactionnelle avant la mise à niveau  
 Avant d’effectuer la mise à niveau de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)], vous devez vérifier que toutes les transactions validées des tables publiées ont été traitées par l’Agent de lecture du journal. Pour vous assurer que toutes les transactions ont été traitées, effectuez les étapes suivantes pour chaque base de données qui contient des publications transactionnelles :  
  
1.  Assurez-vous que l'Agent de lecture du journal s'exécute pour la base de données. Par défaut, cet agent s'exécute en permanence.    
2.  Arrêtez l'activité des utilisateurs sur les tables publiées.  
3.  Laissez à l'Agent de lecture du journal le temps de copier des transactions vers la base de données de distribution, puis arrêtez-le.  
4.  Exécutez [sp_replcmds](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) pour vérifier que toutes les transactions ont été traitées. Le jeu de résultats de cette procédure doit être vide.   
5.  Exécutez [sp_replflush](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md) pour fermer la connexion à partir de sp_replcmds.   
6.  Effectuez la mise à niveau du serveur vers la version la plus récente de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].   
7.  Redémarrez l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l'Agent de lecture du journal s'ils ne démarrent pas automatiquement après la mise à niveau.  
  
## <a name="run-agents-for-merge-replication-after-upgrade"></a>Exécution des agents de réplication de fusion après la mise à niveau  
 Après la mise à niveau, exécutez l'Agent d'instantané pour chaque publication de fusion et l'Agent de fusion pour chaque abonnement afin de mettre à jour les métadonnées de réplication. Vous n'avez pas à appliquer le nouvel instantané, car il n'est pas nécessaire pour réinitialiser les abonnements. Les métadonnées d'abonnement sont mises à jour lors de la première exécution de l'Agent de fusion après la mise à niveau. Ceci signifie que la base de données d'abonnement peut rester en ligne et active durant la mise à niveau du serveur de publication.  
  
 La réplication de fusion stocke les métadonnées de publication et d'abonnement dans un certain nombre de tables système des bases de données de publication et d'abonnement. L'Agent d'instantané met à jour les métadonnées de publication et l'Agent de fusion met à jour les métadonnées d'abonnement. Il faut uniquement générer un instantané de publication. Si une publication de fusion utilise des filtres paramétrés, chaque partition a également un instantané. Il n'est pas nécessaire de mettre à jour ces instantanés partitionnés.  
  
 Exécutez les agents à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], du moniteur de réplication ou de la ligne de commande. Pour plus d’informations sur l’exécution de l’Agent d’instantané, consultez les articles suivants :  
  
-   [Créer et appliquer l'instantané initial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)
-   [Créer et appliquer l'instantané initial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Concepts des exécutables de l'agent de réplication](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  

Pour plus d’informations sur l’exécution de l’Agent de fusion, consultez les articles suivants :
-   [Synchroniser un abonnement par extraction (pull)](../../relational-databases/replication/synchronize-a-pull-subscription.md)
-   [Synchroniser un abonnement par émission (push)](../../relational-databases/replication/synchronize-a-push-subscription.md)  
  

Après avoir mis à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans une topologie qui utilise la réplication de fusion, modifiez le niveau de compatibilité de toutes les publications si vous voulez utiliser les nouvelles fonctionnalités.  
  
## <a name="upgrading-to-standard-workgroup-or-express-editions"></a>Mise à niveau vers les éditions Standard, Workgroup ou Express  
Avant la mise à niveau d’une édition de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] vers une autre, vérifiez que les fonctionnalités que vous utilisez actuellement sont prises en charge dans l’édition vers laquelle vous mettez à niveau. Pour plus d’informations, consultez la section sur la réplication dans [Fonctionnalités prises en charge par les éditions de SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md).  

## <a name="steps-to-upgrade-a-replication-topology"></a>Étapes de la mise à niveau d’une topologie de réplication
Ces étapes décrivent l’ordre dans lequel les serveurs d’une topologie de réplication doivent être mis à niveau. Les mêmes étapes s’appliquent si vous exécutez une réplication transactionnelle ou de fusion. Toutefois, ces étapes ne couvrent pas la réplication d’égal à égal, les abonnements de mise à jour en attente, ni les abonnements avec mise à jour immédiate. 

### <a name="in-place-upgrade"></a>Mise à niveau sur place 
1. Mettez à niveau le distributeur. 
2. Mettez à niveau l’éditeur et l’abonné. Ces derniers peuvent être mis à niveau dans n’importe quel ordre. 

 >[!NOTE]
 > Pour SQL 2008 et 2008 R2, la mise à niveau de l’éditeur et de l’abonné doit être effectuée en même temps pour se conformer à la matrice de la topologie de réplication. Un abonné ou éditeur SQL 2008 ou 2008 R2 ne peut pas avoir un abonné ou éditeur SQL 2016 (ou version ultérieure). Si la mise à niveau simultanée est impossible, utilisez une mise à niveau intermédiaire pour mettre à niveau les instances SQL vers SQL 2014, puis vers SQL 2016 (ou version ultérieure).  

### <a name="side-by-side-upgrade"></a>Mise à niveau côte à côte
1. Mettez à niveau le distributeur.
1. Reconfigurez la [distribution](../../relational-databases/replication/configure-distribution.md) sur la nouvelle instance SQL Server.
1. Mettez à niveau l’éditeur.
1. Mettez à niveau l’abonné.
1. Reconfigurez toutes les paires éditeur/abonné, y compris la réinitialisation de l’abonné. 


## <a name="steps-for-side-by-side-migration-of-the-distributor-to-windows-server-2012-r2"></a>Étapes de la migration côte à côte du distributeur vers Windows Server 2012 R2
Si vous envisagez de mettre à niveau votre instance SQL Server vers SQL Server 2016 (ou version ultérieure) et que votre système d’exploitation actuel est Windows 2008 (ou 2008 R2), vous devez effectuer une mise à niveau côte à côte du système d’exploitation vers Windows Server R2 ou version ultérieure. La raison de cette mise à niveau intermédiaire du système d’exploitation est la suivante : SQL Server 2016 ne peut pas être installé sur un système Windows Server 2008/2008 R2 et Windows Server 2008/20008 R2 n’autorise pas les mises à niveau sur place directement vers Windows Server 2016. Bien qu’il soit possible d’effectuer une mise à niveau sur place de Windows Server 2008/2008 R2 vers Windows Server 2012, puis vers Windows Server 2016, cette opération est généralement déconseillée en raison du temps d’arrêt et de la complexité supplémentaire qui empêche un chemin de restauration simple. Une mise à niveau côte à côte est le seul chemin de mise à niveau disponible pour les instances de SQL Server qui participent à un cluster de basculement.  Les étapes suivantes peuvent être effectuées sur une instance SQL Server autonome ou de cluster de basculement Always On.

1. Configurez une nouvelle édition, version et instance de SQL Server (autonome ou de cluster de basculement Always On), en tant que distributeur sur Windows Server 2012 R2/2016, avec un nom d’instance de cluster de basculement SQL Server ou de cluster Windows différent ou un nom d’hôte autonome. Vous devez conserver la même structure de répertoire que l’ancien distributeur afin que les fichiers exécutables des agents de réplication, les dossiers de réplication et les chemins de fichier de base de données se trouvent dans le même chemin sur le nouvel environnement. Les étapes éventuelles à effectuer après la migration/mise à jour s’en trouvent réduites.
1. Vérifiez que la réplication est synchronisée, puis arrêtez tous les agents de réplication. 
1. Arrêtez l’instance du distributeur SQL Server actuelle. S’il s’agit d’une instance autonome, arrêtez le serveur. S’il s’agit d’une instance de cluster de basculement SQL, mettez hors connexion la totalité du rôle SQL Server dans le gestionnaire de cluster, y compris le nom réseau. 
1. Supprimez les entrées d’objet d’ordinateur DNS et AD pour l’ancien environnement (instance du distributeur actuelle). 
1. Changez le nom d’hôte du nouveau serveur afin qu’il corresponde à celui de l’ancien serveur.
    1. S’il s’agit d’une instance de cluster de basculement SQL, attribuez à la nouvelle instance de cluster de basculement SQL le nom de serveur virtuel de l’ancienne instance. 
1. Copiez les fichiers de base de données à partir de l’instance précédente à l’aide d’une redirection SAN, d’une copie de stockage ou d’une copie de fichiers. 
1. Mettez la nouvelle instance SQL Server en ligne. 
1. Redémarrez tous les agents de réplication et vérifiez s’ils s’exécutent correctement.
1. Déterminez si la réplication fonctionne comme prévu. 
1. Utilisez le support d’installation de SQL Server pour exécuter une mise à niveau sur place de votre instance SQL Server vers la nouvelle version de SQL Server. 


  >[!NOTE]
  > Pour réduire les temps d’arrêt, nous vous recommandons d’effectuer la *migration côte à côte* du distributeur et la *mise à niveau sur place vers SQL Server 2016* en tant qu’activités distinctes. Ainsi, vous pouvez adopter une approche progressive, réduire les risques et limiter les temps d’arrêt.

## <a name="web-synchronization-for-merge-replication"></a>Synchronisation Web pour la réplication de fusion  
 L'option de synchronisation Web pour la réplication de fusion requiert la copie de l'Écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (replisapi.dll) dans le répertoire virtuel sur le serveur IIS (Internet Information Services) utilisé pour la synchronisation. Lorsque vous configurez la synchronisation Web, le fichier est copié dans le répertoire virtuel par l'Assistant Configuration de la synchronisation Web. Si vous mettez à niveau les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installés sur le serveur IIS, vous devez manuellement copier replisapi.dll depuis le répertoire COM vers le répertoire virtuel sur le serveur IIS. Pour plus d’informations sur la synchronisation web, consultez [Configuration de la synchronisation web](../../relational-databases/replication/configure-web-synchronization.md).  
  
## <a name="restoring-a-replicated-database-from-an-earlier-version"></a>Restauration d'une base de données répliquée à partir d'une version antérieure  
 Pour vous assurer que les paramètres de réplication sont conservés lorsque vous restaurez la sauvegarde d'une base de données répliquée à partir d'une version précédente : effectuez la restauration vers un serveur et une base de données du même nom que le serveur et la base de données à l'origine de la sauvegarde.  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication SQL Server](../../relational-databases/replication/sql-server-replication.md)  
 [FAQ sur l’administration de la réplication](../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)   
 [Compatibilité descendante de la réplication](../../relational-databases/replication/replication-backward-compatibility.md)   
 [Mises à niveau de la version et de l'édition prises en charge](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [Mise à niveau vers SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 [Mise à niveau d’une topologie de réplication vers SQL Server 2016](/archive/blogs/sql_server_team/upgrading-a-replication-topology-to-sql-server-2016)