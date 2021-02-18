---
description: Créer des requêtes qui utilisent autre chose qu'une table (Visual Database Tools)
title: Créer des requêtes qui utilisent autre chose qu’une table
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], queries
- queries [SQL Server], creating
ms.assetid: 8e4a1f0a-8a42-4733-be8d-e21d6dbddb33
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 95a432ca8912b4d306538906d11bb2a3d5464e5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350456"
---
# <a name="create-queries-using-something-besides-a-table-visual-database-tools"></a>Créer des requêtes qui utilisent autre chose qu'une table (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Pour écrire une requête d'extraction, il faut toujours formuler quelles sont les colonnes et les lignes à obtenir ainsi que le lieu où le processeur de requêtes trouve les données d'origine. En général, une table ou plusieurs tables jointes constituent les données d'origine. Mais la source des données ne se trouve pas forcément dans une table. Les données d'origine peuvent venir de vues, de requêtes, de synonymes ou de fonctions définies par l'utilisateur et retournant une table.  
  
## <a name="using-a-view-in-place-of-a-table"></a>Utilisation d'une vue à la place d'une table  
Il est possible de sélectionner des lignes d'une vue. Par exemple, une base de données contient une vue « ExpensiveBooks » dont tous les titres ont un prix supérieur à 19.99 (19,99 dollars). La définition de la vue peut se présenter de la manière suivante :  
  
```  
SELECT *  
FROM titles  
WHERE price > 19.99  
```  
  
Pour sélectionner les livres de luxe traitant de psychologie, sélectionnez cette catégorie dans la vue ExpensiveBooks. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
```  
SELECT *  
FROM ExpensiveBooks  
WHERE type = 'psychology'  
```  
  
De façon similaire, une vue peut participer à une opération JOIN. Par exemple, il est possible de rechercher les ventes de livres de luxe par une simple jointure entre la table Sales et la vue ExpensiveBooks. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
```  
SELECT *  
FROM sales   
         INNER JOIN   
         ExpensiveBooks   
         ON sales.title_id   
         =  ExpensiveBooks.title_id  
```  
  
Pour plus d’informations sur l’ajout d’une vue à une requête, consultez [Ajouter des tables à des requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-query-in-place-of-a-table"></a>Utilisation d'une requête à la place d'une table  
Il est possible de sélectionner des lignes d'une requête. Par exemple, vous avez déjà une requête qui extrait les titres et les identificateurs des livres signés par plusieurs auteurs. L'instruction SQL peut se présenter de la manière suivante :  
  
```  
SELECT   
     titles.title_id, title, type  
FROM   
     titleauthor   
         INNER JOIN  
         titles   
         ON titleauthor.title_id   
         =  titles.title_id   
GROUP BY   
     titles.title_id, title, type  
HAVING COUNT(*) > 1  
```  
  
Vous pouvez alors écrire une autre requête qui tire parti de ce résultat. Cette requête peut demander par exemple une extraction des livres signés par plusieurs auteurs et traitant de psychologie. Utilisez la requête existante pour écrire la nouvelle. Elle constituera la source des données de la nouvelle requête. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
```  
SELECT   
    title  
FROM   
    (  
    SELECT   
        titles.title_id,   
        title,   
        type  
    FROM   
        titleauthor   
            INNER JOIN  
            titles   
            ON titleauthor.title_id   
            =  titles.title_id   
    GROUP BY   
        titles.title_id,   
        title,   
        type  
    HAVING COUNT(*) > 1  
    )   
    co_authored_books  
WHERE     type = 'psychology'  
```  
  
Le texte en gras montre la requête existante utilisée comme source de données de la nouvelle requête. Remarquez que la nouvelle requête utilise un alias, « co_authored_books », pour indiquer la requête existante. Pour plus d’informations sur les alias, consultez [Créer des alias de tables &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-table-aliases-visual-database-tools.md) et [Créer des alias de colonnes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
De façon similaire, une requête peut participer à une opération JOIN. Par exemple, il est possible de rechercher les ventes de livres de luxe signés par plusieurs auteurs en créant une jointure entre la vue ExpensiveBooks et la requête d'extraction des livres signés par plusieurs auteurs. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
```  
SELECT   
    ExpensiveBooks.title  
FROM   
    ExpensiveBooks   
        INNER JOIN  
        (  
        SELECT   
            titles.title_id,   
            title,   
            type  
        FROM   
            titleauthor   
                INNER JOIN  
                titles   
                ON titleauthor.title_id   
                =  titles.title_id   
        GROUP BY   
            titles.title_id,   
            title,   
            type  
        HAVING COUNT(*) > 1  
        )  
```  
  
Pour plus d’informations sur l’ajout d’une requête à une requête, consultez [Ajouter des tables à des requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-user-defined-function-in-place-of-a-table"></a>Utilisation d'une fonction définie par l'utilisateur à la place d'une table  
Dans SQL Server 2000 ou une version ultérieure, vous pouvez créer une fonction définie par l'utilisateur et retournant une table. Ce type de fonction permet d'utiliser une logique complexe ou procédurale.  
  
Par exemple, supposons que la table employee contienne une colonne supplémentaire, employee.manager_emp_id, et qu'il existe une clé étrangère entre manager_emp_id et employee.emp_id. Sur chaque ligne de la table employee, la colonne manager_emp_id indique le supérieur hiérarchique de l'employé. Plus précisément, elle indique l'ID d'employé du responsable hiérarchique. La fonction que vous créez pourra renvoyer une table contenant une ligne par employé dépendant d'un responsable hiérarchique dont vous préciserez le niveau. Appelons cette fonction fn_GetWholeTeam et prévoyons dans son design l’utilisation d’une variable d’entrée (l’ID d’employé du responsable de l’équipe à récupérer).  
  
Il est possible d'écrire une requête utilisant la fonction fn_GetWholeTeam comme source de données. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
```  
SELECT *   
FROM   
     fn_GetWholeTeam ('VPA30890F')  
```  
  
« VPA30890F » est l'ID d'employé du responsable de l'équipe à extraire. Pour plus d’informations sur l’ajout d’une fonction définie par l’utilisateur à une requête, consultez [Ajouter des tables à des requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md). Pour obtenir une description complète des fonctions définies par l’utilisateur, consultez [Fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/user-defined-functions.md).  
