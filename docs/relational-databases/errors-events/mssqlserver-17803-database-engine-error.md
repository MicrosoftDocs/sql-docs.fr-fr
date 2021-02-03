---
description: MSSQLSERVER_17803
title: MSSQLSERVER_17803 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17803 (Database Engine error)
ms.assetid: 28591a19-258d-4891-b78a-4686789bb2d7
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 15b5f790ac15b43eb51e14e1410ac8053f0b4a91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196657"
---
# <a name="mssqlserver_17803"></a>MSSQLSERVER_17803
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|17803|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SRV_NOMEMORY|  
|Texte du message|Une erreur d'allocation de mémoire s'est produite au cours de l'établissement de la connexion. Réduisez la charge de mémoire qui n'est pas essentielle ou augmentez la quantité de mémoire système. La connexion a été fermée.%.*ls|  
  
## <a name="explanation"></a>Explication  
Le serveur manque de mémoire.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Déterminez la cause de la mémoire insuffisante au niveau du serveur. Les étapes de résolution des problèmes de mémoire génériques peuvent être utiles  
  
