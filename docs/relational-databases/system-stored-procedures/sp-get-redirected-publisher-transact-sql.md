---
description: sp_get_redirected_publisher (Transact-SQL)
title: sp_get_redirected_publisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_get_redirected_publisher_TSQL
- sp_get_redirected_publisher
ms.assetid: d47a9ab5-f2cc-42a8-8be9-a33895ce44f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fa1a1b8f7cffe3f98435ebd48a404fb76df34c8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204757"
---
# <a name="sp_get_redirected_publisher-transact-sql"></a>sp_get_redirected_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilisée par les agents de réplication pour interroger un serveur de distribution afin de déterminer si le serveur de publication d'origine a été redirigé.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_get_redirected_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @bypass_publisher_validation = ] [0 | 1 ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @original_publisher = ] 'original_publisher'` Nom de l’instance de SQL Server qui a initialement publié la base de données. *original_publisher* est de **type sysname**, sans valeur par défaut.
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données en cours de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @bypass_publisher_validation = ] [0 | 1 ]` Utilisé pour ignorer la validation du serveur de publication Redirigé. Si la valeur est 0, la validation est effectuée. Si la valeur est 1, aucune validation n'est effectuée. *bypass_publisher_validation* est de **bit**, avec 0 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**redirected_publisher**|**sysname**|Nom du serveur de publication après redirection.|  
|**error_number**|**int**|Numéro de l'erreur de validation.|  
|**error_severity**|**int**|Gravité de l'erreur de validation.|  
|**error_message**|**nvarchar(4000)**|Texte du message d'erreur de validation.|  
  
## <a name="remarks"></a>Notes  
 *redirected_publisher* retourne le nom de l’éditeur actuel. Retourne la valeur null si le serveur de publication et les bases de données de publication n’ont pas été redirigés à l’aide de **sp_redirect_publisher**.  
  
 Si la validation n’est pas demandée ou si aucune entrée n’existe pour le serveur de publication et la base de données de publication, *ERROR_NUMBER* et *ERROR_SEVERITY* retournent 0 et *ERROR_MESSAGE* retourne la valeur null.  
  
 Si la validation est demandée, la procédure stockée de validation [sp_validate_redirected_publisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md) est appelée pour vérifier que la cible de la redirection est un hôte approprié pour la base de données de publication. Si la validation est réussie, **sp_get_redirected_publisher** retourne le nom du serveur de publication Redirigé, 0 pour les colonnes *ERROR_NUMBER* et *ERROR_SEVERITY* , et la valeur null dans la colonne *ERROR_MESSAGE* .  
  
 Si la validation est requise et échoue, le nom du serveur de publication redirigé est retourné avec les informations d'erreur.  
  
## <a name="permissions"></a>Autorisations  
 L’appelant doit être membre du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** pour la base de données de distribution, ou être membre d’une liste d’accès à une publication pour une publication définie associée à la base de données du serveur de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
