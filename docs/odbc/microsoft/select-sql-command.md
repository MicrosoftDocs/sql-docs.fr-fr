---
description: SELECT, commande SQL
title: Commande SELECT-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- select [ODBC]
ms.assetid: 2149c3ca-3a71-446d-8d53-3d056e2f301a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fc6ae552bc4e6e8bd681aa3d47ffadeaf81fd0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99153517"
---
# <a name="select---sql-command"></a>SELECT, commande SQL
Récupère des données d’une ou plusieurs tables.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro natif pour cette commande. Pour obtenir des informations spécifiques au pilote, consultez la **section Remarques** sur le pilote.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SELECT [ALL | DISTINCT]  
   [Alias.] Select_Item [AS Column_Name]  
   [, [Alias.] Select_Item [AS Column_Name] ...]   
FROM [DatabaseName!]Table [Local_Alias]  
   [, [DatabaseName!]Table [Local_Alias] ...]   
[WHERE JoinCondition [AND JoinCondition  
...]  
   [AND | OR FilterCondition [AND | OR FilterCondition ...]]]  
[GROUP BY GroupColumn [, GroupColumn ...]]  
[HAVING FilterCondition]  
[UNION [ALL] SELECTCommand]  
[ORDER BY Order_Item [ASC | DESC] [, Order_Item [ASC | DESC] ...]]  
```  
  
## <a name="arguments"></a>Arguments  
  
> [!NOTE]  
>  Une *sous-requête*, référencée dans les arguments suivants, est une sélection dans une sélection et doit être placée entre parenthèses. Vous pouvez avoir jusqu’à deux sous-requêtes au même niveau (non imbriqué) dans la clause WHERE. (Consultez cette section des arguments.) Les sous-requêtes peuvent contenir plusieurs conditions de jointure.  
  
 [TOUS LES &#124; DISTINCTS]   [*Alias*.] *Select_Item* [As *Column_Name*] [, [*alias*.] *Select_Item* [As *Column_Name*]...]  
 La clause SELECT spécifie les champs, les constantes et les expressions qui sont affichés dans les résultats de la requête.  
  
 Par défaut, tous affichent toutes les lignes dans les résultats de la requête.  
  
 DISTINCT exclut les doublons de toutes les lignes des résultats de la requête.  
  
> [!NOTE]  
>  Vous ne pouvez utiliser DISTINCT qu’une seule fois par clause SELECT.  
  
 *Alias*. qualifie les noms d’éléments correspondants. Chaque élément que vous spécifiez avec *Select_Item* génère une colonne des résultats de la requête. Si deux ou plusieurs éléments portent le même nom, incluez l’alias de la table et un point avant le nom de l’élément pour empêcher la duplication des colonnes.  
  
 *Select_Item* spécifie un élément à inclure dans les résultats de la requête. Un élément peut être l’un des éléments suivants :  
  
-   Nom d’un champ d’une table dans la clause FROM.  
  
-   Constante qui spécifie que la même valeur de constante doit apparaître dans chaque ligne des résultats de la requête.  
  
-   Expression qui peut être le nom d’une fonction définie par l’utilisateur.  
  
 **Fonctions définies par l’utilisateur avec SELECT**  
  
 Bien que l’utilisation de fonctions définies par l’utilisateur dans la clause SELECT présente des avantages évidents, vous devez également prendre en compte les restrictions suivantes :  
  
-   La vitesse des opérations effectuées avec SELECT peut être limitée par la vitesse à laquelle ces fonctions définies par l’utilisateur sont exécutées. Les manipulations volumineuses impliquant des fonctions définies par l’utilisateur peuvent être mieux accomplies à l’aide d’API et de fonctions définies par l’utilisateur écrites en C ou en langage assembleur.  
  
-   La seule méthode fiable pour passer des valeurs aux fonctions définies par l’utilisateur appelées à partir de SELECT est la liste d’arguments passée à la fonction lorsqu’elle est appelée.  
  
-   Même si vous expérimentez et découvrez une manipulation censée fonctionner correctement dans une certaine version de FoxPro, il n’y a aucune garantie qu’elle continuera à fonctionner dans les versions ultérieures.  
  
 Outre ces restrictions, les fonctions définies par l’utilisateur sont acceptables dans la clause SELECT. Toutefois, n’oubliez pas que l’utilisation de SELECT peut ralentir les performances.  
  
 Les fonctions de champ suivantes peuvent être utilisées avec un élément SELECT qui est un champ ou une expression impliquant un champ :  
  
-   AVG (*Select_Item*) : calcule la moyenne d’une colonne de données numériques.  
  
-   COUNT (*Select_Item*) : compte le nombre d’éléments sélectionnés dans une colonne. COUNT (*) compte le nombre de lignes dans le résultat de la requête.  
  
-   MIN (*Select_Item*) : détermine la plus petite valeur de *Select_Item* dans une colonne.  
  
-   MAX (*Select_Item*) : détermine la plus grande valeur de *Select_Item* dans une colonne.  
  
-   SUM (*Select_Item*) : additionne une colonne de données numériques.  
  
 Vous ne pouvez pas imbriquer des fonctions de champ.  
  
 COMME *Column_Name*  
 Spécifie l’en-tête d’une colonne dans le résultat de la requête. Cela est utile lorsque *Select_Item* est une expression ou contient une fonction de champ et que vous souhaitez attribuer à la colonne un nom explicite. *Column_Name* peut être une expression, mais ne peut pas contenir de caractères (par exemple, des espaces) qui ne sont pas autorisés dans les noms de champs de table.  
  
 FROM [*DatabaseName*!] *Table* [*Local_Alias*] [, [*DatabaseName*!] *Table* [*Local_Alias*]...]  
 Répertorie les tables qui contiennent les données récupérées par la requête. Si aucune table n’est ouverte, Visual FoxPro affiche la boîte de dialogue **ouvrir** pour vous permettre de spécifier l’emplacement du fichier. Une fois ouverte, la table reste ouverte une fois la requête terminée.  
  
 *DatabaseName*! Spécifie le nom d’une base de données autre que celle spécifiée avec la source de données. Vous devez inclure le nom de la base de données qui contient la table si la base de données n’est pas spécifiée avec la source de données. Insérez le délimiteur du point d’exclamation ( !) après le nom de la base de données et avant le nom de la table.  
  
 *Local_Alias* spécifie un nom temporaire pour la table nommée dans la *table*. Si vous spécifiez un alias local, vous devez utiliser l’alias local à la place du nom de la table dans l’instruction SELECT. L’alias local n’affecte pas l’environnement Visual FoxPro.  
  
 OÙ *joinCondition* [et *joinCondition* ...]    [Et &#124; ou *FilterCondition* [et &#124; ou *FilterCondition* ...]]  
 Indique à Visual FoxPro d’inclure uniquement certains enregistrements dans les résultats de la requête. OÙ est requis pour récupérer des données de plusieurs tables.  
  
 *JoinCondition* spécifie les champs qui lient les tables dans la clause from. Si vous incluez plusieurs tables dans une requête, vous devez spécifier une condition de jointure pour chaque table après le premier.  
  
> [!IMPORTANT]  
>  Tenez compte des informations suivantes lorsque vous créez des conditions de jointure :  
  
-   Si vous incluez deux tables dans une requête et que vous ne spécifiez pas de condition de jointure, chaque enregistrement de la première table est joint à chaque enregistrement de la seconde table à condition que les conditions de filtre soient remplies. Une telle requête peut produire des résultats longs.  
  
-   Soyez prudent lors de la jointure de tables avec des champs vides, car Visual FoxPro correspond à des champs vides. Par exemple, si vous vous joignez à CUSTOMER.ZIP et INVOICE.ZIP et si le client contient 100 codes postaux vides et que la facture contient 400 codes postaux vides, le résultat de la requête contient 40 000 enregistrements supplémentaires résultant des champs vides. Utilisez la fonction **Empty ()** pour éliminer les enregistrements vides du résultat de la requête.  
  
-   Vous devez utiliser l’opérateur AND pour connecter plusieurs conditions de jointure. Chaque condition de jointure se présente sous la forme suivante :  
  
     *FieldName1 comparaison FieldName2*  
  
     *FieldName1* est le nom d’un champ d’une table, *FieldName2* est le nom d’un champ d’une autre table et la *comparaison* est l’un des opérateurs décrits dans le tableau suivant.  
  
|Opérateur|Comparaison|  
|--------------|----------------|  
|=|Égal à|  
|==|Exactement égal à|  
|LIKE|SQL COMME|  
|<>, ! =, #|Différent de|  
|>|Plus de|  
|>=|Supérieur ou égal à|  
|<|Inférieur à|  
|<=|Inférieur ou égal à|  
  
 Lorsque vous utilisez l’opérateur = avec des chaînes, il agit différemment, en fonction de la valeur de SET ANSI. Lorsque l’option SET ANSI est désactivée (OFF), Visual FoxPro traite les comparaisons de chaînes d’une manière familière aux utilisateurs de xbase. Lorsque SET ANSI est défini sur ON, Visual FoxPro suit les normes ANSI pour les comparaisons de chaînes. Pour plus d’informations sur la façon dont Visual FoxPro effectue des comparaisons de chaînes, consultez [SET ANSI](../../odbc/microsoft/set-ansi-command.md) et [exact](../../odbc/microsoft/set-exact-command.md) .  
  
 *FilterCondition* spécifie les critères que les enregistrements doivent remplir pour être inclus dans les résultats de la requête. Vous pouvez inclure autant de conditions de filtre dans une requête que vous le souhaitez, en les connectant à l’aide de l’opérateur AND ou OR. Vous pouvez également utiliser l’opérateur NOT pour inverser la valeur d’une expression logique, ou vous pouvez utiliser **Empty ()** pour rechercher un champ vide. *FilterCondition* peut prendre l’une des formes des exemples suivants :  
  
 **Exemple 1** *FieldName1 comparaison FieldName2*  
  
 `customer.cust_id = orders.cust_id`  
  
 **Exemple 2** *expression de comparaison FieldName*  
  
 `payments.amount >= 1000`  
  
 **Exemple 3** *tableau de comparaison FieldName* All (*sous-requête*)  
  
 `company < ALL ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 Lorsque la condition de filtre inclut ALL, le champ doit répondre à la condition de comparaison pour toutes les valeurs générées par la sous-requête avant que son enregistrement soit inclus dans les résultats de la requête.  
  
 **Exemple 4** nombre de *nom_champ comparaison entre* tout &#124; (*sous-requête*)  
  
 `company < ANY ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 Lorsque la condition de filtre comprend une ou plusieurs, le champ doit répondre à la condition de comparaison pour au moins l’une des valeurs générées par la sous-requête.  
  
 L’exemple suivant vérifie si les valeurs du champ se trouvent dans une plage de valeurs spécifiée :  
  
 **Exemple 5** *fieldName* [not] entre *Start_Range* et *End_Range*  
  
 `customer.postalcode BETWEEN 90000 AND 99999`  
  
 L’exemple suivant vérifie si au moins une ligne répond aux critères de la sous-requête. Lorsque la condition de filtre comprend EXISTs, la condition de filtre prend la valeur true (. T.), sauf si la sous-requête est évaluée sur le jeu vide.  
  
 **Exemple 6** [not] Exists (*sous-requête*)  
  
 `EXISTS ;`  
  
 `(SELECT * FROM orders WHERE customer.postalcode =`  
  
 `orders.postalcode)`  
  
 **Exemple 7** *fieldName* [not] dans *Value_Set*  
  
 `customer.postalcode NOT IN ("98052","98072","98034")`  
  
 Lorsque la condition de filtre inclut dans, le champ doit contenir l’une des valeurs avant que son enregistrement soit inclus dans les résultats de la requête.  
  
 **Exemple 8** *fieldName* [not] in (sous-*requête*)  
  
 `customer.cust_id IN ;`  
  
 `(SELECT orders.cust_id FROM orders WHERE orders.city="Seattle")`  
  
 Ici, le champ doit contenir l’une des valeurs retournées par la sous-requête avant que son enregistrement soit inclus dans les résultats de la requête.  
  
 **Exemple 9** *NomChamp* [non] comme *cExpression*  
  
 `customer.country NOT LIKE "USA"`  
  
 Cette condition de filtre recherche chaque champ qui correspond à *cExpression*. Vous pouvez utiliser le signe de pourcentage (%) et les caractères génériques de soulignement (_) dans le cadre de *cExpression*. Le trait de soulignement représente un seul caractère inconnu dans la chaîne.  
  
 GROUP BY *GroupColumn* [, *GroupColumn* ...]  
 Regroupe les lignes de la requête en fonction des valeurs d’une ou de plusieurs colonnes. *GroupColumn* peut prendre l’une des valeurs suivantes :  
  
-   Nom d’un champ de table normal.  
  
-   Champ qui contient une fonction de champ SQL.  
  
-   Expression numérique qui indique l’emplacement de la colonne dans la table de résultats. (Le numéro de colonne le plus à gauche est 1.)  
  
 HAVING *FilterCondition*  
 Spécifie une condition de filtre que les groupes doivent remplir pour être inclus dans les résultats de la requête. HAVING doit être utilisé avec GROUP BY et peut inclure autant de conditions de filtre que vous le souhaitez, connectées par l’opérateur AND ou OR. Vous pouvez également utiliser NOT pour inverser la valeur d’une expression logique.  
  
 *FilterCondition* ne peut pas contenir de sous-requête.  
  
 Une clause HAVING sans clause GROUP BY se comporte comme une clause WHERE. Vous pouvez utiliser des alias locaux et des fonctions de champ dans la clause HAVING. Utilisez une clause WHERE pour des performances plus rapides si votre clause HAVING ne contient pas de fonctions de champ.  
  
 [UNION [ALL] *SELECTCommand*]  
 Combine les résultats finaux d’une sélection avec les résultats finaux d’une autre sélection. Par défaut, UNION vérifie les résultats combinés et élimine les doublons de lignes. Utilisez des parenthèses pour combiner plusieurs clauses UNION.  
  
 ALL empêche UNION d’éliminer les doublons de lignes des résultats combinés.  
  
 Les clauses UNION suivent les règles suivantes :  
  
-   Vous ne pouvez pas utiliser UNION pour combiner des sous-requêtes.  
  
-   Les deux commandes SELECT doivent avoir le même nombre de colonnes dans le résultat de la requête.  
  
-   Chaque colonne dans les résultats d’une requête SELECT doit avoir le même type de données et la même largeur que la colonne correspondante dans l’autre SELECT.  
  
-   Seule la dernière SELECT peut avoir une clause ORDER BY, qui doit faire référence aux colonnes de sortie par nombre. Si une clause ORDER BY est incluse, elle affecte le résultat complet.  
  
 Vous pouvez également utiliser la clause UNION pour simuler une jointure externe.  
  
 Quand vous joignez deux tables dans une requête, seuls les enregistrements avec des valeurs correspondantes dans les champs de jointure sont inclus dans la sortie. Si un enregistrement de la table parent n’a pas d’enregistrement correspondant dans la table enfant, l’enregistrement dans la table parente n’est pas inclus dans la sortie. Une jointure externe vous permet d’inclure tous les enregistrements de la table parente dans la sortie, ainsi que les enregistrements correspondants dans la table enfant. Pour créer une jointure externe dans Visual FoxPro, vous devez utiliser une commande SELECT imbriquée, comme dans l’exemple suivant :  
  
```  
SELECT customer.company, orders.order_id, orders.emp_id ;  
FROM customer, orders ;  
WHERE customer.cust_id = orders.cust_id ;  
UNION ;  
SELECT customer.company, 0, 0 ;  
FROM customer ;  
WHERE customer.cust_id NOT IN ;  
(SELECT orders.cust_id FROM orders)  
```  
  
> [!NOTE]  
>  Veillez à inclure l’espace qui précède immédiatement chaque point-virgule. Dans le cas contraire, vous recevrez une erreur.  
  
 La section de la commande avant la clause UNION sélectionne les enregistrements des deux tables qui ont des valeurs correspondantes. Les sociétés clientes qui n’ont pas de factures associées ne sont pas incluses. La section de la commande après la clause UNION sélectionne les enregistrements de la table Customer qui n’ont pas d’enregistrements correspondants dans la table Orders.  
  
 En ce qui concerne la deuxième section de la commande, notez les points suivants :  
  
-   L’instruction SELECT dans les parenthèses est traitée en premier. Cette instruction crée une sélection de tous les numéros de client dans la table Orders.  
  
-   La clause WHERE recherche tous les numéros de client de la table Customer qui ne se trouvent pas dans la table Orders. Étant donné que la première section de la commande a fourni à toutes les sociétés ayant un numéro de client dans la table Orders, toutes les sociétés de la table Customer sont désormais incluses dans les résultats de la requête.  
  
-   Étant donné que les structures des tables incluses dans une UNION doivent être identiques, il existe deux espaces réservés dans la deuxième instruction SELECT pour représenter *Orders.order_id* et *Orders.emp_id* à partir de la première instruction SELECT.  
  
    > [!NOTE]  
    >  Les espaces réservés doivent être du même type que les champs qu’ils représentent. Si le champ est un type date, l’espace réservé doit être {//}. Si le champ est un champ de caractères, l’espace réservé doit être la chaîne vide ("").  
  
 ORDER BY *Order_Item* [ASC &#124; desc] [, *Order_Item* [ASC &#124; desc]...]  
 Trie les résultats de la requête en fonction des données d’une ou de plusieurs colonnes. Chaque *Order_Item* doit correspondre à une colonne dans les résultats de la requête et peut avoir l’une des valeurs suivantes :  
  
-   Champ dans une table FROM qui est également un élément SELECT dans la clause principale SELECT (pas dans une sous-requête).  
  
-   Expression numérique qui indique l’emplacement de la colonne dans la table de résultats. (La colonne la plus à gauche est le numéro 1.)  
  
 ASC spécifie un ordre croissant pour les résultats de la requête, en fonction de l’élément de commande ou des éléments, et est la valeur par défaut pour la clause ORDER BY.  
  
 DESC spécifie un ordre décroissant pour les résultats de la requête.  
  
 Les résultats de la requête apparaissent non ordonnés si vous ne spécifiez pas de commande avec ORDER BY.  
  
## <a name="remarks"></a>Notes  
 SELECT est une commande SQL intégrée à Visual FoxPro comme n’importe quelle autre commande Visual FoxPro. Lorsque vous utilisez SELECT pour créer une requête, Visual FoxPro interprète la requête et récupère les données spécifiées des tables. Vous pouvez créer une requête SELECT à partir de la fenêtre d’invite de commandes ou d’un programme Visual FoxPro (comme avec n’importe quelle autre commande Visual FoxPro).  
  
> [!NOTE]  
>  SELECT ne respecte pas la condition de filtre actuelle spécifiée avec SET FILTER.  
  
## <a name="driver-remarks"></a>Remarques sur le pilote  
 Lorsque votre application envoie l’instruction SQL ODBC SELECT à la source de données, le pilote ODBC Visual FoxPro convertit la commande en commande Visual FoxPro SELECT sans traduction, sauf si la commande contient une séquence d’échappement ODBC. Les éléments placés dans une séquence d’échappement ODBC sont convertis en syntaxe Visual FoxPro. Pour plus d’informations sur l’utilisation des séquences d’échappement ODBC, consultez [fonctions d’heure et de date](../../odbc/microsoft/time-and-date-functions-visual-foxpro-odbc-driver.md) et dans le *Guide de référence du programmeur Microsoft ODBC*, consultez [séquences d’échappement dans ODBC](../../odbc/reference/develop-app/escape-sequences-in-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE-SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INSERT-SQL](../../odbc/microsoft/insert-sql-command.md)   
 [DÉFINIR ANSI](../../odbc/microsoft/set-ansi-command.md)   
 [DÉFINIR COMME EXACT](../../odbc/microsoft/set-exact-command.md)
