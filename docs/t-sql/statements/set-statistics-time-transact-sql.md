---
description: SET STATISTICS TIME (Transact-SQL)
title: SET STATISTICS TIME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_STATISTICS_TIME_TSQL
- SET STATISTICS TIME
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], statement processing
- time [SQL Server], statement processing statistics
- SET STATISTICS TIME statement
- STATISTICS TIME option
- statements [SQL Server], statistical information
- parsing [SQL Server], SET STATISTICS TIME statement
- compile times [SQL Server]
- execution processing time [SQL Server]
ms.assetid: eec2e1cd-a29d-4cf3-a271-be9d61506f15
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 36db16f45f3f73742c89b6269fedc5c4603e648f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096897"
---
# <a name="set-statistics-time-transact-sql"></a>SET STATISTICS TIME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Affiche le nombre de millisecondes requises pour analyser, compiler et exécuter chaque instruction.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
SET STATISTICS TIME { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes
 Si SET STATISTICS TIME est défini à ON, les statistiques se rapportant à la durée d'une instruction sont affichées. Si l'option est désactivée (OFF), elles ne sont pas affichées.  
  
 L'option SET STATISTICS TIME est appliquée lors de l'exécution, et non pas lors de l'analyse.  
  
 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas fournir de statistiques précises en mode fibre. Ce mode est activé à l’aide de l’option de configuration de **regroupement léger**.  
  
 La colonne **cpu** de la table **sysprocesses** est mise à jour uniquement si une requête s’exécute avec l’option SET STATISTICS TIME définie sur ON (activée). Quand l’option SET STATISTICS TIME est définie sur OFF (désactivée), la valeur **0** est retournée.  
  
 L'activation ou la désactivation de cette option a également une incidence sur la colonne CPU de la vue Informations sur le processus pour l'activité en cours dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Pour utiliser SET STATISTICS TIME, les utilisateurs doivent disposer des autorisations appropriées pour exécuter l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'autorisation SHOWPLAN n'est pas nécessaire.  
  
## <a name="examples"></a>Exemples  
 Cet exemple montre les durées d'exécution, d'analyse et de compilation du serveur.  
  
```sql
USE AdventureWorks2012;  
GO         
SET STATISTICS TIME ON;  
GO  
SELECT ProductID, StartDate, EndDate, StandardCost   
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET STATISTICS TIME OFF;  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
  
(269 row(s) affected)  
  
SQL Server Execution Times:  
   CPU time = 0 ms,  elapsed time = 2 ms.  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET STATISTICS IO &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-io-transact-sql.md)  
  
  
