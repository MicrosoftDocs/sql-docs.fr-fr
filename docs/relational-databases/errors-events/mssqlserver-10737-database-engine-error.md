---
description: MSSQLSERVER_10737
title: MSSQLSERVER_10737 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10737 (Database Engine error)
ms.assetid: 208af6ed-b271-4ab8-803e-7666025385c8
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 05bb027048e38f976a8b32de684e59ad5cf68157
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197318"
---
# <a name="mssqlserver_10737"></a>MSSQLSERVER_10737
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|MSSQLSERVER|  
|ID de l’événement|10737|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|REBUILD_PARTITION_ALL_NOT_SPECIFIED|  
|Texte du message|Dans une instruction ALTER TABLE REBUILD ou ALTER INDEX REBUILD, lorsqu'une partition est spécifiée dans une clause DATA_COMPRESSION, PARTITION=ALL est obligatoire. La clause PARTITION=ALL permet d'imposer que toutes les partitions de la table ou de l'index soient régénérées, même si un seul sous-ensemble est spécifié dans la clause DATA_COMPRESSION.|  
  
## <a name="user-action"></a>Action de l'utilisateur  
Ajoutez la clause PARTITION=ALL à l'instruction ALTER TABLE ou ALTER INDEX. Ou, pour reconstruire une partition spécifique, utilisez REBUILD PARTITION = \<partition-number-expr> WITH (DATA_COMPRESSION={ON | OFF}).  
  
