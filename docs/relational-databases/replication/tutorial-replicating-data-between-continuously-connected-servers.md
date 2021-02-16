---
title: 'Tutoriel : Configurer la réplication transactionnelle'
description: Ce tutoriel vous apprend à configurer la réplication transactionnelle entre deux serveurs SQL Server intégralement connectés.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- replication [SQL Server], tutorials
- wizards [SQL Server replication]
ms.assetid: 7b18a04a-2c3d-4efe-a0bc-c3f92be72fd0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e2a27d4fccfb24aff29668651ea914e11429910f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347122"
---
# <a name="tutorial-configure-replication-between-two-fully-connected-servers-transactional"></a>Tutoriel : configurer la réplication entre deux serveurs intégralement connectés (réplication transactionnelle)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
La réplication transactionnelle constitue une bonne solution au problème de transfert de données entre serveurs connectés en permanence. À l’aide de l’Assistant Réplication, vous pouvez aisément configurer et administrer une topologie de réplication. 

Ce tutoriel vous explique comment configurer une topologie de réplication transactionnelle dans le cas de serveurs connectés en permanence. Pour plus d’informations sur le fonctionnement de la réplication transactionnelle, consultez [Vue d’ensemble de la réplication transactionnelle](./transactional/transactional-replication.md). 
  
## <a name="what-you-will-learn"></a>Contenu du didacticiel  
Ce didacticiel vous explique comment publier des données d'une base de données sur une autre à l'aide de la réplication transactionnelle.  

Dans ce didacticiel, vous apprendrez à :
> [!div class="checklist"]
> * Créer un serveur de publication par le biais de la réplication transactionnelle.
> * Créer un Abonné pour le serveur de publication transactionnelle.
> * Valider l’abonnement et mesurer la latence.
  
  
## <a name="prerequisites"></a>Prérequis  
Ce didacticiel est destiné aux utilisateurs qui sont familiers des opérations élémentaires de base de données, mais dont l'expérience en matière de réplication est limitée. Avant de commencer ce tutoriel, vous devez effectuer le [Tutoriel : Préparer SQL Server pour la réplication](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Pour suivre ce tutoriel, vous avez besoin de SQL Server, SQL Server Management Studio (SSMS) et une base de données AdventureWorks :  
  
- Sur le serveur de publication (source), installez :  
  
   - Toute édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à l’exception de SQL Server Express ou SQL Server Compact. Ces éditions ne peuvent pas constituer des serveurs de publication de réplication.   
   - Exemple de base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Pour des raisons de sécurité, les exemples de bases de données ne sont pas installés par défaut.  
  
- Sur le serveur d’abonné (destination), installez une édition quelconque de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sauf [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] ne peut pas être un abonné dans une réplication transactionnelle.  
  
- Installez [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Téléchargez l’[exemple de base de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Pour obtenir des instructions sur la restauration d’une base de données dans SSMS, consultez [Restauration d’une base de données](../backup-restore/restore-a-database-backup-using-ssms.md). 
 
>[!NOTE]
> - La réplication n’est pas prise en charge sur les instances de SQL Server qui sont séparées de plus de deux versions. Pour plus d’informations, consultez [Supported SQL Server Versions in Replication Topology](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/) (Versions de SQL prises en charge dans la topologie de réplication).
> - Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous devez vous connecter au serveur de publication et à l’abonné à l’aide d’un identifiant de connexion membre du rôle serveur fixe **sysadmin**. Pour plus d’informations sur ce rôle, consultez [Rôles de niveau serveur](../security/authentication-access/server-level-roles.md).  
  
  
**Durée estimée pour terminer ce didacticiel : 60 minutes**  
  
## <a name="configure-the-publisher-for-transactional-replication"></a>Configurer le serveur de publication pour la réplication transactionnelle
Dans cette section, vous créez une publication transactionnelle en utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour publier un sous-ensemble filtré de la table **Product** de l’exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Vous ajoutez également à la liste d'accès à la publication (PAL) le compte de connexion SQL Server utilisée par l'Agent de distribution.


### <a name="create-a-publication-and-define-articles"></a>Créer une publication et définir des articles
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2. Cliquez avec le bouton droit sur **SQL Server Agent** et sélectionnez **Démarrer**. SQL Server Agent doit être en cours d’exécution pour que vous puissiez créer la publication. Si cette étape ne démarre pas l’agent, vous devez le démarrer manuellement à partir du Gestionnaire de configuration SQL Server. 
3. Développez le dossier **Réplication**, cliquez avec le bouton droit sur le dossier **Publications locales**, puis sélectionnez **Nouvelle publication**. Cette étape lance l’Assistant Nouvelle publication :  

   ![Sélections pour démarrer l’Assistant Nouvelle Publication](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
  
3. Dans la page **Base de données de publication**, sélectionnez [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], puis **Suivant**.  
  
4. Dans la page **Type de publication**, sélectionnez **Publication transactionnelle**, puis **Suivant** :  

   ![Page « Type de publication » avec le type de publication sélectionné](media/tutorial-replicating-data-between-continuously-connected-servers/tranrepl.png)
  
5. Dans la page **Articles**, développez le nœud **Tables**, puis cochez la case **Product**. Ensuite, développez **Product** et décochez les cases en regard de **ListPrice** et **StandardCost**. Sélectionnez **Suivant**.  

   ![Page « Articles » avec les articles sélectionnés à publier](media/tutorial-replicating-data-between-continuously-connected-servers/replarticles.png)
  
6. Dans la page **Filtrer les lignes de la table**, sélectionnez **Ajouter**.   
  
7. Dans la boîte de dialogue **Ajouter un filtre**, sélectionnez la colonne **SafetyStockLevel**. Sélectionnez la flèche droite pour ajouter la colonne à la clause WHERE de la requête de filtre. Ensuite, entrez le modificateur de la clause WHERE comme suit :  
  
   ```sql  
   WHERE [SafetyStockLevel] < 500  
   ```
  
   ![Page « Filtrer les flux de table » et boîte de dialogue « Ajouter un filtre »](media/tutorial-replicating-data-between-continuously-connected-servers/filter.png)
  
8. Sélectionnez **OK**, puis **Suivant**.  
  
9. Cochez la case **Créer un instantané immédiatement et garder ce dernier disponible pour l’initialisation des abonnements** et sélectionnez **Suivant** :  

   ![Page « Agent d’instantané » avec case à cocher sélectionnée](media/tutorial-replicating-data-between-continuously-connected-servers/snapshot.png)
  
10. Dans la page **Sécurité de l’agent**, décochez la case **Utiliser les paramètres de sécurité de l’Agent d’instantané**.   
  
    Sélectionnez **Paramètres de sécurité** pour l'Agent d'instantané. Entrez <*nom_ordinateur_serveur_de_publication*> **\repl_snapshot** dans la zone **Compte de processus**, spécifiez le mot de passe du compte, puis sélectionnez **OK**.  

    ![Page« Sécurité de l’agent » et boîte de dialogue « Sécurité de l’Agent d’instantané »](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagentsecurity.png)
  
12. Répétez l’étape précédente pour définir <*nom_ordinateur_serveur_de_publication*> **\repl_logreader** comme compte de processus de l’Agent de lecture du journal, Sélectionnez ensuite **OK**.  

    ![Boîte de dialogue « Sécurité de l’Agent de lecture du journal » et page « Sécurité de l’Agent »](media/tutorial-replicating-data-between-continuously-connected-servers/logreaderagentsecurity.png)   

  
13. Dans la page **Terminer l’Assistant**, entrez **AdvWorksProductTrans** dans la zone **Nom de la publication**, puis sélectionnez **Terminer** :  

    ![Page « Terminer l’Assistant » avec le nom de la publication](media/tutorial-replicating-data-between-continuously-connected-servers/advworksproducttrans.png)
  
14. Une fois la publication créée, sélectionnez **Fermer** pour terminer l’Assistant. 

Vous pouvez rencontrer l’erreur suivante si SQL Server Agent n’est pas en cours d’exécution quand vous essayez de créer la publication. Cette erreur indique que votre publication a été créée avec succès, mais que l’Agent d’instantané n’a pas pu démarrer. Dans ce cas, vous devez démarrer SQL Server Agent, puis démarrer manuellement l’Agent d’instantané. La section suivante fournit des instructions. 

![Avertissement indiquant que l’Agent d’instantané n’a pas pu démarrer](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagenterror.png)
    
  
### <a name="view-the-status-of-snapshot-generation"></a>Afficher l’état d’une génération d’instantané  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud du serveur, puis le dossier **Réplication**.  
  
2. Dans le dossier **Publications locales**, cliquez avec le bouton droit sur **AdvWorksProductTrans**, puis sélectionnez **Afficher l’état de l’Agent d’instantané** :  
   ![Commande du menu contextuel permettant d’afficher l’état de l’Agent d’instantané](media/tutorial-replicating-data-between-continuously-connected-servers/viewsnapshot.png)
  
3. L’état actuel du travail de l’Agent d’instantané pour la publication s’affiche. Vérifiez que le travail d’instantané a bien réussi avant de passer à la section suivante.
          
Si SQL Server Agent n’était pas en cours d’exécution quand vous avez créé la publication, une mention indique que l’Agent d’instantané n’a jamais été exécuté quand vous vérifiez l’état de l’Agent d’instantané pour la publication. Dans ce cas, sélectionnez **Démarrer** pour démarrer l’Agent d’instantané : 

![Bouton « Démarrer » et modification du message d’état indiquant que l’Agent d’instantané a été exécuté](media/tutorial-replicating-data-between-continuously-connected-servers/startsnapshotagent.png)
     
Si une erreur apparaît ici, consultez [Résoudre les erreurs liées à l’Agent d’instantané](troubleshoot-tran-repl-errors.md#find-errors-with-the-snapshot-agent).


  
### <a name="add-the-distribution-agent-login-to-the-pal"></a>Ajouter la connexion de l'Agent de distribution à la liste d'accès à la publication (PAL)  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud du serveur, puis le dossier **Réplication**.  
  
2. Dans le dossier **Publications locales**, cliquez avec le bouton droit sur **AdvWorksProductTrans**, puis sélectionnez **Propriétés**.  La boîte de dialogue **Propriétés de la publication** apparaît.    
  
   a. Sélectionnez la page **Liste d’accès à la publication**, puis sélectionnez **Ajouter**.  
   b. Dans la boîte de dialogue **Ajouter un accès à une publication**, sélectionnez <*nom_ordinateur_serveur_de_publication*> **\repl_distribution** et sélectionnez **OK**.
   
   ![Sélections pour ajouter une connexion à la liste d'accès à la publication](media/tutorial-replicating-data-between-continuously-connected-servers/tranreplproperties.png)

Pour plus d’informations, consultez [Replication Programming Concepts](../../relational-databases/replication/concepts/replication-programming-concepts.md).  
  

## <a name="create-a-subscription-to-the-transactional-publication"></a>Créer un abonnement à la publication transactionnelle
Dans cette section, vous ajoutez un Abonné à la publication que vous avez créée. Ce tutoriel utilise un Abonné distant (NODE2\SQL2016), mais vous pouvez également ajouter un abonnement localement au serveur de publication. 

### <a name="create-the-subscription"></a>Créer l’abonnement  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud du serveur, puis le dossier **Réplication**.  
  
2. Dans le dossier **Publications locales**, cliquez avec le bouton droit sur la publication **AdvWorksProductTrans**, puis sélectionnez **Nouveaux abonnements**. L’Assistant Nouvel abonnement démarre : 
 
   ![Sélections pour démarrer l’Assistant Nouvel abonnement](media/tutorial-replicating-data-between-continuously-connected-servers/newsub.png)     
  
3. Dans la page **Publication**, sélectionnez **AdvWorksProductTrans**, puis sélectionnez **Suivant** :  

   ![Page « Publication » avec la publication sélectionnée](media/tutorial-replicating-data-between-continuously-connected-servers/selectpub.png)
  
4. Dans la page **Emplacement de l’Agent de distribution**, sélectionnez **Exécuter tous les agents sur le serveur de distribution**, puis sélectionnez **Suivant**.  Pour plus d’informations sur les abonnements par extraction et par émission de données, consultez [S’abonner à des publications](./subscribe-to-publications.md).

   ![Page « Emplacement de l'Agent de distribution » avec l’option Exécuter tous les agents sur le serveur de distribution sélectionnée](media/tutorial-replicating-data-between-continuously-connected-servers/runagentsatdist.png)
  
5. Dans la page **Abonnés**, si le nom de l’instance de l’Abonné n’est pas affiché, sélectionnez **Ajouter un Abonné**, puis sélectionnez **Ajouter un Abonné SQL Server** dans la liste déroulante. Cette étape ouvre la boîte de dialogue **Se connecter au serveur**. Entrez le nom de l’instance de l’Abonné, puis sélectionnez **Se connecter**.  
    
   Une fois l’abonné ajouté, cochez la case en regard du nom de son instance. Ensuite, sélectionnez **Nouvelle base de données** sous **Base de données d’abonnement**.   

   ![Page « Abonnés » avec les sélections pour ajouter un serveur d’abonné](media/tutorial-replicating-data-between-continuously-connected-servers/addsub.png)

7. La boîte de dialogue **Nouvelle base de données** apparaît. Entrez **ProductReplica** dans la zone **Nom de la base de données**, sélectionnez **OK**, puis sélectionnez **suivant** : 
  
   ![Saisie du nom de la base de données d'abonnement](media/tutorial-replicating-data-between-continuously-connected-servers/productreplica.png)
  
8. Dans la page **Sécurité de l’Agent de distribution**, sélectionnez le bouton représentant des points de suspension ( **…** ). Entrez <*nom_ordinateur_serveur_de_publication>* > **\repl_distribution** dans la zone **Compte de processus**, entrez le mot de passe du compte, sélectionnez **OK**, puis **Suivant**.

   ![Informations sur le compte de distribution dans la boîte de dialogue « Sécurité de l'Agent de distribution »](media/tutorial-replicating-data-between-continuously-connected-servers/adddistaccount.png)
  
9. Sélectionnez **Terminer** pour accepter les valeurs par défaut des pages restantes et terminer l’Assistant.  
  
### <a name="set-database-permissions-at-the-subscriber"></a>Définir des autorisations de base de données sur l'abonné  
  
1. Connectez-vous à l’abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Développez **Sécurité**, cliquez avec le bouton droit sur **Connexions**, puis sélectionnez **Nouvelle connexion**.     
  
   a. Dans la page **Général**, sous **Nom de connexion**, sélectionnez **Rechercher**, puis ajoutez le nom de connexion pour <*nom_ordinateur_Abonné>* > **\repl_distribution**.

   b. Dans la page **Mappages d’utilisateur**, accordez l’appartenance **db_owner** pour la base de données **ProductReplica**. 

   ![Sélections pour configurer la connexion sur l’abonné](media/tutorial-replicating-data-between-continuously-connected-servers/loginforsub.png)

2. Sélectionnez **OK** pour fermer la boîte de dialogue **Nouvelle connexion**. 

  
### <a name="view-the-synchronization-status-of-the-subscription"></a>Afficher l'état de synchronisation de l'abonnement  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Développez le nœud du serveur, puis développez le dossier **Réplication**.  
  
2. Dans le dossier **Publications locales**, développez la publication **AdvWorksProductTrans**, cliquez avec le bouton droit sur l’abonnement dans la base de données **ProductReplica**, puis sélectionnez **Afficher l’état de synchronisation**. L’état de synchronisation de l’abonnement s’affiche :

   ![Sélections pour ouvrir la boîte de dialogue « Afficher l'état de synchronisation »](media/tutorial-replicating-data-between-continuously-connected-servers/viewsyncstatus.png)
3. Si l’abonnement n’apparaît pas sous **AdvWorksProductTrans**, appuyez sur la touche F5 pour actualiser la liste.  
  
Pour plus d'informations, consultez les pages suivantes :  
- [Initialiser un abonnement avec un instantané](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Créer un abonnement par émission (push)](../../relational-databases/replication/create-a-push-subscription.md)  
- [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  

## <a name="measure-replication-latency"></a>Mesurer la latence de réplication
Dans cette section, vous utilisez les jetons de suivi pour vérifier que les modifications sont bien répliquées sur l’abonné et pour déterminer la latence. La latence est le temps nécessaire pour qu’une modification apportée sur le serveur de publication apparaisse sur l’abonné.
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Développez le nœud du serveur, cliquez avec le bouton droit sur le dossier **Réplication**, puis sélectionnez **Lancer le moniteur de réplication** :

   ![Commande « Lancer le moniteur de réplication » du menu contextuel](media/tutorial-replicating-data-between-continuously-connected-servers/launchreplmonitor.png)

2. Développez un groupe de serveurs de publication dans le volet gauche, développez une instance du serveur de publication, puis sélectionnez la publication **AdvWorksProductTrans**.  
  
   a. Sélectionnez l’onglet **Jetons de suivi**.  
   b. Sélectionnez **Insérer un suivi**.    
   c. Affichez la durée calendaire pour le jeton de suivi dans les colonnes suivantes : **Du serveur de publication vers le serveur de distribution**, **Du serveur de distribution vers l’Abonné**, **Latence totale**. Une valeur **En attente** indique que le jeton n’a pas atteint un point spécifié.

   ![Informations pour le jeton de suivi](media/tutorial-replicating-data-between-continuously-connected-servers/tracertoken.png)


Pour plus d'informations, consultez les pages suivantes : 
- [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)
- [Rechercher des erreurs avec les agents de réplication transactionnelle](troubleshoot-tran-repl-errors.md)


## <a name="next-steps"></a>Étapes suivantes
Vous avez correctement configuré votre serveur de publication et votre abonné pour la réplication transactionnelle. Vous pouvez maintenant insérer, mettre à jour ou supprimer des données dans la table **Product** sur le serveur de publication. Ensuite, vous pouvez interroger la table **Product** sur l’abonné pour voir les modifications répliquées. 

L’article suivant va vous apprendre à configurer la réplication de fusion :  

> [!div class="nextstepaction"]
> [Tutoriel : configurer la réplication entre un serveur et des clients mobiles (fusion)](tutorial-replicating-data-with-mobile-clients.md)