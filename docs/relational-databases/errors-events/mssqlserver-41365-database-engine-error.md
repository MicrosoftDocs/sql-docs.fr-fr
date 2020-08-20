---
description: MSSQLSERVER_41365
title: MSSQLSERVER_41365 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 41365 (Database Engine error)
ms.assetid: 4fc7ec15-b722-4e3d-b7f9-3d39d171e96e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e370cafb16d5eceb2caed96b561862ea93b68205
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88460920"
---
# <a name="mssqlserver_41365"></a>MSSQLSERVER_41365
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|41365|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|HK_MERGE_SCHEDULE_ERROR|  
|Texte du message|La demande de fusion de la plage des transactions [%d, %d] dans la base de données %.*ls n'a pas été planifiée. Les fichiers de point de contrôle représentant la plage sont soit indisponibles pour la fusion, soit ils font partie d'une fusion en cours.|  
  
## <a name="explanation"></a>Explication  
Les fichiers de point de contrôle représentant la plage sont soit indisponibles pour la fusion, soit ils font partie d'une fusion en cours.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Optimisez la plage de transactions pour les opérations de fusion/attente avant de réexécuter la même demande. Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
## <a name="see-also"></a>Voir aussi  
[OLTP en mémoire &#40;Optimisation en mémoire&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
