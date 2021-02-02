---
description: Créer une publication à partir d'une base de données Oracle
title: Créer une publication à partir d’une base de données Oracle | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], Oracle databases
- Oracle publishing [SQL Server replication], configuring
ms.assetid: b3812746-14b0-4b22-809e-b4a95e1c8083
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cc7660d1990f556ae3ccfb8d34d66519dd2ecd76
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076777"
---
# <a name="create-a-publication-from-an-oracle-database"></a>Créer une publication à partir d'une base de données Oracle
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer une publication Oracle dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Composants requis](#Prerequisites)  
  
-   **Pour créer une publication à partir d'une base de données Oracle à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Avant de créer une publication, vous devez installer le logiciel Oracle sur le serveur de distribution [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et configurer la base de données Oracle. Pour plus d’informations, consultez [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Créez une publication transactionnelle ou d'instantané à partir d'une base de données Oracle à l'aide de l'Assistant Nouvelle publication.  
  
 La première fois que vous créez une publication à partir d'une base de données Oracle, vous devez identifier le serveur de publication Oracle sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (ce ne sera plus nécessaire pour les publications suivantes effectuées à partir de la même base de données). Pour identifier le serveur de distribution Oracle, ouvrez l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>**  ; cette rubrique illustre la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** .  
  
#### <a name="to-identify-the-oracle-publisher-at-the-sql-server-distributor"></a>Pour identifier le serveur de publication Oracle sur le serveur de distribution SQL Server  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], connectez-vous à l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que le serveur de publication Oracle utilisera comme serveur de distribution puis développez le nœud du serveur.  
  
2.  Cliquez avec le bouton droit sur le dossier **Réplication** , puis cliquez sur **Propriétés du serveur de distribution**.  
  
3.  Dans la page **Serveurs de publication** de la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** , cliquez sur **Ajouter**, puis sur **Ajouter un serveur de publication Oracle**.  
  
4.  Dans la boîte de dialogue **Se connecter au serveur** , cliquez sur le bouton **Options** .  
  
5.  Dans l'onglet **Connexion** :  
  
    1.  Entrez le nom de l'instance de base de données Oracle ou sélectionnez **Parcourir** dans la zone de liste déroulante **Instance de serveur** .  
  
    2.  Sélectionnez **Authentification standard Oracle** (recommandé) ou **Authentification Windows**.  
  
         Si vous sélectionnez **Authentification Windows**: le serveur Oracle doit être configuré pour autoriser les connexions à l'aide des informations d'identification Windows (pour plus d'informations, consultez la documentation Oracle) et vous devez être actuellement connecté avec le même compte [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows que celui spécifié pour le schéma utilisateur d'administration de réplication.  
  
    3.  Si vous sélectionnez **Authentification standard Oracle**, entrez le nom de connexion et le mot de passe du schéma utilisateur d'administration de réplication créé sur le serveur de publication Oracle pendant la configuration.  
  
6.  Dans l'onglet **Propriétés de la connexion** , sélectionnez un type de serveur de publication ( **Passerelle** ou **Complet**).  
  
     L'option **Complet** a pour but de fournir un instantané et des publications transactionnelles avec le jeu complet de fonctionnalités prises en charge pour le serveur de publication Oracle. L'option **Passerelle** permet l'optimisation de la conception en améliorant les performances pour les cas où la réplication sert de passerelle entre les systèmes. Vous ne pouvez pas utiliser l'option **Passerelle** si vous envisagez de publier la même table dans plusieurs publications transactionnelles. Une table peut apparaître au maximum dans une publication transactionnelle et dans une ou plusieurs publications d'instantané si vous sélectionnez **Passerelle**.  
  
7.  Cliquez sur **Se connecter**, qui crée une connexion au serveur de publication Oracle et le configure pour la réplication. La boîte de dialogue **Se connecter au serveur** se ferme et vous revenez dans la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** .  
  
    > [!NOTE]  
    >  En cas de problème lié à la configuration réseau, vous recevrez un message d'erreur à ce stade. Si vous rencontrez des difficultés pour vous connecter à la base de données Oracle, consultez la section « Connexion impossible du serveur de distribution SQL Server à l'instance de base de données Oracle » dans [Troubleshooting Oracle Publishers](../../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-create-a-publication-from-an-oracle-database"></a>Pour créer une publication à partir d'une base de données Oracle  
  
1.  Connectez-vous à l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que le serveur de publication Oracle utilisera comme serveur de distribution puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** .  
  
3.  Cliquez avec le bouton droit sur le dossier **Publications locales** , puis cliquez sur **Nouvelle publication Oracle**.  
  
4.  Dans la page **Serveur de publication Oracle** de l'Assistant Nouvelle publication, sélectionnez le serveur de publication Oracle. S'il ne s'affiche pas, cliquez sur **Ajouter un serveur de publication Oracle**, puis suivez les étapes décrites dans la procédure précédente.  
  
5.  Dans la page **Type de publication** , sélectionnez **Publication d'instantané** ou **Publication transactionnelle**.  
  
6.  Dans la page **Articles** , sélectionnez les objets de base de données à publier.  
  
     Retirez éventuellement des colonnes de table en développant une table et en désactivant la case à cocher d'une ou plusieurs colonnes. Cliquez sur **Propriétés de l'article** pour afficher et modifier les propriétés de l'article et spécifier, le cas échéant, d'autres mappages de types de données. Pour plus d’informations sur les mappages des types de données, consultez [Spécifier des mappages de types de données pour un serveur de publication Oracle](../../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md).  
  
7.  Dans la page **Filtrer les lignes de la table** , appliquez éventuellement des filtres pour publier un sous-ensemble de données d'une ou plusieurs tables.  
  
8.  Dans la page **Agent d'instantané** , désactivez **Créer un instantané immédiatement** uniquement si vous avez créé tous les objets et ajouté toutes les données requises dans la base de données d'abonnement.  
  
9. Dans la page **Sécurité de l'Agent** , spécifiez des informations d'identification pour l'Agent d'instantané (pour toutes les publications) et l'Agent de lecture du journal (pour les publications transactionnelles). Les Agents s'exécutent et se connectent au serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l'aide du contexte du compte [!INCLUDE[msCoName](../../../includes/msconame-md.md)] spécifié. Les Agents se connectent à la base de données Oracle avec le contexte du compte spécifié comme schéma utilisateur d'administration de réplication. Pour plus d’informations, consultez [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
     Pour plus d'informations sur les autorisations requises par chaque Agent, consultez [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) et [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
10. Dans la page **Actions de l'Assistant** , créez éventuellement un script de la publication. Pour plus d'informations, voir [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
11. Dans la page **Terminer l'Assistant** , spécifiez un nom pour la publication.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Une fois la base de données Oracle configurée comme serveur de publication, vous pouvez créer une publication transactionnelle ou d’instantané de la même façon que vous le feriez à partir un serveur de publication [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], en utilisant des procédures stockées système.  
  
#### <a name="to-create-an-oracle-publication"></a>Pour créer une publication Oracle  
  
1.  Configurez la base de données Oracle en tant que serveur de publication. Pour plus d’informations, consultez [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
2.  S'il n'existe pas de serveur de distribution distant, configurez-en un. Pour plus d’informations, consultez [Configure Publishing and Distribution](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
3.  Sur le serveur de distribution distant que le serveur de publication Oracle utilisera, exécutez [sp_adddistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md). Spécifiez le nom TNS (Transparent Network Substrate) de l’instance de base de données Oracle pour **\@publisher** et une valeur **ORACLE** ou **ORACLE GATEWAY** pour **\@publisher_type**. `Specify` le mode de sécurité utilisé lors de la connexion entre le Serveur de publication Oracle et le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] distant parmi les modes suivants :  
  
    -   Pour utiliser l’authentification standard Oracle (valeur par défaut), affectez la valeur **0** à **\@security_mode**, spécifiez la connexion du schéma utilisateur d’administration de réplication que vous avez créée sur le serveur de publication Oracle au cours de la configuration pour **\@login** et le mot de passe pour **\@password**.  
  
        > [!IMPORTANT]  
        >  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous stockez des informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher tout accès non autorisé.  
  
    -   Pour utiliser l’authentification Windows, affectez la valeur **1** à **\@security_mode**.  
  
        > [!NOTE]  
        >  Pour utiliser l'authentification Windows, le serveur Oracle doit être configuré pour autoriser les connexions à l'aide des informations d'identification Windows (pour plus d'informations, consultez la documentation Oracle) et vous devez être actuellement connecté avec le même compte Microsoft Windows que celui spécifié pour le schéma utilisateur d'administration de réplication.  
  
4.  Créez un travail de l'Agent de lecture du journal pour la base de données de publication.  
  
    -   Si vous ne savez pas si un travail de l’Agent de lecture du journal existe pour une base de données publiée, exécutez [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) au niveau du serveur de distribution utilisé par le serveur de publication Oracle dans la base de données de distribution. Spécifiez le nom du serveur de publication Oracle pour **\@publisher**. Si le jeu de résultats est vide, un travail de l'Agent de lecture du journal doit être créé.  
  
    -   Si un travail de l'Agent de lecture du journal existe déjà pour la base de données de publication, passez à l'étape 5.  
  
    -   Dans la base de données de distribution sur le serveur de distribution utilisé par le serveur de publication Oracle, exécutez [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Spécifiez les informations d’identification Windows sous lesquelles l’agent s’exécute pour **\@job_login** et **\@job_password**.  
  
        > [!NOTE]  
        >  Le paramètre **\@job_login** doit correspondre à la connexion fournie à l’étape 3. Ne fournissez pas d'informations de sécurité pour le serveur de publication. L'Agent de lecture du journal se connecte au serveur de publication à l'aide des informations de sécurité fournies à l'étape 3.  
  
5.  Dans la base de données de distribution sur le serveur de distribution, exécutez [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) pour créer la publication. Pour plus d’informations, voir [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
6.  Dans la base de données de distribution sur le serveur de distribution, exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez le nom de publication utilisé à l’étape 4 pour **\@publication** et les informations d’identification Windows sous lesquelles l’Agent d’instantané s’exécute pour **\@job_name** et **\@password**. Pour utiliser l’authentification standard Oracle lors de la connexion au serveur de publication, vous devez également affecter la valeur **0** à **\@publisher_security_mode** et spécifier les informations de connexion Oracle pour **\@publisher_login** et **\@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Configurer le travail d’un jeu de transactions pour un serveur de publication Oracle &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [Vue d’ensemble de la publication Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)   
 [Script pour l’attribution d’autorisations Oracle](../../../relational-databases/replication/non-sql/script-to-grant-oracle-permissions.md)  
  
  
