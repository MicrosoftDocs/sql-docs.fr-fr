---
title: Estimer la taille d’une base de données | Microsoft Docs
description: Lorsque vous concevez une base de données dans SQL Server, l’estimation de sa taille peut vous aider à déterminer la configuration matérielle dont vous avez besoin pour les performances et l’espace disque.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- space allocation [SQL Server], database size
- calculating database size
- increasing database size
- database size [SQL Server], estimating
- predicting database size
- size [SQL Server], databases
- estimating database size
- designing databases [SQL Server], estimating size
ms.assetid: 5b240161-eba4-44b0-946c-61a8fc00fc8c
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0645fdf69d5d1cc2859388c20e6170e3a152c0d9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474130"
---
# <a name="estimate-the-size-of-a-database"></a>Estimer la taille d'une base de données
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Lorsque vous concevez une base de données, il peut être utile d'estimer quelle taille elle aura une fois remplie. Cette estimation peut vous aider à déterminer la configuration matérielle dont vous aurez besoin pour :  
  
-   atteindre le niveau de performances requis par vos applications ;  
  
-   garantir la quantité d'espace disque physique nécessaire pour stocker les données et les index.  
  
 L'estimation de la taille de la base de données peut aussi vous aider à déterminer si des améliorations de conception s'imposent. Par exemple, vous pouvez arriver à la conclusion que la base de données risque d'atteindre une taille trop importante pour que son implémentation s'effectue dans de bonnes conditions dans votre organisation et qu'il faut aller dans le sens d'une plus grande normalisation. À l'inverse, la taille estimée peut être plus petite que prévu, ce qui vous donne la possibilité de dénormaliser la base de données pour améliorer les performances des requêtes.  
  
 Pour estimer la taille d'une base de données, faites une estimation pour chaque table, puis additionnez les valeurs trouvées. La taille d'une table dépend de la présence ou non d'index et, le cas échéant, de leur type.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Estimer la taille d’une table](../../relational-databases/databases/estimate-the-size-of-a-table.md)|Définit les étapes et les calculs nécessaires pour estimer l'espace requis pour le stockage des données dans une table et les index associés.|  
|[Estimer la taille d’un segment de mémoire](../../relational-databases/databases/estimate-the-size-of-a-heap.md)|Définit les étapes et les calculs nécessaires pour estimer l'espace requis pour le stockage des données dans un segment. Un segment est une table qui n'a pas d'index cluster.|  
|[Estimer la taille d'un index cluster](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)|Définit les étapes et les calculs nécessaires pour estimer l'espace requis pour le stockage des données dans un index cluster.|  
|[Estimer la taille d'un index non-cluster](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)|Définit les étapes et les calculs nécessaires pour estimer l'espace requis pour le stockage des données dans un index non-cluster.|  
  
  
