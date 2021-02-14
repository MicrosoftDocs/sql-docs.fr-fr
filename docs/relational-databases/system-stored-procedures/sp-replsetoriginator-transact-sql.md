---
description: sp_replsetoriginator (Transact-SQL)
title: sp_replsetoriginator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replsetoriginator
- sp_replsetoriginator_TSQL
helpviewer_keywords:
- sp_replsetoriginator
ms.assetid: 030e5226-0585-439f-b8cd-36f48367d86d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c26248551ad949bcf27433d28898ba4ec98dc68b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2021
ms.locfileid: "99211638"
---
# <a name="sp_replsetoriginator-transact-sql"></a>sp_replsetoriginator (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Utilisée pour appeler un traitement et une détection en boucle au cours des réplications transactionnelles bidirectionnelles. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replsetoriginator [ @server_name= ] 'server_name'   
    , [ @database_name= ] 'database_name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @server_name = ] 'server_name'` Nom du serveur sur lequel la transaction est appliquée. *originating_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @database_name = ] 'database_name'` Nom de la base de données dans laquelle la transaction est appliquée. *originating_db* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replsetoriginator** est exécutée par le agent de distribution pour enregistrer la source de transactions appliquée par la réplication. Cette information est utilisée pour appeler une détection en boucle des abonnements transactionnels bidirectionnels qui possèdent le jeu de propriétés de la boucle.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** sur le serveur de publication, les membres du rôle de base de données fixe **db_owner** sur la base de données de publication ou les utilisateurs de la liste d’accès aux publications (PAL) peuvent exécuter **sp_replsetoriginator**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
