---
title: Expressions conditionnelles (XQuery) | Microsoft Docs
description: En savoir plus sur les expressions conditionnelles prises en charge par XQuery.
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
- XQuery, conditional expressions
- expressions [XQuery], conditional
- effective Boolean value [XQuery]
- if-then-else statement [XQuery]
- conditional expressions [XQuery]
- EBV
ms.assetid: b280dd96-c80f-4c51-bc06-a88d42174acb
author: rothja
ms.author: jroth
ms.openlocfilehash: 5fdc8703482f570d0ecb749b471af170b62770ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349374"
---
# <a name="conditional-expressions-xquery"></a>Expressions conditionnelles (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery prend en charge l’instruction **if-then-else** suivante :  
  
```  
if (<expression1>)  
then  
  <expression2>  
else  
  <expression3>  
```  
  
 Suivant la valeur booléenne effective de `expression1`, `expression2` ou `expression3` est évaluée. Par exemple :  
  
-   Si l'expression de test, `expression1`, aboutit à une séquence vide, le résultat est False.  
  
-   Si l'expression de test, `expression1`, aboutit à une valeur booléenne simple, cette valeur est le résultat de l'expression.  
  
-   Si l'expression de test, `expression1`, aboutit à une séquence d'un ou plusieurs nœuds, le résultat de l'expression est True.  
  
-   Dans le cas contraire, une erreur statique est générée.  
  
 En outre, notez les points suivants :  
  
-   L'expression de test doit figurer entre parenthèses.  
  
-   L’expression **else** est obligatoire. Si vous n'en avez pas besoin, vous pouvez renvoyer « ( ) », comme le montrent les exemples de cette rubrique.  
  
 Par exemple, la requête suivante est spécifiée par rapport à la variable de type **XML** . La condition **If** teste la valeur de la variable SQL ( @v ) à l’intérieur de l’expression XQuery à l’aide de la fonction d’extension de [fonction SQL : variable ()](../xquery/xquery-extension-functions-sql-variable.md) . Si la valeur de la variable est « FirstName », elle retourne l' `FirstName` élément <>. Sinon, elle retourne l' `LastName` élément <>.  
  
```  
declare @x xml  
declare @v varchar(20)  
set @v='FirstName'  
set @x='  
<ROOT rootID="2">  
  <FirstName>fname</FirstName>  
  <LastName>lname</LastName>  
</ROOT>'  
SELECT @x.query('  
if ( sql:variable("@v")="FirstName" ) then  
  /ROOT/FirstName  
 else  
   /ROOT/LastName  
')  
```  
  
 Voici le résultat obtenu :  
  
```  
<FirstName>fname</FirstName>  
```  
  
 La requête suivante extrait les deux premières descriptions de fonctionnalités de la description de catalogue de produit d'un modèle de produit spécifique. S’il y a plus de fonctionnalités dans le document, il ajoute un <`there-is-more` élément> avec un contenu vide.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /p1:ProductDescription/@ProductModelID }  
          { /p1:ProductDescription/@ProductModelName }   
          {  
            for $f in /p1:ProductDescription/p1:Features/*[position()\<=2]  
            return  
            $f   
          }  
          {  
            if (count(/p1:ProductDescription/p1:Features/*) > 2)  
            then \<there-is-more/>  
            else ()  
          }   
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Dans la requête précédente, la condition dans l’expression **If** vérifie s’il existe plus de deux éléments enfants dans <`Features`>. Si tel est le cas, elle renvoie l'élément `\<there-is-more/>` dans le résultat.  
  
 Voici le résultat obtenu :  
  
```  
<Product ProductModelID="19" ProductModelName="Mountain 100">  
  \<p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    \<p1:WarrantyPeriod>3 years\</p1:WarrantyPeriod>  
    \<p1:Description>parts and labor\</p1:Description>  
  \</p1:Warranty>  
  \<p2:Maintenance xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    \<p2:NoOfYears>10 years\</p2:NoOfYears>  
    \<p2:Description>maintenance contract available through your dealer or any AdventureWorks retail store.\</p2:Description>  
  \</p2:Maintenance>  
  \<there-is-more />  
</Product>  
```  
  
 Dans la requête suivante, un `Location` élément <> avec un attribut LocationID est retourné si l’emplacement du poste de travail ne spécifie pas les heures d’installation.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        for $WC in //AWMI:root/AWMI:Location  
        return  
        if ( $WC[not(@SetupHours)] )  
        then  
          <WorkCenterLocation>  
             { $WC/@LocationID }   
          </WorkCenterLocation>  
         else  
           ()  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Voici le résultat obtenu :  
  
```  
<WorkCenterLocation LocationID="30" />  
<WorkCenterLocation LocationID="45" />  
<WorkCenterLocation LocationID="60" />  
```  
  
 Cette requête peut être écrite sans la clause **If** , comme illustré dans l’exemple suivant :  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in //AWMI:root/AWMI:Location[not(@SetupHours)]   
        return  
          <Location>  
             { $WC/@LocationID }   
          </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
