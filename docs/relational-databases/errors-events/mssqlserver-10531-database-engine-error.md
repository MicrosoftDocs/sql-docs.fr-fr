---
description: MSSQLSERVER_10531
title: MSSQLSERVER_10531 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 10531 (Database Engine error)
ms.assetid: bb40e994-231c-44ce-933f-8d767fb2f450
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c8e91f999a9a7526e0419f49e06cdc4710d8db92
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88338581"
---
# <a name="mssqlserver_10531"></a>MSSQLSERVER_10531
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|10531|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|PG_NO_ELIGIBLE_STMT|  
|Texte du message|Impossible de créer le repère de plan '%.*ls' à partir du cache, car l'utilisateur ne dispose pas des autorisations nécessaires. Accordez l'autorisation VIEW SERVER STATE à l'utilisateur qui crée le repère de plan.|  
  
## <a name="explanation"></a>Explication  
L'utilisateur ne dispose pas des autorisations adéquates pour créer le repère de plan spécifié à partir du cache du plan.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Accordez l’autorisation VIEW SERVER STATE à l’utilisateur qui crée le repère de plan.  
  
## <a name="see-also"></a>Voir aussi  
[Repères de plan](~/relational-databases/performance/plan-guides.md)  
[sp_create_plan_guide &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
