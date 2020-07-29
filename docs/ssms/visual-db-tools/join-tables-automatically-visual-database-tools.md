---
title: Joindre automatiquement des tables
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- automatic joins
- joins [SQL Server], creating
- joins [SQL Server], automatic
ms.assetid: f152af82-bcb6-49ca-af19-48cdb7fc9ac6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: a98ac8db359d4e47911ce3a2ca97b6cdd5494d90
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "86011703"
---
# <a name="join-tables-automatically-visual-database-tools"></a>Joindre automatiquement des tables (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Quand vous ajoutez plusieurs tables à une requête, le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) tente de déterminer si elles sont liées. Si c'est le cas, le Concepteur de requêtes et de vues place automatiquement des lignes de jointure entre les rectangles représentant des tables ou des objets de type table.  
  
Le Concepteur de requêtes et de vues identifie des tables comme étant jointes dans les cas suivants :  
  
-   La base de données contient des informations qui spécifient que les tables sont liées.  
  
-   Deux colonnes, une dans chaque table, ont les mêmes nom et type de données. La colonne doit constituer la clé primaire d'au moins une des tables. Par exemple, si vous ajoutez les tables `employee` et `jobs` , si la colonne `job_id` est la clé primaire de la table `jobs` et enfin si chaque table possède une colonne appelée `job_id` comportant le même type de données, le Concepteur de requêtes et de vues joint automatiquement les tables.  
  
    > [!NOTE]  
    > Le Concepteur de requêtes et de vues ne crée qu'une seule jointure basée sur les colonnes présentant un nom et un type de données identiques. S'il existe plusieurs possibilités de jointure, le Concepteur de requêtes et de vues s'arrête après avoir créé une jointure fondée sur le premier jeu de colonnes correspondantes qu'il trouve.  
  
-   Le Concepteur de requêtes et de vues détecte qu'une condition de recherche (une clause WHERE) constitue en fait une condition de jointure. Par exemple, vous pouvez ajouter les tables `employee` et `jobs`, puis créer une condition de recherche qui cherche la même valeur dans la colonne `job_id` des deux tables. Dans ce cas, le Concepteur de requêtes et de vues détermine que la condition de recherche donne lieu à une jointure et crée ensuite une condition de jointure basée sur la condition de recherche.  
  
Si le Concepteur de requêtes et de vues a créé une jointure qui n'est pas adaptée à votre requête, vous pouvez modifier cette jointure ou la supprimer. Pour plus d’informations, consultez [Modifier des opérateurs de jointure &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/modify-join-operators-visual-database-tools.md) et [Supprimer des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/remove-joins-visual-database-tools.md).  
  
Si le Concepteur de requêtes et de vues ne joint pas automatiquement les tables dans votre requête, vous pouvez créer vous-même une jointure. Pour plus d’informations, consultez [Joindre manuellement des tables &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-tables-manually-visual-database-tools.md).  
  
## <a name="see-also"></a>Voir aussi  
[Représentation des jointures dans le Concepteur de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/how-the-query-and-view-designer-represents-joins-visual-database-tools.md)  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Interroger avec des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
  
