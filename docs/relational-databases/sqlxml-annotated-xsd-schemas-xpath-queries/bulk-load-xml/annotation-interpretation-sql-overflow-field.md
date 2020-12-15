---
title: 'SQL : overflow-field (SQLXML)'
description: 'Découvrez comment utiliser l’annotation sql : overflow-field pour identifier une colonne en tant que colonne de dépassement qui recevra toutes les données non consommées du document XML.'
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- overflow-field annotation
- unconsumed data
- overflow data [SQLXML]
- sql:overflow-field
ms.assetid: f005182b-6151-432d-ab22-3bc025742cd3
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4d0fa21bcd570a5f5196dfbc1bdf8e1bb6186cb8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462940"
---
# <a name="annotation-interpretation---sqloverflow-field"></a>Interprétation des annotations - sql:overflow-field
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Dans un schéma, vous pouvez identifier une colonne en tant que colonne de dépassement destinée à recevoir toutes les données non consommées du document XML. Cette colonne est spécifiée dans le schéma à l’aide de l’annotation **SQL : overflow-field** . Un schéma peut contenir plusieurs colonnes de dépassement.  
  
 Chaque fois qu’un nœud XML (élément ou attribut) pour lequel une annotation **SQL : overflow-field** est définie entre dans la portée, la colonne de dépassement est activée et reçoit des données non consommées. Lorsque le nœud sort de la portée, la colonne de dépassement n'est plus active et le chargement en masse XML active le champ de dépassement précédent (le cas échéant).  
  
 À mesure qu’il stocke des données dans la colonne de dépassement, le chargement en masse XML stocke également les balises d’ouverture et de fermeture de l’élément parent pour lequel **SQL : overflow-field** est défini.  
  
 Par exemple, le schéma suivant décrit les **\<Customers>** **\<CustOrder>** éléments et. Chacun de ces éléments identifie une colonne de dépassement :  
  
```  
<?xml version="1.0" ?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
 <xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustCustOrder"  
        parent="Cust"  
        parent-key="CustomerID"  
        child="CustOrder"  
        child-key="CustomerID" />  
  </xsd:appinfo>  
 </xsd:annotation>  
 <xsd:element name="ROOT" sql:is-constant="1">  
  <xsd:complexType>  
    <xsd:sequence>   
      <xsd:element name="Customers"   
                   sql:relation="Cust"  
                   sql:overflow-field="OverflowColumn">  
        <xsd:complexType>  
             <xsd:sequence>   
             <xsd:element name="CustomerID" type="xsd:integer"/>  
             <xsd:element name="CompanyName"  type="xsd:string"/>  
             <xsd:element name="City" type="xsd:string"/>  
             <xsd:element name="Order"  
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder"  
                          sql:overflow-field="OverflowColumn">  
               <xsd:complexType>  
                 <xsd:attribute name="OrderID"/>  
                 <xsd:attribute name="CustomerID"/>  
               </xsd:complexType>  
             </xsd:element>  
          </xsd:sequence>   
        </xsd:complexType>  
      </xsd:element>  
    </xsd:sequence>  
  </xsd:complexType>  
 </xsd:element>  
</xsd:schema>  
```  
  
 Dans le schéma, l' **\<Customer>** élément est mappé à la table Cust et l' **\<Order>** élément est mappé à la table CustOrder.  
  
 Les **\<Customer>** éléments et **\<Order>** identifient une colonne de dépassement. Ainsi, le chargement en masse XML enregistre tous les éléments enfants et attributs non consommés de l' **\<Customer>** élément dans la colonne de dépassement de la table Cust, ainsi que tous les éléments enfants et attributs non consommés de l' **\<Order>** élément dans la colonne Overflow de la table CustOrder.  
  
### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Enregistrez le schéma fourni dans cet exemple sous le nom SampleSchema.xml.  
  
2.  Créez les tables suivantes :  
  
    ```  
    CREATE TABLE Cust (  
              CustomerID     int         PRIMARY KEY,  
              CompanyName    varchar(20) NOT NULL,  
              City           varchar(20) DEFAULT 'Seattle',  
              OverflowColumn nvarchar(200))  
    GO  
    CREATE TABLE CustOrder (  
              OrderID        int         PRIMARY KEY,  
              CustomerID     int         FOREIGN KEY REFERENCES  
                                              Cust(CustomerID),  
              OverflowColumn nvarchar(200))  
    GO  
    ```  
  
3.  Enregistrez l'exemple de données XML ci-après sous le nom SampleXMLData.xml :  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City><![CDATA[NY]]> </City>  
          <Junk>garbage in overflow</Junk>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
     </Customers>  
     <Customers>  
       <CustomerID>1112</CustomerID>  
       <CompanyName>Toms Spezialitten</CompanyName>  
       <City><![CDATA[LA]]> </City>  
       <xyz><address>111 Maple, Seattle</address></xyz>     
       <Order OrderID="3" />  
     </Customers>  
       <Customers>  
       <CustomerID>1113</CustomerID>  
       <CompanyName>Victuailles en stock</CompanyName>  
       <Order OrderID="4" />  
      </Customers>  
    </ROOT>  
    ```  
  
4.  Pour exécuter le chargement en masse XML, enregistrez cet exemple [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition (VBScript) sous le nom Sample.vbs et exécutez-le :  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
  
