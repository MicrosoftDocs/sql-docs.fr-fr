---
description: sp_validate_redirected_publisher (Transact-SQL)
title: sp_validate_redirected_publisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_validate_redirected_publisher
- sp_validate_redirected_publisher_TSQL
helpviewer_keywords:
- sp_validate_redirected_publisher
ms.assetid: 2b7fdbad-17e4-4442-b0b2-9b5e8f84b91d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dd0e7a98d9f9a5886d829c5567d91795ca388b6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211313"
---
# <a name="sp_validate_redirected_publisher-transact-sql"></a>sp_validate_redirected_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Vérifie que l'hôte actuel de la base de données de publication peut prendre en charge la réplication. Doit être exécutée à partir d'une base de données de distribution. Cette procédure est appelée par **sp_get_redirected_publisher**.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
      sp_validate_redirected_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @redirected_publisher = ] 'new_publisher' output  
```  
  
## <a name="arguments"></a>Arguments  
`[ @original_publisher = ] 'original_publisher'` Nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a initialement publié la base de données. *original_publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données en cours de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @redirected_publisher = ] 'redirected_publisher'` Cible de redirection spécifiée lors de l’appel de **sp_redirect_publisher** pour la paire serveur de publication/base de données. *redirected_publisher* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 Si aucune entrée n’existe pour le serveur de publication et la base de données de publication, **sp_validate_redirected_publisher** retourne la valeur null dans le paramètre de sortie *\@ redirected_publisher*. Si une entrée existe, elle est retournée dans le paramètre de sortie dans les deux cas (réussite et échec).  
  
 Si la validation réussit, **sp_validate_redirected_publisher** retourne une indication de réussite.  
  
 Si la validation échoue, des erreurs sont générées, qui décrivent l'échec.  
  
## <a name="permissions"></a>Autorisations  
 L’appelant doit être membre du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** pour la base de données de distribution, ou être membre d’une liste d’accès à une publication pour une publication définie associée à la base de données du serveur de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_get_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
