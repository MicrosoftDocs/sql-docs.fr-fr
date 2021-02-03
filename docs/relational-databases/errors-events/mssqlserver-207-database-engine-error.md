---
title: MSSQLSERVER_207 | Microsoft Docs
description: Une erreur de requête dans SQL Server concernant un nom de colonne non valide. Consultez une explication de l’erreur et les solutions possibles.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 207 (Database Engine error)
ms.assetid: d1ab00c7-0331-437a-84fe-bae53b82feec
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7c39bf5d5b820a906cc5b14fd270ed59aa3f4084
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196430"
---
# <a name="mssqlserver_207"></a>MSSQLSERVER_207
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|207|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQ_BADCOL|  
|Texte du message|Nom de colonne non valide : '%.*ls'.|  
  
## <a name="explanation"></a>Explication  
Cette erreur de requête peut être causée par l'un des problèmes suivants.  
  
-   Le nom de colonne est mal orthographié ou la colonne n'existe pas dans les tables spécifiées.  
  
-   Le classement de la base de données respecte la casse et la casse du nom de colonne spécifié dans la requête ne correspond pas à celle de la colonne définie dans la table. Par exemple, quand une colonne est définie dans une table en tant que **LastName** et que la base de données utilise un classement qui respecte la casse, les requêtes qui font référence à cette colonne en tant que **Lastname** ou **lastname** retournent l’erreur 207, car le nom de colonne ne correspond pas.  
  
-   Un alias de colonne, défini dans la clause SELECT, est référencé dans une autre clause telle que WHERE ou GROUP BY. Par exemple, la requête suivante définit l'alias de colonne `Year` dans la clause SELECT et y fait référence dans la clause GROUP BY.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT DATEPART(yyyy,OrderDate) AS Year, SUM(TotalDue) AS Total  
    FROM Sales.SalesOrderHeader  
    GROUP BY Year;  
    ```  
  
    En raison de l'ordre dans lequel les clauses de requête sont logiquement traitées, l'exemple retourne l'erreur 207. L'ordre de traitement est le suivant :  
  
    1.  FROM  
  
    2.  ACTIVÉ  
  
    3.  JOIN  
  
    4.  WHERE  
  
    5.  GROUP BY  
  
    6.  WITH CUBE ou WITH ROLLUP  
  
    7.  HAVING  
  
    8.  SELECT  
  
    9. DISTINCT  
  
    10. ORDER BY  
  
    11. Haut de la page  
  
    Étant donné qu'un alias de colonne n'est pas défini tant que la clause SELECT n'est pas traitée, le nom d'alias est inconnu lorsque la clause GROUP BY est traitée.  
  
-   L’instruction MERGE génère cette erreur quand la clause *<merge_matched>* fait référence à des colonnes de la table source mais qu’aucune ligne n’est retournée par la table source dans la clause WHEN NOT MATCHED BY SOURCE. L'erreur se produit car les colonnes dans la table source sont inaccessibles lorsque aucune ligne n'est retournée à la requête. Par exemple, la clause `WHEN NOT MATCHED BY SOURCE THEN UPDATE SET TargetTable.Col1 = SourceTable.Col1` peut faire en sorte que l'instruction échoue si `Col1` dans la table source est inaccessible.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Vérifiez les informations suivantes et corrigez l'instruction comme il convient.  
  
-   Le nom de colonne existe dans la table et il est correctement orthographié. L’exemple suivant interroge la vue de catalogue **sys.columns** pour retourner tous les noms de colonnes pour une table donnée.  
  
    ```  
    SELECT name FROM sys.columns WHERE object_id = OBJECT_ID('schema_name.table_name');  
    ```  
  
-   Respect de la casse du classement de base de données. L'instruction suivante retourne le classement de la base de données spécifiée.  
  
    ```  
    SELECT collation_name FROM sys.databases WHERE name = 'database_name';  
    ```  
  
    L’abréviation CS dans le nom du classement indique que celui-ci respecte la casse. Par exemple, Latin1_General_CS_AS indique un classement qui respecte la casse et les accents. Modifiez le nom de colonne de sorte qu'il corresponde au nom de colonne tel que défini dans la table.  
  
-   Un alias de colonne est référencé de manière incorrecte. Modifiez l'instruction en répétant l'expression qui définit l'alias dans la clause appropriée ou en utilisant une table dérivée. L'exemple suivant répète les expressions qui définissent l'alias `Year` dans la clause GROUP BY.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT DATEPART(yyyy,OrderDate) AS Year ,SUM(TotalDue) AS Total  
    FROM Sales.SalesOrderHeader  
    GROUP BY DATEPART(yyyy,OrderDate);  
    ```  
  
    L'exemple suivant utilise une table dérivée pour rendre le nom d'alias accessible à d'autres clauses dans la requête. Notez que l'alias `Year` est défini dans la clause FROM, qui est traitée en premier, ce qui rend l'alias accessible pour une utilisation dans d'autres clauses de la requête.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT d.Year, SUM(TotalDue) AS Total  
    FROM (SELECT DATEPART(yyyy,OrderDate) AS Year, TotalDue  
          FROM Sales.SalesOrderHeader)AS d  
    GROUP BY Year;  
    ```  
  
-   La clause WHEN NOT MATCHED BY SOURCE de l'instruction MERGE fait référence à une valeur accessible. Modifiez l'instruction MERGE de façon qu'une ligne au moins soit retournée par la table source dans la clause WHEN NOT MATCHED BY SOURCE. Par exemple, vous devrez peut-être ajouter ou modifier le critère de recherche spécifié pour la clause. Vous pouvez aussi modifier la clause de façon à spécifier une valeur qui ne fait pas référence à la table source. Par exemple : `WHEN NOT MATCHED BY SOURCE THEN UPDATE SET TargetTable.Col1 = <expression, or other available value>`.  
  
## <a name="see-also"></a>Voir aussi  
[MERGE &#40;Transact-SQL&#41;](~/t-sql/statements/merge-transact-sql.md)  
[FROM &#40;Transact-SQL&#41;](~/t-sql/queries/from-transact-sql.md)  
[SELECT &#40;Transact-SQL&#41;](~/t-sql/queries/select-transact-sql.md)  
[UPDATE &#40;Transact-SQL&#41;](~/t-sql/queries/update-transact-sql.md)  
  
