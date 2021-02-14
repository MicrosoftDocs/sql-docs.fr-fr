---
title: ID, fonction (XQuery) | Microsoft Docs
description: 'Découvrez comment utiliser la fonction XQuery ID pour retourner une séquence d’éléments dans l’instance XML, dans l’ordre des documents, avec les valeurs XS : IDREF fournies.'
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
- fn:id function
- id function
ms.assetid: de99fc60-d0ad-4117-a17d-02bdde6512b4
author: rothja
ms.author: jroth
ms.openlocfilehash: c6ae0aedd4971d6345811e6ee0bfff5afa395c15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344985"
---
# <a name="functions-on-sequences---id"></a>Fonctions sur les séquences : id
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne la séquence de nœuds d’élément avec des valeurs XS : ID qui correspondent aux valeurs d’une ou plusieurs des valeurs XS : IDREF fournies dans *$arg*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:id($arg as xs:IDREF*) as element()*  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Une ou plusieurs valeurs xs:IDREF.  
  
## <a name="remarks"></a>Notes  
 Le résultat de la fonction est une séquence d'éléments de l'instance XML, dans l'ordre du document, qui ont une valeur xs:ID égale à une ou plusieurs des valeurs xs:IDREF de la liste des candidats xs:IDREF.  
  
 Si la valeur xs:IDREF ne correspond à aucun élément, la fonction renvoie la séquence vide.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de données.  
  
### <a name="a-retrieving-elements-based-on-the-idref-attribute-value"></a>R. Récupération des éléments en fonction de la valeur de l'attribut IDREF  
 L’exemple suivant utilise FN : ID pour récupérer les <`employee`> éléments, en fonction de l’attribut IDREF Manager. Dans cet exemple, l'attribut manager est de type IDREF et l'attribut eid est de type ID.  
  
 Pour une valeur d’attribut Manager spécifique, la fonction **ID ()** recherche le <`employee`> élément dont la valeur de l’attribut type d’ID correspond à la valeur IDREF d’entrée. En d’autres termes, pour un employé spécifique, la fonction **ID ()** retourne le responsable de l’employé.  
  
 Voici ce qui se passe dans l'exemple :  
  
-   Une collection de schémas XML est créée.  
  
-   Une variable **XML** typée est créée à l’aide de la collection de schémas XML.  
  
-   La requête récupère l’élément qui a une valeur d’attribut ID référencée par l’attribut **Manager** IDREF de l' `employee` élément <>.  
  
```  
-- If exists, drop the XML schema collection (SC).  
-- drop xml schema collection SC  
-- go  
  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:e="emp" targetNamespace="emp">  
            <element name="employees" type="e:EmployeesType"/>  
            <complexType name="EmployeesType">  
                 <sequence>  
                      <element name="employee" type="e:EmployeeType" minOccurs="0" maxOccurs="unbounded" />  
                 </sequence>  
            </complexType>    
  
            <complexType name="EmployeeType">  
                        <attribute name="eid" type="ID" />  
                        <attribute name="name" type="string" />  
                        <attribute name="manager" type="IDREF" />  
            </complexType>         
</schema>'  
go  
```  
  
```  
declare @x xml(SC)  
set @x='<e:employees xmlns:e="emp">  
<employee eid="e1" name="Joe" manager="e10" />  
<employee eid="e2" name="Bob" manager="e10" />  
<employee eid="e10" name="Dave" manager="e10" />  
</e:employees>'  
  
select @x.value(' declare namespace e="emp";   
 (fn:id(e:employees/employee[@name="Joe"]/@manager)/@name)[1]', 'varchar(50)')   
Go  
```  
  
 La requête renvoie la valeur « Dave », ce qui indique que Dave est le supérieur hiérarchique de Joe.  
  
### <a name="b-retrieving-elements-based-on-the-orderlist-idrefs-attribute-value"></a>B. Récupération des éléments en fonction de la valeur de l'attribut IDREFS OrderList  
 Dans l’exemple suivant, l’attribut OrderList de l' `Customer` élément <> est un attribut de type IDREFS. Il répertorie les ID de commande se rapportant à un client particulier. Pour chaque ID de commande, il existe un <`Order`> élément enfant sous le <`Customer`> fournissant la valeur de la commande.  
  
 L'expression de la requête, `data(CustOrders:Customers/Customer[1]/@OrderList)[1]`, récupère la première valeur de la liste IDREFS pour le premier client. Cette valeur est ensuite transmise à la fonction **ID ()** . La fonction recherche ensuite le <`Order`> élément dont la valeur d’attribut OrderID correspond à l’entrée de la fonction **ID ()** .  
  
```  
drop xml schema collection SC  
go  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:Customers="Customers" targetNamespace="Customers">  
            <element name="Customers" type="Customers:CustomersType"/>  
            <complexType name="CustomersType">  
                        <sequence>  
                            <element name="Customer" type="Customers:CustomerType" minOccurs="0" maxOccurs="unbounded" />  
                        </sequence>  
            </complexType>  
             <complexType name="OrderType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="OrderValue" type="integer" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="OrderID" type="ID" />  
            </complexType>  
  
            <complexType name="CustomerType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="spouse" type="string" minOccurs="0" maxOccurs="unbounded"/>  
                                <element name="Order" type="Customers:OrderType" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="CustomerID" type="string" />  
                <attribute name="OrderList" type="IDREFS" />  
            </complexType>  
 </schema>'  
go  
declare @x xml(SC)  
set @x='<CustOrders:Customers xmlns:CustOrders="Customers">  
                <Customer CustomerID="C1" OrderList="OrderA OrderB"  >  
                              <spouse>Jenny</spouse>  
                                <Order OrderID="OrderA"><OrderValue>11</OrderValue></Order>  
                                <Order OrderID="OrderB"><OrderValue>22</OrderValue></Order>  
  
                </Customer>  
                <Customer CustomerID="C2" OrderList="OrderC OrderD" >  
                                <spouse>John</spouse>  
                                <Order OrderID="OrderC"><OrderValue>33</OrderValue></Order>  
                                <Order OrderID="OrderD"><OrderValue>44</OrderValue></Order>  
  
                        </Customer>  
                <Customer CustomerID="C3"  OrderList="OrderE OrderF" >  
                                <spouse>Jane</spouse>  
                                <Order OrderID="OrderE"><OrderValue>55</OrderValue></Order>  
                                <Order OrderID="OrderF"><OrderValue>55</OrderValue></Order>  
                </Customer>  
                <Customer CustomerID="C4"  OrderList="OrderG"  >  
                                <spouse>Tim</spouse>  
                                <Order OrderID="OrderG"><OrderValue>66</OrderValue></Order>  
                        </Customer>  
                <Customer CustomerID="C5"  >  
                </Customer>  
                <Customer CustomerID="C6" >  
                </Customer>  
                <Customer CustomerID="C7"  >  
                </Customer>  
</CustOrders:Customers>'  
select @x.query('declare namespace CustOrders="Customers";  
  id(data(CustOrders:Customers/Customer[1]/@OrderList)[1])')  
  
-- result  
<Order OrderID="OrderA">  
  <OrderValue>11</OrderValue>  
</Order>  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne prend pas en charge la version à deux arguments de **ID ()**.  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] requiert que le type d’argument **ID ()** soit un sous-type de XS : IDREF *.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions sur les séquences](./xquery-functions-against-the-xml-data-type.md)  
  
