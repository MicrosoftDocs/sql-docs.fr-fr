---
description: DROP SYNONYM (Transact-SQL)
title: DROP SYNONYM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SYNONYM
- DROP_SYNONYM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting synonyms
- synonyms [SQL Server], removing
- removing synonyms
- DROP SYNONYM statement
- dropping synonyms
ms.assetid: 23578932-e4de-4c39-a5a0-ce45139c4269
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cb3e1a7c66aba0234145ba840f36643c6ca49d9c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099404"
---
# <a name="drop-synonym-transact-sql"></a>DROP SYNONYM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Supprime un synonyme d'un schéma spécifié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP SYNONYM [ IF EXISTS ] [ schema. ] synonym_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *IF EXISTS*  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à la [version actuelle](https://go.microsoft.com/fwlink/p/?LinkId=299658)).
  
 Supprime, de manière conditionnelle, le synonyme uniquement s’il existe déjà.  
  
 *schema*  
 Spécifie le schéma dans lequel le synonyme existe. Si le schéma n'est pas spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise le schéma par défaut de l'utilisateur actuel.  
  
 *synonym_name*  
 Nom du synonyme à supprimer.  
  
## <a name="remarks"></a>Notes  
 Les références aux synonymes ne sont pas liées aux schémas ; vous pouvez supprimer un synonyme à n'importe quel moment. Les références aux synonymes supprimés ne se rencontrent que lors de l'exécution.  
  
 Vous pouvez créer, supprimer et référencer des synonymes dans des instructions SQL dynamiques.  
  
## <a name="permissions"></a>Autorisations  
 Pour supprimer un synonyme, un utilisateur doit satisfaire au moins l'une des conditions suivantes. L'utilisateur doit être :  
  
-   Le propriétaire actuel d'un synonyme.  
  
-   Un bénéficiaire possédant une autorisation CONTROL sur un synonyme.  
  
-   Un bénéficiaire possédant une autorisation ALTER SCHEMA sur le schéma conteneur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée d'abord un synonyme, `MyProduct`, puis le supprime.  
  
```sql  
USE tempdb;  
GO  
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
-- Drop synonym MyProduct.  
USE tempdb;  
GO  
DROP SYNONYM MyProduct;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/create-synonym-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
