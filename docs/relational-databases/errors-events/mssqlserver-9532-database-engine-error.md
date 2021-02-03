---
description: MSSQLSERVER_9532
title: MSSQLSERVER_9532 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9532 (Database Engine error)
ms.assetid: ab95cce8-4f97-4aea-a746-a73eea7c9aab
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1bd1948e2475b792198465ed8befc838ab00db48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187577"
---
# <a name="mssqlserver_9532"></a>MSSQLSERVER_9532
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|9532|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|XMLERR_COLUMNSET_CANNOT_CONVERT_FROM_TO|  
|Texte du message|Dans l’opération de requête/DML impliquant le jeu de colonnes '%.*ls', la conversion a échoué lors de la conversion du type de données '%ls' en type de données '%ls' pour la colonne '%.\*ls'.|  
  
## <a name="explanation"></a>Explication  
Un jeu de colonnes est une représentation XML non typée qui associe certaines colonnes d'une table dans une sortie structurée. Quand vous insérez ou mettez à jour des valeurs de colonnes éparses à l’aide du jeu de colonnes XML, les valeurs insérées dans les colonnes éparses sous-jacentes sont converties implicitement à partir du type de données **xml**. Une valeur a été fournie qui ne peut pas être convertie vers le type de données de la colonne.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Comme la valeur fournie n'a pas pu être convertie implicitement, il peut s'agir d'une entrée non valide. Corrigez l'erreur et réessayez. Si la valeur est correcte, modifiez l'instruction pour utiliser les colonnes individuelles à la place du jeu de colonnes. Cela vous permettra de convertir explicitement le type de la valeur vers le type de données correct.  
  
