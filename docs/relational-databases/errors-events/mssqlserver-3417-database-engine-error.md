---
description: MSSQLSERVER_3417
title: MSSQLSERVER_3417 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3417 (Database Engine error)
ms.assetid: 005829c8-cf57-4828-818a-bbe8ee2e00f0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: bf4dac403156a5fff0b0b5cdb59a98d4a58abd0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201609"
---
# <a name="mssqlserver_3417"></a>MSSQLSERVER_3417
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|3417|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|REC_BADMASTER|  
|Texte du message|Impossible de récupérer la base de données master. Impossible d'exécuter SQL Server. Restaurez la base master à partir d'une sauvegarde complète, réparez-la ou reconstruisez-la. Pour plus d'informations sur la façon de reconstruire la base de données master, consultez la documentation en ligne de SQL Server.|  
  
## <a name="explanation"></a>Explication  
SQL Server ne peut pas démarrer la base de données **MASTER**. Si les bases de données **MASTER** ou **tempdb** ne peuvent pas être mises en ligne, SQL Server ne peut pas s’exécuter. Cette erreur est généralement précédée d'autres erreurs. Étudiez les journaux d'erreurs pour trouver la racine du problème.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Restaurez la sauvegarde de la base de données ou réparez la base de données.  
  
