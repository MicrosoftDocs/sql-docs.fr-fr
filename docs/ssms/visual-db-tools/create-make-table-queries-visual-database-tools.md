---
title: Créer des requêtes Make Table
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- queries [SQL Server], types
- table creation [SQL Server], Make Table query
- inserting tables
- Make Table query
- adding tables
ms.assetid: 4493cffa-7b2d-4c24-8ef0-d49329198972
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: dc2c7c425bc9f75304917ec37531a2db7cab57da
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "86010393"
---
# <a name="create-make-table-queries-visual-database-tools"></a>Créer des requêtes Make Table (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous pouvez copier des lignes dans une nouvelle table à l'aide d'une requête Make Table, très utile pour créer des sous-ensembles de données que vous pouvez par la suite manipuler ou pour copier le contenu d'une table d'une base de données vers une autre. Une requête Make Table ressemble à une requête Insert Results à cette différence près qu'elle crée une nouvelle table vers laquelle copier les lignes.  
  
Lorsque vous créez une requête Make Table, spécifiez :  
  
-   le nom de la nouvelle table de base de données (table de destination) ;  
  
-   les tables à partir desquelles copier les lignes (les tables sources), la copie pouvant être réalisée à partir d'une seule table ou de tables jointes ;  
  
-   les colonnes de la table source dont vous souhaitez copier le contenu ;  
  
-   l'ordre de tri, afin de copier, le cas échéant, les lignes dans un ordre précis ;  
  
-   les conditions de recherche pour définir les lignes à copier ;  
  
-   les options Group By, si vous ne souhaitez copier que des informations de synthèse.  
  
Dans l’exemple suivant, la requête crée une nouvelle table appelée `uk_customers` et copie les informations de la table `customers` vers celle-ci :  
  
```  
SELECT *   
INTO uk_customers  
FROM customers  
WHERE country = 'UK'  
```  
  
Il existe deux conditions préalables à l'exécution correcte d'une requête Make Table :  
  
-   Votre base de données doit prendre en charge la syntaxe SELECT...INTO.  
  
-   Vous devez disposer d'une autorisation pour créer une table dans la base de données cible.  
  
### <a name="to-create-a-make-table-query"></a>Pour créer une requête Make Table  
  
1.  Ajoutez la ou les tables sources dans le volet Schéma.  
  
2.  Dans le menu **Concepteur de requêtes** , pointez sur **Modifier le type**, puis cliquez sur **Créer une table**.  
  
3.  Dans la boîte de dialogue **Création de table** , tapez le nom de la table de destination. Le Concepteur de requêtes et de vues ne vérifie pas si le nom existe déjà ou si vous êtes autorisé à créer la table.  
  
    Pour créer une table de destination dans une autre base de données, spécifiez un nom de table complet avec le nom de la base de données cible, le propriétaire (si nécessaire) ainsi que le nom de la table.  
  
4.  Spécifiez les colonnes à copier en les ajoutant à la requête. Pour plus d’informations, consultez [Ajouter des colonnes à des requêtes](../../ssms/visual-db-tools/add-columns-to-queries-visual-database-tools.md). Les colonnes ne sont copiées que si vous les ajoutez à la requête. Pour copier des lignes entières, choisissez **#42; (Toutes les colonnes)** .  
  
    Le Concepteur de requêtes et de vues ajoute les colonnes sélectionnées à la colonne **Colonne** du volet Critères.  
  
5.  Si vous souhaitez copier les lignes dans un ordre précis, spécifiez un ordre de tri. Pour plus d’informations, consultez **Tri et regroupement des résultats de la requête**.  
  
6.  Spécifiez les lignes à copier en entrant des conditions de recherche. Pour plus d’informations, consultez [Spécifier des critères de recherche](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md).  
  
    Si vous ne spécifiez pas de condition de recherche, toutes les lignes de la table source seront copiées vers la table de destination.  
  
    > [!NOTE]  
    > Lorsque vous ajoutez, dans le volet Critères, une colonne à utiliser dans une condition de recherche, le Concepteur de requêtes et de vues l'ajoute également à la liste des colonnes à copier. Pour utiliser une colonne dans le cadre de la recherche sans toutefois la copier, désactivez la case à cocher en regard du nom de la colonne dans le rectangle représentant la table ou l'objet de type table.  
  
7.  Si vous souhaitez copier des informations de synthèse, spécifiez des options Group By. Pour plus d’informations, consultez [Résumer les résultats de requête](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md).  
  
Quand vous exécutez une requête Make Table, aucun résultat n’apparaît dans le [volet Résultats](../../ssms/visual-db-tools/results-pane-visual-database-tools.md). En fait, un message indiquant le nombre de lignes copiées s'affiche.  
  
## <a name="see-also"></a>Voir aussi  
[Rubriques de procédures relatives au Concepteur de vues et de requêtes](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Types de requêtes(../../ssms/visual-db-tools/types-of-queries-visual-database-tools.md)  
  
