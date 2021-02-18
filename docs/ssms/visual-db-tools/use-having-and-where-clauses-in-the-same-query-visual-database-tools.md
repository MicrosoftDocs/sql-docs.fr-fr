---
description: Utiliser les clauses HAVING et WHERE dans la même requête (Visual Database Tools)
title: Utiliser les clauses HAVING et WHERE dans la même requête
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search criteria [SQL Server], excluding rows
- search criteria [SQL Server], WHERE clause
- search conditions [SQL Server], HAVING clause
- row excluded in search [SQL Server]
- search criteria [SQL Server], HAVING clause
- HAVING clause, search criteria
- search conditions [SQL Server], WHERE clause
- WHERE clause, search criteria
- excluding rows
ms.assetid: 1e07cf56-b4b7-4c49-8ddd-c276812a7148
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: cdaf2096589b5a1b16753c79c7eac6cda08fff68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350442"
---
# <a name="use-having-and-where-clauses-in-the-same-query-visual-database-tools"></a>Utiliser les clauses HAVING et WHERE dans la même requête (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Il peut arriver que vous souhaitiez exclure des lignes individuelles de groupes (à l'aide d'une clause WHERE) avant d'appliquer une condition aux groupes dans leur ensemble (à l'aide d'une clause HAVING).  
  
Une clause HAVING est similaire à une clause WHERE si ce n'est qu'elle s'applique uniquement aux groupes dans leur ensemble (c'est-à-dire aux lignes du jeu de résultats qui représentent des groupes), tandis que la clause WHERE s'applique aux lignes individuelles. Une requête peut comporter à la fois une clause WHERE et une clause HAVING. Dans ce cas :  
  
-   La clause WHERE est d'abord appliquée aux lignes individuelles dans les tables ou les objets table du volet Schéma. Seules les lignes répondant aux conditions précisées dans la clause WHERE sont groupées.  
  
-   La clause HAVING est ensuite appliquée aux lignes du jeu de résultats. Seuls les groupes qui répondent aux conditions HAVING figurent dans le résultat de la requête. Vous ne pouvez appliquer une clause HAVING qu'aux colonnes qui apparaissent également dans la clause GROUP BY ou dans une fonction d'agrégation.  
  
Imaginons, par exemple, que vous décidiez de joindre les tables `titles` et `publishers` pour créer une requête affichant le prix moyen d'un livre pour un ensemble d'éditeurs. Seul le prix moyen d’un ensemble donné d’éditeurs vous intéresse, en l’occurrence ceux répertoriés en Californie. Par ailleurs, vous ne souhaitez afficher le prix moyen que si ce dernier est supérieur à 10 $.  
  
Vous définissez la première condition en incluant une clause WHERE qui rejette tous les éditeurs non répertoriés en Californie, avant de calculer les prix moyens. La seconde condition nécessite une clause HAVING dans la mesure où elle est fondée sur les résultats du regroupement et de la synthèse des données. L’instruction SQL obtenue peut se présenter de la manière suivante :  
  
```  
SELECT titles.pub_id, AVG(titles.price)  
FROM titles INNER JOIN publishers  
   ON titles.pub_id = publishers.pub_id  
WHERE publishers.state = 'CA'  
GROUP BY titles.pub_id  
HAVING AVG(price) > 10  
```  
  
Vous pouvez créer à la fois une clause HAVING et une clause WHERE dans le volet Critères. Par défaut, si vous spécifiez une condition de recherche pour une colonne, la condition fait alors partie de la clause HAVING. Toutefois, vous pouvez modifier la condition de sorte qu'elle devienne une clause WHERE.  
  
Il est possible de créer une clause WHERE et une clause HAVING qui s'appliquent à une même colonne. Pour cela, vous devez ajouter deux fois la colonne au volet Critères, puis spécifier qu'une instance doit faire partie de la clause HAVING et l'autre de la clause WHERE.  
  
### <a name="to-specify-a-where-condition-in-an-aggregate-query"></a>Pour spécifier une condition WHERE dans une requête d'agrégation  
  
1.  Spécifiez les groupes de votre requête. Pour plus d’informations, consultez [Regrouper des lignes dans les résultats de requête](../../ssms/visual-db-tools/group-rows-in-query-results-visual-database-tools.md).  
  
2.  Si elle ne figure pas encore dans le volet Critères, ajoutez la colonne sur laquelle vous souhaitez baser la condition WHERE.  
  
3.  Supprimez la colonne **Sortie** à moins que la colonne de données fasse partie de la clause GROUP BY ou soit incluse dans une fonction d’agrégation.  
  
4.  Dans la colonne **Filtre** , spécifiez la condition WHERE. Le Concepteur de requêtes et de vues ajoute la condition à la clause HAVING de l'instruction SQL.  
  
    > [!NOTE]  
    > La requête illustrée dans cet exemple de procédure joint deux tables, `titles` et `publishers`.  
  
    À ce stade de la requête, l'instruction SQL contient une clause HAVING :  
  
    ```  
    SELECT titles.pub_id, AVG(titles.price)  
    FROM titles INNER JOIN publishers   
       ON titles.pub_id = publishers.pub_id  
    GROUP BY titles.pub_id  
    HAVING publishers.state = 'CA'  
    ```  
  
5.  Dans la colonne **Grouper par** , sélectionnez **Where** dans la liste des options de synthèse et de groupe. Le Concepteur de requêtes et de vues supprime la condition de la clause HAVING dans l'instruction SQL et l'ajoute à la clause WHERE.  
  
    L'instruction SQL est modifiée afin d'inclure à la place une clause WHERE :  
  
    ```  
    SELECT titles.pub_id, AVG(titles.price)  
    FROM titles INNER JOIN publishers   
       ON titles.pub_id = publishers.pub_id  
    WHERE publishers.state = 'CA'  
    GROUP BY titles.pub_id  
    ```  
  
## <a name="see-also"></a>Voir aussi  
[Trier et grouper les résultats de requête](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[Résumer les résultats de requête](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
  
