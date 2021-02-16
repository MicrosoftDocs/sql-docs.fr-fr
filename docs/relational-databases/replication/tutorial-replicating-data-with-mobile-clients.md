---
title: 'Tutoriel : Configurer la réplication de fusion'
description: Ce tutoriel vous apprend à configurer la réplication de fusion entre un serveur SQL Server et un client mobile.
ms.custom: seo-lt-2019
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: af673514-30c7-403a-9d18-d01e1a095115
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3f29e39d8b66b5adafb4c84de6a8f1df99ce95bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348612"
---
# <a name="tutorial-configure-replication-between-a-server-and-mobile-clients-merge"></a>Tutoriel : configurer la réplication entre un serveur et des clients mobiles (fusion)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
La réplication de fusion constitue une bonne solution au problème de transfert des données entre un serveur central et des clients mobiles qui ne sont connectés que de façon occasionnelle. Grâce aux Assistants de réplication, vous pouvez aisément configurer et administrer une topologie de réplication de fusion. 

Ce didacticiel vous explique comment configurer une topologie de réplication pour des clients mobiles. Pour plus d’informations sur la réplication de fusion, consultez la [présentation de la réplication de fusion](./merge/merge-replication.md).
  
## <a name="what-you-will-learn"></a>Contenu du didacticiel  
Dans ce tutoriel, vous allez découvrir comment utiliser la réplication de fusion pour publier des données issues d’une base de données centrale dans un ou plusieurs comptes d’utilisateurs mobiles, afin que chaque utilisateur obtienne le sous-ensemble de données filtré dont il a besoin. 

Dans ce didacticiel, vous apprendrez à :
> [!div class="checklist"]
> * Configurer un serveur de publication pour la réplication de fusion.
> * Ajouter un abonné mobile à la publication de fusion.
> * Synchroniser l’abonnement avec la publication de fusion.
  
## <a name="prerequisites"></a>Prérequis  
Ce tutoriel est destiné aux utilisateurs qui sont familiers des opérations essentielles de base de données, mais pas de la réplication. Avant de commencer ce tutoriel, vous devez effectuer le [Tutoriel : Préparer SQL Server pour la réplication](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Pour suivre ce tutoriel, vous avez besoin de SQL Server, SQL Server Management Studio (SSMS) et une base de données AdventureWorks : 
  
- Sur le serveur de publication (source), installez :  
  
   - N’importe quelle édition de SQL Server, à l’exception de SQL Server Express et SQL Server Compact. Ces éditions ne peuvent pas constituer un serveur de publication de réplication.   
   - Exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Pour des raisons de sécurité, les exemples de bases de données ne sont pas installés par défaut.  
  
- Sur le serveur d’abonné (destination), installez une édition quelconque de SQL Server, sauf SQL Server Express ou SQL Server Compact. La publication créée dans ce tutoriel ne prend pas en charge SQL Server Express ou SQL Server Compact. 

- Installez [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Téléchargez l’[exemple de base de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Pour obtenir des instructions sur la restauration d’une base de données dans SSMS, consultez [Restauration d’une base de données](../backup-restore/restore-a-database-backup-using-ssms.md).  
 
  
>[!NOTE]
> - La réplication n’est pas prise en charge sur les instances de SQL Server qui sont séparées de plus de deux versions. Pour plus d’informations, consultez [Supported SQL Server Versions in Replication Topology](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/) (Versions de SQL prises en charge dans la topologie de réplication).
> - Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous devez vous connecter au serveur de publication et à l’abonné à l’aide d’un identifiant de connexion membre du rôle serveur fixe **sysadmin**. Pour plus d’informations sur ce rôle, consultez [Rôles de niveau serveur](../security/authentication-access/server-level-roles.md).  
  
  
**Durée estimée pour terminer ce didacticiel : 60 minutes**  
  
## <a name="configure-a-publisher-for-merge-replication"></a>Configurer un serveur de publication pour la réplication de fusion
Dans cette leçon, vous allez créer une publication de fusion à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour publier un sous-ensemble des tables **Employee**, **SalesOrderHeader** et **SalesOrderDetail** dans l’exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Ces tables sont filtrées avec des filtres de lignes paramétrables pour que chaque abonnement contienne une partition unique des données. Vous ajouterez également la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée par l’Agent de fusion à la liste d’accès à la publication.  
  
### <a name="create-merge-publication-and-define-articles"></a>Créer une publication de fusion et définir des articles  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2. Pour démarrer SQL Server Agent, cliquez dessus avec le bouton droit dans l’Explorateur d’objets, puis sélectionnez **Démarrer**. Si cette étape ne démarre pas l’agent, vous devez le démarrer manuellement à partir du Gestionnaire de configuration SQL Server.  
3. Développez le dossier **Réplication**, cliquez avec le bouton droit sur **Publications locales**, puis sélectionnez **Nouvelle publication**. L’Assistant Nouvelle publication démarre :  

   ![Sélections pour démarrer l’Assistant Nouvelle Publication](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
3. Dans la page **Base de données de publication**, sélectionnez [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], puis **Suivant**. 

      
4. Dans la page **Type de publication**, sélectionnez **Publication de fusion**, puis **Suivant**.  
   
5. Dans la page **Types d’abonnés**, vérifiez que seul [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou version ultérieure est sélectionné, puis sélectionnez **Suivant** : 

    ![Pages « Type de publication » et « Types d’abonnés »](media/tutorial-replicating-data-with-mobile-clients/mergerpl.png)
  
   
6. Dans la page **Articles**, développez le nœud **Tables**. Sélectionnez les trois tables suivantes : **Employee**, **SalesOrderHeader** et **SalesOrderDetail**. Sélectionnez **Suivant**.  

   ![Sélections de table dans la page « articles »](media/tutorial-replicating-data-with-mobile-clients/mergearticles.png)

   >[!NOTE]
   > La table **Employee** contient une colonne (**OrganizationNode**) avec le type de données **hierarchyid**. Ce type de données est pris en charge pour la réplication uniquement dans SQL Server 2017. 
   >
   > Si vous utilisez une build antérieure à SQL Server 2017, un message s’affiche au bas de l’écran pour vous signaler que l’utilisation de cette colonne dans une réplication bidirectionnelle peut entraîner une perte de données. Pour les besoins de ce tutoriel, vous pouvez ignorer ce message. Toutefois, vous ne devez pas répliquer ce type de données dans un environnement de production, sauf si vous utilisez la build prise en charge.
   > 
   > Pour plus d’informations sur la réplication du type de données **hierarchyid**, consultez [Utilisation de colonnes hierarchyid dans les tables répliquées](../../t-sql/data-types/hierarchyid-data-type-method-reference.md#using-hierarchyid-columns-in-replicated-tables).
    
  
7. Dans la page **Filtrer les lignes de la table**, sélectionnez **Ajouter**, puis **Ajouter un filtre**.  
  
8. Dans la boîte de dialogue **Ajouter un filtre**, sélectionnez **Employee (HumanResources)** sous **Select the table to filter** (Sélectionnez la table à filtrer). Sélectionnez la colonne **LoginID**, sélectionnez la flèche droite pour ajouter la colonne à la clause WHERE de la requête de filtre, puis modifiez la clause WHERE comme illustré ci-après :  
  
   ```sql 
    WHERE [LoginID] = HOST_NAME()  
   ```  
  
   Sélectionnez **Une ligne de cette table ira à un seul abonnement**, puis **OK**.  
 
   ![Sélections pour l’ajout d’un filtre](media/tutorial-replicating-data-with-mobile-clients/mergeaddfilter.png)

    
  
10. Dans la page **Filtrer les lignes de la table**, sélectionnez **Employee (HumanResources)** . Ensuite, sélectionnez **Ajouter**, puis **Ajouter une jointure pour étendre le filtre sélectionné**.  
  
    a. Dans la boîte de dialogue **Ajouter une jointure**, sélectionnez **Sales.SalesOrderHeader** sous **Table jointe**. Sélectionnez **Write the join statement manually** (Écrire l’instruction de jointure manuellement), puis finalisez l’instruction de jointure de la façon suivante :  
  
    ```sql  
    ON [Employee].[BusinessEntityID] =  [SalesOrderHeader].[SalesPersonID] 
    ```  
  
    b. Dans **Spécifiez les options de jointure**, sélectionnez **Clé unique**, puis sélectionnez **OK**.

    ![Sélections pour l’ajout d’une jointure au filtre](media/tutorial-replicating-data-with-mobile-clients/mergeaddjoin.png)

  
13. Dans la page **Filtrer les lignes de la table**, sélectionnez **SalesOrderHeader**. Ensuite, sélectionnez **Ajouter**, puis **Ajouter une jointure pour étendre le filtre sélectionné**.  
  
    a. Dans la boîte de dialogue **Ajouter une jointure** , sélectionnez **Sales.SalesOrderDetail** sous **Table jointe**.    
    b. Sélectionnez **Utiliser le générateur pour créer l’instruction**.  
    c. Dans la zone **d’aperçu**, vérifiez que l’instruction de jointure ressemble à ceci :  
  
    ```sql  
    ON [SalesOrderHeader].[SalesOrderID] = [SalesOrderDetail].[SalesOrderID] 
    ```  
  
    d. Dans **Spécifiez les options de jointure**, sélectionnez **Clé unique**, puis sélectionnez **OK**. Sélectionnez **Suivant**. 

    ![Sélections pour l’ajout d’une autre jointure, pour les commandes client](media/tutorial-replicating-data-with-mobile-clients/joinsalestables.png)
  
21. Sélectionnez **Créer un instantané immédiatement**, décochez **Planifier l’exécution de l’Agent d’instantané aux heures suivantes**, puis sélectionnez **Suivant** :  

    ![Sélection pour la création immédiate d’un instantané](media/tutorial-replicating-data-with-mobile-clients/snapshotagent.png)
  
22. Dans la page **Sécurité de l’agent**, sélectionnez **Paramètres de sécurité**. Entrez <*nom_ordinateur_serveur_de_publication*> **\repl_snapshot** dans la zone **Compte de processus**, spécifiez le mot de passe du compte, puis sélectionnez **OK**. Sélectionnez **Suivant**.  

    ![Sélections pour la définition de la sécurité de l’agent d’instantané](media/tutorial-replicating-data-with-mobile-clients/snapshotagentsecurity.png)
  
23. Dans la page **Terminer l’Assistant**, entrez **AdvWorksSalesOrdersMerge** dans la zone **Nom de la publication**, puis sélectionnez **Terminer** :  

    ![Page « Terminer l’Assistant » avec le nom de la publication](media/tutorial-replicating-data-with-mobile-clients/namemergerepl.png)
  
24. Une fois la publication créée, sélectionnez **Fermer**. Sous le nœud **Réplication**, dans l’**Explorateur d’objets**, cliquez avec le bouton droit sur **Publications locales** puis sélectionnez **Actualiser** pour afficher la nouvelle réplication de fusion.  
  
### <a name="view-the-status-of-snapshot-generation"></a>Afficher l’état d’une génération d’instantané  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud du serveur, puis le dossier **Réplication**.  
  
2. Dans le dossier **Publications locales**, cliquez avec le bouton droit sur **AdvWorksSalesOrdersMerge**, puis sélectionnez **Afficher l’état de l’Agent d’instantané** :  

   ![Sélections pour l’affichage de l’état de l’Agent d’instantané](media/tutorial-replicating-data-with-mobile-clients/viewsnapshotagentstatus.png)
  
3. L’état actuel du travail de l’Agent d’instantané pour la publication s’affiche. Vérifiez que le travail d'instantané a bien réussi avant de passer à la leçon suivante.  
  
### <a name="add-the-merge-agent-login-to-the-pal"></a>Ajouter la connexion de l’Agent de fusion à la liste d’accès à la publication  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud du serveur, puis le dossier **Réplication**.  
  
2. Dans le dossier **Publications locales**, cliquez avec le bouton droit sur **AdvWorksSalesOrdersMerge**, puis sélectionnez **Propriétés**.  
  
   a. Sélectionnez la page **Liste d’accès à la publication**, puis sélectionnez **Ajouter**. 
  
   b. Dans la boîte de dialogue **Ajouter un accès à une publication**, sélectionnez <*nom_ordinateur_serveur_de_publication*> **\repl_merge**, puis **OK**. Sélectionnez **OK** à nouveau. 

   ![Sélections pour l’ajout de la connexion de l’Agent de fusion](media/tutorial-replicating-data-with-mobile-clients/mergepal.png) 

  
Pour plus d'informations, consultez les pages suivantes :  
- [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md) 
- [Filtres de lignes paramétrés](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
- [Définir un article](../../relational-databases/replication/publish/define-an-article.md)  
  
  
## <a name="create-a-subscription-to-the-merge-publication"></a>Créer un abonnement à la publication de fusion
Dans cette section, vous allez ajouter un abonnement à la publication de fusion que vous avez créée. Ce tutoriel utilise l’abonné distant (NODE2\SQL2016). Ensuite, vous définirez les autorisations sur la base de données d’abonnement et génèrerez manuellement l’instantané filtré des données du nouvel abonnement.   
  
### <a name="add-a-subscriber-for-merge-publication"></a>Ajouter un abonné à la publication de fusion
  
1. Connectez-vous à l’abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur. Développez le dossier **Réplication**, cliquez avec le bouton droit sur le dossier **Abonnements locaux**, puis sélectionnez **Nouveaux abonnements**. L’Assistant Nouvel abonnement démarre :

   ![Sélections pour démarrer l’Assistant Nouvel abonnement](media/tutorial-replicating-data-with-mobile-clients/newsub.png)
  
2. Dans la page **Publication**, sélectionnez **Rechercher un serveur de publication SQL** dans la liste **Serveur de publication**.  
  
   Dans la boîte de dialogue **Se connecter au serveur**, entrez le nom de l’instance du serveur de publication dans la zone **Nom du serveur**, puis sélectionnez **Se connecter**. 

   ![Sélections pour l’ajout d’un serveur de publication](media/tutorial-replicating-data-with-mobile-clients/publication.png)
  
4. Sélectionnez **AdvWorksSalesOrdersMerge**, puis **Suivant**.  
  
5. Dans la page **Emplacement de l’Agent de fusion**, sélectionnez **Exécuter chaque agent sur son Abonné**, puis **Suivant** :  

   ![Option « Exécuter chaque agent sur son Abonné »](media/tutorial-replicating-data-with-mobile-clients/pullsub.png)
  
6. Dans la page **Abonnés**, sélectionnez le nom d’instance du serveur d’abonné. Sous **Base de données d’abonnement**, sélectionnez **Nouvelle base de données** dans la liste.  
  
   Dans la boîte de dialogue **Nouvelle base de données**, entrez **SalesOrdersReplica** dans la zone **Nom de la base de données**. Sélectionnez **OK**, puis **Suivant**. 

   ![Sélections pour l’ajout d’une base de données à l’abonné](media/tutorial-replicating-data-with-mobile-clients/addsubdb.png)
  
8. Dans la page **Sécurité de l’agent de fusion**, cliquez sur le bouton de sélection ( **...** ). Entrez <*nom_ordinateur_abonné*> **\repl_merge** dans la zone **Compte de processus** et fournissez le mot de passe pour ce compte. Sélectionnez **OK**, **Suivant**, puis à nouveau **Suivant**.  

   ![Sélections pour la sécurité de l’Agent de fusion](media/tutorial-replicating-data-with-mobile-clients/mergeagentsecurity.png)

9. Dans la page **Planification de synchronisation**, configurez la **Planification de l’agent** sur **Exécuter uniquement à la demande**. Sélectionnez **Suivant**.  

   ![Sélection « Exécuter uniquement à la demande » pour l’agent](media/tutorial-replicating-data-with-mobile-clients/mergesyncschedule.png)
  
9. Dans la page **Initialiser les abonnements**, sélectionnez **Lors de la première synchronisation** dans la liste **À quel moment**. Sélectionnez **Suivant** pour passer à la page **Type d’abonnement**, puis sélectionnez le type d’abonnement approprié. Ce tutoriel utilise **Client**. Après avoir sélectionné le type d’abonnement, sélectionnez à nouveau **Suivant**. 

   ![Sélections pour l’initialisation des abonnements lors de la première synchronisation](media/tutorial-replicating-data-with-mobile-clients/firstsync.png)

10. Dans la page **Valeurs de HOST_NAME**, entrez **adventure-works\pamela0** dans la zone **Valeur de HOST_NAME**. Sélectionnez **Terminer**.  

    ![Page « Valeurs de HOST_NAME »](media/tutorial-replicating-data-with-mobile-clients/hostname.png)
  
11. Sélectionnez **Terminer**. Une fois l’abonnement créé, sélectionnez **Fermer**.  

### <a name="set-server-permissions-at-the-subscriber"></a>Définir des autorisations de serveur pour l’abonné  
  
1. Connectez-vous à l’abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Développez **Sécurité**, cliquez avec le bouton droit sur **Connexions**, puis sélectionnez **Nouvelle connexion**.  
  
   Dans la page **Général**, sélectionnez **Rechercher**, puis entrez <*nom_ordinateur_abonné*> **\repl_merge** dans la zone **Entrer le nom d’objet**. Sélectionnez **Vérifier les noms**, puis sélectionnez **OK**. 
    
   ![Sélections pour la définition de la connexion](media/tutorial-replicating-data-with-mobile-clients/sublogin.png)
  
1. Dans la page **Mappage d’utilisateur**, sélectionnez la base de données **SalesOrdersReplica**, puis sélectionnez le rôle **db_owner**. Dans la page **Éléments sécurisables**, accordez l’autorisation **Explicite** à **Modifier une trace**. Sélectionnez **OK**.

   ![Pages « Mappage d’utilisateur » et « Éléments sécurisables »](media/tutorial-replicating-data-with-mobile-clients/setdbo.png)
  
### <a name="create-the-filtered-data-snapshot-for-the-subscription"></a>Créer l’instantané filtré des données de l’abonnement  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud du serveur, puis le dossier **Réplication**.  
  
2. Dans le dossier **Publications locales**, cliquez avec le bouton droit sur la publication **AdvWorksSalesOrdersMerge**, puis sélectionnez **Propriétés**.  
   
   a. Sélectionnez la page **Partitions de données**, puis sélectionnez **Ajouter**.   
   b. Dans la boîte de dialogue **Ajouter une partition de données**, entrez **adventure-works\pamela0** dans la zone **Valeur de HOST_NAME**, puis sélectionnez **OK**.  
   c. Sélectionnez la partition nouvellement ajoutée, sélectionnez **Générer les instantanés sélectionnés maintenant**, puis **OK**. 

   ![Sélections pour l’ajout d’une partition](media/tutorial-replicating-data-with-mobile-clients/partition.png)
  
  
Pour plus d'informations, consultez les pages suivantes :  
- [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
- [Créer un abonnement par extraction](../../relational-databases/replication/create-a-pull-subscription.md)  
- [Instantanés des publications de fusion avec des filtres paramétrés](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  

## <a name="synchronize-the-subscription-to-the-merge-publication"></a>Synchroniser l’abonnement avec la publication de fusion

Dans cette section, vous allez démarrer l’Agent de fusion pour initialiser l’abonnement à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous allez également utiliser cette procédure pour effectuer la synchronisation avec le serveur de publication.   
  
### <a name="start-synchronization-and-initialize-the-subscription"></a>Démarrer la synchronisation et initialiser l’abonnement  
  
1. Connectez-vous à l’abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
2. Vérifiez que SQL Server Agent est en cours d’exécution. Si ce n’est pas le cas, cliquez avec le bouton droit sur SQL Server Agent dans l’Explorateur d’objets, puis sélectionnez **Démarrer**. Si l’agent ne démarre pas, vous devez le démarrer manuellement à partir du Gestionnaire de configuration SQL Server. 
  
2. Développez le nœud **Réplication**. Dans le dossier **Abonnements locaux**, cliquez avec le bouton droit sur l’abonnement de la base de données **SalesOrdersReplica**, puis sélectionnez **Afficher l’état de synchronisation**.  
  
   Sélectionnez **Démarrer** pour initialiser l’abonnement. 

   ![État de synchronisation avec le bouton « Démarrer »](media/tutorial-replicating-data-with-mobile-clients/mergesyncstatus.png)
    
  
  
## <a name="next-steps"></a>Étapes suivantes  
Vous avez correctement configuré votre serveur de publication et votre abonné pour la réplication de fusion. Vous pouvez également :

1. Insérer, mettre à jour ou supprimer des données dans la table **SalesOrderHeader** ou **SalesOrderDetail** sur le serveur de publication ou l’abonné.
2. Répéter cette procédure quand la connectivité réseau est disponible pour synchroniser les données entre le serveur de publication et l’abonné.
3. Interroger la table **SalesOrderHeader** ou **SalesOrderDetail** sur l’autre serveur pour voir les modifications répliquées.  
  
Pour plus d'informations, consultez les pages suivantes :   
- [Initialiser un abonnement avec un instantané](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Synchroniser les données](../../relational-databases/replication/synchronize-data.md)  
- [Synchroniser un abonnement par extraction](../../relational-databases/replication/synchronize-a-pull-subscription.md)  
  
  
  
