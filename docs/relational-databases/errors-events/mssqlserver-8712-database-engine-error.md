---
description: MSSQLSERVER_8712
title: MSSQLSERVER_8712 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 8712 (Database Engine error)
ms.assetid: 292fb3bc-062e-41e4-a566-b5d3d0b21977
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: fefaaae1f1bed8001fd9c55ff8a26fad71daec51
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88491128"
---
# <a name="mssqlserver_8712"></a>MSSQLSERVER_8712
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|8712|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|USEPLAN_ERR_NO_INDEX|  
|Texte du message|L'index '%.*ls' spécifié dans l'indicateur USE PLAN n'existe pas. Indiquez un index existant ou créez-en un avec le nom spécifié.|  
  
## <a name="explanation"></a>Explication  
Un index spécifié dans l'indicateur USE PLAN n'existe pas.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Veillez à ce que tous les index spécifiés dans l'indicateur USE PLAN existent.  
  
## <a name="see-also"></a>Voir aussi  
[Indicateurs de requête &#40;Transact-SQL&#41;](~/t-sql/queries/hints-transact-sql-query.md)  
[Repères de plan](~/relational-databases/performance/plan-guides.md)  
  
