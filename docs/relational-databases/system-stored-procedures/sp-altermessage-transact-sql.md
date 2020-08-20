---
description: sp_altermessage (Transact-SQL)
title: sp_altermessage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_altermessage_TSQL
- sp_altermessage
dev_langs:
- TSQL
helpviewer_keywords:
- sp_altermessage
ms.assetid: 1b28f280-8ef9-48e9-bd99-ec14d79abaca
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: a4af3e9a072e7ff2dca9ece41dd2e0e40a03ef76
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88464575"
---
# <a name="sp_altermessage-transact-sql"></a>sp_altermessage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie l'état des messages système ou définis par l'utilisateur dans une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Les messages définis par l’utilisateur peuvent être affichés à l’aide de l’affichage catalogue **sys. messages** .  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_altermessage [ @message_id = ] message_number   ,[ @parameter = ]'write_to_log'  
   ,[ @parameter_value = ]'value'   
```  
  
## <a name="arguments"></a>Arguments  
 [** @message_id =** ] *message_number*  
 Numéro d’erreur du message à modifier à partir de **sys. messages**. *message_number* est de **type int** sans valeur par défaut.  
  
`[ @parameter = ] 'write\_to\_log_'`Est utilisé avec ** \@ parameter_value** pour indiquer que le message doit être écrit dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Journal des applications Windows. *write_to_log* est de **type sysname** et n’a pas de valeur par défaut. *write_to_log* doit avoir la valeur WITH_LOG ou null. Si *write_to_log* a la valeur WITH_LOG ou null et que la valeur de ** \@ parameter_value** est **true**, le message est écrit dans le journal des applications Windows. Si *write_to_log* a la valeur WITH_LOG ou null et que la valeur de ** \@ parameter_value** est **false**, le message n’est pas toujours écrit dans le journal des applications Windows, mais peut être écrit en fonction de la façon dont l’erreur a été générée. Si *write_to_log* est spécifié, la valeur de ** \@ parameter_value** doit également être spécifiée.  
  
> [!NOTE]  
>  Si un message est écrit dans le journal des applications Windows, il est également écrit dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fichier journal des erreurs.  
  
`[ @parameter_value = ]'value_'`Est utilisé avec le ** \@ paramètre** pour indiquer que l’erreur doit être écrite dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Journal des applications Windows. la *valeur* est de type **varchar (5)**, sans valeur par défaut. Si la **valeur est true**, l’erreur est toujours écrite dans le journal des applications Windows. Si la **valeur est false**, l’erreur n’est pas toujours écrite dans le journal des applications Windows, mais elle peut être écrite en fonction de la façon dont l’erreur a été générée. Si la *valeur* est spécifiée, *write_to_log* doit également être spécifié pour le ** \@ paramètre** .  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 L’effet de **sp_altermessage** avec l’option WITH_LOG est similaire à celui du paramètre RAISERROR with log, à ceci près que **sp_altermessage** modifie le comportement de journalisation d’un message existant. Si un message a été modifié avec l'option WITH_LOG, il est toujours écrit dans le journal des applications Windows, quelle que soit la manière dont un utilisateur appelle l'erreur. Même si RAISERROR est exécuté sans l'option WITH_LOG, l'erreur est écrite dans le journal des applications Windows.  
  
 Les messages système peuvent être modifiés à l’aide de **sp_altermessage**.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **ServerAdmin** .  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, le message `55001` est enregistré dans le journal des applications Windows.  
  
```  
EXECUTE sp_altermessage 55001, 'WITH_LOG', 'true';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
