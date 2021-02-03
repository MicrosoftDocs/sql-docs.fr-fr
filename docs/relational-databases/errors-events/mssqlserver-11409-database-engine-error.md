---
description: MSSQLSERVER_11409
title: MSSQLSERVER_11409 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 11409 (Database Engine error)
ms.assetid: 99b71a1c-a72d-4ca9-9d00-4230c9042ba5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4ad5ea6caf9aec21ffcad0c7b2c0ebc9ec48b76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197204"
---
# <a name="mssqlserver_11409"></a>MSSQLSERVER_11409
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|11409|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|ALTERCOL_COLSET_DROP|  
|Texte du message|Impossible de supprimer le jeu de colonnes '%.*ls' dans la table '%.\*ls', car celle-ci contient plus de 1 025 colonnes.|  
  
## <a name="explanation"></a>Explication  
Les tables peuvent contenir un maximum de 1 024 colonnes non désignées en tant que colonnes éparses ou calculées. Lorsqu'une table dépasse 1 024 colonnes en raison de colonnes éparses, il convient de définir un jeu de colonnes pour la table en question. La table référencée comporte plus de 1 024 colonnes et vous avez essayé de supprimer le jeu de colonnes.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Étant donné le nombre actuel de colonnes dans la table, vous devez conserver le jeu de colonnes.  
  
Pour supprimer le jeu de colonnes, vous devez au préalable supprimer de la table un nombre de colonnes qui vous permettra de réduire leur nombre à 1 024. Vous pourrez alors supprimer le jeu de colonnes.  
  
