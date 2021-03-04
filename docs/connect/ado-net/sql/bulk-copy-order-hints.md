---
title: Indicateurs d’ordre pour les opérations de copie en bloc
description: Explique comment utiliser les indicateurs d’ordre dans les opérations de copie en bloc.
ms.date: 06/15/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: ba652ab9eb1cbe6564a8f1b2aa5d7309cba86ac4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837103"
---
# <a name="order-hints-for-bulk-copy-operations"></a>Indicateurs d’ordre pour les opérations de copie en bloc

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

Les opérations de copie en bloc offrent de gros avantages du point de vue des performances par rapport à d’autres méthodes de chargement des données dans une table SQL Server. Les indicateurs d’ordre permettent d’améliorer encore les performances. Si vous en spécifiez pour vos opérations de copie en bloc, vous pouvez réduire le délai d’insertion des données triées dans les tables avec index cluster.

Par défaut, l’opération d’insertion en bloc considère que les données entrantes ne sont pas ordonnées. SQL Server force un tri intermédiaire de ces données avant de les charger en masse. Si vous savez que vos données entrantes sont déjà triées, vous pouvez utiliser des indicateurs d’ordre pour indiquer à l’opération de copie en bloc l’ordre de tri de toutes les colonnes de destination qui font partie d’un index cluster.
  
## <a name="adding-order-hints-to-a-bulk-copy-operation"></a>Ajout d’indicateurs d’ordre à une opération de copie en bloc  
L’exemple suivant copie en bloc les données d’une table source de l’exemple de base de données **AdventureWorks** vers une table de destination située dans la même base de données. Un objet SqlBulkCopyColumnOrderHint est créé pour définir l’ordre de tri de la colonne **ProductNumber** dans la table de destination. L’indicateur d’ordre est ensuite ajouté à l’instance SqlBulkCopy, qui ajoute à la requête `INSERT BULK` résultante l’argument correspondant à l’indicateur.

[!code-csharp [SqlBulkCopy.ColumnOrderHint#1](~/../sqlclient/doc/samples/SqlBulkCopy_ColumnOrderHint.cs#1)]

## <a name="next-steps"></a>Étapes suivantes
- [Opérations de copie en bloc dans SQL Server](bulk-copy-operations-sql-server.md)
