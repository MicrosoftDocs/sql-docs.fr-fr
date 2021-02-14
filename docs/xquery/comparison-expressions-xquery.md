---
title: Expressions de comparaison (XQuery) | Microsoft Docs
description: Découvrez comment utiliser des expressions de comparaison XQuery qui contiennent des opérateurs de comparaison général, de valeur, de nœud et d’ordre de nœud.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- node comparison operators [XQuery]
- comparison expressions [XQuery]
- node order comparison operators [XQuery]
- expressions [XQuery], comparison
- comparison operators [XQuery]
- value comparison operators
ms.assetid: dc671348-306f-48ef-9e6e-81fc3c7260a6
author: rothja
ms.author: jroth
ms.openlocfilehash: 4824cd52001305a05c00e197e8c4140598267d4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349344"
---
# <a name="comparison-expressions-xquery"></a>Expressions de comparaison (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery fournit les types d'opérateurs de comparaison suivants :  
  
-   Opérateurs de comparaison générale  
  
-   Opérateurs de comparaison de valeurs  
  
-   Opérateurs de comparaison de nœuds  
  
-   Opérateurs de comparaison de l’ordre des nœuds  
  
## <a name="general-comparison-operators"></a>Opérateurs de comparaison générale  
 Les opérateurs de comparaison générale peuvent être utilisés pour comparer des valeurs atomiques, des séquences ou une combinaison des deux.  
  
 Le tableau suivant énumère les opérateurs généraux.  
  
|Opérateur|Description|  
|--------------|-----------------|  
|=|Égal à|  
|!=|Différent de|  
|\<|Inférieur à|  
|>|Supérieur à|  
|\<=|Inférieur ou égal à|  
|>=|Supérieur ou égal à|  
  
 Lorsque vous comparez deux séquences à l'aide des opérateurs de comparaison générale et qu'il existe, dans la deuxième séquence, une valeur qui compare True à une valeur de la première séquence, le résultat global est True. Dans le cas contraire, le résultat est False. Par exemple, (1, 2, 3) = (3, 4) est True car la valeur 3 apparaît dans les deux séquences.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2,3) = (3,4)')    
```  
  
 La comparaison s'attend à ce que les valeurs soient de types comparables. Ces derniers sont d'ailleurs vérifiés de manière statique. En cas de comparaisons numériques, la promotion du type numérique peut se produire. Par exemple, si une valeur décimale de 10 est comparée à une valeur double précision de 1e1, la valeur décimale est convertie en valeur double précision. Sachez que cela peut entraîner des résultats erronés puisque les comparaisons double précision ne peuvent pas être exactes.  
  
 Si l'une des valeurs est non typée, elle est convertie dans le type de l'autre valeur. Dans l'exemple suivant, la valeur 7 est traitée comme un entier. Avant la comparaison, la valeur non typée de /a[1] est convertie en entier. La comparaison des entiers renvoie la valeur True.  
  
```  
declare @x xml  
set @x='<a>6</a>'  
select @x.query('/a[1] < 7')  
```  
  
 Inversement, si la valeur non typée est comparée à une chaîne ou à une autre valeur non typée, elle sera convertie en xs:string. Dans la requête suivante, la chaîne 6 est comparée à la chaîne "17". La requête suivante renvoie la valeur False suite à la comparaison des chaînes.  
  
```  
declare @x xml  
set @x='<a>6</a>'  
select @x.query('/a[1] < "17"')  
```  
  
 La requête suivante renvoie les illustrations petit format d'un modèle de produit à partir du catalogue de produits fourni dans l'exemple de base de données AdventureWorks. La requête compare une séquence de valeurs atomiques renvoyée par `PD:ProductDescription/PD:Picture/PD:Size` à une séquence singleton, "small". Si la comparaison a la valeur true, elle retourne l' \> élément image <.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
SELECT CatalogDescription.query('         
    for $P in /PD:ProductDescription/PD:Picture[PD:Size = "small"]         
    return $P') as Result         
FROM   Production.ProductModel         
WHERE  ProductModelID=19         
```  
  
 La requête suivante compare une séquence de numéros de téléphone dans <\> éléments Number au littéral de chaîne « 112-111-1111 ». La requête compare la séquence des éléments numéro de téléphone de la colonne AdditionalContactInfo pour déterminer s'il existe dans le document un numéro de téléphone spécifique pour un client spécifique.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.value('         
   /aci:AdditionalContactInfo//act:telephoneNumber/act:number = "112-111-1111"', 'nvarchar(10)') as Result         
FROM Person.Contact         
WHERE ContactID=1         
```  
  
 La requête renvoie la valeur True. ce qui indique que le numéro existe dans le document. La requête suivante est une version légèrement modifiée de la précédente. Dans cette requête, les valeurs de numéro de téléphone récupérées à partir du document sont comparées à une séquence de deux valeurs de numéro de téléphone. Si la comparaison est true, l’élément <Number \> est retourné.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.query('         
  if (/aci:AdditionalContactInfo//act:telephoneNumber/act:number = ("222-222-2222","112-111-1111"))         
  then          
     /aci:AdditionalContactInfo//act:telephoneNumber/act:number         
  else         
    ()') as Result         
FROM Person.Contact         
WHERE ContactID=1  
  
```  
  
 Voici le résultat obtenu :  
  
```  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
    111-111-1111  
\</act:number>  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
    112-111-1111  
\</act:number>   
```  
  
## <a name="value-comparison-operators"></a>Opérateurs de comparaison de valeurs  
 Les opérateurs de comparaison de valeurs servent à comparer des valeurs atomiques. Notez que vous pouvez utiliser des opérateurs de comparaison générale à la place des opérateurs de comparaison de valeurs dans vos requêtes.  
  
 Le tableau suivant énumère les opérateurs de comparaison de valeurs.  
  
|Opérateur|Description|  
|--------------|-----------------|  
|eq|Égal à|  
|ne|Différent de|  
|lt|Inférieur à|  
|gt|Supérieur à|  
|le|Inférieur ou égal à|  
|ge|Supérieur ou égal à|  
  
 Si les deux valeurs sont identiques selon l'opérateur choisi, l'expression renvoie la valeur True. Dans le cas contraire, elle retourne la valeur False. Si l'une des valeurs est une séquence vide, le résultat de l'expression est False.  
  
 Ces opérateurs fonctionnent uniquement sur des valeurs atomiques singleton. Autrement dit, vous ne pouvez pas spécifier une séquence en tant qu'opérande.  
  
 Par exemple, la requête suivante récupère \<Picture> des éléments pour un modèle de produit où la taille de l’image est «petite :  
  
```  
SELECT CatalogDescription.query('         
              declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";         
              for $P in /PD:ProductDescription/PD:Picture[PD:Size eq "small"]         
              return         
                    $P         
             ') as Result         
FROM Production.ProductModel         
WHERE ProductModelID=19         
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   `declare namespace` définit le préfixe d'espace de noms qui est utilisé par la suite dans la requête.  
  
-   La \<Size> valeur de l’élément est comparée à la valeur atomique spécifiée, « Small ».  
  
-   Notez que, étant donné que les opérateurs de valeur fonctionnent uniquement sur les valeurs atomiques, la fonction **Data ()** est implicitement utilisée pour récupérer la valeur du nœud. Autrement dit, `data($P/PD:Size) eq "small"` donne le même résultat.  
  
 Voici le résultat obtenu :  
  
```  
\<PD:Picture   
  xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
  \<PD:Angle>front\</PD:Angle>  
  \<PD:Size>small\</PD:Size>  
  \<PD:ProductPhotoID>31\</PD:ProductPhotoID>  
\</PD:Picture>  
```  
  
 Notez que les règles de promotion de type sont identiques qu'il s'agisse d'opérateur de comparaison générale ou de valeurs. De plus, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] applique les mêmes règles de conversion pour les valeurs non typées au cours des comparaisons, qu'elles portent sur des valeurs ou soient générales. En revanche, les règles de la spécification XQuery convertissent toujours la valeur non typée en xs:string lors des comparaisons de valeurs.  
  
## <a name="node-comparison-operator"></a>Opérateurs de comparaison de nœuds  
 L’opérateur de comparaison de nœuds, **is**, s’applique uniquement aux types de nœuds. Le résultat renvoyé indique si les deux nœuds transmis comme opérandes représentent le même nœud dans le document source. Cet opérateur renvoie la valeur True si les deux opérandes identifient le même nœud. Dans le cas contraire, il renvoie la valeur False.  
  
 La requête suivante vérifie que le poste de travail 10 est le premier dans le processus de fabrication d'un modèle de produit spécifique.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions' AS AWMI)  
  
SELECT ProductModelID, Instructions.query('         
    if (  (//AWMI:root/AWMI:Location[@LocationID=10])[1]         
          is          
          (//AWMI:root/AWMI:Location[1])[1] )          
    then         
          <Result>equal</Result>         
    else         
          <Result>Not-equal</Result>         
         ') as Result         
FROM Production.ProductModel         
WHERE ProductModelID=7           
```  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID       Result          
-------------- --------------------------  
7              <Result>equal</Result>      
```  
  
## <a name="node-order-comparison-operators"></a>Opérateurs de comparaison de l'ordre des nœuds  
 Les opérateurs de comparaison de l'ordre des nœuds comparent des paires de nœuds en fonction de la position qu'ils occupent dans un document.  
  
 Ces comparaisons sont faites en fonction de l'ordre du document :  
  
-   `<<` : L' **opérande 1** précède l' **opérande 2** dans l’ordre du document.  
  
-   `>>` : L' **opérande 1** suit l' **opérande 2** dans l’ordre du document.  
  
 La requête suivante retourne la valeur true si la description du catalogue de produits contient l' \<Warranty> élément qui apparaît avant l' \<Maintenance> élément dans l’ordre du document pour un produit particulier.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS WM)  
  
SELECT CatalogDescription.value('  
     (/PD:ProductDescription/PD:Features/WM:Warranty)[1] <<   
           (/PD:ProductDescription/PD:Features/WM:Maintenance)[1]', 'nvarchar(10)') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La méthode **value ()** du type de données **XML** est utilisée dans la requête.  
  
-   Le résultat booléen de la requête est converti en **nvarchar (10)** et retourné.  
  
-   La requête renvoie la valeur True.  
  
## <a name="see-also"></a>Voir aussi  
 [Système de type &#40;XQuery&#41;](../xquery/type-system-xquery.md)   
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
