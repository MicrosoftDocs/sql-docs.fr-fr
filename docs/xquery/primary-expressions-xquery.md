---
title: Expressions primaires (XQuery) | Microsoft Docs
description: En savoir plus sur les expressions primaires XQuery qui incluent des littéraux, des références de variables, des expressions d’élément de contexte, des constructeurs et des appels de fonction.
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
- variable references [XQuery]
- primary expressions [XQuery]
- function calls [XQuery]
- expressions [XQuery], primary
- literals [XQuery]
- context item expressions [XQuery]
ms.assetid: d4183c3e-12b5-4ca0-8413-edb0230cb159
author: rothja
ms.author: jroth
ms.openlocfilehash: 2f474aae3e764224bec01830b42766216a27bc9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339948"
---
# <a name="primary-expressions-xquery"></a>Expressions primaires (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Les expressions primaires XQuery incluent : littéraux, références à une variable, expressions d'élément contextuel, constructeurs et appels de fonction.  
  
## <a name="literals"></a>Littéraux  
 Les littéraux XQuery peuvent être des littéraux numériques ou des chaînes de caractères. Un littéral chaîne peut inclure des références d'entité prédéfinies. Une référence d'entité est une séquence de caractères qui commence par un « et commercial » et qui représente un seul et unique caractère qui aurait autrement une signification syntaxique particulière. Voici quelques exemples de références d'entité prédéfinies pour XQuery.  
  
|Référence d'entité|Représente|  
|----------------------|----------------|  
|`&lt;`|\<|  
|`&gt;`|>|  
|`&amp;`|&|  
|`&quot;`|"|  
|`&apos;`|'|  
  
 Un littéral chaîne peut également comprendre une référence de caractère (référence de style XML à un caractère Unicode) qui est identifiée par un point de code décimal ou hexadécimal. Par exemple, le symbole euro peut être représenté par la référence de caractère « &\# 8364 ; ».  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s'appuie sur XML version 1.0 pour l'analyse.  
  
### <a name="examples"></a>Exemples  
 Les exemples suivants expliquent comment utiliser des littéraux, ainsi que des références d'entité et de caractère.  
  
 Ce code retourne une erreur, car les caractères `<'` et `'>` ont une signification particulière.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <SalaryRange>Salary > 50000 and < 100000</SalaryRange>')  
GO  
```  
  
 Si vous utilisez une référence d'entité à la place, la requête aboutit.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <SalaryRange>Salary &gt; 50000 and &lt; 100000</SalaryRange>')  
GO  
```  
  
 L'exemple suivant montre comment utiliser une référence de caractère pour représenter le symbole Euro.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <a>€12.50</a>')  
```  
  
 Voici l'ensemble de résultats.  
  
 `<a>€12.50</a>`  
  
 Dans l'exemple suivant, la requête est délimitée par des apostrophes. Par conséquent, l'apostrophe de la valeur de chaîne doit donc être doublée.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query('<a>I don''t know</a>')  
Go  
```  
  
 Voici l'ensemble de résultats.  
  
 `<a>I don't know</a>`  
  
 Les fonctions booléennes intégrées, **true ()** et **false ()**, peuvent être utilisées pour représenter des valeurs booléennes, comme indiqué dans l’exemple suivant.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query('<a>{true()}</a>')  
GO  
```  
  
 Le constructeur direct d'élément spécifie une expression entre accolades, qui est remplacée par sa valeur dans le code XML qui en résulte.  
  
 Voici l'ensemble de résultats.  
  
 `<a>true</a>`  
  
## <a name="variable-references"></a>Références à une variable  
 Dans XQuery, les références à une variable se présentent sous la forme d'un QName précédé d'un signe $. Cette mise en œuvre ne prend en charge que les références faites à des variables non préfixées. Par exemple, la requête suivante définit la variable `$i` dans l'expression FLWOR.  
  
```  
DECLARE @var XML  
SET @var = '<root>1</root>'  
SELECT @var.query('  
 for $i in /root return data($i)')  
GO  
```  
  
 La requête suivante ne fonctionnera pas car un préfixe d'espace de noms est ajouté au nom de la variable.  
  
```  
DECLARE @var XML  
SET @var = '<root>1</root>'  
SELECT @var.query('  
DECLARE namespace x="https://X";  
for $x:i in /root return data($x:i)')  
GO  
```  
  
 Vous pouvez utiliser la fonction d’extension SQL : variable () pour faire référence à des variables SQL, comme illustré dans la requête suivante.  
  
```  
DECLARE @price money  
SET @price=2500  
DECLARE @x xml  
SET @x = ''  
SELECT @x.query('<value>{sql:variable("@price") }</value>')  
```  
  
 Voici l'ensemble de résultats.  
  
 `<value>2500</value>`  
  
#### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limites de la mise en œuvre sont les suivantes :  
  
-   Les variables dotées d'un préfixe d'espace de noms ne sont pas prises en charge.  
  
-   L'importation de modules n'est pas prise en charge.  
  
-   Les déclarations de variable externe ne sont pas prises en charge. Une solution consiste à utiliser la [fonction SQL : variable ()](../xquery/xquery-extension-functions-sql-variable.md).  
  
## <a name="context-item-expressions"></a>Expressions d'élément contextuel  
 L'élément contextuel est l'élément en cours de traitement dans le cadre d'une expression de chemin d'accès. Il est initialisé dans une instance non-NULL de type XML avec le nœud de document. Elle peut également être modifiée par la méthode nodes (), dans le contexte des expressions XPath ou des prédicats [].  
  
 L'élément contextuel est renvoyé par une expression contenant un point (.). Par exemple, la requête suivante évalue chaque élément <`a`> pour la présence de l’attribut `attr` . Si l'attribut est présent, l'élément est renvoyé. Notez que la condition exprimée dans le prédicat indique que le nœud de contexte est spécifié par un simple point.  
  
```  
DECLARE @var XML  
SET @var = '<ROOT>  
<a>1</a>  
<a attr="1">2</a>  
</ROOT>'  
SELECT @var.query('/ROOT[1]/a[./@attr]')  
```  
  
 Voici l'ensemble de résultats.  
  
 `<a attr="1">2</a>`  
  
## <a name="function-calls"></a>Appels de fonction  
 Vous pouvez appeler les fonctions XQuery intégrées et les [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fonctions SQL : variable () et SQL : Column (). Pour obtenir la liste des fonctions implémentées, consultez [fonctions XQuery sur le type de données XML](../xquery/xquery-functions-against-the-xml-data-type.md).  
  
#### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limites de la mise en œuvre sont les suivantes :  
  
-   Il est impossible de déclarer une fonction dans le prologue XQuery.  
  
-   L'importation de fonctions n'est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Construction XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)
 
