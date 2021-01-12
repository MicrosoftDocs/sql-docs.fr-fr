---
description: END CONVERSATION (Transact-SQL)
title: END CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- END DIALOG
- END CONVERSATION
- END_DIALOG_TSQL
- END_CONVERSATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [Service Broker], conversations
- dialogs [Service Broker], ending
- closing conversations
- END CONVERSATION statement
- conversations [Service Broker], ending
- ending conversations [SQL Server]
ms.assetid: 4415a126-cd22-4a5e-b84a-d8c68515c83b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 16dd2a5201d001a54103408386036cee39040523
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093433"
---
# <a name="end-conversation-transact-sql"></a>END CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Met fin à un côté d'une conversation existante.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
END CONVERSATION conversation_handle  
   [   [ WITH ERROR = failure_code DESCRIPTION = 'failure_text' ]  
     | [ WITH CLEANUP ]  
    ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *conversation_handle*  
 Handle de conversation pour la conversation à terminer.  
  
 WITH ERROR =*failure_code*  
 Est le code d’erreur. *failure_code* est de type **int**. Le code d’échec est un code défini par l’utilisateur qui est inclus dans le message d’erreur envoyé à l’autre côté de la conversation. Le code d'échec doit être supérieur à 0.  
  
 DESCRIPTION =*failure_text*  
 Est le message d’erreur. *failure_text* est de type **nvarchar(3000)**. Le texte d'échec est un texte défini par l'utilisateur qui est inclus dans le message d'erreur envoyé à l'autre côté de la conversation.  
  
 WITH CLEANUP  
 Supprime tous les messages et les entrées d'affichage catalogue pour un côté d'une conversation qui ne peut pas se terminer normalement. L'autre côté de la conversation n'est pas notifié du nettoyage. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supprime le point de terminaison, tous les messages de la conversation dans la file d’attente de transmission, ainsi que tous les messages pour la conversation dans la file d’attente du service. Les administrateurs peuvent utiliser cette option pour supprimer des conversations qui ne peuvent pas se terminer normalement. Si, par exemple, le service distant a été supprimé définitivement, un administrateur peut utiliser WITH CLEANUP pour supprimer les conversations avec ce service. N’utilisez pas WITH CLEANUP dans le code d’une application [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Si END CONVERSATION WITH CLEANUP est exécuté avant que le point de terminaison de réception n'accuse réception d'un message, le point de terminaison d'envoi renverra le message. Cela peut éventuellement entraîner la réexécution du dialogue.  
  
## <a name="remarks"></a>Remarques  
 Mettre fin à une conversation verrouille le groupe de conversations auquel appartient le *conversation_handle* fourni. Lorsqu'une conversation se termine, [!INCLUDE[ssSB](../../includes/sssb-md.md)] supprime tous les messages qui la concernent de la file d'attente du service.  
  
 Une fois qu'une conversation est terminée, aucune application ne peut plus envoyer ni recevoir des messages pour cette conversation. Les deux participants d'une conversation doivent appeler END CONVERSATION pour achever la conversation. Si [!INCLUDE[ssSB](../../includes/sssb-md.md)] n'a pas reçu de message de fin de dialogue ou de message d'erreur de la part de l'autre participant de la conversation, [!INCLUDE[ssSB](../../includes/sssb-md.md)] avertit l'autre participant que la conversation est terminée. Dans ce cas, bien que le handle de conversation ne soit plus valide, le point de terminaison de la conversation reste actif jusqu'à ce que l'instance qui héberge le service distant accuse réception du message.  
  
 Si [!INCLUDE[ssSB](../../includes/sssb-md.md)] n'a pas encore traité le message de fin de dialogue ou d'erreur de la conversation, [!INCLUDE[ssSB](../../includes/sssb-md.md)] avertit le côté distant que la conversation est terminée. Les messages que [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoie au service distant dépendent des options spécifiées :  
  
-   Si la conversation se termine sans erreurs et qu’elle est toujours active avec le service distant, [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoie un message de type `https://schemas.microsoft.com/SQL/ServiceBroker/EndDialog` au service distant. [!INCLUDE[ssSB](../../includes/sssb-md.md)] ajoute ce message à la file d'attente de transmission en respectant l'ordre de la conversation. [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoie tous les messages de cette conversation qui se trouvent actuellement dans la file d'attente de transmission avant d'envoyer ce message.  
  
-   Si la conversation se termine avec une erreur et qu’elle est toujours active avec le service distant, [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoie un message de type `https://schemas.microsoft.com/SQL/ServiceBroker/Error` au service distant. [!INCLUDE[ssSB](../../includes/sssb-md.md)] supprime, dans ce cas, tous les messages de cette conversation qui sont encore dans la file d'attente de transmission.  
  
-   La clause WITH CLEANUP permet à un administrateur de base de données de supprimer les conversations qui ne peuvent pas se terminer normalement. Cette option supprime tous les messages et les entrées d'affichage catalogue concernant la conversation. Notez, dans ce cas, que le côté distant de la conversation n'est pas averti de la fin de la conversation et risque de ne pas recevoir les messages qui ont été envoyés par une application mais qui n'ont pas encore été transmis sur le réseau. Évitez d'utiliser cette option, sauf si la conversation ne peut pas se terminer normalement.  
  
 Au terme d'une conversation, une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] SEND qui spécifie le descripteur de conversation provoque une erreur [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si des messages liés à cette conversation sont envoyés par l'autre côté de la conversation, [!INCLUDE[ssSB](../../includes/sssb-md.md)] les refuse.  
  
 Si une conversation se termine alors que le service distant possède toujours des messages non envoyés pour cette conversation, le service distant supprime ces messages. Cela n'est pas considéré comme une erreur et le service distant n'est pas averti de la suppression des messages.  
  
 Les codes d'échec spécifiés dans la clause WITH ERROR doivent être des nombres positifs. Les nombres négatifs sont réservés aux messages d'erreur de [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 END CONVERSATION n'est pas valide dans une fonction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Pour mettre fin à une conversation active, l'utilisateur actif doit en être le propriétaire, un membre du rôle serveur fixe sysadmin ou un membre du rôle de base de données fixe db_owner.  
  
 Un membre du rôle serveur fixe sysadmin ou du rôle de base de données fixe db_owner peut utiliser la clause WITH CLEANUP pour supprimer les métadonnées d'une conversation qui est déjà terminée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-ending-a-conversation"></a>R. Fin d'une conversation  
 Cet exemple met fin au dialogue spécifié par `@dialog_handle`.  
  
```sql 
END CONVERSATION @dialog_handle ;  
```  
  
### <a name="b-ending-a-conversation-with-an-error"></a>B. Fin d'une conversation avec une erreur  
 L'exemple suivant met fin au dialogue spécifié par `@dialog_handle` avec une erreur si l'instruction de traitement signale une erreur. Notez qu'il s'agit d'une approche très simplifiée de la gestion des erreurs qui ne convient pas à certaines applications.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER,  
        @ErrorSave INT,  
        @ErrorDesc NVARCHAR(100) ;  
BEGIN TRANSACTION ;  
  
<receive and process message>  
  
SET @ErrorSave = @@ERROR ;  
  
IF (@ErrorSave <> 0)  
  BEGIN  
      ROLLBACK TRANSACTION ;  
      SET @ErrorDesc = N'An error has occurred.' ;  
      END CONVERSATION @dialog_handle   
      WITH ERROR = @ErrorSave DESCRIPTION = @ErrorDesc ;  
  END  
ELSE  
  
COMMIT TRANSACTION ;  
```  
  
### <a name="c-cleaning-up-a-conversation-that-cannot-complete-normally"></a>C. Nettoyage d'une conversation qui ne peut pas se terminer normalement  
 Cet exemple met fin au dialogue spécifié par `@dialog_handle`. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supprime immédiatement tous les messages de la file d'attente du service et de la file d'attente de transmission, sans en notifier le service distant. Dans la mesure où le service distant n’est pas averti, utilisez cette solution dans le seul cas où le service distant n’est pas en mesure de recevoir de message **EndDialog** ou **Error**.  
  
```sql  
END CONVERSATION @dialog_handle WITH CLEANUP ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
