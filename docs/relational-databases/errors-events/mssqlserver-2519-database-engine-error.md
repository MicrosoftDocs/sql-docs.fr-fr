---
description: MSSQLSERVER_2519
title: MSSQLSERVER_2519 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 2519 (Database Engine error)
ms.assetid: 8dc6ad98-5db8-4c88-8dea-6d455e63b839
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6136c47919f6578c0caaf7a1ac5408763a17bda0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88428771"
---
# <a name="mssqlserver_2519"></a>MSSQLSERVER_2519
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|2519|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC_NO_EXPRESSION_EVALUATOR|  
|Texte du message|Impossible de vérifier les colonnes calculées et les types définis par l'utilisateur pour l'ID d'objet O_ID (objet « O_NAME ») car l'évaluateur d'expression interne n'a pas pu être initialisé.|  
  
## <a name="explanation"></a>Explication  
Ce message d'information indique que le processeur de requêtes n'a pas pu fournir à DBCC un objet interne pour permettre l'évaluation de colonnes calculées et de types clr (common language runtime) définis par l'utilisateur. Cela signifie que l'exactitude des colonnes calculées et des types clr définis par l'utilisateur ne sera pas vérifiée et que ceux-ci ne seront pas utilisés lorsque DBCC vérifiera la cohérence entre les index et les tables de base.  
  
## <a name="user-action"></a>Action de l'utilisateur  
None  
  
## <a name="see-also"></a>Voir aussi  
[MSSQLSERVER_2518](~/relational-databases/errors-events/mssqlserver-2518-database-engine-error.md)  
  
