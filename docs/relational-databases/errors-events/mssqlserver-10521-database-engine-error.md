---
description: MSSQLSERVER_10521
title: MSSQLSERVER_10521 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10521 (Database Engine error)
ms.assetid: ba2d7e44-207c-4428-b5f0-c975ac122c0d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6fc717b713ec5b3e6ca48ccb64cc43713cd663a2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197429"
---
# <a name="mssqlserver_10521"></a>MSSQLSERVER_10521
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|10521|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|PG_PARAM_NEEDED|  
|Texte du message|Impossible de créer le repère de plan '%.\*ls', car **\@type** a été spécifié en tant que '%ls' et le paramètre '%ls' a la valeur Null. Ce type requiert une valeur autre que Null pour le paramètre. Spécifiez une valeur autre que Null pour le paramètre ou remplacez le type par un type qui autorise une valeur Null pour le paramètre.|  
  
## <a name="explanation"></a>Explication  
Le type spécifié dans **\@type** requiert une valeur autre que Null pour le paramètre spécifié ; toutefois, une valeur Null a été fournie.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Spécifiez une valeur autre que Null pour le paramètre ou remplacez le type par un type qui autorise une valeur Null pour le paramètre.  
  
## <a name="see-also"></a>Voir aussi  
[sp_create_plan_guide &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[Repères de plan](~/relational-databases/performance/plan-guides.md)  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
