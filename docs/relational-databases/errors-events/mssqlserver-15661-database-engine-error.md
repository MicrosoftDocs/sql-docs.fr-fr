---
description: MSSQLSERVER_15661
title: MSSQLSERVER_15661 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15661 (Database Engine error)
ms.assetid: 88b01bfb-74ce-4aa0-aec0-7885261c7ef3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 74f8aa7af5629408865b253409eef9fdb2b7ce91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196871"
---
# <a name="mssqlserver_15661"></a>MSSQLSERVER_15661
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|15661|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum15661|  
|Texte du message|La procédure stockée sp_estimate_data_compression_savings ne peut pas être utilisée pour les tables temporaires.|  
  
## <a name="explanation"></a>Explication  
Une table temporaire a été utilisée comme argument de la procédure stockée sp_estimate_data_compression_savings. Bien que la compression de tables temporaires soit prise en charge, vous ne pouvez pas utiliser sp_estimate_data_compression_savings pour estimer les économies de compression.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Supprimez la table temporaire comme argument de sp_estimate_data_compression_savings.  
  
