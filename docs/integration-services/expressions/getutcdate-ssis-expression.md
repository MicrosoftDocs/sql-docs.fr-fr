---
description: GETUTCDATE (expression SSIS)
title: GETUTCDATE (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- dates [Integration Services], GETUTCDATE
- current date
- UTC time
- GETUTCDATE function
ms.assetid: 2282339c-c24f-493e-8e66-181ea8af5ad0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5e14b4477f6a9ea6d39769cdb2f8a2883b361939
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88347985"
---
# <a name="getutcdate-ssis-expression"></a>GETUTCDATE (expression SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Renvoie la date actuelle du système en temps UTC (Universal Time Coordinate ou Greenwich Mean Time) au format DT_DBTIMESTAMP. La fonction GETUTCDATE ne comprend aucun argument.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
GETUTCDATE()  
```  
  
## <a name="arguments"></a>Arguments  
 Aucun  
  
## <a name="result-types"></a>Types des résultats  
 DT_DBTIMESTAMP  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant renvoie l'année de la date actuelle en temps UTC.  
  
```  
DATEPART("year",GETUTCDATE())  
```  
  
 L’exemple suivant retourne le nombre de jours entre une date de la colonne **ModifiedDate** et la date UTC actuelle.  
  
```  
DATEDIFF("dd",ModifiedDate,GETUTCDATE())  
```  
  
 L'exemple suivant ajoute trois mois à la date UTC actuelle.  
  
```  
DATEADD("Month",3,GETUTCDATE())  
```  
  
## <a name="see-also"></a>Voir aussi  
 [GETDATE &#40;expression SSIS&#41;](../../integration-services/expressions/getdate-ssis-expression.md)   
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
