---
description: MSSQLSERVER_611
title: MSSQLSERVER_611 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 611 (Database Engine error)
ms.assetid: ac6a213f-5c38-44ad-bc85-a62863786614
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7a9776bca7b50043f4e61034476e140f68e804fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211377"
---
# <a name="mssqlserver_611"></a>MSSQLSERVER_611
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|611|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|VAR_SIZE_TOO_BIG|  
|Texte du message|Impossible d'insérer ou de mettre à jour une ligne, car la taille totale de colonne variable, surcharge comprise, dépasse la limite de %d octets.|  
  
## <a name="explanation"></a>Explication  
La taille de colonne variable maximale est supérieure à celle autorisée par le schéma. L'erreur 611 est retournée lorsque la colonne variable est supérieure à la limite dans le cas où le format de stockage VarDecimal est activé, ou lorsque la taille de la colonne variable est supérieure à la limite autorisée dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] pour un enregistrement de données compressé.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Réduisez la taille de l'enregistrement.  
  
