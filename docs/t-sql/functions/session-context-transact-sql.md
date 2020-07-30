---
title: SESSION_CONTEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SESSION_CONTEXT
- sys.SESSION_CONTEXT
- SESSION_CONTEXT_TSQL
- sys.SESSION_CONTEXT_TSQL
helpviewer_keywords:
- SESSION_CONTEXT function
ms.assetid: b6bdbc54-331a-43cc-ab3d-3872d6a12100
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 00056403e2b2ef00862b7552c76fd98b851cabe5
ms.sourcegitcommit: df1f0f2dfb9452f16471e740273cd1478ff3100c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87394764"
---
# <a name="session_context-transact-sql"></a>SESSION_CONTEXT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Renvoie la valeur de la clé spécifiée dans le contexte de la session en cours. La valeur est définie à l’aide de la procédure [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SESSION_CONTEXT(N'key')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 'key'  
 Clé (type sysname) de la valeur en cours de récupération.  
  
## <a name="return-type"></a>Type de retour  
 **sql_variant**  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur associée à la clé spécifiée dans le contexte de session, ou NULL si aucune valeur n’a été définie pour cette clé.  
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur peut lire le contexte de session pour sa session.  
  
## <a name="remarks"></a>Notes  
 Le comportement MARS de SESSION_CONTEXT est similaire à celui de CONTEXT_INFO. Si un lot MARS définit une paire clé-valeur, la nouvelle valeur n’est pas renvoyée dans d’autres lots MARS sur la même connexion, sauf s’ils ont démarré après le lot qui a défini la nouvelle valeur. Si plusieurs lots MARS sont actifs sur une connexion, les valeurs ne peuvent pas être définies en « read_only ». Ceci empêche les conditions de concurrence et le non-déterminisme quant à la valeur qui « gagne ».  
  
## <a name="examples"></a>Exemples  
 L’exemple simple suivant attribue la valeur 4 au contexte de session pour la clé `user_id`, puis utilise la fonction **SESSION_CONTEXT** pour extraire la valeur.  
  
```  
EXEC sp_set_session_context 'user_id', 4;  
SELECT SESSION_CONTEXT(N'user_id');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)   
 [CURRENT_TRANSACTION_ID &#40;Transact-SQL&#41;](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO  &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
