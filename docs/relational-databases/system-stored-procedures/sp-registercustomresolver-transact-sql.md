---
description: sp_registercustomresolver (Transact-SQL)
title: sp_registercustomresolver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_registercustomresolver
- sp_registercustomresolver_TSQL
helpviewer_keywords:
- sp_registercustomresolver
ms.assetid: 6d2b0472-0e1f-4005-833c-735d1940fe93
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4a4c08e8643aedaecd415177ef87f9fadc69563f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185695"
---
# <a name="sp_registercustomresolver-transact-sql"></a>sp_registercustomresolver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet d'inscrire un gestionnaire de logique métier ou un résolveur personnalisé COM qui peut être appelé lors du processus de synchronisation de réplication de fusion. Cette procédure stockée est exécutée sur le serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_registercustomresolver [ @article_resolver = ] 'article_resolver'   
    [ , [ @resolver_clsid = ] 'resolver_clsid' ]  
    [ , [ @is_dotnet_assembly = ] 'is_dotnet_assembly' ]  
    [ , [ @dotnet_assembly_name = ] 'dotnet_assembly_name' ]  
    [ , [ @dotnet_class_name = ] 'dotnet_class_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @article_resolver = ] 'article_resolver'` Spécifie le nom convivial de la logique métier personnalisée en cours d’enregistrement. *article_resolver* est de type **nvarchar (255)**, sans valeur par défaut.  
  
`[ @resolver_clsid = ] 'resolver_clsid'` Spécifie la valeur CLSID de l’objet COM qui est inscrit. La logique métier personnalisée *resolver_clsid* est de type **nvarchar (50)**, avec NULL comme valeur par défaut. Ce paramètre doit avoir pour valeur un CLSID valide ou la valeur NULL lors de l'inscription d'un assembly de gestionnaire de logique métier.  
  
`[ @is_dotnet_assembly = ] 'is_dotnet_assembly'` Spécifie le type de logique métier personnalisée en cours d’inscription. *is_dotnet_assembly* est de type **nvarchar (50)**, avec false comme valeur par défaut. **true** indique que la logique métier personnalisée en cours d’inscription est un assembly de gestionnaire de logique métier ; **false** indique qu’il s’agit d’un composant com.  
  
`[ @dotnet_assembly_name = ] 'dotnet_assembly_name'` Nom de l’assembly qui implémente le gestionnaire de logique métier. *dotnet_assembly_name* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Vous devez spécifier le chemin d'accès complet à l'assembly si celui-ci n'est pas déployé dans le même répertoire que l'exécutable de l'Agent de fusion, que l'application qui démarre de façon synchronisée l'Agent de fusion, ou dans le GAC (Global Assembly Cache).  
  
`[ @dotnet_class_name = ] 'dotnet_class_name'` Est le nom de la classe qui remplace <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> pour implémenter le gestionnaire de logique métier. Le nom doit être spécifié sous la forme **namespace. ClassName**. *dotnet_class_name* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_registercustomresolver** est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_registercustomresolver**.  
  
## <a name="see-also"></a>Voir aussi  
 [Implémenter un gestionnaire de logique métier pour un article de fusion](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [Implémenter un outil personnalisé de résolution des conflits pour un article de fusion](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [sp_lookupcustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
