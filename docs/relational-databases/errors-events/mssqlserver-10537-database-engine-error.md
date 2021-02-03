---
description: MSSQLSERVER_10537
title: MSSQLSERVER_10537 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10537 (Database Engine error)
ms.assetid: 728469a4-6523-4a37-925f-a950d75420fa
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b6b1cff02deb2b56772b4d8af0c3fb47c7fd93c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197357"
---
# <a name="mssqlserver_10537"></a>MSSQLSERVER_10537
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|10537|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|PG_DUP_ENABLED|  
|Texte du message|Impossible d’activer le repère de plan '%.*ls', car le repère de plan activé '%.\*ls' contient les mêmes étendue et valeur de décalage de départ que l’instruction. Désactivez le repère de plan existant avant d'activer le repère de plan spécifié.|  
  
## <a name="explanation"></a>Explication  
Un repère de plan existant contient les mêmes étendue et valeur de décalage de départ que l'instruction du repère de plan spécifié.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Désactivez le repère de plan existant avant d'activer le repère de plan spécifié.  
  
## <a name="see-also"></a>Voir aussi  
[sp_create_plan_guide &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[Repères de plan](~/relational-databases/performance/plan-guides.md)  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
