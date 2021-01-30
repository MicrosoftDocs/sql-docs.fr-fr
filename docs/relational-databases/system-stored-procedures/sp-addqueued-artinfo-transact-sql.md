---
description: sp_addqueued_artinfo (Transact-SQL)
title: sp_addqueued_artinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addqueued_artinfo
- sp_addqueued_artinfo_TSQL
helpviewer_keywords:
- sp_addqueued_artinfo
ms.assetid: decdb6eb-3dcd-4053-a21d-fd367c3fbafb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c0899ff490b553471fc766413fdf0cca27955fa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206655"
---
# <a name="sp_addqueued_artinfo-transact-sql"></a>sp_addqueued_artinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  
  
> [!IMPORTANT]  
>  La procédure [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) doit être utilisée à la place de **sp_addqueued_artinfo**. [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) génère un script qui contient les appels de **sp_addqueued_artinfo** et de **sp_addsynctrigger** .  
  
 Crée la table [MSsubscription_articles](../../relational-databases/system-tables/mssubscription-articles-transact-sql.md) sur l’abonné utilisé pour suivre les informations d’abonnement de l’article (mise à jour en attente et mise à jour immédiate avec mise à jour en file d’attente comme basculement). Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addqueued_artinfo [ @artid= ] 'artid'  
        , [ @article= ] 'article'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @dest_table= ] 'dest_table'  
        , [ @owner = ] 'owner'  
        , [ @cft_table= ] 'cft_table'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @artid = ] 'artid'` Nom de l’ID de l’article. *artid* est de **type int**, sans valeur par défaut  
  
`[ @article = ] 'article'` Nom de l’article pour lequel générer un script. *article* est de **type sysname** et n’a pas de valeur par défaut  
  
`[ @publisher = ] 'publisher'` Est le nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication à écrire. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @dest_table = ] _'dest_table'` Nom de la table de destination. *dest_table* est de **type sysname**, sans valeur par défaut.  
  
 [**@owner =** ] **'**_propriétaire_**'**  
 Propriétaire de l’abonnement. *owner* est de **type sysname**, sans valeur par défaut.  
  
`[ @cft_table = ] 'cft_table'` Nom de la table de conflits de mise à jour en attente pour cet article. *cft_table* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addqueued_artinfo** est utilisé par le agent de distribution dans le cadre de l’initialisation de l’abonnement. Cette procédure stockée n'est généralement pas exécutée par les utilisateurs, mais peut s'avérer utile si l'utilisateur doit configurer manuellement un abonnement.  
  
 [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) au lieu de **sp_addqueued_artinfo**.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addqueued_artinfo**.  
  
## <a name="see-also"></a>Voir aussi  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [MSsubscription_articles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mssubscription-articles-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
