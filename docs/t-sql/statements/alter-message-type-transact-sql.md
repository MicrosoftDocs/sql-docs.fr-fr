---
description: ALTER MESSAGE TYPE (Transact-SQL)
title: ALTER MESSAGE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_MESSAGE_TYPE_TSQL
- ALTER MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER MESSAGE TYPE statement
- modifying message types
- message types [Service Broker], modifying
ms.assetid: 98c94176-2bdf-4725-b4bc-d33b6b14817d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c8b1cbf21f5875ee32174a37f29dfe4a32fbf195
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092130"
---
# <a name="alter-message-type-transact-sql"></a>ALTER MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie les propriétés d'un type de message.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql 
ALTER MESSAGE TYPE message_type_name  
   VALIDATION =  
    {  NONE   
     | EMPTY   
     | WELL_FORMED_XML   
     | VALID_XML WITH SCHEMA COLLECTION schema_collection_name }  
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *message_type_name*  
 Nom du type de message à modifier. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés.  
  
 VALIDATION  
 Spécifie comment [!INCLUDE[ssSB](../../includes/sssb-md.md)] valide le corps des messages de ce type.  
  
 Aucune  
 Aucune validation n'est effectuée. Le corps du message peut contenir tout type de données ou avoir la valeur NULL.  
  
 EMPTY  
 Le corps du message doit être NULL.  
  
 WELL_FORMED_XML  
 Le corps du message doit contenir un document XML bien formé.  
  
 VALID_XML_WITH_SCHEMA = *schema_collection_name*  
 Le corps du message doit contenir du code XML conforme à un schéma de la collection de schémas spécifiée. *schema_collection_name* doit être le nom d’une collection de schémas XML existante.  
  
## <a name="remarks"></a>Notes  
 Modifier la validation d'un type de message n'a aucun impact sur les messages qui ont déjà été remis à une file d'attente.  
  
 L'instruction ALTER AUTHORIZATION permet de modifier l'AUTORISATION pour un type de message.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, l’autorisation de modification d’un type de message est octroyée au propriétaire du type de message, aux membres des rôles de base de données fixes **db_ddladmin** ou **db_owner**, ainsi qu’aux membres du rôle serveur fixe **sysadmin**.  
  
 Si l'instruction ALTER MESSAGE TYPE indique une collections de schémas, l'utilisateur qui exécute cette instruction doit disposer de l'autorisation REFERENCES sur la collection de schémas spécifiée.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant indique comment changer le type de message `//Adventure-Works.com/Expenses/SubmitExpense` pour demander que le corps du message contienne un document XML correctement formé.  
  
```sql  
ALTER MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = WELL_FORMED_XML ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
