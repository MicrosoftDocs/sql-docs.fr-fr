---
description: MSSQLSERVER_12329
title: MSSQLSERVER_12329 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 12329 (Database Engine error)
ms.assetid: 43f90287-36d5-46c2-ac91-a37202dcf6d3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 73c58c5c97f71ed355f476bd5395197d5f0b0af1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197054"
---
# <a name="mssqlserver_12329"></a>MSSQLSERVER_12329
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|12329|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|HK_UNSUPPORTED_NON_LATIN_CODEPAGE|  
|Texte du message|Les types de données char(n) et varchar(n) qui utilisent un classement avec une page de codes différente de 1252 ne sont pas pris en charge par *construction*.|  
  
## <a name="explanation"></a>Explication  
N'utilisez pas les types de données char(n) et varchar(n) qui utilisent un classement avec une page de codes différente de 1252.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Une situation inattendue pouvant générer cette erreur est :  
  
```  
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = 'us_english')  
```  
  
Utilisez ceci à la place :  
  
```  
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
```  
  
