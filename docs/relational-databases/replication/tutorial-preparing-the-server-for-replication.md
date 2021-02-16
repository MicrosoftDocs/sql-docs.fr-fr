---
title: 'Tutoriel : Préparer pour la réplication'
description: Ce tutoriel vous montre comment préparer votre serveur de publication, votre serveur de distribution et votre Abonné pour la réplication en créant des comptes Windows, en préparant le dossier d’instantané et en configurant la distribution.
ms.custom: seo-lt-2019
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: ce30a095-2975-4387-9377-94a461ac78ee
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7506b3b57d81187269c26fae3cafc2d3cdbc6628
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348619"
---
# <a name="tutorial-prepare-sql-server-for-replication-publisher-distributor-subscriber"></a>Tutoriel : Préparer SQL Server pour la réplication (serveur de publication, serveur de distribution, abonné)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Vous devez planifier la sécurité avant de configurer la topologie de réplication. Ce didacticiel vous explique comment mieux sécuriser une topologie de réplication. Il montre également comment configurer la distribution, ce qui constitue la première étape de la réplication des données. Ce didacticiel doit être effectué avant les autres didacticiels.  
  
> [!NOTE]  
> Pour répliquer les données de façon sécurisée entre les serveurs, vous devez implémenter l’ensemble des recommandations de la rubrique [Méthodes préconisées en matière de sécurité de réplication](../../relational-databases/replication/security/replication-security-best-practices.md).  
  
## <a name="what-you-will-learn"></a>Contenu du didacticiel  
Ce tutoriel vous apprend à préparer un serveur afin que la réplication puisse s’exécuter de façon sécurisée avec les privilèges minimaux.  

Dans ce didacticiel, vous apprendrez à :
> [!div class="checklist"]
> * Créer des comptes Windows pour la réplication.
> * Préparer le dossier d’instantanés.
> * Configurer la distribution.

## <a name="prerequisites"></a>Prérequis
Ce tutoriel est destiné aux utilisateurs qui sont familiers des opérations essentielles de base de données, mais dont l’expérience en matière de réplication est limitée. 

Pour suivre ce tutoriel, vous avez besoin de SQL Server, SQL Server Management Studio (SSMS) et une base de données AdventureWorks :  
  
- Sur le serveur de publication (source), installez :  
  
   - Toute édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à l’exception de SQL Server Express ou SQL Server Compact. Ces éditions ne peuvent pas constituer des serveurs de publication de réplication.   
   - Exemple de base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Pour des raisons de sécurité, les exemples de bases de données ne sont pas installés par défaut.  
  
- Sur le serveur d’abonné (destination), installez une édition quelconque de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sauf [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] ne peut pas être un abonné dans une réplication transactionnelle.  
  
- Installez [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Téléchargez l’[exemple de base de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Pour obtenir des instructions sur la restauration d’une base de données dans SSMS, consultez [Restauration d’une base de données](../backup-restore/restore-a-database-backup-using-ssms.md). 
    
>[!NOTE]
> - La réplication n’est pas prise en charge sur les instances de SQL Server qui sont séparées de plus de deux versions. Pour plus d’informations, consultez [Supported SQL Server Versions in Replication Topology](replication-backward-compatibility.md) (Versions de SQL prises en charge dans la topologie de réplication).
> - Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous devez vous connecter au serveur de publication et à l’abonné à l’aide d’un identifiant de connexion membre du rôle serveur fixe **sysadmin**. Pour plus d’informations sur ce rôle, consultez [Rôles de niveau serveur](../security/authentication-access/server-level-roles.md).  


**Durée estimée pour terminer ce didacticiel : 30 minutes**
  
## <a name="create-windows-accounts-for-replication"></a>Créer des comptes Windows pour la réplication
Dans cette section, vous créez des comptes Windows pour exécuter les agents de réplication. Vous créez un compte Windows distinct sur le serveur local pour les agents suivants :  
  
|Agent|Emplacement|Nom du compte|  
|---------|------------|----------------|  
|Agent d'instantané|Serveur de publication|<*nom_ordinateur*>\repl_snapshot|  
|l'Agent de lecture du journal ;|Serveur de publication|<*nom_ordinateur*>\repl_logreader|  
|Agent de distribution|Serveur de publication et Abonné|<*nom_ordinateur*>\repl_distribution|  
|Agent de fusion|Serveur de publication et Abonné|<*nom_ordinateur*>\repl_merge|  
  
> [!NOTE]  
> Dans les didacticiels de réplication, le serveur de publication et le serveur de distribution partagent la même instance (NODE1\SQL2016) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’instance de l’abonné (NODE2\SQL2016) est distante. Le serveur de publication et l’abonné peuvent partager la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais ce n’est pas une obligation. Si le serveur de publication et l'Abonné partagent la même instance, les étapes de création de comptes pour l'Abonné ne sont pas requises.  

### <a name="create-local-windows-accounts-for-replication-agents-at-the-publisher"></a>Créer des comptes Windows locaux pour les agents de réplication sur le serveur de publication
  
1. Sur le serveur de publication, dans le Panneau de configuration, dans **Outils d’administration**, ouvrez **Gestion de l’ordinateur**.  
  
2. Dans **Outils système**, développez **Utilisateurs et groupes locaux**.  
  
3. Cliquez avec le bouton droit sur **Utilisateurs**, puis sélectionnez **Nouvel utilisateur**.  
     
4. Entrez **repl_snapshot** dans la zone **Nom d’utilisateur**, fournissez le mot de passe et autres informations appropriées, puis sélectionnez **Créer** pour créer le compte repl_snapshot : 

   ![Boîte de dialogue « Nouvel utilisateur »](media/tutorial-preparing-the-server-for-replication/newuser.png)
  
5. Répétez l’étape précédente pour créer les comptes repl_logreader, repl_distribution et repl_merge :  
 
   ![Liste des utilisateurs de la réplication](media/tutorial-preparing-the-server-for-replication/replusers.png)
  
6. Sélectionnez **Fermer**.  
  
### <a name="create-local-windows-accounts-for-replication-agents-at-the-subscriber"></a>Créer des comptes Windows locaux pour les agents de réplication sur l'Abonné  
  
1. Sur l’Abonné, dans le Panneau de configuration, dans **Outils d’administration**, ouvrez **Gestion de l’ordinateur**.  
  
2. Dans **Outils système**, développez **Utilisateurs et groupes locaux**.  
  
3. Cliquez avec le bouton droit sur **Utilisateurs**, puis sélectionnez **Nouvel utilisateur**.  
  
4. Entrez **repl_distribution** dans la zone **Nom d’utilisateur**, fournissez le mot de passe et autres informations appropriées, puis sélectionnez **Créer** pour créer le compte repl_distribution.  
  
5. Répétez l'étape précédente pour créer le compte repl_merge.  
  
6. Sélectionnez **Fermer**.  

Pour plus d'informations, voir [Présentation des agents de réplication](../../relational-databases/replication/agents/replication-agents-overview.md).  

## <a name="prepare-the-snapshot-folder"></a>Préparer le dossier d’instantanés
Dans cette section, vous configurez le dossier d’instantanés utilisé pour créer et stocker l’instantané des publications. 

### <a name="create-a-share-for-the-snapshot-folder-and-assign-permissions"></a>Créer un partage pour le dossier d’instantanés et attribuer les autorisations  
  
1. Dans l’Explorateur de fichiers, accédez au dossier de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'emplacement par défaut est : C:\Program Files\Microsoft SQL Server\MSSQL.X\MSSQL\Data.  
  
2. Créez un dossier nommé **repldata**.  
  
3. Cliquez avec le bouton droit sur ce dossier et sélectionnez **Propriétés**.  
  
   a. Sous l’onglet **Partage** de la boîte de dialogue **Propriétés de repldata**, sélectionnez **Partage avancé**.  
  
   b. Dans la boîte de dialogue **Partage avancé**, sélectionnez **Partager ce dossier**, puis sélectionnez **Autorisations**.  

   ![Sélections pour le partage du dossier repldata](media/tutorial-preparing-the-server-for-replication/repldata.png)

6. Dans la boîte de dialogue **Autorisations pour repldata**, sélectionnez **Ajouter**. Dans la zone de texte **Sélectionnez Utilisateurs, Ordinateurs, Compte de service ou Groupes**, tapez le nom du compte d’Agent d’instantané créé précédemment, sous la forme <*nom_ordinateur_serveur_de_publication*> **\repl_snapshot**. Sélectionnez **Vérifier les noms**, puis sélectionnez **OK**.  

   ![Sélections pour ajouter des autorisations de partage](media/tutorial-preparing-the-server-for-replication/addshareperms.png)

7. Répétez l’étape 6 pour ajouter les deux autres comptes que vous avez créés précédemment : <*nom_ordinateur_serveur_de_publication*> **\repl_merge** et <*nom_ordinateur_serveur_de_publication*> **\repl_distribution**.

8. Après avoir ajouté les trois comptes, attribuez les autorisations suivantes :      
   - repl_distribution : **Lire**  
   - repl_merge : **Lire**  
   - repl_snapshot : **Contrôle total**    

   ![Autorisations partagées pour chaque compte](media/tutorial-preparing-the-server-for-replication/sharedpermissions.png)

9. Une fois vos autorisations de partage correctement configurées, sélectionnez **OK** pour fermer la boîte de dialogue **Autorisations pour repldata**. Sélectionnez **OK** pour fermer la boîte de dialogue **Partage avancé**. 

10. Dans la boîte de dialogue **Propriétés de repldata**, sous l’onglet **Sécurité**, sélectionnez **Modifier** :  

    ![Bouton « Modifier » sous l’onglet « Sécurité »](media/tutorial-preparing-the-server-for-replication/editsecurity.png)   

11. Dans la boîte de dialogue **Autorisations pour repldata**, sélectionnez **Ajouter**. Dans la zone de texte **Sélectionnez Utilisateurs, Ordinateurs, Compte de service ou Groupes**, tapez le nom du compte d’Agent d’instantané créé précédemment, sous la forme <*nom_ordinateur_serveur_de_publication*> **\repl_snapshot**. Sélectionnez **Vérifier les noms**, puis sélectionnez **OK**.  

    ![Sélections pour ajouter des autorisations de sécurité](media/tutorial-preparing-the-server-for-replication/addsecuritypermissions.png)

  
12. Répétez l’étape précédente pour ajouter des autorisations pour l’Agent de distribution, sous la forme <*nom_ordinateur_serveur_de_publication*> **\repl_distribution**, et pour l’Agent de fusion, sous la forme <*nom_ordinateur_serveur_de_publication*> **\repl_merge**.  
    
  
13. Vérifiez que les autorisations suivantes sont accordées :  
  
    - repl_distribution : **Lire**
    - repl_merge : **Lire**
    - repl_snapshot : **Contrôle total**   
 
    ![Autorisations d'utilisateur pour les données de réplication](media/tutorial-preparing-the-server-for-replication/replpermissions.png) 

14. Sous l’onglet **Partage**, notez le **Chemin réseau** pour le partage. Vous aurez besoin de ce chemin quand vous configurerez votre dossier d’instantanés.  

    ![Chemin réseau sous l’onglet « Partage »](media/tutorial-replicating-data-between-continuously-connected-servers/networkpath.png)

15. Sélectionnez **OK** pour fermer la boîte de dialogue **Propriétés de repldata**. 
 
Pour plus d’informations, consultez [Sécuriser le dossier d’instantanés](../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  

## <a name="configure-distribution"></a>Configurer la distribution
Dans cette section, vous configurez la distribution sur le serveur de publication et définir les autorisations nécessaires sur les bases de données de publication et de distribution. Si vous avez déjà configuré le serveur de distribution, vous devez désactiver la publication et la distribution avant de commencer cette section. Ne procédez pas ainsi si vous devez conserver une topologie de réplication existante, notamment en production.   
  
La configuration d'un serveur de publication avec un serveur de distribution distant dépasse le cadre de ce didacticiel.  

### <a name="configure-distribution-at-the-publisher"></a>Configurer la distribution sur le serveur de publication  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2. Cliquez avec le bouton droit sur le dossier **Réplication**, puis sélectionnez **Configurer la distribution** :  

   ![Commande « Configurer la distribution » du menu contextuel](media/tutorial-preparing-the-server-for-replication/configuredistribution.png)
  
   > [!NOTE]  
   > - Si vous êtes connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de **localhost** au lieu du nom du serveur réel, un avertissement vous indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas se connecter à **localhost ou à l’adresse IP**. Sélectionnez **OK** dans la boîte de dialogue d’avertissement. Dans la boîte de dialogue **Se connecter au serveur**, remplacez le **nom du serveur** qui indique **localhost ou une adresse IP** par le nom de votre serveur. Sélectionnez **Connecter**.  
   > - Il existe actuellement un problème connu avec SQL Server Management Studio (SSMS) 18.0 (et versions ultérieures) lors duquel un message d’avertissement _ne s’affiche pas_ si vous tentez de vous connecter au serveur de distribution avec l’adresse IP alors qu’une telle connexion n’est pas possible. Le nom du serveur doit être utilisé lors de la connexion au serveur de distribution. 
   
   L'Assistant Configuration de la distribution démarre.  
  
3. Dans la page **Serveur de distribution**, sélectionnez < *'nom_serveur'* > **agit comme son propre serveur de distribution ; SQL Server crée une base de données de distribution et un journal**. Sélectionnez ensuite **Suivant**.  

   ![Option permettant au serveur d’agir comme son propre serveur de distribution](media/tutorial-preparing-the-server-for-replication/serverdistributor.png)
  
4. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ne s’exécute pas, dans la page **Démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent**, sélectionnez **Oui, configurez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour qu’il démarre automatiquement**. Sélectionnez **Suivant**.  

     
5. Entrez le chemin \\\\<*nom_ordinateur_serveur_de_publication*> **\repldata** dans la zone **Dossier d’instantanés**, puis sélectionnez **Suivant**. Ce chemin doit correspondre au **Chemin réseau** du dossier des propriétés de repldata, tel que vous l’avez défini quand vous avez configuré les propriétés du partage. 

   ![Comparaison des chemins réseau dans la boîte de dialogue « Propriétés de repldata » et dans l’Assistant Configuration de la distribution](media/tutorial-preparing-the-server-for-replication/repldatasnapshot.png)
  
6. Acceptez les valeurs par défaut sur les pages restantes de l'Assistant.  
    
   ![Dernière page de l’Assistant](media/tutorial-preparing-the-server-for-replication/distributionwizarddefaults.png)
  
7. Sélectionnez **Terminer** pour activer la distribution. 

L’erreur suivante peut apparaître quand vous configurez le serveur de distribution. Elle indique que le compte utilisé pour démarrer le compte SQL Server Agent n’est pas administrateur sur le système. Vous devez alors démarrer SQL Server Agent manuellement, accorder ces autorisations à un compte existant ou changer le compte utilisé par SQL Server Agent. 

![Message d’erreur de configuration de SQL Server Agent](media/tutorial-preparing-the-server-for-replication/startingagenterror.png)

Si votre instance SQL Server Management Studio s’exécute avec des droits d’administration, vous pouvez démarrer l’Agent SQL manuellement à partir de SSMS :  
    
![Sélection de l’option « Démarrer » du menu contextuel de l’agent dans SSMS](media/tutorial-preparing-the-server-for-replication/ssmsstartagent.png) 

>[!NOTE]
> Si l’Agent SQL semble ne pas démarrer, cliquez avec le bouton droit sur SQL Server Agent dans SSMS, puis sélectionnez **Actualiser**. S’il demeure dans l’état arrêté, démarrez-le manuellement à partir du Gestionnaire de configuration SQL Server.    
  
## <a name="set-database-permissions"></a>Définir les autorisations de base de données  
  
1. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez **Sécurité**, cliquez avec le bouton droit sur **Connexions**, puis sélectionnez **Nouvelle connexion** :  

   ![Commande « Nouvelle connexion » du menu contextuel](media/tutorial-preparing-the-server-for-replication/newlogin.png)
  
2. Dans la page **Général**, sélectionnez **Rechercher**. Entrez <*nom_ordinateur_serveur_de_publication*> **\repl_snapshot** dans la zone **Entrer le nom d’objet à sélectionner**, sélectionnez **Vérifier les noms**, puis sélectionnez **OK**.  

   ![Sélections pour entrer le nom de l’objet](media/tutorial-preparing-the-server-for-replication/addsnapshotlogin.png)
  
3. Sur la page **Mappage de l'utilisateur**, dans la liste **Utilisateurs mappés à cette connexion**, sélectionnez les bases de données **distribution** et [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
   Dans la liste Appartenance au rôle de base de données, sélectionnez le rôle **db_owner** pour la connexion pour les deux bases de données.  

   ![Sélection des bases de données et de leur rôle](media/tutorial-preparing-the-server-for-replication/dbowner.png)
  
4. Sélectionnez **OK** pour créer la connexion.  
  
5. Répétez les étapes 1 à 4 pour créer une connexion pour les autres comptes locaux (repl_distribution, repl_logreader et repl_merge). Ces connexions doivent aussi être mappées aux utilisateurs membres du rôle de base de données fixe **db_owner** des bases de données de **distribution** et **AdventureWorks**.  

   ![Affichage des quatre comptes dans l’Explorateur d’objets](media/tutorial-preparing-the-server-for-replication/usersinssms.png)
   
  
Pour plus d'informations, consultez les pages suivantes :
- [Configurer la distribution](../../relational-databases/replication/configure-distribution.md) 
- [Modèle de sécurité de l'Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md)  

## <a name="next-steps"></a>Étapes suivantes
Vous avez correctement préparé votre serveur pour la réplication. L’article suivant vous apprend à configurer la réplication transactionnelle : 

> [!div class="nextstepaction"]
> [Tutoriel : configurer la réplication entre deux serveurs intégralement connectés (réplication transactionnelle)](tutorial-replicating-data-between-continuously-connected-servers.md)

  
