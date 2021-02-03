---
description: MSSQLSERVER_1461
title: MSSQLSERVER_1461 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1461 (Database Engine error)
ms.assetid: fce10907-4753-441b-b624-f28e00ed7520
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2dea7c8af8be207fd164563596d2084b87747acd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196951"
---
# <a name="mssqlserver_1461"></a>MSSQLSERVER_1461
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|1461|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBM_SAFETY_MISMATCH|  
|Texte du message|Plusieurs niveaux de sécurité de mise en miroir de bases de données ont été détectés pour la base de données « %.*ls ». Le niveau de sécurité FULL sera utilisé.|  
  
## <a name="explanation"></a>Explication  
Les connexions de mise en miroir ont été interrompues lorsque le niveau de sécurité des transactions a été modifié car les paramètres de sécurité des transactions étaient incohérents entre la base de données principale et la base de données miroir. Le paramètre de sécurité par défaut, à savoir la sécurité totale des transactions, sera utilisé. La session sera exécutée en mode haute sécurité.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour désactiver la sécurité des transactions, réexécutez l’instruction ALTER DATABASE *nom_base_de_données* SET PARTNER SAFETY OFF sur la base de données principale.  
  
