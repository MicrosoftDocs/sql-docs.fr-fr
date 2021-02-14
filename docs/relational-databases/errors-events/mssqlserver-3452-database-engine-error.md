---
description: MSSQLSERVER_3452
title: MSSQLSERVER_3452 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3452 (Database Engine error)
ms.assetid: bf838f02-7186-4b33-b01e-361b0c02de1f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1fe941e7ffaa594daa11e11e72140b4ace07cd33
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623791"
---
# <a name="mssqlserver_3452"></a>MSSQLSERVER_3452
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|3452|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|REC_CHECKIDENTITY|  
|Texte du message|La récupération de la base de données '%.*ls' (%d) a détecté une possible incohérence de valeurs d'identité dans la table ID %d. Exécutez DBCC CHECKIDENT (’%.\*ls’).|  
  
## <a name="explanation"></a>Explication  

Au cours d’une mise à niveau ou de l’application d’une mise à jour de maintenance, le processus de récupération des valeurs d’identité dans la base de données a détecté une incohérence dans les métadonnées.  
  
## <a name="user-action"></a>Action de l'utilisateur

Exécutez DBCC CHECKIDENT.  
  
