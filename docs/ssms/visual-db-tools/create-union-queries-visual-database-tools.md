---
description: Créer des requêtes UNION (Visual Database Tools)
title: Créer des requêtes UNION
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- queries [SQL Server], types
- UNION queries
- Select query
- combining query results
- merged SELECT query [SQL Server]
ms.assetid: b5aafb1d-e4ed-4922-b790-56abc5ec551a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 12731ff7960a8376c8f0a79d933d62fde43e76dd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88314985"
---
# <a name="create-union-queries-visual-database-tools"></a>Créer des requêtes UNION (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Le mot clé UNION vous permet d'inclure les résultats de deux instructions SELECT dans une table résultante. Toutes les lignes retournées par l'une ou l'autre instruction SELECT sont mixées dans le résultat de l'expression UNION. Pour obtenir des exemples, consultez [Exemples (Transact-SQL)](https://msdn.microsoft.com/9b9caa3d-e7d0-42e1-b60b-a5572142186c).  
  
> [!NOTE]  
> Le volet Schéma ne peut afficher qu'une clause SELECT. Par conséquent, lorsque vous utilisez une requête UNION, le Concepteur de requêtes masque le volet Table Operations.  
  
### <a name="to-create-a-merged-select-query"></a>Pour créer une requête SELECT fusionnée  
  
1.  Ouvrez une requête ou créez-en une nouvelle.  
  
2.  Dans le volet SQL, tapez une expression UNION valide.  
  
    L'exemple suivant est une expression UNION valide.  
  
    ```  
    SELECT ProductModelID, Name  
    FROM Production.ProductModel  
    UNION  
    SELECT ProductModelID, Name   
    FROM dbo.Gloves;  
    ```  
  
3.  Dans le menu **Concepteur de requêtes** , cliquez sur **Exécuter SQL** pour exécuter la requête.  
  
    Votre requête UNION est à présent mise en forme par le Concepteur de requêtes.  
  
## <a name="see-also"></a>Voir aussi  
[Types de requêtes pris en charge](../../ssms/visual-db-tools/supported-query-types-visual-database-tools.md)  
[Rubriques de procédures relatives au Concepteur de vues et de requêtes](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Effectuer des opérations de base avec des requêtes](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
[UNION (Transact-SQL)](https://msdn.microsoft.com/607c296f-8a6a-49bc-975a-b8d0c0914df7)
