---
description: Valider des données répliquées
title: Valider des données répliquées | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Subscribers [SQL Server replication], data validation
- replication [SQL Server], validating data
- transactional replication, validating data
- validating data
- merge replication data validation [SQL Server replication], SQL Server Management Studio
ms.assetid: 215b4c9a-0ce9-4c00-ac0b-43b54151dfa3
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 1b5109c2bf93a49370f005e94a824fd5bfb70921
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250593"
---
# <a name="validate-replicated-data"></a>Valider des données répliquées
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique décrit comment valider les données sur l'abonné dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou des objets RMO (Replication Management Objects).  
  
La réplication transactionnelle et de fusion vous permet de vérifier que les données sur l'Abonné correspondent aux données sur le serveur de publication. La validation peut être réalisée pour des abonnements spécifiques ou pour tous les abonnements à une publication. Spécifiez un des types de validation suivants et l'Agent de distribution ou l'Agent de fusion validera les données lors de sa prochaine exécution :  
  
-   **Calculer uniquement le nombre de lignes.** Ceci vérifie si la table sur l'Abonné a le même nombre de lignes que la table sur le serveur de publication, mais ne vérifie pas que le contenu des lignes correspond. La validation du nombre de lignes fournit une approche allégée de la validation qui vous permet de savoir qu'il existe des problèmes au niveau des données.   
-   **Calculer le nombre de lignes et comparer les sommes de contrôle binaires**. Outre le comptage des lignes sur le serveur de publication et sur l'Abonné, une somme de contrôle de toutes les données est calculée à l'aide de l'algorithme de somme de contrôle. Si le nombre de lignes est erroné, la somme de contrôle n'est pas effectuée.  
  
 En plus de vérifier que les données sur l'Abonné et sur le serveur de publication correspondent, la réplication de fusion donne la possibilité de vérifier que les données sont partitionnées correctement pour chaque Abonné. Pour plus d’informations, consultez [Valider des informations de partition pour un Abonné de fusion](../../relational-databases/replication/validate-partition-information-for-a-merge-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
   
## <a name="how-data-validation-works"></a>Fonctionnement de la validation des données  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide les données en calculant un nombre de lignes et/ou une somme de contrôle sur le serveur de publication, puis en comparant ces valeurs au nombre de lignes et/ou à la somme de contrôle calculé sur l'Abonné. Une valeur est calculée pour l'ensemble de la table de publication et une autre est calculée pour l'ensemble de la table d'abonnement, mais les données des colonnes **text**, **ntext** ou **image** ne sont pas prises en compte dans les calculs.  
  
 Pendant l'exécution des calculs, des verrous partagés sont temporairement placés sur les tables pour lesquelles le nombre de lignes ou la somme de contrôle est calculé. Ces calculs sont cependant effectués rapidement et les verrous partagés sont généralement retirés au bout de quelques secondes.  
  
 Lors de l'utilisation des sommes de contrôle binaires, le contrôle de redondance cyclique (CRC) 32 bits est effectué colonne par colonne, et non pas sur la ligne physique de la page de données. Ainsi, quel que soit l'ordre physique des colonnes de la table dans la page de données, elles participent au même calcul de CRC de ligne. La validation par somme de contrôle binaire peut être utilisée lorsque des filtres de lignes ou de colonnes sont appliqués à la publication.  

 La validation des données est en processus comptant trois parties :  
  
1.  Un abonnement unique ou tous les abonnements à une publication sont *marqués* pour la validation. Marquez des abonnements pour validation dans les boîtes de dialogue **Valider l’abonnement**, **Valider les abonnements** et **Valider tous les abonnements**, disponibles dans les dossiers **Publications locales** et **Abonnements locaux** de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez aussi marquer des abonnements à partir de l'onglet **Tous les abonnements** , de l'onglet **Liste de suivi des abonnements** et du nœud des publications dans le Moniteur de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
2.  Un abonnement est validé lors de sa prochaine synchronisation par l'Agent de distribution (réplication transactionnelle) ou l'Agent de fusion (réplication de fusion). L'Agent de distribution s'exécute généralement en continu, auquel cas la validation se produit immédiatement ; l'Agent de fusion s'exécute généralement à la demande, auquel cas la validation se produit après l'exécution de l'agent.  
  
3.  Affichez les résultats de la validation :   
    -   dans la fenêtre détaillée du moniteur de réplication : sous l'onglet **Historique du serveur de distribution vers l'Abonné** pour la réplication transactionnelle et l'onglet **Historique de synchronisation** pour la réplication de fusion ;    
    -   dans la boîte de dialogue **Afficher l'état de synchronisation** de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
 
## <a name="considerations-and-restrictions"></a>Considérations et restrictions  
 Prenez en compte les problèmes suivants lors de la validation des données :  
  
-   Vous devez arrêter toutes les activités de mise à jour sur les Abonnés avant de valider les données (il n'est pas nécessaire d'arrêter les activités sur le serveur de publication pendant la validation).  
-   Dans la mesure où les sommes de contrôle et les sommes de contrôle binaires peuvent nécessiter des ressources processeur importantes lors de la validation d'un jeu de données de grande taille, il est préférable de planifier la validation au moment où l'activité est la plus faible sur les serveurs utilisés dans la réplication.   
-   La réplication valide seulement des tables ; elle ne vérifie pas si des articles de schéma uniquement (tels que des procédures stockées) sont identiques sur le serveur de publication et sur l'Abonné.   
-   La somme de contrôle binaire peut être utilisée avec toutes les tables publiées. La somme de contrôle valide les tables avec des filtres de colonnes, ou des structures de table logique où les décalages des colonnes diffèrent (à cause d'instructions ALTER TABLE qui suppriment ou ajoutent des colonnes).   
-   La validation de réplication utilise les fonctions **checksum** et **binary_checksum** . Pour plus d’informations sur leur comportement, consultez [CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md) et [BINARY_CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md).   
-   Une validation utilisant la somme de contrôle binaire ou la somme de contrôle peut signaler de façon incorrecte un échec si les types de données sont différents entre l'Abonné et le serveur de publication. Cela peut se produire si vous effectuez l'une des opérations suivantes :    
    -   Vous définissez explicitement les options de schéma pour qu'elles correspondent aux types de données des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
    -   Vous définissez le niveau de compatibilité de la publication pour une publication de fusion sur une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et les tables publiées contiennent un ou plusieurs types de données qui doivent être mappés pour cette version.    
    -   Vous initialisez manuellement un abonnement et utilisez différents types de données sur l'Abonné.   
-   Les validations de somme de contrôle binaire et de somme de contrôle ne sont pas prises en charge pour les abonnements transformables dans le cadre de la réplication transactionnelle.   
-   La validation n'est pas prise en charge pour les données répliquées vers des Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .    
-   Les procédures du moniteur de réplication concernent uniquement les abonnements par envoi de données (push) car ce type d'abonnement ne peut pas être synchronisé dans le moniteur de réplication. Vous pouvez toutefois marquer un abonnement pour validation et afficher les résultats de la validation pour les abonnements par extraction de données (pull) dans le moniteur de réplication.    
-   Les résultats de la validation indiquent si la validation a échoué ou a raté, mais ne précisent pas les lignes défectueuses en cas d'échec. Pour comparer des données sur le serveur de publication et sur l'Abonné, utilisez l' [tablediff Utility](../../tools/tablediff-utility.md). Pour plus d’informations sur cet utilitaire, consultez [Comparer des tables répliquées pour identifier les différences &#40;programmation de la réplication&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  


## <a name="data-validation-results"></a>Résultats de la validation des données  
 Quand la validation est terminée, l'Agent de distribution ou l'Agent de fusion consigne des messages relatifs au succès ou à l'échec (la réplication ne rapporte pas quelles lignes ont échoué). Ces messages peuvent être affichés dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans le moniteur de réplication et dans les tables système de réplication. La rubrique de procédure ci-dessus montre comment exécuter une validation et afficher les résultats.  
  
 Pour gérer les échecs de validation, considérez les points suivants :  
  
-   Configurez l'alerte de réplication nommée **Réplication : l'Abonné n'a pas réussi la validation des données** pour recevoir une notification de l'échec. Pour plus d’informations, consultez [Configurer des alertes de réplication prédéfinies &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md).  
  
-   Le fait que la validation a échoué est-il un problème pour votre application ? Si l'échec de la validation constitue un problème, mettez à jour manuellement les données pour qu'elles soient synchronisées, ou bien réinitialisez l'abonnement :  
  
    -   Les données peuvent être mises à jour à l'aide de l' [utilitaire tablediff](../../tools/tablediff-utility.md). Pour plus d’informations sur l’utilisation de cet utilitaire, consultez [Comparer des tables répliquées pour identifier les différences &#40;programmation de réplication&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  
  
    -   Pour plus d’informations sur la réinitialisation, consultez [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md).  

  
  
## <a name="articles-in-transactional-replication"></a>Articles de la réplication transactionnelle 

### <a name="using-sql-server-management-studio"></a>Utilisation de SQL Server Management Studio
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.    
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .    
3.  Cliquez avec le bouton droit sur la publication dont vous souhaitez valider les abonnements, puis cliquez sur **Valider les abonnements**.    
4.  Dans la boîte de dialogue **Valider les abonnements** , sélectionnez les abonnements à valider :   
    -   Sélectionnez **Valider tous les abonnements SQL Server**.    
    -   Sélectionnez **Valider les abonnements suivants**, puis sélectionnez un ou plusieurs abonnements.    
5.  Pour spécifier le type de validation à effectuer (nombre de lignes, ou nombre de lignes et somme de contrôle), cliquez sur **Options de validation**, puis spécifiez les options dans la boîte de dialogue **Options de validation d'abonnement** .  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]   
7.  Affichez les résultats de la validation dans le moniteur de réplication ou dans la boîte de dialogue **Afficher l'état de synchronisation** . Pour chaque abonnement :   
    1.  Développez la publication, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher l'état de synchronisation**.    
    2.  Si l'agent n'est pas en cours d'exécution, cliquez sur **Démarrer** dans la boîte de dialogue **Afficher l'état de synchronisation** . La boîte de dialogue affiche des messages d'information concernant la validation.    
     Si aucun message concernant la validation ne s'affiche, l'agent a déjà consigné un message à ce sujet dans le journal. Dans ce cas, affichez les résultats de la validation dans le moniteur de réplication. Pour plus d'informations, consultez les procédures du moniteur de réplication dans cette rubrique.  

### <a name="using-transact-sql"></a>Utilisation de Transact-SQL

#### <a name="all-articles"></a>Tous les articles 
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md). Spécifiez `@publication` et l’une des valeurs suivantes pour `@rowcount_only` :  
  
    -   **1** - contrôle du nombre de lignes uniquement (par défaut)    
    -   **2** - nombre de lignes et somme de contrôle binaire.  
  
    > [!NOTE]  
    >  Lorsque vous exécutez [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md), [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) est exécuté pour chaque article de la publication. Pour exécuter [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md) avec succès, vous devez avoir les autorisations SELECT sur toutes les colonnes des tables de la base publiée.    
2.  (Facultatif) Démarrez l'Agent de distribution pour chaque abonnement s'il n'est pas déjà en cours d'exécution. Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) et [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
3.  Vérifiez la sortie de l'agent pour le résultat de la validation. 
  
#### <a name="single-article"></a>Article unique  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md). Spécifiez `@publication`, le nom de l’article pour `@article`, et l’une des valeurs suivantes pour `@rowcount_only` :  
  
    -   **1** - contrôle du nombre de lignes uniquement (par défaut)    
    -   **2** - nombre de lignes et somme de contrôle binaire.  
  
    > [!NOTE]  
    >  Pour exécuter [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) avec succès, vous devez avoir les autorisations SELECT sur toutes les colonnes des tables de la base publiée.  
  
2.  (Facultatif) Démarrez l'Agent de distribution pour chaque abonnement s'il n'est pas déjà en cours d'exécution. Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) et [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
3.  Vérifiez la sortie de l'agent pour le résultat de la validation.
  
#### <a name="single-subscriber"></a>Abonné unique 
  
1.  Dans la base de données de publication sur le serveur de publication, ouvrez une transaction explicite en utilisant [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md).    
2.  Dans la base de données de publication du serveur de publication, exécutez [sp_marksubscriptionvalidation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md). Spécifiez la publication pour `@publication`, le nom de l’Abonné pour `@subscriber` et le nom de la base de données d’abonnement pour `@destination_db`.    
3.  (Facultatif) Répétez l'étape 2 pour chaque abonnement en cours de validation.    
4.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md). Spécifiez `@publication`, le nom de l’article pour `@article`, et l’une des valeurs suivantes pour `@rowcount_only` :    
    -   **1** - contrôle du nombre de lignes uniquement (par défaut)    
    -   **2** - nombre de lignes et somme de contrôle binaire.  
  
    > [!NOTE]  
    >  Pour exécuter [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) avec succès, vous devez avoir les autorisations SELECT sur toutes les colonnes des tables de la base publiée.  
  
5.  Dans la base de données de publication sur le serveur de publication, validez la transaction en utilisant [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md).    
6.  (Facultatif) Répétez les étapes 1 à 5 pour chaque article en cours de validation.   
7.  (Facultatif) Démarrez l'Agent de distribution s'il n'est pas déjà en cours d'exécution. Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) et [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
8.  Vérifiez la sortie de l'agent pour le résultat de la validation. Pour plus d'informations, voir [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

## <a name="all-push-subscriptions-to-a-transactional-publication"></a>Tous les abonnements par émission de données à une publication transactionnelle 

### <a name="using-replication-monitor"></a>Utilisation du moniteur de réplication
  
1.  Dans le moniteur de réplication, développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.   
2.  Cliquez avec le bouton droit sur la publication dont vous souhaitez valider les abonnements, puis cliquez sur **Valider les abonnements**.   
3.  Dans la boîte de dialogue **Valider les abonnements** , sélectionnez les abonnements à valider :  
  
    -   Sélectionnez **Valider tous les abonnements SQL Server**.    
    -   Sélectionnez **Valider les abonnements suivants**, puis sélectionnez un ou plusieurs abonnements.    
4.  Pour spécifier le type de validation à effectuer (nombre de lignes, ou nombre de lignes et somme de contrôle), cliquez sur **Options de validation**, puis spécifiez les options dans la boîte de dialogue **Options de validation d'abonnement** .    
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  Cliquez sur l'onglet **Tous les abonnements** .  
7.  Affichez les résultats de la validation. Pour chaque abonnement par envoi de données :    
    1.  Si l'agent n'est pas en cours d'exécution, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.    
    2.  Cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**.   
    3.  Affichez les informations dans l'onglet **Historique du serveur de distribution vers l'Abonné** de la zone de texte **Actions dans la session sélectionnée** .  
  
## <a name="for-a-single-subscription-to-a-merge-publication"></a>Pour un abonnement unique à une publication de fusion

### <a name="using-sql-server-management-studio"></a>Utilisation de SQL Server Management Studio
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.    
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .   
3.  Développez la publication dont vous souhaitez valider les abonnements, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Valider l'abonnement**.    
4.  Dans la boîte de dialogue **Valider l'abonnement** , sélectionnez **Valider cet abonnement**.    
5.  Pour spécifier le type de validation à effectuer (nombre de lignes, ou nombre de lignes et somme de contrôle), cliquez sur **Options**, puis spécifiez les options dans la boîte de dialogue **Options de validation d'abonnement** .    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  Affichez les résultats de la validation dans le moniteur de réplication ou dans la boîte de dialogue **Afficher l'état de synchronisation** :  
  
    1.  Développez la publication, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher l'état de synchronisation**.    
    2.  Si l'agent n'est pas en cours d'exécution, cliquez sur **Démarrer** dans la boîte de dialogue **Afficher l'état de synchronisation** . La boîte de dialogue affiche des messages d'information concernant la validation.  
  
     Si aucun message concernant la validation ne s'affiche, l'agent a déjà consigné un message à ce sujet dans le journal. Dans ce cas, affichez les résultats de la validation dans le moniteur de réplication. Pour plus d'informations, consultez les procédures du moniteur de réplication dans cette rubrique.  
  
## <a name="for-all-subscriptions-to-a-merge-publication"></a>Pour tous les abonnements à une publication de fusion

### <a name="using-sql-server-management-studio"></a>Utilisation de SQL Server Management Studio  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.    
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .   
3.  Cliquez avec le bouton droit sur la publication dont vous souhaitez valider les abonnements, puis cliquez sur **Valider tous les abonnements**.    
4.  Dans la boîte de dialogue **Valider tous les abonnements** , spécifiez le type de validation à effectuer (nombre de lignes, ou nombre de lignes et total de contrôle).   
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  Affichez les résultats de la validation dans le moniteur de réplication ou dans la boîte de dialogue **Afficher l'état de synchronisation** . Pour chaque abonnement :    
    1.  Développez la publication, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher l'état de synchronisation**.    
    2.  Si l'agent n'est pas en cours d'exécution, cliquez sur **Démarrer** dans la boîte de dialogue **Afficher l'état de synchronisation** . La boîte de dialogue affiche des messages d'information concernant la validation.  
  
     Si aucun message concernant la validation ne s'affiche, l'agent a déjà consigné un message à ce sujet dans le journal. Dans ce cas, affichez les résultats de la validation dans le moniteur de réplication. Pour plus d'informations, consultez les procédures du moniteur de réplication dans cette rubrique.  
  
 
## <a name="for-a-single-push-subscription-to-a-merge-publication"></a>Pour un abonnement par émission de données unique à une publication de fusion 

### <a name="using-replication-monitor"></a>Utilisation du moniteur de réplication  
1.  Dans le moniteur de réplication, développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.    
2.  Cliquez sur l'onglet **Tous les abonnements** .    
3.  Cliquez avec le bouton droit sur l'abonnement que vous souhaitez valider, puis cliquez sur **Valider l'abonnement**.    
4.  Dans la boîte de dialogue **Valider l'abonnement** , sélectionnez **Valider cet abonnement**.    
5.  Pour spécifier le type de validation à effectuer (nombre de lignes, ou nombre de lignes et somme de contrôle), cliquez sur **Options**, puis spécifiez les options dans la boîte de dialogue **Options de validation d'abonnement** .    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  Cliquez sur l'onglet **Tous les abonnements** .    
8.  Affichez les résultats de la validation :    
    1.  Si l'agent n'est pas en cours d'exécution, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.    
    2.  Cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**.    
    3.  Affichez les informations dans l'onglet **Historique de synchronisation** de la zone de texte **Dernier message de la session sélectionnée** .  

### <a name="using-transact-sql"></a>Utilisation de Transact-SQL
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_validatemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergesubscription-transact-sql.md). Spécifiez `@publication`, le nom de l’Abonné pour `@subscriber`, le nom de la base de données d’abonnement pour `@subscriber_db`, et l’une des valeurs suivantes pour `@level` :   
    -   **1** - validation du nombre de lignes uniquement.    
    -   **3** - validation de la somme de contrôle binaire du nombre de lignes.  
  
     Les abonnements sélectionnés sont ainsi marqués pour la validation.  
  
2.  Démarrez l'agent de fusion pour chaque abonnement. Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) et [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
3.  Vérifiez la sortie de l'agent pour le résultat de la validation.   
4.  Répétez les étapes 1 à 3 pour chaque abonnement en cours de validation.  
  
> [!NOTE]  
>  Un abonnement à une publication de fusion peut également être validé à la fin d'une synchronisation en spécifiant le paramètre **-Validate** lors de l'exécution de l' [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
## <a name="for-all-push-subscriptions-to-a-merge-publication"></a>Pour tous les abonnements par émission de données à une publication de fusion
  
### <a name="using-replication-monitor"></a>Utilisation du moniteur de réplication    
1.  Dans le moniteur de réplication, développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.    
2.  Cliquez avec le bouton droit sur la publication dont vous souhaitez valider les abonnements, puis cliquez sur **Valider tous les abonnements**.    
3.  Dans la boîte de dialogue **Valider tous les abonnements** , spécifiez le type de validation à effectuer (nombre de lignes, ou nombre de lignes et total de contrôle).    
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
5.  Cliquez sur l'onglet **Tous les abonnements** .    
6.  Affichez les résultats de la validation. Pour chaque abonnement par envoi de données :    
    1.  Si l'agent n'est pas en cours d'exécution, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.    
    2.  Cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**.    
    3.  Affichez les informations dans l'onglet **Historique de synchronisation** de la zone de texte **Dernier message de la session sélectionnée** . 
  
### <a name="using-transact-sql"></a>Utilisation de Transact-SQL
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_validatemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergepublication-transact-sql.md). Spécifiez `@publication` et l’une des valeurs suivantes pour `@level` :    
    -   **1** - validation du nombre de lignes uniquement.   
    -   **3** - validation de la somme de contrôle binaire du nombre de lignes.  
  
     Tous les abonnements sont ainsi marqués pour la validation.   
2.  Démarrez l'agent de fusion pour chaque abonnement. Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) et [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
3.  Vérifiez la sortie de l'agent pour le résultat de la validation. Pour plus d'informations, voir [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

  
## <a name="validate-data-using-merge-agent-parameters"></a>Valider des données à l’aide des paramètres de l’Agent de fusion  
  
1.  Démarrez l'Agent de fusion sur l'Abonné (abonnement par extraction) ou sur le serveur de distribution (abonnement par émission de données) à partir de l'invite de commandes de l'une des façons suivantes.   
    -   Spécifiez une valeur de **1** (nombre de lignes) ou **3** (nombre de lignes et somme de contrôle de binaire) pour le paramètre **-Validate** .    
    -   Spécifiez la **validation du nombre de lignes** ou le **nombre de lignes et la validation de la somme de contrôle** pour le paramètre **-ProfileName** .  
  
     Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) ou [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 La réplication permet d'utiliser les objets RMO (Replication Management Objects) pour vérifier par programmation que les données de l'Abonné correspondent à celles du serveur de publication. Les objets que vous utilisez dépendent du type de topologie de réplication. La réplication transactionnelle requiert la validation de tous les abonnements à une publication.  
  
> [!NOTE]  
>  Consultez l' [Exemple (RMO)](#RMOExample)plus loin dans cette rubrique.  
  
#### <a name="to-validate-data-for-all-articles-in-a-transactional-publication"></a>Pour valider les données de tous les articles d'une publication transactionnelle  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication>. Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication. Définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en spécifiant la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés restantes de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.TransPublication.ValidatePublication%2A> . Passez les éléments suivants :  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationOption>  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationMethod>  
  
    -   Une valeur booléenne qui indique s'il faut arrêter l'Agent de distribution une fois la validation terminée.  
  
     Cela marque les articles pour la validation.  
  
5.  S'il n'est pas déjà en cours d'exécution, démarrez l'Agent de distribution pour synchroniser chaque abonnement. Pour plus d'informations, consultez [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) ou [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md). Le résultat de l'opération de validation est écrit dans l'historique de l'agent. Pour plus d'informations, voir [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
#### <a name="to-validate-data-in-all-subscriptions-to-a-merge-publication"></a>Pour valider les données de tous les abonnements à une publication de fusion  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication. Définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en spécifiant la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés restantes de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergePublication.ValidatePublication%2A> . Passez le <xref:Microsoft.SqlServer.Replication.ValidationOption>souhaité.  
  
5.  Exécutez l'Agent de fusion pour chaque abonnement pour démarrer la validation ou attendez que l'agent planifié suivant ne s'exécute. Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) et [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md). Le résultat de l'opération de validation est écrit dans l'historique de l'agent, que vous affichez en utilisant le moniteur de réplication. Pour plus d'informations, voir [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
#### <a name="to-validate-data-in-a-single-subscription-to-a-merge-publication"></a>Pour valider les données d'un seul abonnement à une publication de fusion  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication. Définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en spécifiant la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés restantes de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergePublication.ValidateSubscription%2A> . Passez le nom de l'Abonné et de la base de données d'abonnement en cours de validation, ainsi que le <xref:Microsoft.SqlServer.Replication.ValidationOption>souhaité.  
  
5.  Exécutez l'Agent de fusion de l'abonnement pour démarrer la validation ou attendez que l'agent planifié suivant ne s'exécute. Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) et [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md). Le résultat de l'opération de validation est écrit dans l'historique de l'agent, que vous affichez en utilisant le moniteur de réplication. Pour plus d'informations, voir [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
###  <a name="example-rmo"></a><a name="RMOExample"></a> Exemple (RMO)  
 Cet exemple marque tous les abonnements à une publication transactionnelle pour une validation du nombre de lignes.  
  
 [!code-cs[HowTo#rmo_ValidateTranPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatetranpub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateTranPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatetranpub)]  
  
 Cet exemple marque un abonnement spécifique à une publication de fusion pour une validation du nombre de lignes.  
  
 [!code-cs[HowTo#rmo_ValidateMergeSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatemergesub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateMergeSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatemergesub)]  
  
 ## <a name="see-also"></a>Voir aussi  
[Best Practices for Replication Administration](../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  
