---
description: MSSQLSERVER_7901
title: MSSQLSERVER_7901 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7901 (Database Engine error)
ms.assetid: 2d0d19b9-947b-4474-9ff8-7e03019ab93d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3723349a65a5e5fafc185ec6a342965992041a80
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204589"
---
# <a name="mssqlserver_7901"></a>MSSQLSERVER_7901
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|7901|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC2_DATABASE_IN_EMERGENCY_MODE|  
|Texte du message|L’instruction de réparation n’a pas été traitée. Ce niveau de réparation n'est pas pris en charge lorsque la base de données est en mode d'urgence.|  
  
## <a name="explanation"></a>Explication  
La base de données est en mode d'urgence et un niveau de réparation autre que REPAIR_ALLOW_DATA_LOSS a été spécifié. Les réparations ne peuvent pas être effectuées en mode d'urgence si REPAIR_ALLOW_DATA_LOSS n'est pas spécifié.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Exécutez de nouveau la commande et spécifiez l'option REPAIR_ALLOW_DATA_LOSS.  
  
