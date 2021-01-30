---
description: sp_helplogins (Transact-SQL)
title: sp_helplogins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helplogins_TSQL
- sp_helplogins
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helplogins
ms.assetid: f9ad3767-5b9f-420d-8922-b637811404f7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b58e74a30ddd86fc2edbfc8db553ddf9449181db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183125"
---
# <a name="sp_helplogins-transact-sql"></a>sp_helplogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur les connexions et les utilisateurs associés dans chaque base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helplogins [ [ @LoginNamePattern = ] 'login' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @LoginNamePattern = ] 'login'` Nom de connexion. *login* est de type **sysname**, avec NULL comme valeur par défaut. la *connexion* doit exister si elle est spécifiée. Si la *connexion* n’est pas spécifiée, des informations sur toutes les connexions sont retournées.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le premier rapport contient des informations sur chaque connexion spécifiée (voir le tableau ci-dessous).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Nom de connexion.|  
|**SID**|**varbinary(85)**|ID de sécurité de la connexion (SID).|  
|**DefDBName**|**sysname**|Base de données par défaut utilisée par **LoginName** lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**DefLangName**|**sysname**|Langue par défaut utilisée par **LoginName**.|  
|**Auser**|**Char (5)**|Oui = **LoginName** a un nom d’utilisateur associé dans une base de données.<br /><br /> Non = **LoginName** n’a pas de nom d’utilisateur associé.|  
|**ARemote**|**Char (7)**|Oui = **LoginName** a une connexion distante associée.<br /><br /> Non = **LoginName** n’a pas de connexion associée.|  
  
 Le deuxième rapport contient des informations à propos des utilisateurs mappés à chaque connexion et des appartenances aux rôles de la connexion, comme illustré dans le tableau suivant.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Nom de connexion.|  
|**@**|**sysname**|Base de données par défaut utilisée par **LoginName** lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**UserName**|**sysname**|Compte d’utilisateur auquel **LoginName** est mappé dans **dbname** et les rôles dont **LoginName** est membre dans **dbname**.|  
|**UserOrAlias**|**Char (8)**|MemberOf = **username** est un rôle.<br /><br /> User = **username** est un compte d’utilisateur.|  
  
## <a name="remarks"></a>Notes  
 Avant de supprimer une connexion, utilisez **sp_helplogins** pour identifier les comptes d’utilisateur mappés à la connexion.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **securityadmin** .  
  
 Pour identifier tous les comptes d’utilisateur mappés à un compte de connexion donné, **sp_helplogins** devez vérifier toutes les bases de données au sein du serveur. Par conséquent, chaque base de données du serveur doit remplir une des conditions suivantes :  
  
-   L’utilisateur qui exécute **sp_helplogins** a l’autorisation d’accéder à la base de données.  
  
-   Le compte d’utilisateur **invité** est activé dans la base de données.  
  
 Si **sp_helplogins** ne parvient pas à accéder à une base de données, **sp_helplogins** retournera autant d’informations que possible et affichera le message d’erreur 15622.  
  
## <a name="examples"></a>Exemples  
 Le code exemple suivant fournit des informations sur la connexion `John`.  
  
```  
EXEC sp_helplogins 'John';  
GO  
  
LoginName SID                        DefDBName DefLangName AUser ARemote   
--------- -------------------------- --------- ----------- ----- -------   
John      0x23B348613497D11190C100C  master    us_english  yes   no  
  
(1 row(s) affected)  
  
LoginName   DBName   UserName   UserOrAlias   
---------   ------   --------   -----------   
John        pubs     John       User          
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_helpdb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
