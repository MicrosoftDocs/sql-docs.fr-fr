---
description: DBCC HELP (Transact-SQL)
title: DBCC HELP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC HELP
- DBCC_HELP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC statement syntax information
- DBCC HELP statement
ms.assetid: 306092c6-4354-4e47-928b-606124fbdc6e
author: pmasl
ms.author: umajay
ms.openlocfilehash: 3440683db3fb0b72987e26913a43ed4ba2a852ec
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88311165"
---
# <a name="dbcc-help-transact-sql"></a>DBCC HELP (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Retourne les informations de syntaxe correspondant à l'instruction DBCC spécifiée.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC HELP ( 'dbcc_statement' | @dbcc_statement_var | '?' )  
[ WITH NO_INFOMSGS ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *dbcc_statement* |  *\@dbcc_statement_var*  
 Nom de la commande DBCC pour laquelle les informations de syntaxe sont reçues. Fournit uniquement la partie de la commande DBCC qui suit DBCC, par exemple, CHECKDB plutôt que DBCC CHECKDB.  
  
 ?  
 Retourne toutes les commandes DBCC pour lesquelles l'aide est disponible.  
  
 WITH NO_INFOMSGS  
 Supprime tous les messages d'information dont les niveaux de gravité sont compris entre 0 et 10.  
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC HELP retourne un ensemble de résultats affichant la syntaxe de la commande DBCC spécifiée.
  
## <a name="permissions"></a>Autorisations  
Nécessite l'appartenance au rôle serveur fixe **sysadmin** .
  
## <a name="examples"></a>Exemples  
### <a name="a-using-dbcc-help-with-a-variable"></a>R. Utilisation de DBCC HELP avec une variable  
L'exemple suivant retourne les informations de syntaxe pour DBCC `CHECKDB`.
  
```sql  
DECLARE @dbcc_stmt sysname;  
SET @dbcc_stmt = 'CHECKDB';  
DBCC HELP (@dbcc_stmt);  
GO  
```  
  
### <a name="b-using-dbcc-help-with-the--option"></a>B. Utilisation de DBCC HELP avec l'option Option  
L'exemple suivant retourne toutes les instructions DBCC pour lesquelles l'Aide est disponible.
  
```sql  
DBCC HELP ('?');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
  
