---
description: sp_cursorclose (Transact-SQL)
title: sp_cursorclose (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_cursor_close_TSQL
- sp_cursor_close
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorclose
ms.assetid: d9b7b44d-cdff-456e-97df-7031a3b9beb6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0363b69839d6cf58eebaa0394a591050c3443cd6
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536623"
---
# <a name="sp_cursorclose-transact-sql"></a>sp_cursorclose (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ferme et annule l’allocation du curseur et libère toutes les ressources associées. autrement dit, elle supprime la table temporaire utilisée pour la prise en charge des **curseurs**KEYSET ou static. sp_cursorclose est appelée en spécifiant ID = 9 dans un paquet tabular data stream (TDS).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursorclose cursor  
```  
  
## <a name="arguments"></a>Arguments  
 *cursor*  
 Valeur de *handle* de curseur générée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et retournée par la procédure sp_cursoropen. *Cursor* est un paramètre obligatoire qui requiert une valeur d’entrée **int** .  
  
> [!NOTE]  
>  La valeur d'entrée -1 s'appliquera vers tous les curseurs de la connexion actuelle.  
  
## <a name="remarks"></a>Notes  
 le *curseur* retourne des messages d’erreur si la procédure a été exécutée après la fermeture du curseur ou si un handle non valide est spécifié.  
  
 L'état RPC indique le succès ou l'échec global.  
  
 Le nombre de lignes DONE est toujours égal à 0.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
