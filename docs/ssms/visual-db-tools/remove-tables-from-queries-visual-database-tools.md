---
description: Supprimer des tables des requêtes (Visual Database Tools)
title: Supprimer des tables dans des requêtes
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- deleting tables
- removing tables
- dropping tables
- queries [SQL Server], tables
ms.assetid: 8fea0b4f-99b7-4050-8d6f-a97ffb839619
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 6c92732dc2cb4fcd817a8c954fa9d29048cd57bd
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92038405"
---
# <a name="remove-tables-from-queries-visual-database-tools"></a>Supprimer des tables des requêtes (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous pouvez supprimer une table, ou un objet table, dans une requête.  
  
> [!NOTE]  
> Lorsque vous procédez à la suppression d'une table ou d'un objet table, cette suppression ne s'effectue que dans la requête en cours et non dans la base de données. Pour plus d’informations sur la suppression d’une table d’une base de données, consultez [Procédure : Supprimer les tables d’une base de données](../../relational-databases/tables/delete-tables-database-engine.md).  
  
### <a name="to-remove-a-table-or-table-structured-object"></a>Pour supprimer une table ou un objet structuré comme une table  
  
-   Dans le **volet Schéma**, sélectionnez la table, la vue, la fonction définie par l’utilisateur, le synonyme ou la requête, puis appuyez sur la touche Suppr ou cliquez avec le bouton droit sur l’objet et sélectionnez **Supprimer** dans la boîte de dialogue qui s’affiche. Vous pouvez sélectionner et supprimer plusieurs objets à la fois.  
  
    - ou -  
  
-   Supprimez toutes les références à l’objet dans le **volet SQL**.  
  
Quand vous supprimez une table ou un objet table, le Concepteur de requêtes et de vues supprime automatiquement les jointures relatives à cette table ou à cet objet table, ainsi que les références aux colonnes de l’objet dans le **volet SQL** et le **volet Critères**. Si la requête comporte toutefois des expressions complexes relatives à l'objet, la suppression automatique de cet objet ne s'effectue pas tant que toutes les références le concernant n'ont pas été supprimées.  
  
## <a name="see-also"></a>Voir aussi  
[Ajouter des tables à des requêtes](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md)  
[Créer des alias de table](../../ssms/visual-db-tools/create-table-aliases-visual-database-tools.md)  
[Spécifier des critères de recherche](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
[Résumer les résultats de requête](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
[Effectuer des opérations de base avec des requêtes](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
