---
title: Afficher et modifier les paramètres de sécurité de la réplication | Microsoft Docs
description: Découvrez comment afficher et modifier les paramètres de sécurité de réplication dans SQL Server à l’aide de SQL Server Management Studio, Transact-SQL ou Replication Management Objects.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying replication security settings
- replication [SQL Server], security
- security [SQL Server replication], viewing settings
- viewing replication security settings
- security [SQL Server replication], modifying settings
ms.assetid: 67d79532-1482-4de1-ac9f-4a23d162c85e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 4eb7b7530c498a6ce590584aa83b3f38684960a7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170771"
---
# <a name="view-and-modify-replication-security-settings"></a>Afficher et modifier les paramètres de sécurité de la réplication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique décrit comment afficher et modifier les paramètres de sécurité de la réplication dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects). Par exemple, vous voulez peut-être modifier la connexion de l'Agent de lecture du journal au serveur de publication de l'authentification SQL Server à l'authentification intégrée de Windows, ou vous avez peut-être besoin de modifier les informations d'identification utilisées pour exécuter un travail de l'Agent lorsque le mot de passe du compte Windows a changé. Pour plus d’informations sur les autorisations requises par chaque agent, consultez [Modèle de sécurité de l’Agent de réplication](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour afficher et modifier les paramètres de sécurité de la réplication à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
-   **Suivi :**  [après avoir modifié les paramètres de sécurité de la réplication](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Les procédures stockées à utiliser dépendent du type d'agent et du type de connexion serveur.  
  
-   Les classes et les propriétés RMO que vous utilisez dépendent du type d'agent et du type de connexion au serveur.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Pour des raisons de sécurité, les valeurs réelles des mots de passe sont masquées dans les jeux de résultats retournés par les procédures stockées de réplication.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Affichez et modifiez les paramètres de sécurité dans les boîtes de dialogue suivantes :  
  
1.  La boîte de dialogue **Mettre à jour les mots de passe de réplication** , disponible à partir du dossier **Réplication** de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Si vous modifiez le mot de passe d'un compte [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou d'un compte Windows sur un serveur de la topologie de réplication, utilisez cette boîte de dialogue plutôt que de mettre à jour le mot de passe pour chaque agent utilisant le compte. Si des agents utilisent le même compte sur plus d'un serveur, vous devez vous connecter à chaque serveur et modifier le mot de passe. Les mots de passe sont mis à jour partout où la réplication utilise le mot de passe. Le mot de passe n'est pas mis à jour ailleurs, comme sur les serveurs liés.  
  
2.  La page **Sécurité de l’agent** de la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
3.  La boîte de dialogue **Propriétés de l’abonnement - \<Subscription>** . Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [View and Modify Push Subscription Properties](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md) et [View and Modify Pull Subscription Properties](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
4.  Les boîtes de dialogue **Propriétés du serveur de distribution - \<Distributor>** et **Propriétés de la base de données de distribution - \<Database>** . Pour plus d'informations sur l'accès à ces boîtes de dialogue, consultez [View and Modify Distributor and Publisher Properties](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
5.  La boîte de dialogue **Propriétés du serveur de publication - \<Publisher>** . Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [View and Modify Distributor and Publisher Properties](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  

#### <a name="to-change-the-password-for-an-account-used-by-one-or-more-agents"></a>Pour modifier le mot de passe d'un compte utilisé par un ou plusieurs agents  
  
1.  S'il s'agit d'un compte SQL Server, cette boîte de dialogue modifiera également le mot de passe pour le compte SQL Server. S'il s'agit d'un compte Windows, modifiez d'abord le mot de passe dans Windows. Pour plus d'informations, consultez la documentation Windows.  
  
    > [!NOTE]  
    >  Après avoir modifié un mot de passe de réplication, vous devez arrêter puis redémarrer chaque Agent qui utilise ce mot de passe afin que les modifications apportées prennent effet.  
  
2.  Connectez-vous au serveur dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]puis développez le nœud du serveur.  
  
3.  Cliquez avec le bouton droit sur le dossier **Réplication** , puis cliquez sur **Mettre à jour les mots de passe de réplication**.  
  
4.  Dans la boîte de dialogue **Mettre à jour les mots de passe de réplication** , spécifiez le compte et le nouveau mot de passe.  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-snapshot-agent"></a>Pour modifier les paramètres de sécurité pour l'Agent d'instantané  
  
1.  Dans la page **Sécurité de l’agent** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur le bouton **Paramètres de sécurité** à côté de la zone de texte **Agent d’instantané**.  
  
2.  Dans la boîte de dialogue **Sécurité de l'Agent d'instantané** , spécifiez le compte sous lequel l'Agent doit s'exécuter :  
  
    -   Entrez un nouveau compte Windows dans la zone de texte **Compte de l'agent** .  
  
    -   Entrez un nouveau mot de passe fort dans les zones de texte **Mot de passe** et **Confirmer le mot de passe** .  
  
3.  Spécifiez le contexte sous lequel l'Agent doit se connecter du serveur de distribution au serveur de publication. Si vous sélectionnez **En utilisant la connexion SQL Server suivante**, vous devez également spécifier la connexion :  
  
    -   Entrez une connexion dans la zone de texte **Connexion** .  
  
    -   Entrez un nouveau mot de passe fort dans les zones de texte **Mot de passe** et **Confirmer le mot de passe** .  
  
    > [!NOTE]  
    >  Si le serveur de publication est un serveur de publication Oracle, le contexte de connexion est spécifié dans la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** . Consultez la procédure de modification du contexte ci-dessous.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-log-reader-agent"></a>Pour modifier les paramètres de sécurité pour l'Agent de lecture du journal  
  
1.  Dans la page **Sécurité de l’agent** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur le bouton **Paramètres de sécurité** à côté de la zone de texte **Agent de lecture du journal**.  
  
2.  Dans la boîte de dialogue **Sécurité de l'Agent de lecture du journal** , spécifiez le compte sous lequel l'Agent doit s'exécuter :  
  
    -   Entrez un nouveau compte Windows dans la zone de texte **Compte de l'agent** .  
  
    -   Entrez un nouveau mot de passe fort dans les zones de texte **Mot de passe** et **Confirmer le mot de passe** .  
  
3.  Spécifiez le contexte sous lequel l'Agent doit se connecter du serveur de distribution au serveur de publication. Si vous sélectionnez **En utilisant la connexion SQL Server suivante**, vous devez également spécifier la connexion :  
  
    -   Entrez une connexion dans la zone de texte **Connexion** .  
  
    -   Entrez un nouveau mot de passe fort dans les zones de texte **Mot de passe** et **Confirmer le mot de passe** .  
  
    > [!NOTE]  
    >  Si le serveur de publication est un serveur de publication Oracle, le contexte de connexion est spécifié dans la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** . Modifiez le contexte à l'aide de la procédure suivante.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Il existe un Agent de lecture du journal pour chaque base de données publiée. La modification des paramètres de sécurité sur l'agent d'une publication affecte les paramètres de toutes les publications dans la base de données de publication.  
  
#### <a name="to-change-the-context-under-which-the-snapshot-agent-and-log-reader-agent-for-an-oracle-publication-make-connections-to-the-publisher"></a>Pour modifier le contexte sous lequel l'Agent d'instantané et l'Agent de lecture du journal d'une publication Oracle établissent des connexions sur le serveur de publication  
  
1.  Dans la page **Serveurs de publication** de la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** , cliquez sur le bouton des propriétés ( **...** ) à côté d’un serveur de publication.  
  
2.  Dans la section **Connexion de l'Agent au serveur de publication** , spécifiez le nom d'accès et le mot de passe utilisés par le schéma utilisateur d'administration de réplication que vous avez configuré. Pour plus d’informations, consultez [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-push-subscription"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de distribution pour un abonnement par émission de données  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Subscription>** sur le serveur de publication, vous pouvez effectuer les modifications suivantes :  
  
    -   Pour modifier le compte sous lequel l’Agent de distribution s’exécute et établit des connexions au serveur de distribution, cliquez sur la ligne **Compte de processus de l’agent**, puis sur le bouton des propriétés ( **...** ) dans la ligne. Spécifiez un compte et un mot de passe dans la boîte de dialogue **Sécurité de l'Agent de distribution** .  
  
    -   Pour modifier le contexte sous lequel l’Agent de distribution se connecte à l’Abonné, cliquez sur la ligne **Connexion de l’Abonné**, puis sur le bouton des propriétés ( **…** ) dans la ligne. Spécifiez le contexte dans la boîte de dialogue **Entrer les informations de connexion** .  
  
         Si vous utilisez les abonnements de mise à jour en attente, l'Agent de lecture de la file d'attente utilise également le contexte spécifié ici pour les connexions à l'Abonné.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-pull-subscription"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de distribution pour un abonnement par extraction  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Subscription>** sur l’abonné, vous pouvez effectuer les modifications suivantes :  
  
    -   Pour modifier le compte sous lequel l’Agent de distribution s’exécute et établit des connexions à l’Abonné, cliquez sur la ligne **Compte de processus de l’agent**, puis sur le bouton des propriétés ( **...** ) dans la ligne. Spécifiez un compte et un mot de passe dans la boîte de dialogue **Sécurité de l'Agent de distribution** .  
  
         Si vous utilisez les abonnements de mise à jour en attente, l'Agent de lecture de la file d'attente utilise également le contexte spécifié ici pour les connexions à l'Abonné.  
  
    -   Pour modifier le contexte sous lequel l’Agent de distribution se connecte au serveur de distribution, cliquez sur la ligne **Connexion du serveur de distribution**, puis sur le bouton des propriétés ( **…** ) dans la ligne. Spécifiez le contexte dans la boîte de dialogue **Entrer les informations de connexion** .  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-push-subscription"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de fusion pour un abonnement par émission de données  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Subscription>** sur le serveur de publication, vous pouvez effectuer les modifications suivantes :  
  
    -   Pour modifier le compte sous lequel l’Agent de fusion s’exécute et établit des connexions au serveur de publication et au serveur de distribution, cliquez sur la ligne **Compte de processus de l’agent**, puis sur le bouton des propriétés ( **...** ) dans la ligne. Spécifiez un compte et un mot de passe dans la boîte de dialogue **Sécurité de l'Agent de fusion** .  
  
    -   Pour modifier le contexte sous lequel l’Agent de fusion se connecte à l’Abonné, cliquez sur la ligne **Connexion de l’Abonné**, puis sur le bouton des propriétés ( **…** ) dans la ligne. Spécifiez le contexte dans la boîte de dialogue **Entrer les informations de connexion** .  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-pull-subscription"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de fusion pour un abonnement par extraction  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Subscription>** sur l’abonné, vous pouvez effectuer les modifications suivantes :  
  
    -   Pour modifier le compte sous lequel l’Agent de fusion s’exécute et établit des connexions à l’Abonné, cliquez sur la ligne **Compte de processus de l’agent**, puis sur le bouton des propriétés ( **...** ) dans la ligne. Spécifiez un compte et un mot de passe dans la boîte de dialogue **Sécurité de l'Agent de fusion** .  
  
    -   Pour modifier le contexte sous lequel l’Agent de fusion se connecte au serveur de publication et au serveur de distribution, cliquez sur la ligne **Connexion du serveur de publication**, puis sur le bouton des propriétés ( **…** ) dans la ligne. Spécifiez le contexte dans la boîte de dialogue **Entrer les informations de connexion** .  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-the-account-under-which-the-queue-reader-agent-runs"></a>Pour modifier le compte sous lequel s'exécute l'Agent de lecture de la file d'attente  
  
1.  Dans la page **Général** de la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** , cliquez sur le bouton des propriétés ( **…** ) en regard de la base de données de distribution.  
  
2.  Dans la boîte de dialogue **Propriétés de la base de données de distribution - \<Database>** , cliquez sur le bouton **Paramètres de sécurité** en regard de la zone de texte **Compte de processus de l’agent**.  
  
3.  Dans la boîte de dialogue **Sécurité de l'Agent de lecture de la file d'attente** , spécifiez le compte sous lequel l'agent s'exécute et établit des connexions sur le serveur de distribution :  
  
    -   Entrez un nouveau compte Windows dans la zone de texte **Compte de processus** .  
  
    -   Entrez un nouveau mot de passe fort dans les zones de texte **Mot de passe** et **Confirmer le mot de passe** .  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Il existe un Agent de lecture de la file d'attente pour chaque base de données de distribution. La modification des paramètres de sécurité pour l'agent affecte les paramètres de toutes les publications sur tous les serveurs de publication qui utilisent cette base de données de distribution.  
  
#### <a name="to-change-the-context-under-which-the-queue-reader-agent-makes-connections-to-the-publisher"></a>Pour modifier le contexte sous lequel l'Agent de lecture de la file d'attente établit des connexions au serveur de publication  
  
1.  Dans la page **Serveurs de publication** de la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** , cliquez sur le bouton des propriétés ( **...** ) en regard du serveur de publication.  
  
2.  Dans la section **Connexion de l'Agent au serveur de publication** , spécifiez une valeur **Imiter le compte de processus de l'Agent** ou **Authentification SQL Server** pour l'option **Mode de connexion de l'agent** . Si vous spécifiez **Authentification SQL Server**, indiquez également des valeurs pour **Nom d'accès** et **Mot de passe**.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Il existe un Agent de lecture de la file d'attente pour chaque base de données de distribution. La modification des paramètres de sécurité pour l'agent affecte les paramètres de toutes les publications sur tous les serveurs de publication qui utilisent cette base de données de distribution.  
  
#### <a name="to-change-the-context-under-which-the-queue-reader-agent-makes-connections-to-the-subscriber"></a>Pour modifier le contexte sous lequel l'Agent de lecture de la file d'attente établit des connexions sur l'Abonné  
  
-   L'Agent de lecture de la file d'attente utilise le même contexte de connexion que l'Agent de distribution pour l'abonnement. Pour plus d'informations, consultez les procédures ci-dessus pour l'Agent de distribution.  
  
#### <a name="to-change-security-settings-for-an-immediate-updating-pull-subscription"></a>Pour modifier les paramètres de sécurité pour un abonnement par extraction de données (pull) mis à jour immédiatement  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Subscription>** sur l’abonné, cliquez sur la ligne **Connexion du serveur de publication** puis cliquez sur le bouton des propriétés ( **&#x2026;** ) dans la ligne.  
  
2.  Dans la boîte de dialogue **Entrer les informations de connexion** , sélectionnez une des options suivantes :  
  
    -   **Utiliser une connexion d'un serveur lié ou d'un serveur distant**. Sélectionnez cette option si vous avez défini un serveur distant ou un serveur lié entre l’Abonné et le serveur de publication à l’aide de [sp_addserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md), [sp_addlinkedserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md), [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou d’une autre méthode.  
  
    -   **Utiliser l'authentification SQL Server avec la connexion et le mot de passe suivants**. Sélectionnez cette option si vous n'avez pas défini un serveur distant ou un serveur lié entre l'Abonné et le serveur de publication. La réplication va créer un serveur lié à votre place. Le compte que vous spécifiez doit déjà exister sur le serveur de publication.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
> [!NOTE]  
>  Cette procédure modifie la méthode utilisée par les déclencheurs de réplication pour se connecter de l'Abonné au serveur de publication lorsque des modifications sont effectuées sur l'Abonné. Vous pouvez également modifier les paramètres associés avec l'Agent de distribution pour un abonnement mis à jour immédiatement. Pour plus d'informations, consultez les procédures plus haut dans cette rubrique.  
>   
>  Cette procédure s'applique uniquement aux abonnements par extraction de données (pull). Pour les abonnements par émission de données, utilisez la procédure stockée [sp_link_publication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md).  
  
#### <a name="to-change-the-password-for-the-administrative-connection-from-the-publisher-to-the-distributor"></a>Pour modifier le mot de passe pour la connexion administrative du serveur de publication au serveur de distribution  
  
1.  Dans la page **Serveurs de publication** de la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** , entrez un mot de passe fort dans les zones de texte **Mot de passe** et **Confirmer le mot de passe**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
3.  Dans la page **Général** de la boîte de dialogue **Propriétés du serveur de publication - \<Publisher>** , entrez un mot de passe fort dans les zones de texte **Mot de passe** et **Confirmer le mot de passe**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
> [!IMPORTANT]  
>  Dans toutes les procédures répertoriées ci-dessous, lorsque cela est possible, demandez aux utilisateurs de fournir les informations d'identification de sécurité au moment de l'exécution. Si vous stockez des informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher tout accès non autorisé.  
  
#### <a name="to-change-all-instances-of-a-stored-password-at-a-replication-server"></a>Pour modifier toutes les instances d'un mot de passe stocké sur un serveur de réplication  
  
1.  Exécutez [sp_changereplicationserverpasswords](../../../relational-databases/system-stored-procedures/sp-changereplicationserverpasswords-transact-sql.md)sur la base de données MASTER d'un serveur inclus dans une topologie de réplication. Spécifiez le compte [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows ou le compte de connexion [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dont le mot de passe doit être changé pour `@login`, et le nouveau mot de passe du compte ou de la connexion pour `@password`. Cela modifie chaque instance du mot de passe utilisée par tous les agents sur le serveur lors de la connexion à d'autres serveurs dans la topologie.  
  
    > [!NOTE]  
    >  Pour modifier uniquement la connexion et le mot de passe pour une connexion à un serveur particulier dans la topologie (tel que le serveur de distribution ou l’Abonné), spécifiez le nom de ce serveur pour `@server`.  
  
2.  Répétez l'étape 1 sur chaque serveur dans la topologie de réplication où le mot de passe doit être mis à jour.  
  
    > [!NOTE]  
    >  Après avoir modifié un mot de passe de réplication, vous devez arrêter puis redémarrer chaque Agent qui utilise ce mot de passe afin que les modifications apportées prennent effet.  
  
#### <a name="to-change-security-settings-for-the-snapshot-agent"></a>Pour modifier les paramètres de sécurité pour l'Agent d'instantané  
  
1.  Sur le serveur de publication, exécutez [sp_helppublication_snapshot](../../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md), en spécifiant `@publication`. Cela retourne les paramètres de sécurité actuels pour l'Agent d'instantané.  
  
2.  Sur le serveur de publication, exécutez [sp_changepublication_snapshot](../../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md), en spécifiant `@publication` et un ou plusieurs des paramètres de sécurité suivants à modifier :  
  
    -   Pour changer le compte Windows sous lequel l’agent s’exécute ou seulement le mot de passe de ce compte, spécifiez `@job_login` et `@job_password`.  
  
    -   Pour changer le mode de sécurité utilisé lors de la connexion au serveur de publication, spécifiez la valeur **1** ou **0** pour `@publisher_security_mode`.  
  
    -   Lorsque vous changez le mode de sécurité utilisé pour la connexion au serveur de publication de **1** à **0** ou lorsque vous changez un compte de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilisé pour cette connexion, spécifiez `@publisher_login` et `@publisher_password`.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-log-reader-agent"></a>Pour modifier les paramètres de sécurité pour l'Agent de lecture du journal  
  
1.  Sur le serveur de publication, exécutez [sp_helplogreader_agent](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md), en spécifiant `@publisher`. Cela retourne les paramètres de sécurité actuels pour l'Agent de lecture du journal.  
  
2.  Sur le serveur de publication, exécutez [sp_changelogreader_agent](../../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md), en spécifiant `@publication` et un ou plusieurs des paramètres de sécurité suivants à modifier :  
  
    -   Pour changer le compte Windows sous lequel l’agent s’exécute ou seulement le mot de passe de ce compte, spécifiez `@job_login` et `@job_password`.  
  
    -   Pour changer le mode de sécurité utilisé lors de la connexion au serveur de publication, spécifiez la valeur **1** ou **0** pour `@publisher_security_mode`.  
  
    -   Lorsque vous changez le mode de sécurité utilisé pour la connexion au serveur de publication de **1** à **0** ou lorsque vous changez un compte de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilisé pour cette connexion, spécifiez `@publisher_login` et `@publisher_password`.  
  
    > [!NOTE]  
    >  Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-push-subscription"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de distribution pour un abonnement par émission de données  
  
1.  Sur la base de données de publication du serveur de publication, exécutez [sp_helpsubscription](../../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md), en spécifiant `@publication` et `@subscriber`. Cela retourne les propriétés de l'abonnement, y compris les paramètres de sécurité relatifs à l'Agent de distribution qui s'exécute sur le serveur de distribution.  
  
2.  Sur la base de données de publication du serveur de publication, exécutez [sp_changesubscription](../../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md), en spécifiant `@publication`, `@subscriber`, `@subscriber_db**`, la valeur `all` pour `@article`, le nom de la propriété de sécurité pour `@property` et la nouvelle valeur de la propriété pour `@value`.  
  
3.  Répétez l'étape 2 pour chacune des propriétés de sécurité suivantes à modifier :  
  
    -   Pour changer le compte Windows sous lequel l’agent s’exécute ou seulement le mot de passe de ce compte, spécifiez la valeur **distrib_job_password** pour `@property` et un nouveau mot de passe pour `@value`. Si vous changez le compte, répétez l’étape 2 en spécifiant la valeur **distrib_job_login** pour `@property` et le nouveau compte Windows pour `@value`.  
  
    -   Pour changer le mode de sécurité utilisé quand vous vous connectez à l’Abonné, spécifiez la valeur **subscriber_security_mode** pour `@property` et la valeur **1** (authentification Windows intégrée) ou **0** (authentification SQL Server) pour `@value`.  
  
    -   Si vous changez le mode de sécurité de l’Abonné en spécifiant l’authentification SQL Server ou si vous modifiez les informations de connexion pour l’authentification SQL Server, spécifiez la valeur **subscriber_password** pour `@property` et le nouveau mot de passe pour `@value`. Répétez l’étape 2, en spécifiant la valeur **subscriber_login** pour `@property` et le nouveau nom de connexion pour `@value`.  
  
    > [!NOTE]  
    >  Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour toutes les propriétés, y compris **distrib_job_login** et **distrib_job_password**, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-pull-subscription"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de distribution pour un abonnement par extraction  
  
1.  Sur l’Abonné, exécutez [sp_helppullsubscription](../../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md), en spécifiant `@publication`. Cela retourne les propriétés de l'abonnement, y compris les paramètres de sécurité relatifs à l'Agent de distribution qui s'exécute sur l'Abonné.  
  
2.  Sur la base de données d’abonnement de l’Abonné, exécutez [sp_change_subscription_properties](../../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md), en spécifiant `@publisher`, `@publisher_db`, `@publication`, le nom de la propriété de sécurité pour `@property` et la nouvelle valeur de la propriété pour `@value`.  
  
3.  Répétez l'étape 2 pour chacune des propriétés de sécurité suivantes à modifier :  
  
    -   Pour changer le compte Windows sous lequel l’agent s’exécute ou seulement le mot de passe de ce compte, spécifiez la valeur **distrib_job_password** pour `@property` et un nouveau mot de passe pour `@value`. Si vous changez le compte, répétez l’étape 2 en spécifiant la valeur **distrib_job_login** pour `@property` et le nouveau compte Windows pour `@value`.  
  
    -   Pour changer le mode de sécurité utilisé quand vous vous connectez au serveur de distribution, spécifiez la valeur **distributor_security_mode** pour `@property` et la valeur **1** (authentification intégrée Windows) ou **0** (authentification SQL Server) pour `@value`.  
  
    -   Si vous changez le mode de sécurité du serveur de distribution en spécifiant l’authentification SQL Server ou si vous modifiez les informations de connexion pour l’authentification SQL Server, spécifiez la valeur **distributor_password** pour `@property` et le nouveau mot de passe pour `@value`. Répétez l’étape 2, en spécifiant la valeur **distributor_login** pour `@property` et le nouveau nom de connexion pour `@value`.  
  
    > [!NOTE]  
    >  Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-push-subscription"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de fusion pour un abonnement par émission de données  
  
1.  Sur la base de données de publication du serveur de publication, exécutez [sp_helpmergesubscription](../../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md), en spécifiant `@publication`, `@subscriber,` et `@subscriber_db`. Cela retourne les propriétés de l'abonnement, y compris les paramètres de sécurité relatifs à l'Agent de fusion qui s'exécute sur le serveur de distribution.  
  
2.  Sur la base de données de publication du serveur de publication, exécutez [sp_changemergesubscription](../../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md), en spécifiant `@publication`, `@subscriber`, `@subscriber_db`, le nom de la propriété de sécurité pour `@property` et la nouvelle valeur de la propriété pour `@value`.  
  
3.  Répétez l'étape 2 pour chacune des propriétés de sécurité suivantes à modifier :  
  
    -   Pour changer le compte Windows sous lequel l’agent s’exécute ou seulement le mot de passe de ce compte, spécifiez la valeur **merge_job_password** pour `@property` et un nouveau mot de passe pour `@value`. Si vous changez le compte, répétez l’étape 2 en spécifiant la valeur **merge_job_login** pour `@property` et le nouveau compte Windows pour `@value`.  
  
    -   Pour changer le mode de sécurité utilisé quand vous vous connectez à l’Abonné, spécifiez la valeur **subscriber_security_mode** pour `@property` et la valeur **1** (authentification Windows intégrée) ou **0** (authentification SQL Server) pour `@value`.  
  
    -   Si vous changez le mode de sécurité de l’Abonné en spécifiant l’authentification SQL Server ou si vous modifiez les informations de connexion pour l’authentification SQL Server, spécifiez la valeur **subscriber_password** pour `@property` et le nouveau mot de passe pour `@value`. Répétez l’étape 2, en spécifiant la valeur **subscriber_login** pour `@property` et le nouveau nom de connexion pour `@value`.  
  
    -   Pour changer le mode de sécurité utilisé quand vous vous connectez au serveur de publication, spécifiez la valeur **publisher_security_mode** pour `@property` et la valeur **1** (authentification Windows intégrée) ou **0** (authentification SQL Server) pour `@value`.  
  
    -   Si vous changez le mode de sécurité du serveur de publication en spécifiant l’authentification SQL Server ou si vous modifiez les informations de connexion pour l’authentification SQL Server, spécifiez la valeur **publisher_password** pour `@property`et le nouveau mot de passe pour `@value`. Répétez l’étape 2, en spécifiant la valeur **publisher_login** pour `@property` et le nouveau nom de connexion pour `@value`.  
  
    > [!NOTE]  
    >  Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour toutes les propriétés, y compris **merge_job_login** et **merge_job_password**, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-pull-subscription"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de fusion pour un abonnement par extraction  
  
1.  Sur l’Abonné, exécutez [sp_helpmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md), en spécifiant ``@publication``. Cela retourne les propriétés de l'abonnement, y compris les paramètres de sécurité relatifs à l'Agent de fusion qui s'exécute sur l'Abonné.  
  
2.  Sur la base de données d’abonnement de l’Abonné, exécutez [sp_change_subscription_properties](../../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md), en spécifiant `@publisher`, `@publisher_db`, `@publication`, le nom de la propriété de sécurité pour `@property` et la nouvelle valeur de la propriété pour `@value`.  
  
3.  Répétez l'étape 2 pour chacune des propriétés de sécurité suivantes à modifier :  
  
    -   Pour changer le compte Windows sous lequel l’agent s’exécute ou seulement le mot de passe de ce compte, spécifiez la valeur **merge_job_password** pour `@property` et un nouveau mot de passe pour `@value`. Si vous changez le compte, répétez l’étape 2 en spécifiant la valeur **merge_job_login** pour `@property` et le nouveau compte Windows pour `@value`.  
  
    -   Pour changer le mode de sécurité utilisé quand vous vous connectez au serveur de distribution, spécifiez la valeur **distributor_security_mode** pour `@property` et la valeur **1** (authentification intégrée Windows) ou **0** (authentification SQL Server) pour `@value`.  
  
    -   Si vous changez le mode de sécurité du serveur de distribution en spécifiant l’authentification SQL Server ou si vous modifiez les informations de connexion pour l’authentification SQL Server, spécifiez la valeur **distributor_password** pour `@property` et le nouveau mot de passe pour `@value`. Répétez l’étape 2, en spécifiant la valeur **distributor_login** pour `@property` et le nouveau nom de connexion pour `@value`.  
  
    -   Pour changer le mode de sécurité utilisé quand vous vous connectez au serveur de publication, spécifiez la valeur **publisher_security_mode** pour `@property` et la valeur **1** (authentification Windows intégrée) ou **0** (authentification SQL Server) pour `@value`.  
  
    -   Si vous changez le mode de sécurité du serveur de publication en spécifiant l’authentification SQL Server ou si vous modifiez les informations de connexion pour l’authentification SQL Server, spécifiez la valeur **publisher_password** pour `@property`et le nouveau mot de passe pour `@value`. Répétez l’étape 2, en spécifiant la valeur **publisher_login** pour `@property` et le nouveau nom de connexion pour `@value`.  
  
    > [!NOTE]  
    >  Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
#### <a name="to-change-security-settings-for-the-snapshot-agent-to-generate-a-filtered-snapshot-for-a-subscriber"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent d'instantané pour générer un instantané filtré pour un Abonné  
  
1.  Sur le serveur de publication, exécutez [sp_helpdynamicsnapshot_job](../../../relational-databases/system-stored-procedures/sp-helpdynamicsnapshot-job-transact-sql.md), en spécifiant `@publication`. Dans le jeu de résultats, notez la valeur de **job_name** pour la partition de l'Abonné à modifier.  
  
2.  Sur le serveur de publication, exécutez [sp_changedynamicsnapshot_job](../../../relational-databases/system-stored-procedures/sp-changedynamicsnapshot-job-transact-sql.md), en spécifiant `@publication`, la valeur obtenue à l’étape 1 pour `dynamic_snapshot_jobname` ainsi qu’un nouveau mot de passe pour `@job_password` ou un nom de connexion et un mot de passe pour le compte Windows sous lequel l’agent s’exécute pour `@job_login` et `@job_password`.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-queue-reader-agent"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de lecture de la file d'attente  
  
1.  Sur le serveur de distribution, exécutez [sp_helpqreader_agent](../../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md). Cela retourne le compte Windows actuel sous lequel l'Agent de lecture de la file d'attente s'exécute.  
  
    -   Sur le serveur de distribution, exécutez [sp_changeqreader_agent](../../../relational-databases/system-stored-procedures/sp-changeqreader-agent-transact-sql.md), en spécifiant les paramètres du compte Windows pour `@job_login` et `@job_password`.  
  
    > [!NOTE]  
    >  Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet. Il existe un Agent de lecture de la file d'attente pour chaque base de données de distribution. La modification des paramètres de sécurité pour l'agent affecte les paramètres de toutes les publications sur tous les serveurs de publication qui utilisent cette base de données de distribution.  
  
2.  L'Agent de lecture de la file d'attente établit des connexions avec l'Abonné en utilisant le même contexte de connexion que l'Agent de distribution pour l'abonnement.  
  
#### <a name="to-change-security-mode-used-by-an-immediate-updating-subscriber-when-connecting-to-the-publisher"></a>Pour modifier le mode de sécurité utilisé par un Abonné avec mise à jour immédiate lors de la connexion au serveur de publication  
  
1.  Dans la base de données d’abonnement de l’Abonné, exécutez [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md). Spécifiez `@publisher`, `@publication`, le nom de la base de données de publication pour `@publisher_db`, et l’une des valeurs suivantes pour `@security_mode`:  
  
    -   **0** pour utiliser l'authentification SQL Server pour effectuer des mises à jour sur le serveur de publication. Avec cette option, vous devez spécifier une connexion valide sur le serveur de publication pour `@login` et `@password`.  
  
    -   **1** pour utiliser le contexte de sécurité de l'utilisateur qui apporte des modifications sur l'Abonné lors de la connexion au serveur de publication. Consultez [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md) pour connaître les restrictions en rapport avec ce mode de sécurité.  
  
    -   **2** pour utiliser une connexion de serveur lié existante, définie par l’utilisateur et créée à l’aide de [sp_addlinkedserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
#### <a name="to-change-the-password-for-a-remote-distributor"></a>Pour modifier le mot de passe d'un serveur de distribution distant  
  
1.  Sur la base de données de distribution du serveur de distribution, exécutez [sp_changedistributor_password](../../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md), en spécifiant le nouveau mot de passe pour cette connexion pour `@password`.  
  
    > [!IMPORTANT]  
    >  Ne modifiez pas directement le mot de passe pour **distributor_admin** .  
  
2.  Sur chaque serveur de publication qui utilise ce serveur de distribution distant, exécutez [sp_changedistributor_password](../../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md), en spécifiant le mot de passe défini à l’étape 1 pour `@password`.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification, utilisez les [Services de chiffrement](/previous-versions/aa719848(v=vs.71)) fournis par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-change-all-instances-of-a-password-stored-on-a-replication-server"></a>Pour modifier toutes les instances d'un mot de passe stockées sur un serveur de réplication  
  
1.  Créez une connexion au serveur de réplication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer> au moyen de la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationServer.ChangeReplicationServerPasswords%2A> . Spécifiez les paramètres suivants :  
  
    -   *security_mode* – une valeur <xref:Microsoft.SqlServer.Replication.ReplicationSecurityMode> qui spécifie le type d'authentification pour lequel toutes les instances du mot de passe sont modifiées.  
  
    -   *login* – la connexion pour laquelle toutes les instances du mot de passe sont modifiées.  
  
    -   *password* – la nouvelle valeur du mot de passe.  
  
        > [!IMPORTANT]  
        >  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification, utilisez les [Services de chiffrement](/previous-versions/aa719848(v=vs.71)) fournis par Windows .NET Framework.  
  
        > [!NOTE]  
        >  Seul un membre du rôle serveur fixe **sysadmin** peut appeler cette méthode.  
  
4.  Répétez les étapes 1-3 pour chaque serveur dans la topologie de réplication où le mot de passe doit être actualisé.  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-push-subscription-to-a-transactional-publication"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de distribution pour un abonnement par émission de données à une publication transactionnelle  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>et <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> pour l'abonnement et définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 3, soit l'abonnement n'existe pas.  
  
5.  Définissez une ou plusieurs des propriétés de sécurité suivantes sur l'instance de <xref:Microsoft.SqlServer.Replication.TransSubscription>:  
  
    -   Pour modifier les informations d'identification relatives au compte Windows sous lequel l'agent s'exécute, définissez les champs <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>.  
  
    -   Pour spécifier l'authentification intégrée Windows comme type d'authentification utilisé par l'agent lorsqu'il se connecte à l'Abonné, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> de la propriété **P:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity**.  
  
    -   Pour spécifier l'authentification SQL Server comme type d'authentification utilisé par l'agent lorsqu'il se connecte à l'Abonné, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> de la propriété **false** et spécifiez les informations d'identification de connexion à l'Abonné pour les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
        > [!NOTE]  
        >  La connexion de l'agent au serveur de distribution est toujours établie à l'aide des informations d'identification Windows spécifiées par <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>. Ce compte permet également d'établir des connexions à distance à l'aide de l'authentification Windows.  
  
6.  (Facultatif) Si vous avez spécifié la valeur **true** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur. Si vous avez spécifié la valeur **false** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (valeur par défaut), les modifications sont envoyées immédiatement au serveur.  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-pull-subscription-to-a-transactional-publication"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de distribution pour un abonnement par extraction à une publication transactionnelle  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>et <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> pour l'abonnement et définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 3, soit l'abonnement n'existe pas.  
  
5.  Définissez une ou plusieurs des propriétés de sécurité suivantes sur l'instance de <xref:Microsoft.SqlServer.Replication.TransPullSubscription>:  
  
    -   Pour modifier les informations d'identification relatives au compte Windows sous lequel l'agent s'exécute, définissez les champs <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>.  
  
    -   Pour spécifier l'authentification intégrée Windows comme type d'authentification utilisé par l'agent lorsqu'il se connecte au serveur de distribution, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> de la propriété **P:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity**.  
  
    -   Pour spécifier l'authentification SQL Server comme type d'authentification utilisé par l'agent lorsqu'il se connecte au serveur de distribution, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> de la propriété **false** et spécifiez les informations d'identification de connexion au serveur de distribution pour les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
        > [!NOTE]  
        >  La connexion de l'agent à l'Abonné est toujours établie à l'aide des informations d'identification Windows spécifiées par <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>. Ce compte permet également d'établir des connexions à distance à l'aide de l'authentification Windows.  
  
6.  (Facultatif) Si vous avez spécifié la valeur **true** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur. Si vous avez spécifié la valeur **false** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (valeur par défaut), les modifications sont envoyées immédiatement au serveur.  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-pull-subscription-to-a-merge-publication"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de fusion pour un abonnement par extraction à une publication de fusion  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>et <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> pour l'abonnement et définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 3, soit l'abonnement n'existe pas.  
  
5.  Définissez une ou plusieurs des propriétés de sécurité suivantes sur l'instance de <xref:Microsoft.SqlServer.Replication.MergePullSubscription>:  
  
    -   Pour modifier les informations d'identification relatives au compte Windows sous lequel l'agent s'exécute, définissez les champs <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>.  
  
    -   Pour spécifier l'authentification intégrée Windows comme type d'authentification utilisé par l'agent lorsqu'il se connecte au serveur de distribution, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> de la propriété **P:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity**.  
  
    -   Pour spécifier l'authentification SQL Server comme type d'authentification utilisé par l'agent lorsqu'il se connecte au serveur de distribution, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> de la propriété **false** et spécifiez les informations d'identification de connexion au serveur de distribution pour les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
    -   Pour spécifier l'authentification intégrée Windows comme type d'authentification utilisé par l'agent lorsqu'il se connecte au serveur de publication, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> de la propriété **P:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity**.  
  
    -   Pour spécifier l'authentification SQL Server comme type d'authentification utilisé par l'agent lorsqu'il se connecte au serveur de publication, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> de la propriété **false** et spécifiez les informations d'identification de connexion au serveur de publication pour les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
        > [!NOTE]  
        >  La connexion de l'agent à l'Abonné est toujours établie à l'aide des informations d'identification Windows spécifiées par <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>. Ce compte permet également d'établir des connexions à distance à l'aide de l'authentification Windows.  
  
6.  (Facultatif) Si vous avez spécifié la valeur **true** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur. Si vous avez spécifié la valeur **false** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (valeur par défaut), les modifications sont envoyées immédiatement au serveur.  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-push-subscription-to-a-merge-publication"></a>Pour modifier les paramètres de sécurité relatifs à l'Agent de fusion pour un abonnement par émission de données à une publication de fusion  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>et <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> pour l'abonnement et définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 3, soit l'abonnement n'existe pas.  
  
5.  Définissez une ou plusieurs des propriétés de sécurité suivantes sur l'instance de <xref:Microsoft.SqlServer.Replication.MergeSubscription>:  
  
    -   Pour modifier les informations d'identification relatives au compte Windows sous lequel l'agent s'exécute, définissez les champs <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>.  
  
    -   Pour spécifier l'authentification intégrée Windows comme type d'authentification utilisé par l'agent lorsqu'il se connecte à l'Abonné, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> de la propriété **P:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity**.  
  
    -   Pour spécifier l'authentification SQL Server comme type d'authentification utilisé par l'agent lorsqu'il se connecte à l'Abonné, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> de la propriété **false** et spécifiez les informations d'identification de connexion à l'Abonné pour les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
    -   Pour spécifier l'authentification intégrée Windows comme type d'authentification utilisé par l'agent lorsqu'il se connecte au serveur de publication, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.MergeSubscription.PublisherSecurity%2A> de la propriété **P:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity**.  
  
    -   Pour spécifier l'authentification SQL Server comme type d'authentification utilisé par l'agent lorsqu'il se connecte au serveur de publication, affectez la valeur <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> au champ <xref:Microsoft.SqlServer.Replication.MergeSubscription.PublisherSecurity%2A> de la propriété **false** et spécifiez les informations d'identification de connexion au serveur de publication pour les champs <xref:Microsoft.SqlServer.Replication.PublisherConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.PublisherConnectionSecurityContext.SqlStandardPassword%2A> .  
  
        > [!NOTE]  
        >  La connexion de l'agent au serveur de distribution est toujours établie à l'aide des informations d'identification Windows spécifiées par <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>. Ce compte permet également d'établir des connexions à distance à l'aide de l'authentification Windows.  
  
6.  (Facultatif) Si vous avez spécifié la valeur **true** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur. Si vous avez spécifié la valeur **false** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (valeur par défaut), les modifications sont envoyées immédiatement au serveur.  
  
#### <a name="to-change-the-login-information-used-by-an-immediate-updating-subscriber-when-it-connects-to-the-transactional-publisher"></a>Pour modifier les informations de connexion utilisées par un Abonné de mise à jour immédiate lorsqu'il se connecte au serveur de publication transactionnelle  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> pour la base de données d'abonnement. Spécifiez <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.Name%2A> et la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de la base de données ont été définies de manière incorrecte à l'étape 2, soit la base de données d'abonnement n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LinkPublicationForUpdateableSubscription%2A> , en passant les paramètres suivants :  
  
    -   *Publisher* – nom du serveur de publication.  
  
    -   *PublisherDB* – nom de la base de données de publication.  
  
    -   *Publication* – nom de la publication à laquelle l'Abonné de mise à jour immédiate est abonné.  
  
    -   *Distributor* – nom du serveur de distribution.  
  
    -   *PublisherSecurity* - A <xref:Microsoft.SqlServer.Replication.PublisherConnectionSecurityContext> qui spécifie le type de mode de sécurité utilisé par l'Abonné de mise à jour immédiate lorsqu'il se connecte au serveur de publication, et les informations d'identification de connexion pour cette connexion.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Exemple (RMO)  
 Cet exemple vérifie le nom d'accès fourni et modifie tous les mots de passe relatifs à la connexion Windows fournie ou au compte de connexion SQL Server fourni, stockés par la réplication sur le serveur.  
  
 [!code-cs[HowTo#rmo_ChangeServerPasswords](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changeserverpasswords)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeServerPasswords](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changeserverpasswords)]  
  
##  <a name="follow-up-after-you-modify-replication-security-settings"></a><a name="FollowUp"></a> Suivi : après avoir modifié les paramètres de sécurité de la réplication  
 Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts liés à RMO (Replication Management Objects)](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Mettre à niveau les scripts de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md)   
 [Identité et contrôle d’accès pour la réplication](../../../relational-databases/replication/security/identity-and-access-control-replication.md)   
 [Modèle de sécurité de l’Agent de réplication](../../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Afficher et modifier les paramètres de sécurité de la réplication](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
