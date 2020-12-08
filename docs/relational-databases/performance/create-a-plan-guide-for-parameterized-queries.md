---
title: Créer un repère de plan pour les requêtes paramétrables | Microsoft Docs
description: Découvrez comment créer un repère de plan correspondant à la requête qui paramètre selon une forme donnée et commande à SQL Server d’imposer le paramétrage de la requête.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- parameterized queries, plan guides for
- plan guides [SQL Server], parameterized queries
ms.assetid: b532ae16-66e7-4641-9bc8-b0d805853477
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6139f2376322443e0b8e76ddc9e3ae84162d71d5
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505313"
---
# <a name="create-a-plan-guide-for-parameterized-queries"></a>Créer un repère de plan pour les requêtes paramétrables
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Un repère de plan TEMPLATE correspond à des requêtes autonomes paramétrables au format spécifié.  
  
 L'exemple suivant crée un repère de plan correspondant à la requête qui paramètre selon une forme donnée, et commande à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'imposer le paramétrage de la requête. La syntaxe des deux requêtes suivantes est équivalente, seules leurs valeurs littérales constantes diffèrent.  
  
```  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45639;  
  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45640;  
```  
  
 Voici le repère de plan pour la forme paramétrable de la requête :  
  
```  
EXEC sp_create_plan_guide   
    @name = N'TemplateGuide1',  
    @stmt = N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
              INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
                  ON h.SalesOrderID = d.SalesOrderID  
              WHERE h.SalesOrderID = @0',  
    @type = N'TEMPLATE',  
    @module_or_batch = NULL,  
    @params = N'@0 int',  
    @hints = N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
 Dans l'exemple précédent, la valeur du paramètre `@stmt` correspond à la forme paramétrable de la requête. La procédure stockée système [sp_get_query_template](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md) est la seule méthode fiable pour obtenir cette valeur et pouvoir l’utiliser dans sp_create_plan_guide. Vous pouvez utiliser le script suivant à la fois pour obtenir la requête paramétrable et créer ensuite un repère de plan à partir de celle-ci.  
  
```  
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
      INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
          ON h.SalesOrderID = d.SalesOrderID  
      WHERE h.SalesOrderID = 45639;',  
    @stmt OUTPUT,   
    @params OUTPUT  
EXEC sp_create_plan_guide N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
> [!IMPORTANT]  
>  Les valeurs littérales constantes du paramètre `@stmt` transmises à `sp_get_query_template` peuvent affecter le type de données choisi pour le paramètre qui remplace le littéral. Ceci va également affecter la mise en correspondance du repère de plan. Vous devrez peut-être créer plusieurs repères de plan pour traiter plusieurs plages de valeurs.  
  
 Vous pouvez combiner les repères de plan TEMPLATE et les repères de plan SQL. Ainsi, vous pouvez créer un repère de plan TEMPLATE pour vous assurer qu'une classe de requêtes est paramétrable, et ensuite créer un repère de plan SQL sur la forme paramétrable de cette requête.  
  
  
