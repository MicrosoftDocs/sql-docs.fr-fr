---
description: sp_defaultlanguage (Transact-SQL)
title: sp_defaultlanguage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_defaultlanguage
- sp_defaultlanguage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_defaultlanguage
ms.assetid: 908d01cc-e704-45d9-9e85-d2df6da3e6f5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9c382a442e6b87220f2650e65adc03089f2014b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195561"
---
# <a name="sp_defaultlanguage-transact-sql"></a>sp_defaultlanguage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie la langue par défaut d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_defaultlanguage [ @loginame = ] 'login'   
     [ , [ @language = ] 'language' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @loginame = ] 'login'` Nom de la connexion. *login* est de **type sysname**, sans valeur par défaut. la *connexion* peut être une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion existante ou un utilisateur ou un groupe Windows.  
  
`[ @language = ] 'language'` Est la langue par défaut de la connexion. *Language* est de **type sysname**, avec NULL comme valeur par défaut. la *langue* doit être une langue valide sur le serveur. Si la *langue* n’est pas spécifiée, la *langue* est définie sur la langue par défaut du serveur. la langue par défaut est définie  par la sp_configure **langue par défaut** de la variable de configuration. Le changement de la langue par défaut du serveur n'affecte pas la langue par défaut des connexions existantes.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_defaultlanguage** appelle ALTER LOGIN, qui prend en charge des options supplémentaires. Pour plus d’informations sur la modification d’autres paramètres de connexion par défaut, consultez [ALTER login &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md).  
  
 Utilisez l'instruction SET LANGUAGE pour modifier la langue de la session active. Utilisez la @LANGUAGE fonction @ pour afficher le paramètre de langue actuel.  
  
 Si la langue par défaut d'une connexion est supprimée du serveur, cette connexion utilise la langue par défaut du serveur. **sp_defaultlanguage** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
 Les informations sur les langues installées sur le serveur sont visibles dans l’affichage catalogue **sys.syslangues** .  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY LOGIN.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise `ALTER LOGIN` pour changer la langue par défaut de la connexion `Fathima` et choisir l'arabe. Ceci est la méthode privilégiée.  
  
```  
ALTER LOGIN Fathima WITH DEFAULT_LANGUAGE = Arabic;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [@@LANGUAGE &#40;Transact-SQL&#41;](../../t-sql/functions/language-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [ Langages desys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
