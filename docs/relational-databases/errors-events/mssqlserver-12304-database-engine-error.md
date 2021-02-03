---
description: MSSQLSERVER_12304
title: MSSQLSERVER_12304 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 12304 (Database Engine error)
ms.assetid: a2c252c2-e815-4ac8-a101-7af5b32e3233
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 484046de79a34997f4e0c77cc0ff56f9c2c2452c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197101"
---
# <a name="mssqlserver_12304"></a>MSSQLSERVER_12304
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|12304|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|HK_UNSUPPORTED_IDENTITY_TABLE_VAR|  
|Texte du message|L'utilisation d'un type de table optimisée en mémoire utilisant la propriété IDENTITY avec l'une de ses colonnes n'est pas prise en charge lors de l'utilisation du type hors du contexte de la procédure stockée compilée en mode natif.|  
  
## <a name="user-action"></a>Action de l'utilisateur  
N'utilisez pas un type de table optimisée en mémoire qui utilise la propriété d'identité avec l'une de ses colonnes.  
  
## <a name="see-also"></a>Voir aussi  
[OLTP en mémoire &#40;Optimisation en mémoire&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
