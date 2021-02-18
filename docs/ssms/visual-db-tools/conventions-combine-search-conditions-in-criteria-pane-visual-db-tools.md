---
description: Conventions pour la combinaison de conditions de recherche dans le volet Critères (Visual Database Tools)
title: Conventions pour la combinaison de conditions de recherche dans le volet Critères
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search conditions [SQL Server], combining
- precedence [SQL Server], Criteria pane
- search criteria [SQL Server], combining conditions
- multiple OR clauses
- combining search conditions
- OR operator
- AND, Criteria pane
- multiple AND clauses
ms.assetid: d4859be5-ff5b-48b2-a101-ad40c6dbcc68
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: fe9c75b0b173bf2f72437233d2c06305b5041132
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355203"
---
# <a name="conventions-for-combining-search-conditions-in-the-criteria-pane-visual-database-tools"></a>Conventions pour la combinaison de conditions de recherche dans le volet Critères (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous pouvez créer des requêtes qui incluent un nombre quelconque de conditions de recherche, reliées par un nombre quelconque d'opérateurs AND et OR. Une requête avec une combinaison de clauses AND et OR peut devenir complexe ; il est donc utile de comprendre comment une telle requête est interprétée quand vous l’exécutez et comment elle est représentée dans les volets [Critères](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md) et [SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md).  
  
> [!NOTE]  
> Pour plus d’informations sur les conditions de recherche qui contiennent un seul opérateur AND ou OR, consultez [Spécifier plusieurs conditions de recherche pour une colonne &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-one-column-visual-database-tools.md) et [Spécifier plusieurs conditions de recherche pour plusieurs colonnes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-multiple-columns-visual-database-tools.md).  
  
Vous trouverez ci-dessous des informations sur les sujets suivants :  
  
-   L'ordre de priorité des opérateurs AND et OR dans les requêtes où ils apparaissent tous les deux.  
  
-   La relation logique entre les conditions dans les clauses AND et OR.  
  
-   La façon dont le Concepteur de requêtes et de vues représente les requêtes qui contiennent à la fois AND et OR dans le volet Critères.  
  
Dans les paragraphes suivants, nous prendrons comme exemple une table `employee` contenant les colonnes `hire_date`, `job_lvl`et `status`. Nous supposerons que vous désirez savoir depuis combien de temps un employé travaille dans la société (quelle est sa date d'embauche), quel type de fonction il exerce (quel est son niveau de travail) et quel est son statut (par exemple, retraité).  
  
## <a name="precedence-of-and-and-or"></a>Ordre de priorité des opérateurs AND et OR  
Lorsqu'une requête est exécutée, elle évalue d'abord les clauses reliées par AND, puis celles reliées par OR.  
  
> [!NOTE]  
> L'opérateur NOT est prioritaire sur AND et sur OR.  
  
Par exemple, pour rechercher les employés qui travaillent dans la société depuis plus de cinq ans, avec un niveau de qualification faible ou les employés avec un niveau de qualification moyen, quelle que soit leur date d'embauche, vous pouvez construire la clause WHERE suivante :  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   job_lvl = 100 OR  
   job_lvl = 200  
```  
  
Pour substituer la priorité par défaut de AND sur OR, vous pouvez mettre certaines conditions entre parenthèses dans le volet SQL. Les conditions entre parenthèses sont toujours évaluées en premier. Par exemple, pour rechercher tous les employés qui travaillent dans la société depuis plus de cinq ans avec un niveau de qualification faible ou moyen, vous pouvez créer la clause WHERE suivante :  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
> [!TIP]  
> Par souci de clarté, nous vous conseillons d'inclure systématiquement des parenthèses lorsque vous combinez des clauses AND et OR, plutôt que de compter uniquement sur la priorité par défaut.  
  
## <a name="how-and-works-with-multiple-or-clauses"></a>Utilisation de AND avec plusieurs clauses OR  
La maîtrise des relations entre les clauses AND et OR facilitera la création et la compréhension de requêtes complexes dans le Concepteur de requêtes et de vues.  
  
Si vous reliez plusieurs conditions avec AND, le premier groupe de conditions reliées par AND s'applique à toutes les conditions du deuxième groupe. Autrement dit, une condition reliée par AND à une autre condition est distribuée à toutes les conditions du second groupe. Par exemple, dans la représentation schématique suivante, une condition AND est reliée à un groupe de conditions OR :  
  
```  
A AND (B OR C)  
```  
  
La représentation précédente équivaut logiquement à la représentation schématique suivante, qui montre bien comment la condition AND est distribuée au second groupe de conditions :  
  
```  
(A AND B) OR (A AND C)  
```  
  
Ce principe distributif influe sur la manière dont vous utilisez le Concepteur de requêtes et de vues. Par exemple, supposons que vous recherchiez tous les employés qui travaillent dans la société depuis plus de cinq ans, avec un niveau de qualification faible ou moyen. Vous inclurez alors la clause WHERE suivante dans l'instruction à l'intérieur du volet SQL :  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
La clause reliée par AND s'applique aux deux clauses reliées par OR. La formulation sera plus claire si vous répétez la condition AND pour chaque condition dans la clause OR. L'instruction suivante équivaut à la précédente, mais elle est plus explicite (et plus longue) :  
  
```  
WHERE    (hire_date < '01/01/95' ) AND  
  (job_lvl = 100) OR   
  (hire_date < '01/01/95' ) AND   
  (job_lvl = 200)  
```  
  
Le principe de la distribution des clauses AND aux clauses reliées par OR s'applique, quel que soit le nombre de conditions individuelles incluses dans l'instruction. Par exemple, supposons que vous vouliez rechercher les employés avec un niveau de qualification supérieur ou moyen, qui travaillent dans la société depuis plus de cinq ans ou qui sont à la retraite. La clause WHERE pourra se présenter de la manière suivante :  
  
```  
WHERE   
   (job_lvl = 200 OR job_lvl = 300) AND  
   (hire_date < '01/01/95' ) OR (status = 'R')  
```  
  
Après la distribution des clauses reliées par AND, la clause WHERE aura l'aspect suivant :  
  
```  
WHERE   
   (job_lvl = 200 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 200 AND status = 'R') OR  
   (job_lvl = 300 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 300 AND status = 'R')  
```  
  
## <a name="how-multiple-and-and-or-clauses-are-represented-in-the-criteria-pane"></a>Représentation de plusieurs clauses AND et OR dans le volet Critères  
Le Concepteur de requêtes et de vues représente vos conditions de recherche dans le [volet Critères](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md). Dans certains cas de figure, cependant, où plusieurs clauses sont reliées par AND et OR, la représentation dans le volet Critères risque de n'être pas conforme à votre attente. En outre, si vous modifiez votre requête dans le volet Critères ou le [volet Schéma](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md), vous constaterez peut-être des différences entre l’instruction SQL que vous avez entrée et celle qui est affichée.  
  
En général, la façon dont les clauses AND et OR apparaissent dans le volet Critères est déterminée par les règles suivantes :  
  
-   Toutes les conditions reliées par AND apparaissent dans la colonne **Filtre** de la grille ou dans la même colonne **Ou...** .  
  
-   Toutes les conditions reliées par OR apparaissent dans des colonnes **Ou...** distinctes.  
  
-   Si le résultat logique d'une combinaison de clauses AND et OR s'obtient en distribuant l'opérateur AND dans plusieurs clauses OR, le volet Critères représente cette distribution de façon explicite en répétant la clause AND autant de fois que nécessaire.  
  
Par exemple, vous pouvez créer dans le volet SQL une condition de recherche similaire à la suivante, où deux clauses reliées par AND sont prioritaires sur une troisième clause reliée par OR :  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  (job_lvl = 100) OR   
  (status = 'R')  
```  
  
Le Concepteur de requêtes et de vues représente cette clause WHERE dans le volet Critères de la manière suivante :  
  
![Précédence de clause OR dans le volet Critères](../../ssms/visual-db-tools/media/vs_criteriapane1.gif "Précédence de clause OR dans le volet Critères")  
  
En revanche, si les clauses liées par OR sont prioritaires sur une clause AND, la clause AND est répétée pour chaque clause OR. Nous dirons qu'elle est distribuée à chaque clause OR. Par exemple, vous pouvez créer dans le volet SQL une clause WHERE du type suivant :  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ( (job_lvl = 100) OR   
  (status = 'R') )  
```  
  
Le Concepteur de requêtes et de vues représente cette clause WHERE dans le volet Critères de la manière suivante :  
  
![Clauses AND et OR multiples dans le volet Critères](../../ssms/visual-db-tools/media/vs_criteriapane2.gif "Clauses AND et OR multiples dans le volet Critères")  
  
Si les clauses liées par OR concernent une seule colonne de données, le Concepteur de requêtes et de vues peut placer l'ensemble de la clause OR dans une seule cellule de la grille, ce qui évite de répéter la clause AND. Par exemple, vous pouvez créer dans le volet SQL une clause WHERE du type suivant :  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ((status = 'R') OR (status = 'A'))  
```  
  
Le Concepteur de requêtes et de vues représente cette clause WHERE dans le volet Critères de la manière suivante :  
  
![Clauses OR liées définies dans le volet Critères](../../ssms/visual-db-tools/media/vs_criteriapane3.gif "Clauses OR liées définies dans le volet Critères")  
  
Si vous apportez des modifications à la requête (en modifiant, par exemple, une valeur dans le volet Critères), le Concepteur de requêtes et de vues recrée l'instruction SQL dans le volet SQL. L'instruction SQL recréée ressemblera à celle qui est affichée dans le volet Critères, et non à votre instruction d'origine. Par exemple, si le volet Critères contient des clauses AND distribuées, l'instruction obtenue dans le volet SQL contiendra des clauses AND distribuées de façon explicite. Pour plus d'informations, consultez « Utilisation de AND avec plusieurs clauses OR », plus haut dans cette rubrique.  
  
## <a name="see-also"></a>Voir aussi  
[Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
