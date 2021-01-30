---
title: sp_validate_replica_hosts_as_publishers (T-SQL)
description: Décrit la procédure stockée sp_validate_replica_hosts_as_publishers qui autorise la validation de tous les réplicas secondaires.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_validate_replica_hosts_as_publishers_TSQL
- sp_validate_replica_hosts_as_publishers
helpviewer_keywords:
- sp_validate_replica_hosts_as_publishers
ms.assetid: 45001fc9-2dbd-463c-af1d-aa8982d8c813
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4f00ca4f972b7fccfa09f9c36c8e5b7fa09420fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205519"
---
# <a name="sp_validate_replica_hosts_as_publishers-transact-sql"></a>sp_validate_replica_hosts_as_publishers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **sp_validate_replica_hosts_as_publishers** est une extension de **sp_validate_redirected_publisher** qui permet à tous les réplicas secondaires d’être validés, et non pas seulement le réplica principal actuel. **sp_validate_replicat_hosts_as_publisher** valide l’intégralité d’une topologie de réplication Always on. les **sp_validate_replica_hosts_as_publishers** doivent être exécutées directement sur le serveur de distribution à l’aide d’une session Bureau à distance pour éviter une erreur de sécurité de double saut (21892).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_validate_replica_hosts_as_publishers   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @redirected_publisher = ] 'new_publisher' output  
```  
  
## <a name="arguments"></a>Arguments  
`[ @original_publisher = ] 'original_publisher'` Nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a initialement publié la base de données. *original_publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données en cours de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @redirected_publisher = ] 'redirected_publisher'` Cible de la redirection lorsque **sp_redirect_publisher** a été appelé pour la paire serveur de publication/base de données publiée d’origine. *redirected_publisher* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 Si aucune entrée n’existe pour le serveur de publication et la base de données de publication, **sp_validate_redirected_publisher** retourne la valeur null pour le paramètre de sortie *\@ redirected_publisher*. Sinon, le serveur de publication redirigé associé est retourné, à la fois en cas de réussite et d'échec.  
  
 Si la validation réussit, **sp_validate_redirected_publisher** retourne une indication de réussite.  
  
 Si la validation échoue, les erreurs appropriées sont générées.  **sp_validate_redirected_publisher** fait le meilleur effort pour signaler tous les problèmes et pas seulement le premier rencontré.  
  
> [!NOTE]  
>  **sp_validate_replica_hosts_as_publishers** échoue avec l’erreur suivante lors de la validation des hôtes de réplica secondaire qui n’autorisent pas l’accès en lecture, ou nécessitent la spécification de l’intention de lecture.  
>   
>  Msg 21899, Niveau 11, État 1, Procédure **sp_hadr_verify_subscribers_at_publisher**, Ligne 109  
>   
>  La requête au serveur de publication redirigé 'MyReplicaHostName' pour déterminer s’il y a des entrées sysserver pour les abonnés du serveur de publication d’origine 'MyOriginalPublisher' a échoué avec l’erreur '976', message d’erreur 'Erreur 976, Niveau 14, État 1, Message : La base de données cible, 'MyPublishedDB', est membre d’un groupe de disponibilité et n’est actuellement pas accessible pour les requêtes. Le déplacement des données est alors suspendu ou le réplica de disponibilité n'est pas activé pour l'accès en lecture. Pour autoriser l'accès en lecture seule à cette base de données et à d'autres dans le groupe de disponibilité, activez l'accès en lecture sur un ou plusieurs réplicas de disponibilité secondaires dans le groupe.  Pour plus d’informations, consultez l’instruction **ALTER AVAILABILITY GROUP** dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
>   
>  Une ou plusieurs erreurs de validation de serveur de publication ont été rencontrées pour l'hôte de réplica 'MyReplicaHostName'.  
  
## <a name="permissions"></a>Autorisations  
 L’appelant doit être membre du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** pour la base de données de distribution, ou être membre d’une liste d’accès à une publication pour une publication définie associée à la base de données du serveur de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_get_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)  
  
  
