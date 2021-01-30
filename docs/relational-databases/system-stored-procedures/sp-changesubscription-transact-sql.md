---
description: sp_changesubscription (Transact-SQL)
title: sp_changesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- changesubscription
- sp_changesubscription
- changesubscription_TSQL
- sp_changesubscription_TSQL
helpviewer_keywords:
- sp_changesubscription
ms.assetid: f9d91fe3-47cf-4915-b6bf-14c9c3d8a029
author: markingmyname
ms.author: maghan
ms.openlocfilehash: db0e26092acb2a8cb0fc546a18c0b8f4010fa8fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189588"
---
# <a name="sp_changesubscription-transact-sql"></a>sp_changesubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie les propriétés d'un abonnement par envoi de données (push) ou par extraction de données (pull) d'instantané ou transactionnel, qui participe à une réplication transactionnelle de mise à jour en attente. Pour modifier les propriétés de tous les autres types d’abonnements par extraction, utilisez [sp_change_subscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md). **sp_changesubscription** est exécutée sur la base de données de publication sur le serveur de publication.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changesubscription [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
        , [ @subscriber = ] 'subscriber'  
        , [ @destination_db = ] 'destination_db'  
        , [ @property = ] 'property'  
        , [ @value = ] 'value'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication à modifier. *publication* est de **type sysname**, sans valeur par défaut  
  
`[ @article = ] 'article'` Nom de l’article à modifier. *article* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, sans valeur par défaut.  
  
`[ @destination_db = ] 'destination_db'` Nom de la base de données d’abonnement. *destination_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @property = ] 'property'` Propriété à modifier pour l’abonnement donné. la *propriété* est de type **nvarchar (30)** et peut prendre l’une des valeurs de la table.  
  
`[ @value = ] 'value'` Nouvelle valeur de la *propriété* spécifiée. la *valeur* est de type **nvarchar (4000)** et peut prendre l’une des valeurs de la table.  
  
|Propriété|Valeur|Description|  
|--------------|-----------|-----------------|  
|**distrib_job_login**||Nom de connexion du compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent s'exécute.|  
|**distrib_job_password**||Mot de passe du compte Windows sous lequel l’agent s’exécute.|  
|**subscriber_catalog**||Catalogue à utiliser lors d’une connexion au fournisseur OLE DB. Cette propriété n’est valide que pour les abonnés non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**subscriber_datasource**||Nom de la source de données tel qu'il est interprété par le fournisseur OLE DB. *Cette propriété n’est valide que pour les non-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Abonnés.*|  
|**subscriber_location**||Emplacement de la base de données tel qu’il est interprété par le fournisseur OLE DB. *Cette propriété n’est valide que pour les non-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Abonnés.*|  
|**subscriber_login**||Nom de la connexion du côté Abonné.|  
|**subscriber_password**||Mot de passe fort pour le nom de connexion fourni.|  
|**subscriber_security_mode**|**1**|Utilise l'authentification Windows pour la connexion à l'Abonné.|  
||**0**|Utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la connexion à l'Abonné.|  
|**subscriber_provider**||Identificateur de programme unique (PROGID) avec lequel le fournisseur OLE DB de la source de données non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est inscrit. *Cette propriété n’est valide que pour les non-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Abonnés.*|  
|**subscriber_providerstring**||Chaîne de connexion propre au fournisseur OLE DB qui identifie la source de données. *Cette propriété n’est valide que pour les non-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Abonnés.*|  
|**SubscriptionStreams**||Nombre de connexions autorisées par Agent de distribution pour appliquer en parallèle des traitements de modifications à un Abonné. Une plage de valeurs comprises entre **1** et **64** est prise en charge pour les serveurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication. Cette propriété doit être **égale à 0** pour les abonnés non-, les serveurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication Oracle ou les abonnements d’égal à égal.|  
|**subscriber_type**|**1**|Serveur de la source de données ODBC.|  
||**3**|Fournisseur OLE DB|  
|**memory_optimized**|**bit**|Indique que l’abonnement prend en charge les tables optimisées en mémoire. *memory_optimized* est de **bits**, où 1 est égal à true (l’abonnement prend en charge les tables optimisées en mémoire).|  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être spécifié pour un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changesubscription** est utilisé dans la réplication transactionnelle et d’instantané.  
  
 **sp_changesubscription** ne peut être utilisé que pour modifier les propriétés des abonnements par émission de type push ou des abonnements par extraction impliqués dans la réplication transactionnelle de mise à jour en attente. Pour modifier les propriétés de tous les autres types d’abonnements par extraction, utilisez [sp_change_subscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md).  
  
 Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_changesubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)  
  
  
