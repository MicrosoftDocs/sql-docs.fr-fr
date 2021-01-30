---
description: sp_link_publication (Transact-SQL)
title: sp_link_publication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_link_publication_TSQL
- sp_link_publication
helpviewer_keywords:
- sp_link_publication
ms.assetid: 1945ed24-f9f1-4af6-94ca-16d8e864706e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b6a6e0b057f0d01c838aa5ac7b57143221fec13c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210776"
---
# <a name="sp_link_publication-transact-sql"></a>sp_link_publication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit les informations de configuration et de sécurité utilisées par les déclencheurs de synchronisation des abonnements avec mise à jour immédiate lors de la connexion au serveur de publication. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
> [!IMPORTANT]
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
> 
> [!IMPORTANT]
>  Dans certaines conditions, cette procédure stockée peut échouer si l’abonné exécute [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 ou version ultérieure, et que le serveur de publication exécute une version antérieure. Si la procédure stockée échoue dans ce scénario, mettez à niveau l'Éditeur vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 ou version ultérieure.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_link_publication [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'   
        , [ @publication = ] 'publication'   
        , [ @security_mode = ] security_mode  
    [ , [ @login = ] 'login' ]  
    [ , [ @password = ]'password' ]  
    [ , [ @distributor = ] 'distributor' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom de l’éditeur à lier. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication vers laquelle établir la liaison. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication à lier. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @security_mode = ] security_mode` Mode de sécurité utilisé par l’abonné pour se connecter à un serveur de publication distant pour la mise à jour immédiate. *security_mode* est de **type int** et peut prendre l’une des valeurs suivantes. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification avec la connexion spécifiée dans cette procédure stockée en tant que *connexion* et *mot de passe*.<br /><br /> Remarque : dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette option était utilisée pour spécifier un appel de procédure distante dynamique (RPC).|  
|**1**|Utilise le contexte de sécurité (authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Windows) de l'utilisateur apportant la modification sur l'Abonné.<br /><br /> Remarque : ce compte doit également exister sur le serveur de publication avec des privilèges suffisants. Lorsque vous utilisez l'authentification Windows, la délégation de compte de sécurité doit être prise en charge.|  
|**2**|Utilise une connexion de serveur lié existante, définie par l’utilisateur et créée à l’aide de **sp_link_publication**.|  
  
`[ @login = ] 'login'` Nom de la connexion. *login* est de type **sysname**, avec NULL comme valeur par défaut. Ce paramètre doit être spécifié lorsque *security_mode* a la **valeur 0**.  
  
`[ @password = ] 'password'` Est le mot de passe. *Password* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre doit être spécifié lorsque *security_mode* a la **valeur 0**.  
  
`[ @distributor = ] 'distributor'` Nom du serveur de distribution. *Distributor* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_link_publication** est utilisé par les abonnements avec mise à jour immédiate lors de la réplication transactionnelle.  
  
 **sp_link_publication** peut être utilisé pour les abonnements par envoi et par extraction. Cette procédure peut être appelée avant ou après la création de l’abonnement. Une entrée est insérée ou mise à jour dans la table système [MSsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) .  
  
 Pour les abonnements par émission de type push, l’entrée peut être nettoyée par [sp_subscription_cleanup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md). Pour les abonnements par extraction, l’entrée peut être nettoyée par [sp_droppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md) ou [sp_subscription_cleanup &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md). Vous pouvez également appeler **sp_link_publication** avec un mot de passe Null pour effacer l’entrée de la table système [MSsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) pour des questions de sécurité.  
  
 Le mode par défaut utilisé par un Abonné de mise à jour immédiate lors de sa connexion au serveur de publication n'autorise pas l'utilisation de l'authentification Windows. La connexion avec un mode d'authentification Windows suppose qu'un serveur lié a été configuré au niveau du serveur de publication, et l'Abonné de mise à jour immédiate doit utiliser cette connexion lors de la mise à jour de l'Abonné. Pour cela, le **sp_link_publication** doit être exécuté avec *security_mode*  =  **2**. Lorsque vous utilisez l'authentification Windows, la délégation de compte de sécurité doit être prise en charge.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent_failover](../../relational-databases/replication/codesnippet/tsql/sp-link-publication-tran_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_link_publication**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [sp_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
