---
title: Créer un abonnement pour un Abonné non-SQL Server
description: Décrit comment créer un abonnement pour un Abonné non-SQL Server dans SQL Server à l’aide de SQL Server Management Studio (SSMS) ou de Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [SQL Server replication], non-SQL Server Subscribers
- Subscribers [SQL Server replication], non-SQL Server Subscribers
- non-SQL Server Subscribers, subscriptions
ms.assetid: 5020ee68-b988-4d57-8066-67d183e61237
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 36c15a9d7de2c613b3a25b16827bfcc037393524
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076221"
---
# <a name="create-a-subscription-for-a-non-sql-server-subscriber"></a>Créer un abonnement pour un Abonné non-SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer un abonnement pour un abonné non-SQL Server dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La réplication transactionnelle et la réplication de capture instantanée prennent en charge la publication de données vers des abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations sur les plateformes d'Abonné prises en charge, consultez [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
 **Dans cette rubrique**  
  
-   **Pour créer une publication destinée à un Abonné non-SQL Server à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Pour créer un abonnement destiné à un abonné non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
1.  Installez et configurez le logiciel client et le ou les fournisseurs OLE DB approprié sur le serveur de distribution [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d'informations, consultez [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md) et [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
2.  Créez une publication avec l'Assistant Nouvelle publication. Pour plus d’informations sur la création de publications, consultez [Créer une publication](../../relational-databases/replication/publish/create-a-publication.md) et [Créer une publication à partir d’une base de données Oracle](../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md). Spécifiez les options suivantes dans l'Assistant Nouvelle publication :  
  
    -   Dans la page **Type de publication** , sélectionnez **Publication d'instantané** ou **Publication transactionnelle**.  
  
    -   Dans la page **Agent d'instantané** , désactivez **Créer un instantané immédiatement**.  
  
         Vous devez créer l'instantané après avoir activé la publication pour les Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin de garantir que l'Agent d'instantané génère un instantané et des scripts d'initialisation qui conviennent pour des Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  Activez la publication pour les Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de la boîte de dialogue **Propriétés de la publication - \<PublicationName>** . Consultez [Publication Properties, Subscription Options](../../relational-databases/replication/publication-properties-subscription-options.md) pour plus d'informations sur cette étape.  
  
4.  Créez un abonnement avec l'Assistant Nouvel abonnement. Cette rubrique fournit des informations sur cette étape.  
  
5.  (Facultatif) Modifiez la propriété d'article **pre_creation_cmd** pour conserver les tables sur l'Abonné. Cette rubrique fournit des informations sur cette étape.  
  
6.  Générez un instantané de la publication. Cette rubrique fournit des informations sur cette étape.  
  
7.  Synchronisez l'abonnement. Pour plus d’informations, consultez [Synchroniser un abonnement par émission (push)](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-enable-a-publication-for-non-sql-server-subscribers"></a>Pour activer une publication pour des Abonnés non-SQL Server  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Cliquez avec le bouton droit sur la publication, puis sur **Propriétés**.  
  
4.  Dans la page **Options d'abonnement** , sélectionnez la valeur **True** pour l'option **Autoriser les Abonnés non-SQL Server**. Cette option modifie certaines propriétés afin que la publication soit compatible avec les Abonnés non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    > [!NOTE]  
    >  Le choix de la valeur **True** définit alors la valeur de l'article **pre_creation_cmd** à « drop ». Ce paramètre indique que la réplication doit supprimer une table au niveau de l'Abonné si elle correspond au nom de la table dans l'article. Si vous disposez de tables existantes au niveau de l'Abonné que vous souhaitez conserver, utilisez la procédure stockée [sp_changearticle](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) pour chaque article ; précisez une valeur 'none' pour **pre_creation_cmd**: `sp_changearticle @publication= 'MyPublication', @article= 'MyArticle', @property='pre_creation_cmd', @value='none'`.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] Vous êtes invité à créer un nouvel instantané de la publication. Si vous ne souhaitez pas le faire immédiatement, vous utiliserez plus tard la procédure décrite plus loin dans la prochaine procédure.  
  
#### <a name="to-create-a-subscription-for-a-non-sql-server-subscriber"></a>Pour créer une publication destinée à un Abonné non-SQL Server  
  
1.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
2.  Cliquez avec le bouton droit sur la publication appropriée, puis sur **Nouveaux abonnements**.  
  
3.  Dans la page **Emplacement de l'Agent de distribution** , vérifiez que **Exécuter tous les agents sur le serveur de distribution** est bien sélectionné. Les Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prennent pas en charge les agents qui s'exécutent au niveau de l'Abonné.  
  
4.  Dans la page **Abonnés** , cliquez sur **Ajouter un Abonné** puis sur **Ajouter un Abonné non-SQL Server**.  
  
5.  Dans la boîte de dialogue **Ajouter un Abonné non-SQL Server** , sélectionnez le type d'Abonné.  
  
6.  Entrez une valeur dans **Nom de la source de données**:  
  
    -   Pour Oracle, cette valeur est le nom du TNS (transparent network substrate) que vous avez configuré.  
  
    -   Pour IBM, ce peut être n'importe quel nom. En règle générale, spécifiez l'adresse réseau de l'Abonné.  
  
     Le nom de la source de données entré à cette étape et les informations de connexion spécifiées à l'étape 9 ne sont pas validés par cet Assistant. Ils ne sont pas utilisés par la réplication avant que l'Agent de distribution ne s'exécute pour l'abonnement. Assurez-vous que toutes les valeurs ont été testées en vous connectant à l'Abonné à l'aide d'un outil client (par exemple **sqlplus** pour Oracle). Pour plus d'informations, consultez [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md) et [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] Dans la page **Abonnés** de l'Assistant, l'Abonné s'affiche maintenant dans la colonne **Abonné** avec une **(destination par défaut)** en lecture seule dans la colonne **Base de données d'abonnement** :  
  
    -   Sur Oracle, un serveur a au plus une base de données, il n'est donc pas nécessaire de la spécifier.  
  
    -   Pour IBM DB2, la base de données est spécifiée dans la propriété **Catalogue initial** de la chaîne de connexion DB2, qui peut être entrée dans le champ **Options de connexion supplémentaires** décrit plus loin.  
  
8.  Dans la page **Sécurité de l'Agent de distribution** , cliquez sur le bouton de propriétés ( **…** ) à côté de l'Abonné pour accéder à la boîte de dialogue **Sécurité de l'Agent de distribution** .  
  
9. Dans la boîte de dialogue **Sécurité de l'Agent de distribution** :  
  
    -   Dans les champs **Compte de processus**, **Mot de passe** et **Confirmer le mot de passe** , entrez le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows et le mot de passe sous lequel l'Agent de distribution doit s'exécuter et se connecter en local sur le serveur de distribution.  
  
         Ce compte a besoin des autorisations minimales suivantes : membre du rôle de base de données fixe **db_owner** dans la base de données de distribution ; membre de la PAL (liste d'accès à la publication) ; autorisations de lecture sur le partage de fichiers d'instantanés ; et autorisation de lecture sur le répertoire d'installation du fournisseur OLE DB. Pour plus d’informations sur la liste d’accès à la publication, consultez [Sécuriser le serveur de publication](../../relational-databases/replication/security/secure-the-publisher.md).  
  
    -   Sous **Connexion à l'Abonné**, dans les champs **Connexion**, **Mot de passe** et **Confirmer le mot de passe** , entrez la connexion et le mot de passe à utiliser pour se connecter à l'Abonné. Cette connexion devrait déjà être configurée et devrait avoir des autorisations suffisantes pour créer des objets dans la base de données d'abonnement.  
  
    -   Dans le champ **Options de connexion supplémentaires** , spécifiez les autres options de connexion pour l'Abonné sous la forme d'une chaîne de connexion (Oracle ne nécessite pas d'options supplémentaires). Chaque option doit être délimitée par un point-virgule. Voici un exemple de chaîne de connexion DB2 (les retours à la ligne facilitent la lisibilité) :  
  
        ```  
        Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
        PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
        Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
        Persist Security Info=False;Connection Pooling=True;  
        ```  
  
         La plupart des options de cette chaîne sont spécifiques du serveur DB2 que vous configurez, mais vous devez attribuer à l'option **Traiter les données binaires comme des caractères** la valeur **False**. Une valeur est exigée de façon que l'option **Catalogue initial** identifie la base de données d'abonnement.  
  
10. Dans la page **Planification de synchronisation** , sélectionnez une planification pour l'Agent de distribution à partir du menu **Planification de l'agent** (en règle générale, **Exécuter en continu**).  
  
11. Dans la page **Initialiser les abonnements** , spécifiez si l'abonnement doit être initialisé et, dans ce cas, quand l'initialiser :  
  
    -   Ne désactivez **Initialiser** que si vous avez créé tous les objets et ajouté toutes les données nécessaires dans la base de données d'abonnement.  
  
    -   Sélectionnez **Immédiatement** dans la liste déroulante de la colonne **À quel moment** pour que l'Agent de distribution transfère les fichiers d'instantanés à l'Abonné lorsque l'Assistant se termine. Sélectionnez **Lors de la première synchronisation** pour que l'Agent transfère les fichiers lors de la prochaine exécution planifiée de l'Agent.  
  
12. Sur la page **Actions de l'Assistant** , scriptez en option l'abonnement. Pour plus d'informations, voir [Scripting Replication](../../relational-databases/replication/scripting-replication.md).  
  
#### <a name="to-retain-tables-at-the-subscriber"></a>Pour conserver les tables sur l'Abonné  
  
-   Par défaut, le fait d'activer une publication pour des Abonnés non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit la valeur de la propriété d'article **pre_creation_cmd** à 'drop'. Ce paramètre indique que la réplication doit supprimer une table au niveau de l'Abonné si elle correspond au nom de la table dans l'article. Si vous disposez de tables existantes au niveau de l'Abonné que vous souhaitez conserver, utilisez la procédure stockée [sp_changearticle](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) pour chaque article ; précisez une valeur 'none' pour **pre_creation_cmd**. `sp_changearticle @publication= 'MyPublication', @article= 'MyArticle', @property='pre_creation_cmd', @value='none'`.  
  
#### <a name="to-generate-a-snapshot-for-the-publication"></a>Pour générer un instantané de la publication  
  
1.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
2.  Cliquez avec le bouton droit sur la publication, puis cliquez sur **Afficher l'état de l'Agent d'instantané**.  
  
3.  Dans la boîte de dialogue **Afficher l’état de l’Agent d’instantané - \<Publication>** , cliquez sur **Démarrer**.  
  
 Lorsque l'Agent d'instantané a terminé, un message s'affiche, par exemple, « [100%] Un instantané de 17 articles a été généré. »  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Vous pouvez créer par programmation des abonnements par émission de données pour des abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant des procédures stockées de réplication.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
#### <a name="to-create-a-push-subscription-for-a-transactional-or-snapshot-publication-to-a-non-sql-server-subscriber"></a>Pour créer un abonnement par émission de données pour une publication transactionnelle ou d'instantané pour un Abonné non-SQL Server  
  
1.  Installez le fournisseur OLE DB le plus récent pour l'Abonné non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au niveau du serveur de publication et du serveur de distribution. Pour connaître les spécifications de réplication pour un fournisseur OLE DB, consultez [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md), [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md), [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
2.  Dans la base de données de publication sur le serveur de publication, vérifiez que la publication prend en charge les Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en exécutant [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md).  
  
    -   Si **enabled_for_het_sub** a la valeur 1, les Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont pris en charge.  
  
    -   Si **enabled_for_het_sub** a la valeur 0, exécutez [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), en spécifiant **enabled_for_het_sub** pour `@property` et **true** pour `@value`.  
  
        > [!NOTE]  
        >  Avant de remplacer la valeur de **enabled_for_het_sub** par **true**, vous devez supprimer tout abonnement existant à la publication. Vous ne pouvez pas affecter la valeur **enabled_for_het_sub** à **true** lorsque la publication prend également en charge la mise à jour des abonnements. La modification de **enabled_for_het_sub** affectera d'autres propriétés de publication. Pour plus d’informations, consultez [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
3.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Spécifiez `@publication`, `@subscriber`, la valeur `(default destination)` pour `@destination_db`, la valeur **push** pour `@subscription_type` et la valeur 3 pour `@subscriber_type` (spécifie un fournisseur OLE DB).  
  
4.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Spécifiez les éléments suivants :  
  
    -   Les paramètres `@subscriber` et `@publication`.  
  
    -   La valeur **(destination par défaut)** pour `@subscriber_db`.  
  
    -   Les propriétés de la source de données non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour `@subscriber_provider`, `@subscriber_datasrc`, `@subscriber_location`, `@subscriber_provider_string` et `@subscriber_catalog`.  
  
    -   Les informations d’identification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lesquelles l’Agent de distribution est exécuté sur le serveur de distribution pour `@job_login` et `@job_password`.  
  
       > [!NOTE]  
       > Les connexions effectuées à l’aide de l’authentification Windows intégrée utilisent toujours les informations d’identification Windows spécifiées par `@job_login` et `@job_password`. L'Agent de distribution crée toujours la connexion locale au serveur de distribution à l'aide de l'authentification intégrée Windows. Par défaut, l'agent se connecte à l'Abonné à l'aide de ces informations.  
  
    -   La valeur **0** pour `@subscriber_security_mode` et les informations de connexion du fournisseur OLE DB pour `@subscriber_login` et `@subscriber_password`.  
  
    -   Planification du travail de l'Agent de distribution pour cet abonnement. Pour plus d’informations, consultez [Spécifier des planifications de synchronisation](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!IMPORTANT]  
    >  Lors de la création d'un abonnement par émission de données sur un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées au serveur de distribution en texte brut. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
## <a name="see-also"></a>Voir aussi  
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)   
 [Autres abonnés non-SQL Server](../../relational-databases/replication/non-sql/other-non-sql-server-subscribers.md)   
 [Concepts liés aux procédures stockées système de réplication](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
