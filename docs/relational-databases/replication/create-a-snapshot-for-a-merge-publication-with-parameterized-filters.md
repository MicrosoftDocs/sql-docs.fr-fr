---
title: Créer un instantané avec des filtres paramétrables (Fusion)
description: Découvrez comment créer un instantané pour une publication de fusion avec des filtres paramétrables.
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- parameterized filters [SQL Server replication], snapshots
- snapshots [SQL Server replication], parameterized filters and
- filters [SQL Server replication], parameterized
ms.assetid: 00dfb229-f1de-4d33-90b0-d7c99ab52dcb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 89a470964db8b0475a6c5d4f20a5007337645cb1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076151"
---
# <a name="create-a-snapshot-for-a-merge-publication-with-parameterized-filters"></a>Créer un instantané d'une publication de fusion avec des filtres paramétrés
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Cette rubrique explique comment créer un instantané pour une publication de fusion avec des filtres paramétrables dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  

Lorsque vous utilisez des filtres de lignes paramétrés dans les publications de fusion, la réplication initialise chaque abonnement avec un instantané en deux parties. Un instantané est d'abord créée. Il contient tous les objets nécessaires à la publication ainsi que le schéma des objets publiés mais pas les données. Ensuite, chaque abonnement est initialisé avec un instantané qui comprend les objets et le schéma provenant de l'instantané du schéma ainsi que les données appartenant à la partition de l'abonnement. Si plusieurs abonnements reçoivent une partition donnée (en d'autres termes, s'ils reçoivent les mêmes schéma et données), l'instantané de cette partition n'est créé qu'une seule fois ; plusieurs abonnements sont initialisés à l'aide du même instantané. Pour plus d'informations sur les filtres de lignes paramétrés, consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 Pour créer des instantanés de publication avec des filtres paramétrés, trois méthodes sont possibles :  
  
-   **Vous pouvez prégénérer des instantanés pour chaque partition.** Cette option vous permet de contrôler le moment où les instantanés sont générés.    
     Vous pouvez également choisir d'actualiser les instantanés selon une planification définie. Les nouveaux Abonnés à une partition dont vous avez créé un instantané reçoivent un instantané à jour.   
-   **Vous pouvez autoriser les Abonnés à demander la génération** et l’application d’un instantané lors de leur première synchronisation. Cette option permet aux nouveaux Abonnés de se synchroniser sans intervention d'un administrateur (l'Agent[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en cours d'exécution sur le serveur de publication pour que l'instantané puisse être généré).  
  
    > [!NOTE]  
    >  Si le filtrage d'un ou plusieurs articles de la publication donne des partitions qui ne se chevauchent pas et sont uniques pour chaque abonnement, les métadonnées sont nettoyées à chaque exécution de l'Agent de fusion. Cela signifie que l'instantané partitionné expire plus rapidement. Lorsque vous optez pour cette méthode, envisagez d'autoriser les Abonnées à initialiser la génération et la remise d'instantané. Pour plus d'informations sur les options de filtrage, consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
-   **Vous pouvez générer manuellement un instantané pour chaque Abonné avec l’Agent d’instantané**. L'Abonné doit alors fournir l'emplacement de l'instantané à l'Agent de fusion afin qu'il puisse extraire et appliquer l'instantané approprié.  
  
    > [!NOTE]  
    >  Cette option est prise en charge pour assurer la compatibilité descendante et n'autorise pas les partages d'instantanés FTP.  
  
 L'approche la plus souple est celle qui consiste à combiner les options d'instantané prégénéré et d'instantané demandé par l'Abonné : les instantanés sont prégénérés et actualisés à intervalles planifiés (généralement pendant les heures creuses), mais un Abonné peut générer son propre instantané en cas de création d'un abonnement nécessitant une nouvelle partition.  
  
 Prenons l'exemple de [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)], qui possède une force de vente nomade chargée d'approvisionner en stock différents magasins. Chaque commercial reçoit un abonnement basé sur son nom de connexion, lequel extrait les données relatives aux magasins dont le commercial est responsable. L'administrateur choisit de prégénérer des instantanés et de les actualiser tous les dimanches. De temps à autre, un nouvel utilisateur est ajouté au système et a besoin de données pour une partition dont il n'existe aucun instantané disponible. L'administrateur décide par ailleurs d'autoriser les instantanés initialisés par l'Abonné pour éviter que ce dernier se retrouve dans une situation où il est incapable de s'abonner à la publication car aucun instantané n'est disponible. Lorsque le nouvel Abonné se connecte pour la première fois, l'instantané est généré pour la partition spécifiée et appliqué à l'Abonné (l'Agent[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en cours d'exécution sur le serveur de publication pour que l'instantané puisse être généré).  
  
 Pour créer un instantané d'une publication avec des filtres paramétrés, consultez [Créer un instantané d'une publication de fusion avec des filtres paramétrés](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
## <a name="security-settings-for-the-snapshot-agent"></a>Paramètres de sécurité de l'Agent d'instantané  
 L'Agent d'instantané crée des instantanés de chaque partition. Pour les instantanés prégénérés et ceux demandés par un Abonné, l'Agent s'exécute et se connecte avec les informations d'identification spécifiées au moment de la création du travail de l'Agent d'instantané pour la publication (ce travail est créé par l'Assistant Nouvelle publication ou par **sp_addpublication_snapshot**). Pour modifier les informations d'identification, utilisez **sp_changedynamicsnapshot_job**. Pour plus d’informations, consultez [sp_changedynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedynamicsnapshot-job-transact-sql.md).  

  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Lors de la génération d'un instantané pour une publication de fusion à l'aide de filtres paramétrables, vous devez commencer par générer un instantané standard (schéma) qui contient l'ensemble des données publiées, ainsi que les métadonnées de l'Abonné pour l'abonnement. Pour plus d’informations, voir [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md). Après avoir créé l'instantané de schéma, vous pouvez générer l'instantané qui contient la partition des données publiées spécifique à l'Abonné.  
  
-   Si le filtrage d'un ou plusieurs articles de la publication donne des partitions qui ne se chevauchent pas et sont uniques pour chaque abonnement, les métadonnées sont nettoyées à chaque exécution de l'Agent de fusion. Cela signifie que l'instantané partitionné expire plus rapidement. Lorsque vous optez pour cette méthode, envisagez d'autoriser les Abonnées à initialiser la génération et la remise d'instantané. 
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Générez des instantanés pour des partitions dans la page **Partitions de données** de la boîte de dialogue **Propriétés de la publication\<Publication>** . Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md). Vous pouvez permettre à des Abonnés de lancer la génération et la livraison d'instantanés et/ou de générer des instantanés.  
  
 Pour générer des instantanés pour une ou plusieurs partitions, vous devez préalablement :  
  
1.  créer une publication de fusion à l'aide de l'Assistant Nouvelle publication et spécifier un ou plusieurs filtres de lignes paramétrables dans la page **Ajouter un filtre** de l'Assistant. Pour plus d'informations, voir [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
2.  Générez un instantané du schéma pour la publication. Par défaut, un instantané du schéma est généré lorsque vous terminez l'Assistant Nouvelle publication ; vous pouvez également générer un instantané du schéma à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

#### <a name="to-generate-a-schema-snapshot"></a>Pour générer un instantané du schéma  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis le dossier **Publications** .  
  
3.  Cliquez avec le bouton droit sur la réplication pour laquelle vous souhaitez créer un instantané, puis cliquez sur **Afficher l'état de l'Agent d'instantané**.  
  
4.  Dans la boîte de dialogue **Afficher l’état de l’Agent d’instantané - \<Publication>** , cliquez sur **Démarrer**.  
  
     Lorsque l'Agent d'instantané termine la génération de l'instantané, un message comme celui-ci s'affiche : « [100 %] Un instantané de 17 article(s) a été généré ».  
  
#### <a name="to-allow-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Pour permettre aux Abonnés d'initier la génération et la remise d'instantanés  
  
1.  Dans la page **Partitions de données** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez **Définir automatiquement une partition et générer un instantané si nécessaire lorsqu’un nouvel Abonné essaie de se synchroniser**.  
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-generate-and-refresh-snapshots"></a>Pour générer et actualiser des instantanés  
  
1.  Dans la page **Partitions de données** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **Ajouter**.  
  
2.  Entrez une valeur pour la valeur **HOST_NAME()** et/ou **SUSER_SNAME()** associée à la partition pour laquelle vous voulez créer un instantané.  
  
3.  En option, spécifiez une planification pour l'actualisation des instantanés :  
  
    1.  Sélectionnez **Planifier l'Agent d'instantané pour l'exécution de cette partition à l'heure ou aux heures suivantes**.  
  
    2.  Acceptez la planification par défaut pour l'actualisation des instantanés, ou cliquez sur **Modifier** pour spécifier une autre planification.  
  
4.  Cliquez sur **OK** pour revenir à la boîte de dialogue **Propriétés de la publication - \<Publication>** .  
  
5.  Sélectionnez la partition dans la grille de propriétés, puis cliquez sur **Générer les instantanés sélectionnés maintenant**.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les procédures stockées et l'Agent d'instantané permettent d'effectuer les tâches suivantes :  
  
-   Vous pouvez autoriser les Abonnés à demander la génération et l'application d'un instantané lors de leur première synchronisation.  
  
-   Vous pouvez prégénérer des instantanés pour chaque partition.  
  
-   générer manuellement un instantané pour chaque Abonné.  
  
    > [!IMPORTANT]  
    >  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
#### <a name="to-create-a-publication-that-allows-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Pour créer une publication qui permet aux Abonnés d'initialiser la génération et la remise d'instantanés  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Spécifiez les paramètres suivants :  
  
    -   Le nom de la publication pour **\@publication**.  
  
    -   La valeur **true** pour **\@allow_subscriber_initiated_snapshot**, qui permet aux Abonnés de lancer le processus d’instantané.  
  
    -   (Facultatif) Le nombre de processus d’instantané dynamique qui peuvent s’exécuter simultanément pour **\@max_concurrent_dynamic_snapshots**. Si le nombre maximal de processus en cours d'exécution est atteint et si un Abonné essaie de générer un instantané, le processus est placé en file d'attente. Par défaut, le nombre de processus simultanés est illimité.  
  
2.  Sur le serveur de publication, exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez le nom de publication utilisé à l’étape 1 pour **\@publication** et les informations d’identification Windows [!INCLUDE[msCoName](../../includes/msconame-md.md)] sous lesquelles [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md) s’exécute pour **\@job_login** et **\@password**. Si l’agent doit utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication, vous devez aussi spécifier la valeur **0** pour **\@publisher_security_mode** et les informations de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication. Pour plus d'informations sur la génération d'un instantané initial et la définition d'une planification personnalisée pour l'Agent d'instantané, consultez [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour ajouter des articles à la publication. Cette procédure stockée doit être exécutée une fois pour chaque article de la publication. Lorsque vous utilisez des filtres paramétrables, vous devez spécifier un filtre de lignes paramétrable pour un ou plusieurs articles à l’aide du paramètre **\@subset_filterclause**. Pour plus d'informations, voir [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Si d’autres articles doivent être filtrés sur la base du filtre de lignes paramétrable, exécutez [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) pour définir la relation de jointure ou d’enregistrements logiques entre les articles. Cette procédure stockée doit être exécutée une fois pour chaque relation en cours de définition. Pour plus d'informations, voir [Définir et modifier un filtre de jointure entre des articles de fusion](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  Lorsque l'Agent de fusion demande à l'instantané d'initialiser l'Abonné, l'instantané de la partition de l'abonnement demandeur est généré automatiquement.  
  
#### <a name="to-create-a-publication-and-pre-generate-or-automatically-refresh-snapshots"></a>Pour créer une publication et prégénérer ou actualiser automatiquement des instantanés  
  
1.  Exécutez [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) pour créer la publication. Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Sur le serveur de publication, exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez le nom de publication utilisé à l’étape 1 pour **\@publication** et les informations d’identification Windows sous lesquelles l’Agent d’instantané s’exécute pour **\@job_login** et **\@password**. Si l’agent doit utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication, vous devez aussi spécifier la valeur **0** pour **\@publisher_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication. Pour plus d'informations sur la génération d'un instantané initial et la définition d'une planification personnalisée pour l'Agent d'instantané, consultez [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour ajouter des articles à la publication. Cette procédure stockée doit être exécutée une fois pour chaque article de la publication. Lorsque vous utilisez des filtres paramétrables, vous devez spécifier un filtre de lignes paramétrable pour un seul article à l’aide du paramètre **\@subset_filterclause**. Pour plus d'informations, voir [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Si d’autres articles doivent être filtrés sur la base du filtre de lignes paramétrable, exécutez [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) pour définir la relation de jointure ou d’enregistrements logiques entre les articles. Cette procédure stockée doit être exécutée une fois pour chaque relation en cours de définition. Pour plus d'informations, voir [Définir et modifier un filtre de jointure entre des articles de fusion](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), en spécifiant la valeur de **\@publication** créée à l’étape 1. Notez la valeur du **snapshot_jobid** dans le jeu de résultats.  
  
6.  Convertissez la valeur du **snapshot_jobid** obtenue à l'étape 5 en **uniqueidentifier**.  
  
7.  Dans la base de données **msdb** sur le serveur de publication, exécutez [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md), en spécifiant la valeur convertie obtenue à l’étape 6 pour **\@job_id**.  
  
8.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md). Spécifiez le nom de la publication créée à l’étape 1 pour **\@publication** et la valeur utilisée afin de définir la partition pour **\@suser_sname** si [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) est utilisé dans la clause de filtre ou pour **\@host_name** si [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) est utilisé dans la clause de filtre.  
  
9. Dans la base de données de publication sur le serveur de publication, exécutez [sp_adddynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md). Spécifiez le nom de la publication définie à l’étape 1 pour **\@publication**, la valeur de **\@suser_sname** ou **\@host_name** définie à l’étape 8 et une planification pour le travail. Le travail qui génère l'instantané paramétrable pour la partition spécifiée est ainsi créé. Pour plus d’informations, consultez [Spécifier des planifications de synchronisation](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!NOTE]  
    >  Ce travail s'exécute à l'aide du même compte Windows que le travail d'instantané initial défini à l'étape 2. Pour supprimer le travail d’instantané paramétrable et sa partition de données associée, exécutez [sp_dropdynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md).  
  
10. Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md) en spécifiant la valeur de **\@publication** définie à l’étape 1 et la valeur de **\@suser_sname** ou **\@host_name** définie à l’étape 8. Notez la valeur du **dynamic_snapshot_jobid** dans le jeu de résultats.  
  
11. Dans la base de données **msdb** sur le serveur de distribution, exécutez [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md), en spécifiant la valeur obtenue à l’étape 9 pour **\@job_id**. Le travail d'instantané paramétrable de la partition est alors démarré.  
  
12. Répétez les étapes 8 à 11 pour générer un instantané partitionné pour chaque abonnement.  
  
#### <a name="to-create-a-publication-and-manually-create-snapshots-for-each-partition"></a>Pour créer une publication et créer manuellement des instantanés pour chaque partition  
  
1.  Exécutez [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) pour créer la publication. Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Sur le serveur de publication, exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez le nom de publication utilisé à l’étape 1 pour **\@publication** et les informations d’identification Windows sous lesquelles l’Agent d’instantané s’exécute pour **\@job_login** et **\@password**. Si l’agent doit utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication, vous devez aussi spécifier la valeur **0** pour **\@publisher_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication. Pour plus d'informations sur la génération d'un instantané initial et la définition d'une planification personnalisée pour l'Agent d'instantané, consultez [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour ajouter des articles à la publication. Cette procédure stockée doit être exécutée une fois pour chaque article de la publication. Lorsque vous utilisez des filtres paramétrables, vous devez spécifier un filtre de lignes paramétrable pour au moins un article à l’aide du paramètre **\@subset_filterclause**. Pour plus d'informations, voir [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Si d’autres articles doivent être filtrés sur la base du filtre de lignes paramétrable, exécutez [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) pour définir la relation de jointure ou d’enregistrements logiques entre les articles. Cette procédure stockée doit être exécutée une fois pour chaque relation en cours de définition. Pour plus d'informations, voir [Définir et modifier un filtre de jointure entre des articles de fusion](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  Démarrez le travail d'instantané ou exécutez l'Agent d'instantané des réplications à partir de l'invite de commandes pour générer le schéma d'instantané standard et d'autres fichiers. Pour plus d’informations, voir [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
6.  Exécutez de nouveau l'Agent d'instantané des réplications à partir de l'invite de commandes pour générer des fichiers de copie en bloc (.bcp), en spécifiant l'emplacement de l'instantané partitionné pour **- DynamicSnapshotLocation** et une des propriétés suivantes ou les deux qui définissent la partition :  
  
    -   **-DynamicFilterHostName** - valeur si [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) est utilisé.  
  
    -   **-DynamicFilterLogin** - valeur si [HOST_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) est utilisé.  
  
7.  Répétez l'étape 6 pour générer un instantané partitionné pour chaque abonnement.  
  
8.  Exécutez l'Agent de fusion pour chaque abonnement afin d'appliquer l'instantané partitionné initial aux Abonnés, en spécifiant les propriétés suivantes :  
  
    -   **-Hostname** - la valeur utilisée pour définir la partition si la valeur réelle de HOST_NAME est remplacée.  
  
    -   **- DynamicSnapshotLocation** - l'emplacement de l'instantané dynamique pour cette partition.  
  
> [!NOTE]  
>  Pour plus d’informations sur la programmation des agents de réplication, consultez [Concepts des exécutables de l’Agent de réplication](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple crée une publication de fusion avec des filtres paramétrables dans laquelle les Abonnés initialisent le processus de génération d'instantané. Les valeurs de **\@job_login** et **\@job_password** sont passées à l’aide de variables de script.  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_1.sql)]  
  
 Cet exemple crée une publication à l'aide d'un filtre paramétrable dans laquelle la partition de chaque Abonné est définie en exécutant [sp_addmergepartition](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md) et le travail d'instantané filtré est créé en exécutant [sp_adddynamicsnapshot_job](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md) et en passant les informations de partitionnement. Les valeurs de **\@job_login** et **\@job_password** sont passées à l’aide de variables de script.  
  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_2.sql)]  
  
 Cet exemple crée une publication à l'aide d'un filtre paramétrable dans laquelle la partition de données et le travail d'instantané filtré de chaque Abonné doivent être créés en fournissant les informations de partitionnement. Un Abonné fournit des informations de partitionnement à l'aide de paramètres de ligne de commande lors de l'exécution manuelle des agents de réplication. Cet exemple suppose qu'un abonnement à la publication a également été créé.  
  
 [!code-sql[HowTo#sp_MergeDynamicPubPartitionManual](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_3.sql)]  
  
```  
  
REM Line breaks are added to improve readability.   
REM In a batch file, commands must be made in a single line.  
REM Run the Snapshot agent from the command line to generate the standard snapshot   
REM schema and other files.   
SET DistPub=%computername%  
SET PubDB=AdventureWorks2012   
SET PubName=AdvWorksSalesPersonMerge  
  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %PubName%    
-Publisher %DistPub% -Distributor  %DistPub%  -PublisherDB %PubDB%  -ReplicationType 2    
-OutputVerboseLevel 1  -DistributorSecurityMode 1  
  
PAUSE  
  
```  
  
```  
  
REM Run the Snapshot agent from the command line, this time to generate   
REM the bulk copy (.bcp) data for each Subscriber partition.    
SET DistPub=%computername%  
SET PubDB=AdventureWorks2012   
SET PubName=AdvWorksSalesPersonMerge  
SET SnapshotDir=\\%DistPub%\repldata\unc\fernando  
  
MD %SnapshotDir%  
  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %PubName%    
-Publisher %DistPub%  -Distributor  %DistPub%  -PublisherDB %PubDB%  -ReplicationType 2    
-OutputVerboseLevel 1  -DistributorSecurityMode 1  -DynamicFilterHostName "adventure-works\Fernando"    
-DynamicSnapshotLocation %SnapshotDir%  
  
PAUSE  
  
```  
  
```  
  
REM Run the Merge Agent for each subscription to apply the partitioned   
REM snapshot for each Subscriber.    
SET Publisher = %computername%  
SET Subscriber = %computername%  
SET PubDB = AdventureWorks2012   
SET SubDB = AdventureWorks2012Replica   
SET PubName = AdvWorksSalesPersonMerge   
SET SnapshotDir=\\%DistPub%\repldata\unc\fernando  
  
"C:\Program Files\Microsoft SQL Server\120\COM\REPLMERG.EXE" -Publisher  %Publisher%    
-Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB %PubDB%    
-SubscriberDB %SubDB% -Publication %PubName%  -PublisherSecurityMode 1  -OutputVerboseLevel 3    
-Output -SubscriberSecurityMode 1  -SubscriptionType 3 -DistributorSecurityMode 1    
-Hostname "adventure-works\Fernando"  -DynamicSnapshotLocation %SnapshotDir%  
  
PAUSE  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Vous pouvez utiliser les Replication Management Objects pour générer par programme des instantanés partitionnés des manières suivantes :  
  
-   Vous pouvez autoriser les Abonnés à demander la génération et l'application d'un instantané lors de leur première synchronisation.  
  
-   Vous pouvez prégénérer des instantanés pour chaque partition.  
  
-   Vous pouvez générer manuellement un instantané pour chaque Abonné en exécutant l'Agent d'instantané.  
  
> [!NOTE]  
>  Lorsque le filtrage d’un article génère des partitions qui ne se chevauchent pas et qui sont uniques pour chaque abonnement (en spécifiant une valeur de F:Microsoft.SqlServer.Replication.PartitionOptions.NonOverlappingSingleSubscription pour P:Microsoft.SqlServer.Replication.MergeArticle.PartitionOption lors de la création d’un article de fusion), les métadonnées sont nettoyées à chaque exécution de l’Agent de fusion. Cela signifie que l'instantané partitionné expire plus rapidement. Lorsque vous optez pour cette méthode, envisagez d'autoriser les Abonnées à demander la génération d'instantanés. Pour plus d'informations, consultez la section Utilisation des options de filtrage appropriées, dans la rubrique [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification, utilisez les [Services de chiffrement](/previous-versions/aa719848(v=vs.71)) fournis par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-create-a-publication-that-allows-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Pour créer une publication qui permet aux Abonnés d'initialiser la génération et la remise d'instantanés  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> pour la base de données de publication, affectez à la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> l'instance de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1, puis appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . If <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> retourne **false**, confirmez que la base de données existe.  
  
3.  If <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> a la valeur **false**, affectez-lui la valeur **true** et appelez <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A>.  
  
4.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> et définissez les propriétés suivantes pour cet objet :  
  
    -   La classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   Le nom de la base de données publiée pour <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Le nom de la publication pour <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   Le nombre maximal de travaux d'instantané dynamique à exécuter pour <xref:Microsoft.SqlServer.Replication.MergePublication.MaxConcurrentDynamicSnapshots%2A>. Comme des demandes d'instantané initialisées par l'Abonné peuvent se produire à tout moment, cette propriété limite le nombre des travaux de l'Agent d'instantané qui peuvent s'exécuter simultanément lorsque plusieurs Abonnés demandent leur instantané partitionné au même moment. Lorsque le nombre maximal de travaux s'exécute, des demandes d'instantanés partitionnés supplémentaires sont mises en file d'attente jusqu'à ce que l'un des travaux en cours d'exécution se termine.  
  
    -   Utilisez l'opérateur OR logique de bits ( **|** dans Visual C# et **Or** dans Visual Basic) pour ajouter la valeur <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowSubscriberInitiatedSnapshot> à <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A>.  
  
    -   Les champs <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> pour fournir les informations d'identification pour le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel le travail de l'Agent d'instantané s'exécute.  
  
        > [!NOTE]  
        >  La définition de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> est recommandée lorsque la publication est créée par un membre du rôle serveur fixe **sysadmin** . Pour plus d’informations, voir [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> pour créer la publication.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour toutes les propriétés, y compris <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, sont envoyées sous forme de texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'appeler la méthode <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> . Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
6.  Utilisez la propriété <xref:Microsoft.SqlServer.Replication.MergeArticle> pour ajouter des articles à la publication. Spécifiez la propriété <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> pour au moins un article qui définit le filtre paramétrable. (Facultatif) Créez des objets <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> qui définissent des filtres de jointure entre les articles. Pour plus d’informations, consultez [définir un Article](../../relational-databases/replication/publish/define-an-article.md).  
  
7.  Si <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> a la valeur **false**, appelez <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> pour créer le travail de l'Agent d'instantané initial pour cette publication.  
  
8.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> de l'objet <xref:Microsoft.SqlServer.Replication.MergePublication> créé à l'étape 4. Cela démarre le travail de l'agent qui génère l'instantané initial. Pour plus d'informations sur la génération d'un instantané initial et la définition d'une planification personnalisée pour l'Agent d'instantané, consultez [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
9. (Facultatif) Cherchez une valeur **true** pour la propriété <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> afin de déterminer quand l'instantané initial est prêt à être utilisé.  
  
10. Lorsque l'Agent de fusion pour un Abonné se connecte pour la première fois, un instantané partitionné est généré automatiquement.  
  
#### <a name="to-create-a-publication-and-pregenerate-or-automatically-refresh-snapshots"></a>Pour créer une publication et prégénérer ou actualiser automatiquement des instantanés  
  
1.  Utilisez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> pour définir une publication de fusion. Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Utilisez la propriété <xref:Microsoft.SqlServer.Replication.MergeArticle> pour ajouter des articles à la publication. Spécifiez la propriété <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> pour au moins un article qui définit le filtre paramétrable et créez tous les objets <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> qui définissent les filtres de jointure entre les articles. Pour plus d’informations, consultez [définir un Article](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Si <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> a la valeur **false**, appelez <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> pour créer le travail de l'Agent d'instantané pour cette publication.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> de l'objet <xref:Microsoft.SqlServer.Replication.MergePublication> créé à l'étape 1. Cette méthode démarre le travail de l'agent qui génère l'instantané initial. Pour plus d'informations sur la génération d'un instantané initial et la définition d'une planification personnalisée pour l'Agent d'instantané, consultez [Créer et appliquer l'instantané initial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
5.  Cherchez une valeur **true** pour la propriété <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> afin de déterminer quand l'instantané initial est prêt à être utilisé.  
  
6.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePartition> et définissez les critères de filtrage paramétrable pour l'Abonné en utilisant une des propriétés suivantes, ou les deux :  
  
    -   Si la partition de l’Abonné est définie par le résultat de [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md), utilisez <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterLogin%2A>.  
  
    -   Si la partition de l’Abonné est définie par le résultat de [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) ou une surcharge de cette fonction, utilisez <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterHostName%2A>.  
  
7.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> et définissez la même propriété qu'à l'étape 6.  
  
8.  Utilisez la classe <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule> pour définir une planification pour générer l'instantané filtré pour la partition de l'Abonné.  
  
9. Utilisez l'instance de <xref:Microsoft.SqlServer.Replication.MergePublication> de l'étape 1 pour appeler <xref:Microsoft.SqlServer.Replication.MergePublication.AddMergePartition%2A>. Passez l'objet <xref:Microsoft.SqlServer.Replication.MergePartition> créé à l'étape 6.  
  
10. Utilisez l'instance de <xref:Microsoft.SqlServer.Replication.MergePublication> de l'étape 1 pour appeler la méthode <xref:Microsoft.SqlServer.Replication.MergePublication.AddMergeDynamicSnapshotJob%2A> . Passez l'objet <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> créé à l'étape 7 et l'objet <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule> créé à l'étape 8.  
  
11. Appelez <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergeDynamicSnapshotJobs%2A>et localisez l'objet <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> correspondant au travail d'instantané partitionné nouvellement ajouté dans le tableau retourné.  
  
12. Obtenez la propriété <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob.Name%2A> pour le travail.  
  
13. Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
14. Créez une instance de la classe <xref:Microsoft.SqlServer.Management.Smo.Server> SMO (SQL Server Management Objects), en passant l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 13.  
  
15. Créez une instance de la classe <xref:Microsoft.SqlServer.Management.Smo.Agent.Job> , en passant la propriété <xref:Microsoft.SqlServer.Management.Smo.Server.JobServer%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Server> créé à l'étape 14 et le nom du travail obtenu à l'étape 12.  
  
16. Appelez la méthode <xref:Microsoft.SqlServer.Management.Smo.Agent.Job.Start%2A> pour démarrer le travail d'instantané partitionné.  
  
17. Répétez les étapes 6 à 16 pour chaque Abonné.  
  
#### <a name="to-create-a-publication-and-manually-create-snapshots-for-each-partition"></a>Pour créer une publication et créer manuellement des instantanés pour chaque partition  
  
1.  Utilisez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> pour définir une publication de fusion. Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Utilisez la propriété <xref:Microsoft.SqlServer.Replication.MergeArticle> pour ajouter des articles à la publication. Spécifiez la propriété <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> pour au moins un article qui définit le filtre paramétrable et créez tous les objets <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> qui définissent les filtres de jointure entre les articles. Pour plus d’informations, consultez [définir un Article](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Générez l'instantané initial. Pour plus d’informations, voir [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
4.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> et définissez les propriétés requises suivantes :  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> – nom du serveur de publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> – nom de la base de données de publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> – nom de la publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> – nom du serveur de distribution  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> – valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> pour utiliser l'authentification intégrée de Windows ou valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> pour utiliser l'authentification SQL Server.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> – valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> pour utiliser l'authentification intégrée de Windows ou valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> pour utiliser l'authentification SQL Server.  
  
5.  Spécifiez la valeur <xref:Microsoft.SqlServer.Replication.ReplicationType.Merge> pour <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
6.  Définissez une ou plusieurs des propriétés suivantes pour définir les paramètres de partitionnement :  
  
    -   Si la partition de l’Abonné est définie par le résultat de [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md), utilisez <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DynamicFilterLogin%2A>.  
  
    -   Si la partition de l’Abonné est définie par le résultat de [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) ou une surcharge de cette fonction, utilisez <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DynamicFilterHostName%2A>.  
  
7.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
8.  Répétez les étapes 4 à 7 pour chaque Abonné.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple crée une publication de fusion qui permet aux Abonnés de demander la génération d'instantanés.  
  
 [!code-cs[HowTo#rmo_CreateMergePub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepub)]  
  
 Cet exemple crée manuellement la partition de l'Abonné et l'instantané filtré pour une publication de fusion avec des filtres de lignes paramétrables.  
  
 [!code-cs[HowTo#rmo_CreateMergePartition](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepartition)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePartition](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepartition)]  
  
 Cet exemple lance manuellement l'Agent d'instantané pour qu'il génère l'instantané des données filtrées pour un Abonné à une publication de fusion avec des filtres de lignes paramétrables.  
  
 [!code-cs[HowTo#rmo_GenerateFilteredSnapshot](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatefilteredsnapshot)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateFilteredSnapshot](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatefilteredsnapshot)]  
  
## <a name="see-also"></a>Voir aussi  
 [Filtres de lignes paramétrés](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Concepts liés aux procédures stockées système de réplication](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
