---
title: Propriétés de l’abonnement, boîte de dialogue
description: Décrit la boîte de dialogue « Propriétés de l’abonnement » dans SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.subproperties.publisher.f1
- sql13.rep.newsubwizard.subproperties.subscriber.f1
ms.assetid: db2be511-c76e-4f21-8be4-6a8c60a50d30
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 1d0af8d38957286080b6e01c4ed73149ee43bc36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272330"
---
# <a name="sql-server-replication-subscription-properties-dialog-box"></a>Réplication SQL Server, boîte de dialogue Propriétés de l’abonnement 
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

### <a name="publisher-properties"></a>Propriétés du serveur de publication
La boîte de dialogue **Propriétés de l'abonnement** du serveur de publication permet d'afficher et de configurer les propriétés des abonnements par envoi de données. Vous pouvez également afficher certaines propriétés des abonnements par extraction. Cependant, la boîte de dialogue **Propriétés de l'abonnement** de l'abonné affiche des propriétés supplémentaires que vous pouvez modifier.  
  
 Chaque propriété de **cette boîte de dialogue** comporte une description. Cliquez sur une propriété pour afficher sa description au bas de la boîte de dialogue. Cette rubrique fournit des informations supplémentaires sur diverses propriétés, dont la plupart sont affichées dans le serveur de publication uniquement pour les abonnements par envoi de données. Les propriétés sont regroupées selon les catégories suivantes :  
  
-   Propriétés appliquées à tous les abonnements.    
-   Propriétés appliquées aux abonnements transactionnels.  
-   Propriétés appliquées aux abonnements de fusion.  
  
 Si une option est affichée en lecture seule, vous pouvez la configurer uniquement lorsque l'abonnement est créé. Si vous voulez configurer des options non disponibles dans l'Assistant Nouvel abonnement, créez l'abonnement avec des procédures stockées. Pour plus d'informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) et [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  

### <a name="subscriber-properties"></a>Propriétés de l’abonné
La boîte de dialogue **Propriétés de l'abonnement** de l'abonné permet d'afficher et de configurer les propriétés des abonnements par extraction.  
  
 Chaque propriété de **cette boîte de dialogue** comporte une description. Cliquez sur une propriété pour afficher sa description au bas de la boîte de dialogue. Cette rubrique fournit des informations sur diverses propriétés, Les propriétés sont regroupées selon les catégories suivantes :    
-   Propriétés appliquées à tous les abonnements.    
-   Propriétés appliquées aux abonnements transactionnels.   
-   Propriétés appliquées aux abonnements de fusion.  
  
 Si une option est affichée en lecture seule, vous pouvez la configurer uniquement lorsque l'abonnement est créé. Si vous voulez configurer des options non disponibles dans l'Assistant Nouvel abonnement, créez l'abonnement avec des procédures stockées. Pour plus d'informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) et [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
> [!NOTE]  
>  - Si un Agent de distribution ou un Agent de fusion n'est pas encore créé pour l'abonnement, de nombreuses propriétés ne sont pas affichées. Pour créer un travail d’agent pour un abonnement par extraction, exécutez [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) (pour un abonnement destiné à une publication transactionnelle ou d’instantané) ou [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) (pour un abonnement destiné à une publication de fusion).  
> - Les bases de données Azure SQL Managed Instance peuvent être un serveur de publication, un serveur de distribution et un abonné pour la réplication transactionnelle et d’instantané. Les bases de données dans Azure SQL Database peuvent uniquement être des abonnés de type push pour la réplication transactionnelle et d’instantané. Pour plus d’informations, consultez [Réplication transactionnelle avec Azure SQL Database](/azure/sql-database/sql-database-managed-instance-transactional-replication). 
  
## <a name="publisher-options-for-all-subscriptions"></a>Options du serveur de publication pour tous les abonnements  
 **Sécurité**  
 Cliquez sur la ligne **Compte de processus de l'agent** , puis sur le bouton des propriétés ( **...** ) pour modifier le compte sous lequel l'Agent de distribution ou de fusion s'exécute sur le serveur de distribution. Pour modifier le compte sous lequel l'Agent de distribution ou de fusion établit les connexions avec l'abonné, cliquez sur **Connexion de l'Abonné**, puis sur le bouton des propriétés ( **...** ).  
  
 Pour plus d'informations sur les autorisations indispensables pour chaque agent, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="publisher-options-for-transactional-subscriptions"></a>Options du serveur de publication pour les abonnements transactionnels  
 **Empêcher le bouclage de la transaction**  
 Détermine si l'Agent de distribution retourne à l'Abonné les transactions créées sur ce dernier. Cette option est utilisée pour la réplication transactionnelle bidirectionnelle. Pour plus d’informations, voir [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).  
  
 **Abonnement pouvant être mis à jour**  
 Détermine si les modifications de l'Abonné sont répliquées dans le serveur de publication. Vous pouvez répliquer les modifications en utilisant la mise à jour immédiate ou en file d'attente. L'option **Méthode de mise à jour de l'Abonné** détermine la méthode à utiliser. Pour plus d’informations, voir [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="publisher-options-for-merge-subscriptions"></a>Options du serveur de publication pour les abonnements de fusion  
 **Définition de la partition (HOST_NAME)**  
 Pour une publication qui utilise des filtres paramétrables, la publication de fusion évalue une des deux fonctions système (ou les deux si le filtre fait référence aux deux) pendant la synchronisation pour déterminer les données qu'un abonné doit recevoir : **SUSER_SNAME()** ou **HOST_NAME()** . Par défaut, **HOST_NAME()** retourne le nom de l'ordinateur sur lequel s'exécute l'Agent de fusion, mais vous pouvez l'ignorer dans l'Assistant Nouvel abonnement. Pour plus d'informations sur les filtres paramétrés et l'annulation de la fonction **HOST_NAME()** , consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Type d'abonnement** et **Priorité**  
 Indique si l'abonnement est un abonnement de client ou de serveur (cette option n'est pas modifiable après la création de l'abonnement). Les abonnements de serveur peuvent republier les données vers d'autres abonnés. Il est possible de leur affecter une priorité pour la résolution des conflits.  
  
 Si vous avez sélectionné un abonnement de type serveur dans l'Assistant Nouvel abonnement, l'abonné a la priorité utilisée lors de la résolution des conflits.  
  
 **Résoudre les conflits interactivement**  
 Détermine s'il faut utiliser l'interface utilisateur du Résolveur interactif pour résoudre les conflits pendant la synchronisation de fusion. Pour cela, l'option **Utiliser le Gestionnaire de synchronisation Windows** doit être active ( **Activer**). Pour plus d’informations, consultez [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  

  
## <a name="subscriber-options-for-all-subscriptions"></a>Options de l’Abonné pour tous les abonnements  
 **Initialise les données publiées à partir d'un instantané**  
 Détermine si les options sont initialisées avec un instantané (par défaut) ou une autre méthode. Pour plus d’informations sur l’initialisation des abonnements, consultez [Initialiser un abonnement](../../relational-databases/replication/initialize-a-subscription.md).  
  
 **Emplacement de l'instantané**  
 Détermine l'emplacement à partir duquel les fichiers d'instantanés sont accessibles lors de l'initialisation ou de la réinitialisation de l'abonnement. Les valeurs suivantes peuvent déterminer l'emplacement :  
  
-   **Emplacement par défaut**: emplacement par défaut défini lors de la configuration d'un serveur de distribution. Pour plus d’informations, consultez [Spécifier les options des instantanés](../../relational-databases/replication/snapshot-options.md).  
  
-   **Autre dossier**: autre emplacement que vous pouvez spécifier dans la boîte de dialogue **Propriétés de la publication** . Pour plus d’informations, consultez [Spécifier les options des instantanés](../../relational-databases/replication/snapshot-options.md).  
  
-   **Dossier d'instantanés dynamiques**: emplacement d'instantané pour les publications de fusion à filtres de lignes paramétrables. Pour plus d'informations, voir [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
-   **Dossier FTP**: dossier accessible sur un serveur FTP (File Transfer Protocol). Pour plus d’informations, consultez [Remettre des instantanés par le biais du protocole FTP](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md).  
  
 **Dossier d'instantanés**  
 Si vous sélectionnez une valeur différente de l' **Emplacement par défaut** de l'option **Emplacement de l'instantané** , vous devez spécifier un chemin d'accès au dossier d'instantanés.  
  
 **Utiliser le Gestionnaire de synchronisation Windows**  
 Détermine s'il est possible de synchroniser l'abonnement à l'aide du Gestionnaire de synchronisation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Sécurité**  
 Cliquez sur la ligne **Compte de processus de l'agent** , puis sur le bouton des propriétés ( **...** ) pour modifier le compte sous lequel l'Agent de distribution ou de fusion s'exécute sur l'Abonné. Les options de sécurité des connexions dépendent du type d'abonnement :  
  
-   Pour les abonnements à une publication transactionnelle : pour modifier le compte sous lequel l'Agent de distribution établit les connexions au serveur de distribution, cliquez sur **Connexion du serveur de distribution**, puis sur le bouton des propriétés ( **...** ).  
  
-   Pour les abonnements avec mise à jour immédiate à une publication transactionnelle : outre la connexion au serveur de distribution décrite ci-dessus, vous pouvez modifier la méthode utilisée pour propager les modifications de l'Abonné au serveur de publication : cliquez sur **Connexion du serveur de publication**, puis sur le bouton des propriétés ( **...** ).  
  
-   Pour les abonnements aux publications de fusion, cliquez **Connexion du serveur de publication**, puis sur le bouton des propriétés ( **...** ).  
  
 Pour plus d'informations sur les autorisations indispensables pour chaque agent, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="subscriber-options-for-transactional-subscriptions"></a>Options de l’Abonné pour les abonnements transactionnels  
 **Abonnement pouvant être mis à jour**  
 Détermine si les modifications de l'Abonné sont répliquées dans le serveur de publication. Vous pouvez répliquer les modifications en utilisant la mise à jour immédiate ou en file d'attente. L'option **Méthode de mise à jour de l'Abonné** détermine la méthode à utiliser. Pour plus d’informations, voir [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="options-for-merge-subscriptions"></a>Options des abonnements de fusion  
 **Définition de la partition (HOST_NAME)**  
 Pour une publication qui utilise des filtres paramétrables, la publication de fusion évalue une des deux fonctions système (ou les deux si le filtre fait référence aux deux) pendant la synchronisation pour déterminer les données qu'un abonné doit recevoir : **SUSER_SNAME()** ou **HOST_NAME()** . Par défaut, **HOST_NAME()** retourne le nom de l'ordinateur sur lequel s'exécute l'Agent de fusion, mais vous pouvez l'ignorer dans l'Assistant Nouvel abonnement. Pour plus d'informations sur les filtres paramétrés et l'annulation de la fonction **HOST_NAME()** , consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Type d'abonnement** et **Priorité**  
 Indique si l'abonnement est un abonnement de client ou de serveur (cette option n'est pas modifiable après la création de l'abonnement). Les abonnements de serveur peuvent republier les données vers d'autres abonnés. Il est possible de leur affecter une priorité pour la résolution des conflits.  
  
 Si vous avez sélectionné un abonnement de type serveur dans l'Assistant Nouvel abonnement, l'abonné a la priorité utilisée lors de la résolution des conflits.  
  
 **Résoudre les conflits interactivement**  
 Détermine s'il faut utiliser l'interface utilisateur du Résolveur interactif pour résoudre les conflits pendant la synchronisation de fusion. Pour cela, l'option **Utiliser le Gestionnaire de synchronisation Windows** doit être active ( **Activer**). Pour plus d’informations, consultez [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
 **Synchronisation Web**  
 L'option **Utiliser la synchronisation Web** détermine s'il est nécessaire de connecter un serveur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Information Services (IIS) pour synchroniser l'abonnement. Cette option est disponible uniquement si la publication est activée pour la synchronisation. Pour plus d’informations, voir [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
 Si vous sélectionnez la valeur **True** pour l'option **Utiliser la synchronisation Web**:  
  
-   Entrez l'adresse complète du serveur IIS dans le champ **Adresse du serveur Web**.    
-   Cliquez sur la ligne **Connexion du serveur Web** , puis sur le bouton de propriétés ( **...** ) pour définir le compte sous lequel l'abonné se connecte au serveur IIS.    
-   Modifiez la valeur **Délai d'attente du serveur Web** si nécessaire. Ce délai d'attente représente le temps (en secondes) écoulé avant l'expiration d'une demande de synchronisation Web. 

 
Pour plus d'informations sur la configuration, consultez [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md). 
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un abonnement par extraction](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Afficher et modifier les propriétés d’un abonnement par émission de données](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)   

  
  
