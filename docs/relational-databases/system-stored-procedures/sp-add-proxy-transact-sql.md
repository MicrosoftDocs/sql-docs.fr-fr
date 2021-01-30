---
description: sp_add_proxy (Transact-SQL)
title: sp_add_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_proxy
- sp_add_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE PROXY statement
- sp_add_proxy
ms.assetid: cb59df37-f103-439b-bec1-2871fb669a8b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c60c3bb1ea9f96352662b72080afbfa1e76b1dce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192398"
---
# <a name="sp_add_proxy-transact-sql"></a>sp_add_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute le proxy [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent spécifié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_proxy  
    [ @proxy_name = ] 'proxy_name' ,  
    [ @enabled = ] is_enabled ,  
    [ @description = ] 'description' ,  
    [ @credential_name = ] 'credential_name' ,  
    [ @credential_id = ] credential_id ,  
    [ @proxy_id = ] id OUTPUT   
```  
  
## <a name="arguments"></a>Arguments  
`[ @proxy_name = ] 'proxy_name'` Nom du proxy à créer. *Proxy_name* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque le *proxy_name* a la valeur null ou est une chaîne vide, le nom du proxy est défini par défaut sur le *user_name* fourni.  
  
`[ @enabled = ] is_enabled` Spécifie si le proxy est activé. L’indicateur de *is_enabled* est de **type tinyint**, avec 1 comme valeur par défaut. Lorsque *is_enabled* a la **valeur 0**, le proxy n’est pas activé et ne peut pas être utilisé par une étape de travail.  
  
`[ @description = ] 'description'` Description du proxy. La description est de type **nvarchar (512)**, avec NULL comme valeur par défaut. La description vous permet de documenter le proxy, mais elle n'est pas utilisée à d'autres fins par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cet argument est donc facultatif.  
  
`[ @credential_name = ] 'credential_name'` Nom des informations d’identification du proxy. *Credential_name* est de **type sysname**, avec NULL comme valeur par défaut. *Credential_name* ou *credential_id* doivent être spécifiés.  
  
`[ @credential_id = ] credential_id` Numéro d’identification des informations d’identification pour le proxy. *Credential_id* est de **type int**, avec NULL comme valeur par défaut. *Credential_name* ou *credential_id* doivent être spécifiés.  
  
`[ @proxy_id = ] id OUTPUT` Numéro d’identification du proxy affecté au proxy si celui-ci a été créé avec succès.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Cette procédure stockée doit être exécutée dans la base de données **msdb** .  
  
 Un proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère la sécurité des étapes de travail qui impliquent des sous-systèmes autres que le sous-système [!INCLUDE[tsql](../../includes/tsql-md.md)]. Chaque proxy correspond à des informations d'identification de sécurité. Un proxy peut avoir accès à plusieurs sous-systèmes.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle de sécurité fixe **sysadmin** peuvent exécuter cette procédure.  
  
 Les membres du rôle de sécurité fixe **sysadmin** peuvent créer des étapes de travail qui utilisent n’importe quel proxy. Utilisez la procédure stockée [sp_grant_login_to_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md) pour accorder à d’autres connexions l’accès au proxy.  
  
## <a name="examples"></a>Exemples  
 Cet exemple crée un proxy pour les informations d'identification `CatalogApplicationCredential`. Le code part du principe que les informations d'identification existent déjà. Pour plus d’informations sur les informations d’identification, consultez [Create credential &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md).  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_proxy  
    @proxy_name = 'Catalog application proxy',  
    @enabled = 1,  
    @description = 'Maintenance tasks on catalog application.',  
    @credential_name = 'CatalogApplicationCredential' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
