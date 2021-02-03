---
description: replace value of (XML DML)
title: replace value of (XML DML)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- XML DML [SQL Server]
- update keyword
- replacement values [XML DML]
- updating node values
- replace value of XML DML statement
ms.assetid: c310f6df-7adf-493b-b56b-8e3143b13ae7
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6882fa93bb1d4837f909bc00f74fdf9e83e7c5c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174392"
---
# <a name="replace-value-of-xml-dml"></a>replace value of (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Met à jour la valeur d'un nœud dans le document.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
replace value of Expression1   
with Expression2  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*Expression1*  
Identifie un nœud dont la valeur doit être mise à jour. Un seul nœud doit être désigné, Autrement dit, *Expression1* doit être un singleton statique. Si le XML est typé, le type du nœud doit être un type simple. Si plusieurs nœuds sont sélectionnés, une erreur est générée. Si *Expression1* retourne une séquence vide, aucun remplacement de valeur n’a lieu et aucune erreur n’est retournée. *Expression1* doit retourner un seul élément avec un contenu de type simple (type de liste ou atomique), un nœud de texte ou un nœud d’attribut. *Expression1* ne peut pas être de type union, de type complexe, une instruction de traitement, un nœud de document ou un nœud de commentaire. Sinon, une erreur est retournée.  
  
*Expression2*  
Identifie la nouvelle valeur du nœud. Peut être une expression qui retourne un nœud de type simple, car **data()** est utilisé implicitement. Si la valeur est une liste de valeurs, l’instruction **update** remplace l’ancienne valeur par la liste. Quand vous modifiez une instance XML typée, *Expression2* doit être du même type que *Expression1* ou un sous-type de ce type. Dans le cas contraire, une erreur est retournée. Quand vous modifiez une instance XML non typée, *Expression2* doit être une expression pouvant être atomisée. Dans le cas contraire, une erreur est retournée.  
  
## <a name="examples"></a>Exemples  
Les exemples ci-dessous d’instruction DML XML DML **replace value of** montrent comment mettre à jour des nœuds dans un document XML.  
  
### <a name="a-replacing-values-in-an-xml-instance"></a>R. Remplacement de valeurs dans une instance XML  
Dans l’exemple suivant, une instance de document est d’abord affectée à une variable de type **xml**. Ensuite, des instructions DML XML **replace value of** mettent à jour des valeurs dans le document.  
  
```sql
DECLARE @myDoc XML;  
SET @myDoc = '<Root>  
<Location LocationID="10"   
            LaborHours="1.1"  
            MachineHours=".2" >Manufacturing steps are described here.  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>';  
SELECT @myDoc;  
  
-- update text in the first manufacturing step  
SET @myDoc.modify('  
  replace value of (/Root/Location/step[1]/text())[1]  
  with "new text describing the manu step"  
');  
SELECT @myDoc;  
-- update attribute value  
SET @myDoc.modify('  
  replace value of (/Root/Location/@LaborHours)[1]  
  with "100.0"  
');  
SELECT @myDoc;  
```  
  
La cible de la mise à jour doit être, tout au plus, un nœud explicitement spécifié dans l’expression de chemin d’accès par l’ajout de « [1] » à la fin de l’expression.  
  
### <a name="b-using-the-if-expression-to-determine-replacement-value"></a>B. Utilisation de l'expression if pour déterminer la valeur de remplacement  
Vous pouvez spécifier l’expression **if** dans Expression2 de l’instruction DML XML **replace value of**, comme dans l’exemple ci-dessous. Expression1 identifie l’attribut LaborHours du premier centre de travail comme étant la valeur à mettre à jour. Expression2 utilise une expression **if** pour déterminer la nouvelle valeur de l’attribut LaborHours.  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<Root>  
<Location LocationID="10"   
            LaborHours=".1"  
            MachineHours=".2" >Manu steps are described here.  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>'  
--SELECT @myDoc  
  
SET @myDoc.modify('  
  replace value of (/Root/Location[1]/@LaborHours)[1]  
  with (  
       if (count(/Root/Location[1]/step) > 3) then  
         "3.0"  
       else  
          "1.0"  
      )  
')  
SELECT @myDoc  
```  
  
### <a name="c-updating-xml-stored-in-an-untyped-xml-column"></a>C. Mise à jour du code XML stocké dans une colonne XML non typée  
L'exemple suivant met à jour du code XML dans une colonne.  
  
```sql
DROP TABLE T  
GO  
CREATE TABLE T (i INT, x XML)  
GO  
INSERT INTO T VALUES(1,'<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>')  
go  
-- verify the current <ProductDescription> element  
SELECT x.query(' /Root/ProductDescription')  
FROM T  
-- update the ProductName attribute value  
UPDATE T  
SET x.modify('  
  replace value of (/Root/ProductDescription/@ProductName)[1]  
  with "New Road Bike" ')  
-- verify the update  
SELECT x.query(' /Root/ProductDescription')  
FROM T  
```  
  
### <a name="d-updating-xml-stored-in-a-typed-xml-column"></a>D. Mise à jour du code XML stocké dans une colonne XML typée  
Cet exemple remplace des valeurs dans un document contenant des instructions de fabrication et stockées dans une colonne XML typée.  
  
Vous créez d'abord une table (T) avec une colonne XML typée dans la base données AdventureWorks. Ensuite, vous copiez une instance XML des instructions de fabrication de la colonne Instructions dans la table ProductModel vers la table T. Les insertions sont ensuite appliquées à l'instance XML dans la table T.  
  
```sql
USE AdventureWorks  
GO  
DROP TABLE T  
GO  
CREATE TABLE T(
  ProductModelID INT PRIMARY KEY,   
  Instructions XML (Production.ManuInstructionsSchemaCollection))  
GO  
INSERT T   
SELECT ProductModelID, Instructions  
FROM Production.ProductModel  
WHERE ProductModelID=7  
GO
--insert a new location - <Location 1000/>.   
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
insert <MI:Location LocationID="1000"  LaborHours="1000"  LotSize="1000" >  
           <MI:step>Do something using <MI:tool>hammer</MI:tool></MI:step>  
         </MI:Location>  
  as first  
  into (/MI:root)[1]  
')  
GO  
SELECT Instructions  
FROM T  
GO  
-- Now replace manu. tool in location 1000  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  replace value of (/MI:root/MI:Location/MI:step/MI:tool)[1]   
  with "screwdriver"  
')  
GO  
SELECT Instructions  
FROM T  
-- Now replace value of lot size  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  replace value of (/MI:root/MI:Location/@LotSize)[1]   
  with 500 cast as xs:decimal ?  
')  
GO  
SELECT Instructions  
FROM T  
```  
  
Remarquez l’utilisation de **cast** pour remplacer la valeur LotSize. Celui-ci est requis lorsque la valeur doit être d’un type spécifique. Dans cet exemple, si 500 était la valeur, aucune conversion explicite ne serait nécessaire.  
  
## <a name="see-also"></a>Voir aussi  
[Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
[Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
[méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)   
[Langage de manipulation de données XML &#40;DML XML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
