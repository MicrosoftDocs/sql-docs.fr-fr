---
description: sp_attachsubscription (Transact-SQL)
title: sp_attachsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_attachsubscription
- sp_attachsubscription_TSQL
helpviewer_keywords:
- sp_attachsubscription
ms.assetid: b9bbda36-a46a-4327-a01e-9cd632e4791b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a7357d6a37472d9d530dfcf3ecfe3c9bddfffd58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206640"
---
# <a name="sp_attachsubscription-transact-sql"></a>sp_attachsubscription (Transact-SQL)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

  Attache une base de données d'abonnement existante à un Abonné. Cette procédure stockée est exécutée sur la base de données master du nouvel Abonné.  
  
> [!IMPORTANT]  
>  Cette fonctionnalité est déconseillée et sera retirée dans une version prochaine. Elle ne doit pas être utilisée dans tout nouveau travail de développement. Dans le cas des publications de fusion partitionnées par le biais de filtres paramétrés, nous vous recommandons d'utiliser plutôt les nouvelles fonctionnalités d'instantanés partitionnés qui simplifient l'initialisation de larges volumes d'abonnements. Pour plus d'informations, voir [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). Dans le cas de publications qui ne sont pas partitionnées, vous pouvez initialiser un abonnement par le biais d'une sauvegarde. Pour plus d’informations, consultez [Initialiser un abonnement transactionnel sans instantané](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_attachsubscription [ @dbname = ] 'dbname'  
        , [ @filename = ] 'filename'  
    [ , [ @subscriber_security_mode = ] 'subscriber_security_mode' ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @publisher_security_mode = ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @db_master_key_password = ] 'db_master_key_password' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @dbname = ] 'dbname'` Chaîne qui spécifie la base de données d’abonnement de destination par son nom. *dbname* est de **type sysname**, sans valeur par défaut.  
  
`[ @filename = ] 'filename'` Nom et emplacement physique du fichier MDF **primaire (fichier de données** de référence). *filename* est de type **nvarchar (260)**, sans valeur par défaut.  
  
`[ @subscriber_security_mode = ] 'subscriber_security_mode'` Mode de sécurité de l’abonné à utiliser lors de la connexion à un abonné lors de la synchronisation. *subscriber_security_mode* est de **type int**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  L'authentification Windows doit être la méthode à employer. Si *subscriber_security_mode* n’est pas **1** (authentification Windows), une erreur est retournée.  
  
`[ @subscriber_login = ] 'subscriber_login'` Nom de connexion de l’abonné à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts. Si *subscriber_security_mode* n’est pas **1** et que *subscriber_login* est spécifié, une erreur est retournée.  
  
`[ @subscriber_password = ] 'subscriber_password'` Mot de passe de l’abonné. *subscriber_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts. Si *subscriber_security_mode* n’est pas **1** et que *subscriber_password* est spécifié, une erreur est retournée.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Mode de sécurité à utiliser lors de la connexion à un serveur de distribution lors de la synchronisation. *distributor_security_mode* est de **type int**, avec **0** comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** spécifie l’authentification Windows. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Connexion du serveur de distribution à utiliser lors de la connexion à un serveur de distribution lors de la synchronisation. *distributor_login* est requis si *distributor_security_mode* a la valeur **0**. *distributor_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @distributor_password = ] 'distributor_password'` Est le mot de passe du serveur de distribution. *distributor_password* est requis si *distributor_security_mode* a la valeur **0**. *distributor_password* est de **type sysname**, avec NULL comme valeur par défaut. La valeur de *distributor_password* doit être inférieure à 120 caractères Unicode.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Mode de sécurité à utiliser lors de la connexion à un serveur de publication lors de la synchronisation. *publisher_security_mode* est de **type int**, avec **1** comme valeur par défaut. Si la **valeur est 0**, spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Si **1**, spécifie l’authentification Windows. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Nom de connexion à utiliser lors de la connexion à un serveur de publication lors de la synchronisation. *publisher_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @publisher_password = ] 'publisher_password'` Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est de **type sysname**, avec NULL comme valeur par défaut. La valeur de *publisher_password* doit être inférieure à 120 caractères Unicode.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @job_login = ] 'job_login'` Nom de connexion du compte Windows sous lequel l’agent s’exécute. *job_login* est de type **nvarchar (257)**, sans valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions des agents au serveur de distribution.  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte Windows sous lequel l’agent s’exécute. *job_password* est de **type sysname**, sans valeur par défaut. La valeur de *job_password* doit être inférieure à 120 caractères Unicode.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @db_master_key_password = ] 'db_master_key_password'` Mot de passe d’une clé principale de base de données définie par l’utilisateur. *db_master_key_password* est de type **nvarchar (524)**, avec NULL comme valeur par défaut. Si *db_master_key_password* n’est pas spécifié, une clé principale de base de données existante est supprimée et recréée.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_attachsubscription** est utilisé dans la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
 Un abonnement ne peut pas être attaché à la publication si la période de rétention de celle-ci a expiré. Si vous spécifiez un abonnement dont la période de rétention est écoulée, une erreur se produit lors de l'attachement de l'abonnement ou de sa première synchronisation. Les publications avec une période de rétention de publication égale à **0** (n’expire jamais) sont ignorées.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_attachsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
