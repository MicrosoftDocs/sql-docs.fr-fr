---
title: Sequence et QNames (XQuery) | Microsoft Docs
description: Découvrez les concepts fondamentaux des séquences et des QNames dans XQuery.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- XQuery, sequence
- QName [XQuery]
- predefined namespaces [XML in SQL Server]
ms.assetid: 3593ac26-dd78-4bf0-bb87-64fbcac5f026
author: rothja
ms.author: jroth
ms.openlocfilehash: 10caff2486f8dae470a4778c6da649b7bba5ddba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339925"
---
# <a name="sequence-and-qnames-xquery"></a>Séquence et QNames (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Cette rubrique décrit les concepts fondamentaux suivants de XQuery :  
  
-   Séquence  
  
-   QNames et espaces de noms prédéfinis  
  
## <a name="sequence"></a>Séquence  
 Dans XQuery, le résultat d'une expression est une séquence composée d'une liste de nœuds XML et d'instances de types atomiques XSD. Une entrée d'une séquence porte le nom d'élément. Un élément d'une séquence peut être :  
  
-   un nœud, tel qu'un élément, attribut, texte, instruction de traitement, commentaire ou document ;  
  
-   une valeur atomique, telle qu'une instance d'un type simple XSD.  
  
 Par exemple, la requête suivante construit une séquence de deux éléments de nœud d'élément :  
  
```  
SELECT Instructions.query('  
     <step1> Step 1 description goes here</step1>,  
     <step2> Step 2 description goes here </step2>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
  
```  
  
 Voici le résultat obtenu :  
  
```  
<step1> Step 1 description goes here </step1>  
<step2> Step 2 description goes here </step2>   
```  
  
 Dans la requête précédente, la virgule (`,`) à la fin de la construction `<step1>` est le constructeur de séquence ; elle est obligatoire. Les espaces dans les résultats sont ajoutés uniquement à titre d'illustration et sont inclus dans tous les résultats d'exemple de cette documentation.  
  
 Voici quelques informations supplémentaires utiles concernant les séquences :  
  
-   Si une requête génère une séquence qui contient une autre séquence, la séquence contenue est aplatie dans la séquence conteneur. Par exemple, la séquence ((1,2, (3,4,5)),6) est aplatie dans le modèle de données sous la forme (1, 2, 3, 4, 5, 6).  
  
    ```  
    DECLARE @x xml;  
    SET @x = '';  
    SELECT @x.query('(1,2, (3,4,5)),6');  
    ```  
  
-   Une séquence vide est une séquence qui ne contient aucun élément. Elle est représentée sous la forme « () ».  
  
-   Une séquence contenant un seul élément peut être traitée comme une valeur atomique, et inversement. À savoir, (1) = 1.  
  
 Dans cette implémentation, la séquence doit être homogène. Autrement dit, vous devez avoir soit une séquence de valeurs atomiques, soit une séquence de nœuds. Par exemple, les séquences suivantes sont valides :  
  
```  
DECLARE @x xml;  
SET @x = '';  
-- Expression returns a sequence of 1 text node (singleton).  
SELECT @x.query('1');  
-- Expression returns a sequence of 2 text nodes  
SELECT @x.query('"abc", "xyz"');  
-- Expression returns a sequence of one atomic value. data() returns  
-- typed value of the node.  
SELECT @x.query('data(1)');  
-- Expression returns a sequence of one element node.   
-- In the expression XML construction is used to construct an element.  
SELECT @x.query('<x> {1+2} </x>');  
```  
  
 La requête suivante retourne une erreur car les séquences hétérogènes ne sont pas prises en charge.  
  
```  
SELECT @x.query('<x>11</x>, 22');  
```  
  
## <a name="qname"></a>QName  
 Chaque identificateur d'une XQuery est un QName. Un QName est constitué d'un préfixe d'espace de noms et d'un nom local. Dans cette implémentation, les noms de variables dans XQuery sont des QNames et ne peuvent pas avoir de préfixes.  
  
 Prenons l’exemple suivant dans lequel une requête est spécifiée par rapport à une variable **XML** non typée :  
  
```  
DECLARE @x xml;  
SET @x = '<Root><a>111</a></Root>';  
SELECT @x.query('/Root/a');  
```  
  
 Dans l'expression (`/Root/a`), `Root` et `a` sont des QNames.  
  
 Dans l’exemple suivant, une requête est spécifiée par rapport à une colonne **XML** typée. La requête effectue une itération sur tous les \<step> éléments au premier emplacement atelier.  
  
```  
SELECT Instructions.query('  
   declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $Step in /AWMI:root/AWMI:Location[1]/AWMI:step  
      return  
           string($Step)   
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 Dans l'expression de requête, notez ce qui suit :  
  
-   `AWMI root`, `AWMI:Location`, `AWMI:step` et `$Step` sont tous des QNames. `AWMI` est un préfixe et `root`, `Location` et `Step` sont tous des noms locaux.  
  
-   La variable `$step` est un QName ; elle n'a pas de préfixe.  
  
 Les espaces de noms suivants sont prédéfinis pour une utilisation avec prise en charge de XQuery dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
|Préfixe|URI|  
|------------|---------|  
|xs|http://www.w3.org/2001/XMLSchema|  
|xsi|http://www.w3.org/2001/XMLSchema-instance|  
|xdt|http://www.w3.org/2004/07/xpath-datatypes|  
|fn|http://www.w3.org/2004/07/xpath-functions|  
|(aucun préfixe)|`urn:schemas-microsoft-com:xml-sql`|  
|sqltypes|`https://schemas.microsoft.com/sqlserver/2004/sqltypes`|  
|Xml|`http://www.w3.org/XML/1998/namespace`|  
|(aucun préfixe)|`https://schemas.microsoft.com/sqlserver/2004/SOAP`|  
  
 Chaque base de données que vous créez possède la collection de schémas XML **sys** . Elle réserve ces schémas de sorte qu'ils soient accessibles à partir de toute collection de schémas XML créée par l'utilisateur.  
  
> [!NOTE]  
>  Cette implémentation ne prend pas en charge le `local` préfixe comme décrit dans la spécification XQuery dans http://www.w3.org/2004/07/xquery-local-functions .  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de XQuery](../xquery/xquery-basics.md)  
  
  
