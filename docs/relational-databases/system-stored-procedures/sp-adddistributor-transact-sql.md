---
description: sp_adddistributor (Transact-SQL)
title: sp_adddistributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistributor
- sp_adddistributor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adddistributor
ms.assetid: 35415502-68d0-40f6-993c-180e50004f1e
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 2e84824fec7a45fcfea24a2d97ced622b2147f34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182801"
---
# <a name="sp_adddistributor-transact-sql"></a>sp_adddistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée une entrée dans la table [sys.sys](../../relational-databases/system-compatibility-views/sys-sysservers-transact-sql.md) Servers (le cas échéant), marque l’entrée de serveur en tant que serveur de distribution et stocke les informations de propriété. Cette procédure stockée est exécutée sur la base de données master du serveur de distribution pour enregistrer et marquer le serveur en tant que serveur de distribution. Dans le cas d'un serveur de distribution distant, la procédure stockée est également exécutée sur le serveur de publication à partir de la base de données master.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adddistributor [ @distributor= ] 'distributor'   
    [ , [ @heartbeat_interval= ] heartbeat_interval ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @from_scripting= ] from_scripting ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @distributor = ] 'distributor'` Nom du serveur de distribution. *Distributor* est de **type sysname**, sans valeur par défaut. Ce paramètre est uniquement utilisé lors de la configuration d'un serveur de distribution distant. Elle ajoute des entrées pour les propriétés du serveur de distribution dans la base de données **msdb. Table MSdistributor** .  

> [!NOTE]
> Le nom du serveur peut être spécifié sous la forme `<Hostname>,<PortNumber>` . Vous devrez peut-être spécifier le numéro de port de votre connexion lorsque SQL Server est déployé sur Linux ou Windows avec un port personnalisé, et que le service Browser est désactivé.

`[ @heartbeat_interval = ] heartbeat_interval` Nombre maximal de minutes pendant lesquelles un agent peut se connecter sans enregistrer de message de progression. *heartbeat_interval* est de **type int**, avec une valeur par défaut de 10 minutes. Un travail de l'Agent SQL Server est créé et s'exécute pendant cet intervalle de temps pour contrôler l'état des agents de réplication en cours d'exécution.  
  
`[ @password = ] 'password']` Mot de passe de la connexion **distributor_admin** . *Password* est de **type sysname**, avec NULL comme valeur par défaut. Si le mot de passe est NULL ou est une chaîne vide, une valeur aléatoire est redéfinie pour lui. Le mot de passe doit être configuré lors de l'ajout du premier serveur de distribution distant. **distributor_admin** connexion et le *mot de passe* sont stockés pour l’entrée de serveur lié utilisée pour une connexion RPC du serveur de *distribution* , y compris les connexions locales. Si le serveur de *distribution* est local, le mot de passe de **distributor_admin** est défini sur une nouvelle valeur. Pour les serveurs de publication disposant d’un serveur de distribution distant, la même valeur de *mot de passe* doit être spécifiée lors de l’exécution de **sp_adddistributor** sur le serveur de publication et le serveur de distribution. [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) peut être utilisé pour modifier le mot de passe du serveur de distribution.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adddistributor** est utilisé dans la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistributor-transa_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_adddistributor**.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributor_property &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_dropdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)  
  
  
