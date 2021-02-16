---
title: Rechercher des erreurs avec la réplication transactionnelle
description: Explique comment rechercher et identifier les erreurs liées à la réplication transactionnelle, et décrit la méthodologie de résolution des problèmes de réplication.
ms.custom: seo-lt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], tutorials
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d39286c1bc228a9da76a163a39e76dbb2022d807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348639"
---
# <a name="troubleshooter-find-errors-with-sql-server-transactional-replication"></a>Outil de résolution des problèmes : Rechercher des erreurs dans la réplication transactionnelle SQL Server 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Le dépannage des erreurs de réplication peuvent être frustrant si vous n’avez pas une connaissance de base du fonctionnement de la réplication transactionnelle. La première étape de création d’une publication consiste à faire en sorte que l’Agent d’instantané crée l’instantané et l’enregistre dans le dossier des instantanés. Ensuite, l’Agent de distribution applique l’instantané à l’abonné. 

Ce processus crée la publication et la place dans l’état *En cours de synchronisation*. La synchronisation fonctionne en trois phases :
1. Des transactions se produisent sur des objets qui sont répliqués et sont marqués « pour réplication » dans le journal des transactions. 
2. L’Agent de lecture du journal parcourt le journal des transactions et recherche les transactions marquées « pour réplication ». Ces transactions sont alors enregistrées dans la base de données de distribution. 
3. L’Agent de distribution parcourt la base de données de distribution en utilisant le thread de lecture. Ensuite, en utilisant le thread d’écriture, cet agent se connecte à l’abonné pour lui appliquer ces modifications.

Des erreurs peuvent se produire à chaque étape de ce processus. La recherche de ces erreurs peut être l’aspect le plus complexe de la résolution des problèmes de synchronisation. Heureusement, l’utilisation du moniteur de réplication facilite ce processus. 

>[!NOTE]
> - L’objectif de ce guide de dépannage est de vous faire découvrir la méthodologie de résolution des problèmes. Il n’est pas conçu pour résoudre une erreur spécifique, mais de façon à fournir des indications générales pour la recherche d’erreurs dans la réplication. Il contient quelques exemples spécifiques, mais leur résolution peut varier en fonction de l’environnement. 
> - Les erreurs présentées à titre d’exemple dans ce guide sont basées sur le tutoriel [Configuration de la réplication transactionnelle](../../relational-databases/replication/tutorial-replicating-data-between-continuously-connected-servers.md).



## <a name="troubleshooting-methodology"></a>Méthodologie de résolution des erreurs 

### <a name="questions-to-ask"></a>Questions à se poser
1. Où la réplication échoue-t-elle dans le processus de synchronisation ?
2. Quel agent rencontre une erreur ?
1. Quand la dernière réplication a-t-elle fonctionné correctement ? Quelque chose a-t-il changé depuis lors ?  

### <a name="steps-to-take"></a>Étapes à suivre
1. Utilisez le moniteur de réplication pour identifier à quel point la réplication rencontre l’erreur (quel agent ?) :
   - Si les erreurs se produisent dans la section **Du serveur de publication vers le serveur de distribution**, c’est que le problème concerne l’Agent de lecture du journal. 
   - Si les erreurs se produisent dans la section **Du serveur de distribution vers l’Abonné**, c’est que le problème concerne l’Agent de distribution.  
2. Examinez l’historique des travaux de cet agent dans le moniteur d’activité des travaux pour identifier les détails de l’erreur. Si l’historique des travaux ne montre pas suffisamment de détails, vous pouvez [activer la journalisation détaillée](#enable-verbose-logging-on-any-agent) sur cet agent spécifique.
3. Essayez de déterminer une solution pour l’erreur.


## <a name="find-errors-with-the-snapshot-agent"></a>Rechercher des erreurs avec l’Agent d’instantané
L’Agent d’instantané génère l’instantané et l’écrit dans le dossier d’instantanés spécifié. 

1. Affichez l’état de votre Agent d’instantané :

    a. Dans l’Explorateur d’objets, développez le nœud **Publications locales** sous **Réplication**.

    b. Cliquez avec le bouton droit sur votre publication **AdvWorksProductTrans** > **Afficher l’état de l’Agent d’instantané**. 

    ![Commande « Afficher l’état de l’Agent d’instantané » sur le menu contextuel](media/troubleshooting-tran-repl-errors/view-snapshot-agent-status.png)

1. Si une erreur est signalée dans l’état de l’Agent d’instantané, vous pouvez trouver plus de détails dans l’historique des travaux de l’Agent d’instantané :

    a. Développez **SQL Server Agent** dans l’Explorateur d’objets, puis ouvrez le moniteur d’activité des travaux. 

    b. Triez par **Catégorie** et identifiez l’Agent d’instantané dans la catégorie **REPL-Instantané**.

    c. Cliquez avec le bouton droit sur l’Agent d’instantané, puis choisissez **Afficher l’historique**. 

   ![Sélections pour l’ouverture de l’historique de l’Agent d’instantané](media/troubleshooting-tran-repl-errors/snapshot-agent-history.png)
    
1. Dans l’historique de l’Agent d’instantané, sélectionnez l’entrée de journal appropriée. Il s’agit généralement d’une ou deux lignes *avant* l’entrée qui signale l’erreur. (Une croix rouge indique des erreurs.) Examinez le texte du message dans la zone sous les journaux : 

    ![Erreur de l’Agent d’instantané en raison d’un accès refusé](media/troubleshooting-tran-repl-errors/snapshot-access-denied.png)

    ```console
    The replication agent had encountered an exception.
    Exception Message: Access to path '\\node1\repldata.....' is denied.
    ```

Si vos autorisations Windows ne sont pas configurées correctement pour votre dossier d’instantanés, vous voyez une erreur « Accès refusé » pour l’Agent d’instantané. Vous devez vérifier les autorisations sur le dossier où votre instantané est stocké et vérifier que le compte utilisé pour exécuter l’Agent d’instantané dispose des autorisations d’accès au partage.  

## <a name="find-errors-with-the-log-reader-agent"></a>Rechercher les erreurs liées à l’Agent de lecture du journal
L’Agent de lecture du journal se connecte à votre base de données du serveur de publication, puis parcourt le journal des transactions à la recherche des transactions marquées « pour réplication ». Il ajoute ensuite ces transactions à la base de données de distribution. 

1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Développez le nœud du serveur, cliquez avec le bouton droit sur le dossier **Réplication**, puis sélectionnez **Lancer le moniteur de réplication** :  

    ![Commande « Lancer le moniteur de réplication » du menu contextuel](media/troubleshooting-tran-repl-errors/launch-repl-monitor.png)
  
    Le Moniteur de réplication s’ouvre : ![Moniteur de réplication](media/troubleshooting-tran-repl-errors/repl-monitor.png) 
   
2. La croix (X) rouge indique que la publication ne se synchronise pas. Développez **Mes serveurs de publication** du côté gauche, puis développez le serveur de publication approprié.  
  
3.  Sélectionnez la publication **AdvWorksProductTrans** sur la gauche, puis recherchez le X rouge sur un des onglets pour identifier l’emplacement du problème. En l’occurrence, le X rouge se trouve sur l’onglet **Agents**, indiquant qu’un des agents rencontre une erreur : 

    ![X rouge sur l’onglet Agents](media/troubleshooting-tran-repl-errors/agent-error.png)

4. Sélectionnez l’onglet **Agents** pour identifier l’agent qui rencontre l’erreur : 

    ![X rouge sur l’Agent de lecture du journal avec une erreur](media/troubleshooting-tran-repl-errors/log-reader-agent-failure.png)


5. Cette vue vous montre deux agents : l’Agent d’instantané et l’Agent de lecture du journal. Celui qui rencontre une erreur a un X rouge. En l’occurrence, il s’agit de l’Agent de lecture du journal. 

    Double-cliquez sur la ligne qui signale l’erreur pour ouvrir l’historique de l’agent pour l’Agent de lecture du journal. Celui-ci fournit plus d’informations sur l’erreur : 
    
    ![Détails de l’erreur pour l’Agent de lecture du journal](media/troubleshooting-tran-repl-errors/log-reader-error.png)

    ```console
    Status: 0, code: 20011, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.
    The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.
    Status: 0, code: 15517, text: 'Cannot execute as the database principal because the principal "dbo" does not exist, this type of principal cannot be impersonated, or you do not have permission.'.
    Status: 0, code: 22037, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.        
    ```

6. L’erreur se produit généralement quand le propriétaire de la base de données du serveur de publication n’est pas défini correctement. Ceci peut se produire quand une base de données est restaurée. Pour vérifier cela :

    a. Développez **Bases de données** dans l’Explorateur d’objets.

    b. Cliquez avec le bouton droit sur **AdventureWorks2012** > **Propriétés**. 

    c. Vérifiez qu’un propriétaire existe sous la page **Fichiers**. Si cette zone est vide, ceci est la cause probable de votre problème. 

   ![Page « Fichiers » dans les propriétés de la base de données, avec une zone « Propriétaire » vide](media/troubleshooting-tran-repl-errors/db-properties.png)

7. Si la zone du propriétaire est vide dans la page **Fichiers**, ouvrez une fenêtre **Nouvelle requête** dans le contexte de la base de données AdventureWorks2012. Exécutez le code T-SQL suivant :

    ```sql
    -- set the owner of the database to 'sa' or a specific user account, without the brackets. 
    EXECUTE sp_changedbowner '<useraccount>'
    -- example for sa: exec sp_changedbowner 'sa'
    -- example for user account: exec sp_changedbowner 'sqlrepro\administrator' 
    ```

8. Il peut être nécessaire de redémarrer l’Agent de lecture du journal.

    a. Développez le nœud **SQL Server Agent** dans l’Explorateur d’objets, puis ouvrez le moniteur d’activité des travaux.

    b. Triez par **Catégorie** et identifiez l’Agent de lecture du journal dans la catégorie **REPL-LogReader**. 

    c. Cliquez avec le bouton droit sur le travail **Agent de lecture du journal** et sélectionnez **Démarrer le travail à l’étape**. 

    ![Sélections pour redémarrer l’Agent de lecture du journal](media/troubleshooting-tran-repl-errors/start-job-at-step.png)

9. Vérifiez que votre publication est maintenant en cours de synchronisation en rouvrant le moniteur de réplication. S’il n’est pas déjà ouvert, vous pouvez le trouver en cliquant avec le bouton droit sur **Réplication** dans l’Explorateur d’objets. 
10. Sélectionnez la publication **AdvWorksProductTrans**, sélectionnez l’onglet **Agents**, puis double-cliquez sur l’Agent de lecture du journal pour ouvrir l’historique de l’agent. Vous devez maintenant voir que l’Agent de lecture du journal est en cours d’exécution, et qu’il réplique des commandes ou qu’il indique « aucune transaction répliquée » :

    ![Agent de lecture du journal en cours d’exécution sans transaction répliquée](media/troubleshooting-tran-repl-errors/log-reader-running.png)

## <a name="find-errors-with-the-distribution-agent"></a>Rechercher les erreurs avec l’Agent de distribution
L’Agent de distribution recherche des données dans la base de données de distribution, puis les applique à l’abonné. 

1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Développez le nœud du serveur, cliquez avec le bouton droit sur le dossier **Réplication**, puis sélectionnez **Lancer le moniteur de réplication**.  
2. Dans **Moniteur de réplication**, sélectionnez la publication **AdvWorksProductTrans**, puis sélectionnez l’onglet **Tous les abonnements**. Cliquez avec le bouton droit sur l’abonnement, puis sélectionnez **Afficher les détails** :

    ![Commande « Afficher les détails » sur le menu contextuel](media/troubleshooting-tran-repl-errors/view-details.png)

2. La boîte de dialogue **Historique du serveur de distribution vers l’Abonné** s’ouvre et donne des indications sur l’erreur rencontrée par l’Agent : 

     ![Détails de l’erreur pour l’Agent de distribution](media/troubleshooting-tran-repl-errors/dist-history-error.png)

    ```console
    Error messages:
    Agent 'NODE1\SQL2016-AdventureWorks2012-AdvWorksProductTrans-NODE2\SQL2016-7' is retrying after an error. 89 retries attempted. See agent job history in the Jobs folder for more details.
    ```

3. L’erreur indique que l’Agent de distribution fait une nouvelle tentative. Pour trouver plus d’informations, consultez l’historique de l’Agent de Distribution : 

    a. Développez **SQL Server Agent** dans Explorateur d’objets > **Moniteur d’activité des travaux**. 
    
    b. Triez les travaux par **Catégorie**. 

    c. Identifiez l’Agent de distribution dans la catégorie **REPL-Distribution**. Cliquez avec le bouton droit sur l’agent, puis sélectionnez **Afficher l’historique**.

    ![Sélections pour l’affichage de l’historique de l’Agent de distribution](media/troubleshooting-tran-repl-errors/view-dist-agent-history.png)

5. Sélectionnez une des entrées d’erreur et consultez le texte de l’erreur en bas de la fenêtre :  

    ![Texte d’erreur qui indique un mot de passe incorrect pour l’Agent de distribution](media/troubleshooting-tran-repl-errors/dist-pw-wrong.png)

    ```console
    Message:
    Unable to start execution of step 2 (reason: Error authenticating proxy NODE1\repl_distribution, system error: The user name or password is incorrect.)
    ```

6. Cette erreur indique que le mot de passe utilisé par l’Agent de distribution est incorrect. Pour la résoudre :

    a. Développez le nœud **Réplication** dans l’Explorateur d’objets.
    
    b. Cliquez avec le bouton droit sur l’abonnement > **Propriétés**.
    
    c. Sélectionnez les points de suspension (...) à côté de **Compte de processus de l’agent**, puis modifiez le mot de passe.

    ![Sélections pour modifier le mot de passe pour l’Agent de Distribution](media/troubleshooting-tran-repl-errors/dist-agent-pw-change.png)

7. Revenez au moniteur de réplication en cliquant avec le bouton droit sur **Réplication** dans l’Explorateur d’objets. Une croix (X) rouge sous **Tous les abonnements** indique que l’Agent de distribution rencontre encore une erreur. 

    Ouvrez l’historique **Du serveur de distribution vers l’Abonné** en cliquant avec le bouton droit sur l’abonnement dans **Moniteur de réplication** > **Afficher les détails**. Ici, l’erreur est désormais différente : 

    ![Erreur indiquant que l’Agent de distribution ne peut pas se connecter.](media/troubleshooting-tran-repl-errors/dist-agent-cant-connect.png)

    ```console
    Connecting to Subscriber 'NODE2\SQL2016'        
    Agent message code 20084. The process could not connect to Subscriber 'NODE2\SQL2016'.
    Number:  18456
    Message: Login failed for user 'NODE2\repl_distribution'.
    ```

8. Cette erreur indique que l’Agent de distribution n’a pas pu se connecter à l’abonné, car la connexion a échoué pour l’utilisateur **NODE2\repl_distribution**. Pour en savoir plus, connectez-vous à l’abonné et ouvrez le journal des erreurs SQL *Actuel* sous le nœud **Gestion** dans l’Explorateur d’objets : 

    ![Erreur indiquant que la connexion a échoué pour l’abonné](media/troubleshooting-tran-repl-errors/login-failed.png)
    
    Si vous voyez cette erreur, c’est que la connexion est manquante sur l’abonné. Pour résoudre cette erreur, consultez [Autorisations pour la réplication](../../relational-databases/replication/security/security-role-requirements-for-replication.md).

9. Une fois que l’erreur de connexion est résolue, revenez au moniteur de réplication. Si tous les problèmes ont été traités, vous devez voir une flèche verte à côté de **Nom de la publication** et l’état **En cours d’exécution** sous **Tous les abonnements**. 

    Cliquez avec le bouton droit sur l’abonnement pour rouvrir l’historique **Du serveur de distribution vers l’Abonné** afin de vérifier que tout est résolu. S’il s’agit de la première exécution de l’Agent de distribution, vous pouvez constater que l’instantané a été copié en bloc vers l’Abonné : 

     ![Agent de distribution avec un état « En cours d’exécution » et un message sur la copie en bloc](media/troubleshooting-tran-repl-errors/dist-agent-success.png)   


## <a name="enable-verbose-logging-on-any-agent"></a>Activer la journalisation détaillée sur un agent
Vous pouvez utiliser la journalisation détaillée pour voir des informations plus détaillées sur les erreurs qui se produisent avec n’importe quel agent dans la topologie de réplication. Les étapes sont identiques pour chaque agent. Veillez simplement à sélectionner l’agent approprié dans le moniteur d’activité des travaux. 

   >[!NOTE]   
   > Les agents peuvent être sur le serveur de publication ou sur l’abonné, selon s’il s’agit d’un abonnement par extraction ou par émission de données. Si vous ne trouvez pas l’agent que vous recherchez sur le serveur que vous examinez, essayez sur l’autre serveur.  

1. Décidez où vous voulez que la journalisation détaillée soit enregistrée et vérifiez que le dossier existe. Cet exemple utilise c:\temp. 
2. Développez le nœud **SQL Server Agent** dans l’Explorateur d’objets, puis ouvrez le moniteur d’activité des travaux. 

    ![Commande « Afficher l’activité du travail » dans le menu contextuel pour le moniteur d’activité des travaux](media/troubleshooting-tran-repl-errors/job-activity-monitor.png)    

1. Triez par **Catégorie** et identifiez l’agent concerné. Cet exemple utilise l’Agent de lecture du journal. Cliquez avec le bouton droit sur l’agent concerné > **Propriétés**.

    ![Sélections pour l’ouverture des propriétés de l’agent](media/troubleshooting-tran-repl-errors/log-agent-properties.png)

1. Sélectionnez la page **Étapes**, puis sélectionnez l’étape **Exécution de l’agent**. Sélectionnez **Modifier**. 

    ![Sélections pour la modification de l’étape « Exécution de l’agent »](media/troubleshooting-tran-repl-errors/edit-steps.png)

1. Dans la zone **Commande**, commencez une nouvelle ligne, entrez le texte suivant et sélectionnez **OK** : 

    ```console
    -Output C:\Temp\OUTPUTFILE.txt -Outputverboselevel 3
    ```

    Vous pouvez modifier l’emplacement et le niveau de détail selon vos préférences.

    ![Sortie détaillée dans les propriétés de l’étape de travail](media/troubleshooting-tran-repl-errors/verbose.png)

   > [!NOTE]
   > Ces éléments peuvent entraîner l’échec de votre agent ou l’absence du fichier de sortie, quand vous ajoutez le paramètre de sortie détaillée :
   > - Il existe un problème de mise en forme là où le tiret est devenu un trait d’union. 
   > - L’emplacement n’existe pas sur le disque ou le compte qui exécute l’agent ne dispose pas d’autorisation d’écrire à l’emplacement spécifié. 
   > - Il manque un espace entre le dernier paramètre et le paramètre `-Output`. 
   > - Les différents agents prennent en charge des niveaux de détail différents. Si vous activez la journalisation détaillée mais que l’agent ne démarre pas, essayez en réduisant le niveau de détail de 1. 

1. Redémarrez l’Agent de lecture du journal en double-cliquant sur l’agent > **Arrêter le travail à l’étape**. Actualisez en sélectionnant l’icône **Actualiser** dans la barre d’outils. Cliquez avec le bouton droit sur l’agent > **Démarrer le travail à l’étape**.
2. Examinez la sortie sur le disque. 

    ![Fichier texte de sortie](media/troubleshooting-tran-repl-errors/output.png)

    
1. Pour désactiver la journalisation détaillée, suivez les mêmes étapes que précédemment pour supprimer l’ensemble de la ligne `-Output` que vous avez ajoutée auparavant. 

Pour plus d’informations, consultez [Activation de la journalisation détaillée pour les agents de réplication](https://support.microsoft.com/help/312292/how-to-enable-replication-agents-for-logging-to-output-files-in-sql-se). 


## <a name="see-also"></a>Voir aussi
<br>[Présentation de la réplication transactionnelle](../../relational-databases/replication/transactional/transactional-replication.md)
<br>[Tutoriels sur la réplication](../../relational-databases/replication/replication-tutorials.md)
<br>[Blog ReplTalk](https://blogs.msdn.microsoft.com/repltalk)

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]

