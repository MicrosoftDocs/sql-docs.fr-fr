---
description: MSSQLSERVER_8621
title: MSSQLSERVER_8621 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8621 (Database Engine error)
ms.assetid: 67f59865-becd-4999-8bb0-90aedd7effbf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 61b164c84b39dc4f1e6e2a9a243d86243e798041
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180914"
---
# <a name="mssqlserver_8621"></a>MSSQLSERVER_8621
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|8621|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|OPTIMIZER_STACK_OVERFLOW_ERR|  
|Texte du message|Espace de pile du processeur de requêtes insuffisant lors de l'optimisation de la requête. Simplifiez la requête.|  
  
## <a name="explanation"></a>Explication  
La taille de la requête développée est la cause la plus probable de l'erreur. La requête développée substitue dans la requête d'origine les définitions de toutes les vues, colonnes calculées, fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] et expressions de table commune auxquelles elle fait référence, ainsi que les actions en cascade telles que la mise à jour des index secondaires, des vues et des déclencheurs.  
  
La requête possède probablement une dimension importante ; par exemple, le nombre de tables référencées par les définitions de vues ou une expression scalaire très importante.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Simplifiez la requête en la divisant en plusieurs requêtes le long de la dimension la plus importante. Commencez par supprimer tous les éléments de la requête qui ne sont pas réellement nécessaires, puis essayez d'ajouter une table temporaire et de diviser la requête en deux.  Le simple déplacement d'une partie de la requête vers une sous-requête, une fonction ou une expression de table commune ne suffit pas, car ces éléments sont réassociés par le compilateur [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
