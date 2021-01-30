---
description: sp_dropmessage (Transact-SQL)
title: sp_dropmessage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropmessage_TSQL
- sp_dropmessage
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropmessage
ms.assetid: 17287a15-cdde-43d1-bb18-9f920bc15db8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 41971224ec830b1fbc17fdc8f754d7ae99e8731e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197637"
---
# <a name="sp_dropmessage-transact-sql"></a>sp_dropmessage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un message d’erreur défini par l’utilisateur spécifié d’une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Les messages définis par l’utilisateur peuvent être affichés à l’aide de l’affichage catalogue **sys. messages** .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropmessage [ @msgnum = ] message_number  
    [ , [ @lang = ] 'language' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @msgnum = ] message_number` Numéro du message à supprimer. *message_number* doit être un message défini par l’utilisateur dont le numéro de message est supérieur à 50000. *message_number* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @lang = ] 'language'` Langue du message à supprimer. Si **All** est spécifié, toutes les versions linguistiques de *message_number* sont supprimées. *Language* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance aux rôles serveur fixes **sysadmin** et **ServerAdmin** .  
  
## <a name="remarks"></a>Notes  
 Sauf si **tout** est spécifié pour la *langue*, toutes les versions localisées d’un message doivent être supprimées avant de pouvoir supprimer la version en anglais des États-Unis du message.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dropping-a-user-defined-message"></a>R. Suppression d'un message défini par l'utilisateur  
 L’exemple suivant supprime un message défini par l’utilisateur, Number `50001` , de **sys. messages**.  
  
```  
USE master;  
GO  
EXEC sp_dropmessage 50001;  
```  
  
### <a name="b-dropping-a-user-defined-message-that-includes-a-localized-version"></a>B. Suppression d'un message défini par l'utilisateur qui comprend une version localisée  
 L'exemple suivant supprime un message défini par l'utilisateur (numéro `60000`) qui comprend une version localisée du message.  
  
```  
USE master;  
GO  
  
-- Create a user-defined message in U.S. English  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'The item named %s already exists in %s.',   
    @lang = 'us_english';  
  
-- Create a localized version of the same message.  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'L''élément nommé %1! existe déjà dans %2!',  
    @lang = 'French';  
GO  
  
-- This statement will fail as long as the localized version  
-- of the message exists.  
EXEC sp_dropmessage 60000;  
GO  
  
-- This statement will drop the message.  
EXEC sp_dropmessage  
    @msgnum = 60000,  
    @lang = 'all';  
GO  
```  
  
### <a name="c-dropping-a-localized-version-of-a-user-defined-message"></a>C. Suppression d'une version localisée d'un message défini par l'utilisateur  
 L'exemple suivant supprime une version localisée d'un message défini par l'utilisateur (numéro `60000`) sans supprimer le message tout entier.  
  
```  
USE master;  
GO  
  
-- Create a user-defined message in U.S. English  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'The item named %s already exists in %s.',   
    @lang = 'us_english';  
  
-- Create a localized version of the same message.  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'L''élément nommé %1! existe déjà dans %2!',  
    @lang = 'French';  
GO  
-- This statement will remove only the localized version of the   
-- message.  
EXEC sp_dropmessage  
    @msgnum = 60000,  
    @lang = 'French';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_altermessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
