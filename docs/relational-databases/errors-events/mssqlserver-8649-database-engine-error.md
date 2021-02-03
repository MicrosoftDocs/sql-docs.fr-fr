---
description: MSSQLSERVER_8649
title: MSSQLSERVER_8649 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8649 (Database Engine error)
ms.assetid: 992dbc74-7c3a-498b-9f1b-b28387640677
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d1041fa6b16b93d2d3a78dbdc0c965228f5145f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205665"
---
# <a name="mssqlserver_8649"></a>MSSQLSERVER_8649
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|8649|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|COST_TOO_HIGH|  
|Texte du message|La requête a été annulée parce que son coût estimé (%d) est supérieur au seuil configuré, %d. Contactez l'administrateur système.|  
  
## <a name="explanation"></a>Explication  
La requête a été annulée parce que son coût estimé est supérieur au seuil configuré pour QUERY_GOVERNOR_COST_LIMIT.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Spécifiez une valeur supérieure pour l'option QUERY_GOVERNOR_COST_LIMIT.  
  
## <a name="see-also"></a>Voir aussi  
[SET QUERY_GOVERNOR_COST_LIMIT &#40;Transact-SQL&#41;](~/t-sql/statements/set-query-governor-cost-limit-transact-sql.md)  
  
