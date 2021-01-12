---
description: MOVE CONVERSATION (Transact-SQL)
title: MOVE CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- MOVE_CONVERSATION_TSQL
- MOVE CONVERSATION
- MOVE_TSQL
- MOVE
dev_langs:
- TSQL
helpviewer_keywords:
- moving conversations
- MOVE CONVERSATION statement
- relocating conversations
- conversations [Service Broker], groups
- conversations [Service Broker], moving
ms.assetid: 1da4d2c9-e767-434e-b49b-615711a7f626
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a789534b4ff40a7d9f6f360c941bd920ce23c44e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095664"
---
# <a name="move-conversation-transact-sql"></a>MOVE CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Déplace une conversation vers un groupe de conversations différent.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
MOVE CONVERSATION conversation_handle  
   TO conversation_group_id  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *conversation_handle*  
 Variable ou constante contenant le descripteur de conversation à déplacer. *conversation_handle* doit être de type **uniqueidentifier**.  
  
 TO *conversation_group_id*  
 Variable ou constante contenant l'identificateur du groupe de conversations destinataire. *conversation_group_id* doit être de type **uniqueidentifier**.  
  
## <a name="remarks"></a>Remarques  
 L’instruction MOVE CONVERSATION déplace la conversation spécifiée par *conversation_handle* vers le groupe de conversations identifié par *conversation_group_id*. Les dialogues ne peuvent être redirigés qu'entre des groupes de conversation associés à la même file d'attente.  
  
> [!IMPORTANT]  
>  Si l’instruction MOVE CONVERSATION n’est pas la première d’un lot ou d’une procédure stockée, l’instruction qui précède doit se terminer par un point-virgule (**;**), le terminateur d’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 L’instruction MOVE CONVERSATION verrouille le groupe de conversations associé à *conversation_handle* et celui spécifié par *conversation_group_id* jusqu’à la validation ou la restauration de la transaction contenant cette instruction.  
  
 MOVE CONVERSATION n'est pas valide dans une fonction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Pour déplacer une conversation, l'utilisateur actif doit être soit propriétaire de la conversation et du groupe de conversations, soit un membre du rôle serveur fixe sysadmin, soit un membre du rôle de base de données fixe db_owner.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant déplace une conversation vers un groupe de conversations différent.  
  
```sql  
DECLARE @conversation_handle UNIQUEIDENTIFIER,  
        @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_handle =  
    <retrieve conversation handle from database> ;  
SET @conversation_group_id =  
    <retrieve conversation group ID from database> ;  
  
MOVE CONVERSATION @conversation_handle TO @conversation_group_id ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [sys.conversation_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
