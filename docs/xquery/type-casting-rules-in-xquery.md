---
title: Règles de conversion de types dans XQuery | Microsoft Docs
description: En savoir plus sur les règles appliquées lors de la conversion explicite ou implicite d’un type de données à un autre dans XQuery.
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
- XQuery, type casting
- casting rules [XML in SQL Server]
- explicit casting [SQL Server]
- type casting rules [XML in SQL Server]
- cast as operator
- implicit casting
ms.assetid: f2e91306-2b1b-4e1c-b6d8-a34fb9980057
author: rothja
ms.author: jroth
ms.openlocfilehash: 0cd4a68b21ae23b60c2bea5618308c0d13300cab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353268"
---
# <a name="type-casting-rules-in-xquery"></a>Règles de conversion de types dans XQuery
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Le diagramme des spécifications suivant, retraçant les fonctions et les opérateurs XQuery 1.0 et XPath 2.0 définis par le W3C, illustre les types de données intégrés. Parmi ces types, on retrouve les types intégrés de primitives et de dérivées.  
  
 ![Hiérarchie de type XQuery 1.0](../xquery/media/xquery-typing-rules.gif "Hiérarchie de type XQuery 1.0")  
  
 Cette rubrique décrit donc les règles de conversion de types applicables suivant une des deux méthodes suivantes :  
  
-   Cast explicite que vous effectuez en utilisant **cast as** ou les fonctions de constructeur de type (par exemple, `xs:integer("5")` ).  
  
-   la conversion implicite qui a lieu pendant la promotion de type à un rang supérieur.  
  
## <a name="explicit-casting"></a>Conversion explicite  
 Le tableau suivant présente la conversion de types autorisée entre les différents types de primitives intégrés.  
  
 ![Décrit les règles de conversion pour XQuery.](../xquery/media/casting-builtin-types.gif "Décrit les règles de conversion pour XQuery.")  
  
-   Un type de primitive intégré peut se convertir en un autre type de primitive intégré en respectant les règles en vigueur sur les données de la table.  
  
-   Un type de primitive peut également se convertir en tout autre type dérivé de ce type de primitive. Par exemple, vous pouvez effectuer un cast de **XS : Decimal** en **XS : Integer** ou de **XS : Decimal** en **XS : long**.  
  
-   Un type dérivé peut être converti en tout autre type dont il constitue l'ancêtre dans la hiérarchie des types, et ce jusqu'à son type de primitive de base intégré. Par exemple, vous pouvez effectuer un cast de **XS : Token** en **XS : normalizedString** ou en **XS : String**.  
  
-   Un type dérivé peut également être converti en type de primitive si l'ancêtre de sa primitive peut aussi être converti dans le type cible. Par exemple, vous pouvez convertir **XS : Integer**, un type dérivé, en un type **XS : String**, primitif, car **XS : Decimal**, **XS : Integer**, ancêtre primitif, peut être converti en **XS : String**.  
  
-   Un type dérivé peut également être converti en autre type dérivé si l'ancêtre de la primitive du type source peut aussi être converti en ancêtre de primitive du type cible. Par exemple, vous pouvez effectuer un cast de **XS : Integer** en **XS : Token**, car vous pouvez effectuer un cast de **XS : Decimal** en **XS : String**.  
  
-   Les règles pour la conversion de types définis par l'utilisateur en types intégrés revient au même que pour la conversion de types intégrés. Par exemple, vous pouvez définir un type **myInteger** dérivé du type **XS : Integer** . Ensuite, **myInteger** peut être converti en **XS : Token**, car **XS : Decimal** peut être converti en **XS : String**.  
  
 Les types de conversion suivants ne sont pas pris en charge :  
  
-   La conversion mettant en œuvre les types de liste n'est pas autorisée. Cela comprend les types de listes définies par l’utilisateur et les types de listes intégrés tels que **XS : IDREFS**, **XS : Entities** et **XS : NMTOKENS**.  
  
-   Le cast vers ou à partir de **XS : QName** n’est pas pris en charge.  
  
-   **XS : notation** et les sous-types entièrement ordonnés de Duration, **xdt : yearMonthDuration** et **xdt : dayTimeDuration**, ne sont pas pris en charge. C'est pour cela que la conversion mettant en œuvre ces types n'est pas prise en charge.  
  
 Les exemples suivants illustrent la conversion explicite de types.  
  
### <a name="example-a"></a>Exemple A  
 L'exemple suivant interroge une variable de type xml. Elle renvoie ainsi une séquence de valeurs de type simple typé en xs:string.  
  
```  
declare @x xml  
set @x = '<e>1</e><e>2</e>'  
select @x.query('/e[1] cast as xs:string?')  
go  
```  
  
### <a name="example-b"></a>Exemple B  
 L'exemple suivant interroge une variable xml typé. Nous procédons ainsi dans un premier temps à la création d'une collection de schémas XML. Nous utilisons ensuite cette collection afin de créer à son tour une variable XML typé. Le schéma fournit alors les informations de typage pour l'instance XML affectée à la variable. Des requêtes sont ensuite lancées sur la variable.  
  
```  
create xml schema collection myCollection as N'  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  
      <xs:element name="root">  
            <xs:complexType>  
                  <xs:sequence>  
                        <xs:element name="A" type="xs:string"/>  
                        <xs:element name="B" type="xs:string"/>  
                        <xs:element name="C" type="xs:string"/>  
                  </xs:sequence>  
            </xs:complexType>  
      </xs:element>  
</xs:schema>'  
go  
```  
  
 La requête suivante renvoie une erreur statique, car vous ne savez pas combien `root` de <> éléments de niveau supérieur se trouvent dans l’instance de document.  
  
```  
declare @x xml(myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
          <root><A>4</A><B>5</B><C>6</baz></C>'  
select @x.query('/root/A cast as xs:string?')  
go  
```  
  
 En spécifiant un <Singleton `root`> élément dans l’expression, la requête est réussie. Elle renvoie ainsi une séquence de valeurs de type simple typé en xs:string.  
  
```  
declare @x xml(myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
              <root><A>4</A><B>5</B><C>6</C></root>'  
select @x.query('/root[1]/A cast as xs:string?')  
go  
```  
  
 Dans l'exemple suivant, la variable de type xml inclut un mot clé document précisant la collection de schémas XML. Cela indique que l'instance XML doit être un document ne possédant qu'un seul élément de premier niveau. Si vous créez deux <`root`> éléments dans l’instance XML, une erreur est retournée.  
  
```  
declare @x xml(document myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
              <root><A>4</A><B>5</B><C>6</C></root>'  
go  
```  
  
 Vous pouvez modifier l'instance afin de n'inclure qu'un seul élément de premier niveau et faire fonctionner ainsi la requête. Elle renvoie donc une séquence de valeurs de type simple typé en xs:string.  
  
```  
declare @x xml(document myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>'  
select @x.query('/root/A cast as xs:string?')  
go  
```  
  
## <a name="implicit-casting"></a>Conversion implicite  
 La conversion implicite n'est autorisée que pour les types numériques et les types atomiques non typés. Par exemple, la fonction **min ()** suivante retourne la valeur minimale des deux valeurs suivantes :  
  
```  
min(xs:integer("1"), xs:double("1.1"))  
```  
  
 Dans cet exemple, les deux valeurs transmises à la fonction XQuery **min ()** sont de types différents. Par conséquent, la conversion implicite est effectuée où le type **entier** est promu en **double** et les deux valeurs **doubles** sont comparées.  
  
 La promotion de type telle qu'elle est décrite dans cet exemple suit les règles suivantes :  
  
-   Un type numérique dérivé peut être promu en son type de base. Par exemple, l' **entier** peut être promu au **format décimal**.  
  
-   Un **nombre décimal** peut être promu en **float** et un **float** peut être promu en **double**.  
  
 La conversion implicite n'étant autorisée que pour les types numériques, ce qui suit n'est pas possible :  
  
-   La conversion implicite mettant en œuvre les types de chaîne (string) n'est pas autorisée. Par exemple, si deux types de **chaînes** sont attendus et que vous transmettez une **chaîne** et un **jeton**, aucune conversion implicite ne se produit et une erreur est retournée.  
  
-   La conversion implicite des types numériques en types de chaînes n'est pas non plus autorisée. Ainsi, si vous transmettez une valeur de type integer à une fonction s'attendant à un paramètre de type string (chaîne), là encore la conversion implicite ne peut pas se produire et entraîne une erreur.  
  
## <a name="casting-values"></a>Conversion de valeurs  
 Lors de la conversion d'un type vers un autre, les valeurs sont en fait transformées de l'espace de valeurs du type source à celui du type cible. Par exemple, la conversion de xs:decimal en xs:double transforme la valeur décimale en valeur double.  
  
 Voici quelques-unes des règles qui s'appliquent lors de la transformation.  
  
##### <a name="casting-a-value-from-a-string-or-untypedatomic-type"></a>Conversion d'une valeur de type string ou de type untypedAtomic  
 Une valeur convertie en un type string (chaîne) ou untypedAtomic (atomique non typé) est transformée comme si elle faisait l'objet d'une validation suivant les règles du type cible. Cela inclut les règles éventuelles de traitement des modèles et des espaces. Par exemple, l'instruction suivante fonctionne et génère ainsi une valeur de type double (1.1e0) :  
  
 `xs:double("1.1")`  
  
 Lors de la conversion vers des types binaires tels que xs:base64Binary ou xs:hexBinary à partir d'un type string (chaîne) ou untypedAtomic (atomique non typé), la valeur d'entrée doit être encodée respectivement au format base64 ou HEX.  
  
##### <a name="casting-a-value-to-a-string-or-untypedatomic-type"></a>Conversion d'une valeur en valeur de type string ou untypedAtomic  
 La conversion vers un type string (chaîne) ou untypedAtomic (atomique non typé) transforme la valeur en sa représentation lexicale canonique XQuery. Plus précisément, on peut dire qu'une valeur ayant répondu à un modèle précis ou à une autre contrainte lors de sa saisie pourrait ne pas être représentée en fonction de ladite contrainte.  Pour informer les utilisateurs de cela, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] signale les types où la contrainte de type peut être un problème en fournissant un avertissement lorsque ces types sont chargés dans la collection de schémas.  
  
 Lors de la conversion d'une valeur de type xs:float ou xs:double, ou encore de l'un de leurs sous-types, en une valeur de type string ou untypedAtomic, ladite valeur est alors représentée sous sa forme scientifique. Cette opération n'est effectuée que si la valeur absolue de la donnée est inférieure à 1,0E-6 ou supérieure ou égale à 1,0E6. Dans ce cas, 0 est sérialisé sous sa forme scientifique de 0,0E0.  
  
 Par exemple, `xs:string(1.11e1)` renvoie la valeur de type chaîne `"11.1"`, alors que `xs:string(-0.00000000002e0)` renvoie la valeur de type chaîne `"-2.0E-11"`.  
  
 Lors de la conversion de types binaires tels que xs:base64Binary ou xs:hexBinary en un type string (chaîne) ou untypedAtomic (atomique non typé), les valeurs binaires sont alors respectivement encodées au format base64 ou HEX.  
  
##### <a name="casting-a-value-to-a-numeric-type"></a>Conversion d'une valeur en valeur de type numérique  
 Lors de la conversion d'une valeur d'un des types numériques en une valeur d'un autre type numérique, la valeur est alors mappée d'un espace de valeurs à l'autre sans passer par la sérialisation par chaîne. Si elle ne satisfait pas la contrainte relative au type cible, les règles suivantes s'appliquent alors :  
  
-   Si la valeur source est déjà de type numérique et le type cible est xs:float ou un de ses sous-types acceptant les valeurs -INF ou INF, et si la conversion de la valeur numérique source provoque un dépassement de capacité, la valeur est alors mappée à INF si elle est positive ou à -INF si elle est négative. Si le type cible n'accepte pas ces valeurs INF ou -INF entraînant ainsi un dépassement de capacité, la conversion échoue et le résultat (dans le cas de cette version de SQL Server) correspond ainsi à la séquence vide.  
  
-   Si la valeur source est déjà de type numérique et le type cible est un type numérique acceptant les valeurs 0, -0e0 ou 0e0 dans sa plage de valeurs, si la conversion de la valeur numérique source provoque un dépassement de capacité, la valeur est mappée selon les cas suivants :  
  
    -   La valeur est mappée à 0 pour un type cible décimal.  
  
    -   La valeur est mappée à -0e0 si la valeur correspond à un dépassement de capacité négatif.  
  
    -   La valeur est mappée sur 0e0 si la valeur correspond à un dépassement de capacité positif pour un type float ou double.  
  
     Si le type cible n'inclut pas le zéro dans son espace de valeurs, la conversion échoue et le résultat est alors la séquence vide.  
  
     Remarque : il se peut que la conversion d'une valeur en un type à virgule flottante binaire, tel que xs:float, xs:double ou un de leurs sous-types, entraîne une perte de précision.  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La valeur à virgule flottante NaN n'est pas prise en charge.  
  
-   Les valeurs convertibles sont limitées par les restrictions d'implémentation des types de cibles. Par exemple, vous ne pouvez pas convertir une chaîne de date avec une année négative en **XS : date**. De telles conversions génèreront la séquence vide si la valeur est fournie au moment de l'exécution (au lieu de déclencher une erreur d'exécution).  
  
## <a name="see-also"></a>Voir aussi  
 [Définir la sérialisation des données XML](../relational-databases/xml/define-the-serialization-of-xml-data.md)  
  
  
