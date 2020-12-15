---
title: 'Identifier les colonnes clés à l’aide de SQL : key-fields (SQLXML)'
description: 'Apprenez à garantir une imbrication correcte dans un résultat de requête SQLXML 4,0 en spécifiant l’annotation sql : key-fields dans une requête XPath pour identifier les colonnes clés.'
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nesting XML results
- proper nesting in results [SQLXML]
- sql:key-fields
- XSD schemas [SQLXML], key columns
- identifying key columns [SQLXML]
- annotated XSD schemas, key columns
- key columns [SQLXML]
- relationships [SQLXML], key columns
- hierarchical relationships [SQLXML]
- key-fields annotation
ms.assetid: 1a5ad868-8602-45c4-913d-6fbb837eebb0
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9e24eac5a88215a1550beb8c8006852ed7263181
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461780"
---
# <a name="identifying-key-columns-using-sqlkey-fields-sqlxml-40"></a>Identification de colonnes clés à l'aide de sql:key-fields (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Lorsqu'une requête XPath est spécifiée contre un schéma XSD, les informations de clés sont requises dans la plupart des cas pour obtenir une imbrication correcte dans le résultat. La spécification de l’annotation **SQL : key-fields** permet de s’assurer que la hiérarchie appropriée est générée.  
  
> [!NOTE]  
>  Pour garantir une imbrication correcte, il est recommandé de spécifier **SQL : key-fields** pour les éléments qui mappent aux tables. Le XML produit est sensible au classement du jeu de résultats sous-jacent. Si **SQL : key-fields** n’est pas spécifié, le code XML généré n’est peut-être pas correctement formé.  
  
 La valeur de **SQL : key-fields** identifie la ou les colonnes qui identifient de manière unique les lignes dans la relation. Si plusieurs colonnes sont requises pour identifier une ligne de manière unique, les valeurs de colonnes sont délimitées par des espaces.  
  
 Vous devez utiliser l’annotation **SQL : key-fields** lorsqu’un élément contient un **\<sql:relationship>** qui est défini entre l’élément et un élément enfant, mais ne fournit pas la clé primaire de la table spécifiée dans l’élément parent.  
  
## <a name="examples"></a>Exemples  
 Pour créer des exemples fonctionnels à l'aide des exemples suivants, vous devez répondre à certaines conditions requises. Pour plus d’informations, consultez [Configuration requise pour l’exécution d’exemples SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-producing-the-appropriate-nesting-when-sqlrelationship-does-not-provide-sufficient-information"></a>R. Production de l’imbrication appropriée lorsque \<sql:relationship> ne fournit pas d’informations suffisantes  
 Cet exemple montre où **SQL : key-fields** doit être spécifié.  
  
 Prenons le schéma suivant : Le schéma spécifie une hiérarchie entre **\<Order>** les **\<Customer>** éléments et dans lesquels l' **\<Order>** élément est le parent et l' **\<Customer>** élément est un enfant.  
  
 La **\<sql:relationship>** balise est utilisée pour spécifier la relation parent-enfant. Elle identifie CustomerID dans la table Sales.SalesOrderHeader comme clé parente qui fait référence à la clé enfant CustomerID dans la table Sales.Customer. Les informations fournies dans **\<sql:relationship>** ne sont pas suffisantes pour identifier de manière unique les lignes dans la table parente (Sales. SalesOrderHeader). Par conséquent, sans l’annotation **SQL : key-fields** , la hiérarchie générée est inexacte.  
  
 Avec **SQL : key-fields** spécifié sur **\<Order>** , l’annotation identifie de façon unique les lignes dans la table parente (Sales. SalesOrderHeader) et ses éléments enfants apparaissent sous son parent.  
  
 Voici le schéma :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrdCust"  
        parent="Sales.SalesOrderHeader"  
        parent-key="CustomerID"  
        child="Sales.Customer"  
        child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader"   
               sql:key-fields="SalesOrderID">  
   <xsd:complexType>  
     <xsd:sequence>  
     <xsd:element name="Customer" sql:relation="Sales.Customer"   
                       sql:relationship="OrdCust"  >  
       <xsd:complexType>  
         <xsd:attribute name="CustID" sql:field="CustomerID" />  
         <xsd:attribute name="SoldBy" sql:field="SalesPersonID" />  
       </xsd:complexType>  
     </xsd:element>  
     </xsd:sequence>  
     <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
     <xsd:attribute name= "CustomerID" type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Pour créer un exemple fonctionnel de ce schéma  
  
1.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier en tant que KeyFields1.xml.  
  
2.  Copiez le modèle suivant et collez-le dans un fichier texte. Enregistrez le fichier en tant que KeyFields1T.xml dans le même répertoire où vous avez enregistré KeyFields1.xml. La requête XPath dans le modèle retourne tous les **\<Order>** éléments dont le CustomerID est inférieur à 3.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="KeyFields1.xml">  
            /Order[@CustomerID < 3]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (KeyFields1.xml) est relatif au répertoire où le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields1.xml"  
    ```  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  

     Pour plus d’informations, consultez [utilisation d’ADO pour exécuter des requêtes SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le jeu de résultats partiel :  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
    <Order SalesOrderID="43860" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="44501" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="45283" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    .....  
</ROOT>  
```  
  
### <a name="b-specifying-sqlkey-fields-to-produce-proper-nesting-in-the-result"></a>B. Spécification de sql:key-fields afin de produire l'imbrication correcte dans le résultat  
 Dans le schéma suivant, aucune hiérarchie n’est spécifiée à l’aide de **\<sql:relationship>** . Le schéma requiert encore la spécification de l’annotation **SQL : key-fields** pour identifier de manière unique les employés dans la table HumanResources. Employee.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="HumanResources.Employee" sql:key-fields="EmployeeID" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="Title">  
          <xsd:complexType>  
            <xsd:simpleContent>  
              <xsd:extension base="xsd:string">  
                 <xsd:attribute name="EmployeeID" type="xsd:integer" />  
              </xsd:extension>  
            </xsd:simpleContent>  
          </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
   </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Pour créer un exemple fonctionnel de ce schéma  
  
1.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier en tant que KeyFields2.xml.  
  
2.  Copiez le modèle suivant et collez-le dans un fichier texte. Enregistrez le fichier en tant que KeyFields2T.xml dans le même répertoire où vous avez enregistré KeyFields2.xml. La requête XPath dans le modèle retourne tous les **\<HumanResources.Employee>** éléments :  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="KeyFields2.xml">  
        /HumanResources.Employee  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (KeyFields2.xml) est relatif au répertoire où le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields2.xml"  
    ```  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [utilisation d’ADO pour exécuter des requêtes SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le résultat obtenu :  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <HumanResources.Employee>  
    <Title EmployeeID="1">Production Technician - WC60</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="2">Marketing Assistant</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="3">Engineering Manager</Title>   
  </HumanResources.Employee>  
  ...  
</ROOT>  
```  
  
  
