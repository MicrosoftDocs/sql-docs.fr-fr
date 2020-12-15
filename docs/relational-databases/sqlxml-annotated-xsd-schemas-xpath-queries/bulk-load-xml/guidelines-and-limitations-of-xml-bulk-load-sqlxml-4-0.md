---
title: Instructions et limitations du chargement en masse XML (SQLXML)
description: En savoir plus sur les instructions et les limitations de l’utilisation du chargement en masse XML dans SQLXML 4,0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XML Bulk Load [SQLXML], about XML Bulk Load
- bulk load [SQLXML], about bulk load
ms.assetid: c5885d14-c7c1-47b3-a389-455e99a7ece1
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b17b1990f168326ae884b4db4f1ff22a63b24e21
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439683"
---
# <a name="guidelines-and-limitations-of-xml-bulk-load-sqlxml-40"></a>Règles et limitations du chargement en masse XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Lorsque vous utilisez le chargement en masse XML, vous devez connaître les indications et les limitations suivantes :  
  
-   Les schémas insérés ne sont pas pris en charge.  
  
     Si vous possédez un schéma inséré dans le document XML source, le chargement en masse XML ignore ce schéma. Vous spécifiez le schéma de mappage pour le chargement en masse XML externe aux données XML. Vous ne pouvez pas spécifier le schéma de mappage au niveau d’un nœud à l’aide de l’attribut **xmlns = "x :Schema"** .  
  
-   Le système vérifie qu'un document XML est correct, mais ne le valide pas.  
  
     Le chargement en masse XML vérifie le document XML pour déterminer s’il est bien formé, c’est-à-dire s’il est conforme aux exigences de syntaxe de la recommandation XML 1,0 de World Wide Web Consortium. Si le document n'est pas correct, le chargement en masse XML annule le traitement et retourne une erreur. La seule exception à ceci est lorsque le document est un fragment (par exemple, le document n'a aucun élément racine unique), auquel cas le chargement en masse XML chargera le document.  
  
     Le chargement en masse XML ne valide pas le document par rapport à tout schéma de données XML ou DTD qui est défini ou référencé dans le fichier de données XML. De plus, le chargement en masse XML ne valide pas le fichier de données XML par rapport au schéma de mappage fourni.  
  
-   Toutes informations de prologue XML sont ignorées.  
  
     Le chargement en masse XML ignore toutes les informations avant et après l' \<root> élément dans le document XML. Par exemple, le chargement en masse XML ignore toutes les déclarations XML, les définitions DTD internes, les références DTD externes, les commentaires, etc.  
  
-   Si vous possédez un schéma de mappage qui définit une relation clé primaire/clé étrangère entre deux tables (par exemple, entre Customer et CustOrder), la table avec la clé primaire doit être décrite en premier dans le schéma. La table avec la colonne de clé étrangère doit apparaître ultérieurement dans le schéma. Cela est dû au fait que l’ordre dans lequel les tables sont identifiées dans le schéma est l’ordre utilisé pour les charger dans la base de données. Par exemple, le schéma XDR suivant génère une erreur lorsqu’il est utilisé dans le chargement en masse XML, car l' **\<Order>** élément est décrit avant l' **\<Customer>** élément. La colonne CustomerID dans CustOrder est une colonne de clé étrangère qui fait référence à la colonne de clé primaire CustomerID dans la table Cust.  
  
    ```  
    <?xml version="1.0" ?>  
    <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
            xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
            xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
        <ElementType name="Order" sql:relation="CustOrder" >  
          <AttributeType name="OrderID" />  
          <AttributeType name="CustomerID" />  
          <attribute type="OrderID" />  
          <attribute type="CustomerID" />  
        </ElementType>  
  
       <ElementType name="CustomerID" dt:type="int" />  
       <ElementType name="CompanyName" dt:type="string" />  
       <ElementType name="City" dt:type="string" />  
  
       <ElementType name="root" sql:is-constant="1">  
          <element type="Customers" />  
       </ElementType>  
       <ElementType name="Customers" sql:relation="Cust"   
                         sql:overflow-field="OverflowColumn"  >  
          <element type="CustomerID" sql:field="CustomerID" />  
          <element type="CompanyName" sql:field="CompanyName" />  
          <element type="City" sql:field="City" />  
          <element type="Order" >   
               <sql:relationship  
                   key-relation="Cust"  
                    key="CustomerID"  
                    foreign-key="CustomerID"  
                    foreign-relation="CustOrder" />  
          </element>  
       </ElementType>  
    </Schema>  
    ```  
  
-   Si le schéma ne spécifie pas de colonnes de dépassement de capacité à l’aide de l’annotation **SQL : overflow-field** , le chargement en masse XML ignore toutes les données présentes dans le document XML, mais n’est pas décrite dans le schéma de mappage.  
  
     Le chargement en masse XML applique le schéma de mappage que vous avez spécifié toutes les fois qu'il rencontre des balises connues dans le flux de données XML. Il ignore les données qui sont présentes dans le document XML mais qui ne sont pas décrites dans le schéma. Par exemple, supposons que vous ayez un schéma de mappage qui décrit un **\<Customer>** élément. Le fichier de données XML a une **\<AllCustomers>** balise racine (qui n’est pas décrite dans le schéma) qui englobe tous les **\<Customer>** éléments :  
  
    ```  
    <AllCustomers>  
      <Customer>...</Customer>  
      <Customer>...</Customer>  
       ...  
    </AllCustomers>  
    ```  
  
     Dans ce cas, le chargement en masse XML ignore l' **\<AllCustomers>** élément et commence le mappage au niveau de l' **\<Customer>** élément. Le chargement en masse XML ignore les éléments qui ne sont pas décrits dans le schéma mais qui sont présents dans le document XML.  
  
     Prenons l’exemple d’un autre fichier de données source XML qui contient des **\<Order>** éléments. Ces éléments ne sont pas décrits dans le schéma de mappage :  
  
    ```  
    <AllCustomers>  
      <Customer>...</Customer>  
        <Order> ... </Order>  
        <Order> ... </Order>  
         ...  
      <Customer>...</Customer>  
        <Order> ... </Order>  
        <Order> ... </Order>  
         ...  
      ...  
    </AllCustomers>  
    ```  
  
     Le chargement en masse XML ignore ces **\<Order>** éléments. Toutefois, si vous utilisez l’annotation **SQL : overflow-field** dans le schéma pour identifier une colonne en tant que colonne de dépassement, le chargement en masse XML stocke toutes les données non consommées dans cette colonne.  
  
-   Les sections CDATA et les références d'entité sont traduites en chaînes équivalentes avant d'être stockées dans la base de données.  
  
     Dans cet exemple, une section CDATA encapsule la valeur de l' **\<City>** élément. Le chargement en masse XML extrait la valeur de chaîne (« NY ») avant d’insérer l' **\<City>** élément dans la base de données.  
  
    ```  
    <City><![CDATA[NY]]> </City>  
    ```  
  
     Le chargement en masse XML ne conserve pas les références d'entité.  
  
-   Si le schéma de mappage spécifie la valeur par défaut pour un attribut et que les données de source XML ne contiennent pas cet attribut, le chargement en masse XML utilise la valeur par défaut.  
  
     L’exemple de schéma XDR suivant affecte une valeur par défaut à l’attribut **HireDate** :  
  
    ```  
    <?xml version="1.0" ?>  
    <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
            xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
            xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
       <ElementType name="root" sql:is-constant="1">  
          <element type="Customers" />  
       </ElementType>  
  
       <ElementType name="Customers" sql:relation="Cust3" >  
          <AttributeType name="CustomerID" dt:type="int"  />  
          <AttributeType name="HireDate"  default="2000-01-01" />  
          <AttributeType name="Salary"   />  
  
          <attribute type="CustomerID" sql:field="CustomerID" />  
          <attribute type="HireDate"   sql:field="HireDate"  />  
          <attribute type="Salary"     sql:field="Salary"    />  
       </ElementType>  
    </Schema>  
    ```  
  
     Dans ces données XML, l’attribut **HireDate** est manquant dans le deuxième **\<Customers>** élément. Lorsque le chargement en masse XML insère le deuxième **\<Customers>** élément dans la base de données, il utilise la valeur par défaut spécifiée dans le schéma.  
  
    ```  
    <ROOT>  
      <Customers CustomerID="1" HireDate="1999-01-01" Salary="10000" />  
      <Customers CustomerID="2" Salary="10000" />  
    </ROOT>  
    ```  
  
-   L’annotation **SQL : URL-Encode** n’est pas prise en charge :  
  
     Vous ne pouvez pas spécifier une URL dans l'entrée de données XML et vous attendre à ce que le chargement en masse lise les données à cet emplacement.  
  
     Les tables qui sont identifiées dans le schéma de mappage sont créées (la base de données doit exister). Si une ou plusieurs tables existent déjà dans la base de données, la propriété SGDropTables détermine si ces tables préexistantes doivent être supprimées et recréées.  
  
-   Si vous spécifiez la propriété SchemaGen (par exemple, SchemaGen = true), les tables identifiées dans le schéma de mappage sont créées. Toutefois, SchemaGen ne crée pas de contraintes (telles que les contraintes de clé primaire/clé étrangère) sur ces tables, à une exception près : si les nœuds XML qui constituent la clé primaire dans une relation sont définis comme ayant un type XML d’ID (autrement dit, **type = "xsd : ID"** pour xsd) et que la propriété SGUseID a la valeur true pour SchemaGen, les relations de clé primaire/clé étrangère sont créées à partir des relations de schéma de mappage.  
  
-   SchemaGen n’utilise pas les facettes et les extensions de schéma XSD pour générer le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] schéma relationnel.  
  
-   Si vous spécifiez la propriété SchemaGen (par exemple, SchemaGen = true) sur le chargement en masse, seules les tables (et non les vues de nom partagé) spécifiées sont mises à jour.  
  
-   SchemaGen fournit uniquement des fonctionnalités de base pour générer le schéma relationnel à partir de XSD annoté. L'utilisateur doit modifier manuellement les tables générées, si nécessaire.  
  
-   Lorsqu’il existe plusieurs relations entre les tables, SchemaGen tente de créer une relation unique qui comprend toutes les clés impliquées entre les deux tables. Cette limitation peut être à l'origine d'une erreur [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   Lorsque vous chargez en masse des données XML dans une base de données, il doit y avoir au moins un attribut ou élément enfant dans le schéma de mappage qui est mappé à une colonne de base de données.  
  
-   Si vous insérez des valeurs de date à l'aide du chargement en masse XML, les valeurs doivent être spécifiées au format (-)SSAA-MM-JJ((+-)TZ). Ceci est le format XSD standard pour la date.  
  
-   Certains indicateurs de propriété ne sont pas compatibles avec d'autres indicateurs de propriété. Par exemple, le chargement en masse ne prend pas en charge **IgnoreDuplicateKeys = true** avec **KeepIdentity = False**. Lorsque **KeepIdentity = False**, le chargement en masse s’attend à ce que le serveur génère les valeurs de clé. Les tables doivent avoir une contrainte d' **identité** sur la clé. Le serveur ne générera pas de clés dupliquées, ce qui signifie qu’il n’est pas nécessaire de définir **IgnoreDuplicateKeys** sur **true**. **IgnoreDuplicateKeys** doit être défini sur **true** uniquement lors du chargement des valeurs de clé primaire à partir des données entrantes dans une table qui contient des lignes et il existe un risque de conflit de valeurs de clé primaire.  
  
  
