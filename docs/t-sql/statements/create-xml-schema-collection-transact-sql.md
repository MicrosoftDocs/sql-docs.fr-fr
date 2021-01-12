---
description: CREATE XML SCHEMA COLLECTION (Transact-SQL)
title: CREATE XML SCHEMA COLLECTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE XML SCHEMA COLLECTION
- CREATE_XML_SCHEMA_COLLECTION_TSQL
- CREATE XML SCHEMA
- CREATE_XML_SCHEMA_TSQL
- COLLECTION
- COLLECTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML SCHEMA COLLECTION statement
- importing schema components
- schema collections [SQL Server], creating
- multiple schema namespaces
- XML schema collections [SQL Server], creating
ms.assetid: 350684e8-b3f6-4b58-9dbc-0f05cc776ebb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 44cfb98daf253a1229d8fd4510b22e8d765c5db4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099452"
---
# <a name="create-xml-schema-collection-transact-sql"></a>CREATE XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Importe les composants d'un schéma dans une base de données.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE XML SCHEMA COLLECTION [ <relational_schema>. ]sql_identifier AS Expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *relational_schema*  
 Identifie le nom du schéma relationnel. Si cet argument n’est pas spécifié, le schéma relationnel par défaut est utilisé.  
  
 *sql_identifier*  
 ID SQL de la collection de schémas XML.  
  
 *Expression*  
 Constante de type chaîne ou variable scalaire. Est de type **varchar**, **varbinary**, **nvarchar** ou **xml**.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez également ajouter des espaces de noms à la collection ou ajouter des composants aux espaces de noms existants de la collection, à l’aide de l’instruction [ALTER XML SCHEMA COLLECTION](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md).  
  
 Pour supprimer des collections, utilisez [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 CREATE XML SCHEMA COLLECTION nécessite au moins l'un des groupes d'autorisations suivants :  
  
-   Autorisation CONTROL sur le serveur  
  
-   Autorisation ALTER ANY DATABASE sur le serveur  
  
-   Autorisation ALTER sur la base de données  
  
-   Autorisation CONTROL dans la base de données  
  
-   Autorisations ALTER ANY SCHEMA et CREATE XML SCHEMA COLLECTION dans la base de données  
  
-   Autorisation ALTER ou CONTROL sur le schéma relationnel et autorisation CREATE XML SCHEMA COLLECTION sur la base de données  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-xml-schema-collection-in-the-database"></a>R. Création d'une collection de schémas XML dans la base de données  
 Le code exemple suivant crée la collection de schémas XML `ManuInstructionsSchemaCollection`. La collection comporte un seul espace de noms.  
  
```sql  
-- Create a sample database in which to load the XML schema collection.  
CREATE DATABASE SampleDB;  
GO  
USE SampleDB;  
GO  
CREATE XML SCHEMA COLLECTION ManuInstructionsSchemaCollection AS  
N'<?xml version="1.0" encoding="UTF-16"?>  
<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   xmlns          ="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   elementFormDefault="qualified"   
   attributeFormDefault="unqualified"  
   xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
  
    <xsd:complexType name="StepType" mixed="true" >  
        <xsd:choice  minOccurs="0" maxOccurs="unbounded" >   
            <xsd:element name="tool" type="xsd:string" />  
            <xsd:element name="material" type="xsd:string" />  
            <xsd:element name="blueprint" type="xsd:string" />  
            <xsd:element name="specs" type="xsd:string" />  
            <xsd:element name="diag" type="xsd:string" />  
        </xsd:choice>   
    </xsd:complexType>  
  
    <xsd:element  name="root">  
        <xsd:complexType mixed="true">  
            <xsd:sequence>  
                <xsd:element name="Location" minOccurs="1" maxOccurs="unbounded">  
                    <xsd:complexType mixed="true">  
                        <xsd:sequence>  
                            <xsd:element name="step" type="StepType" minOccurs="1" maxOccurs="unbounded" />  
                        </xsd:sequence>  
                        <xsd:attribute name="LocationID" type="xsd:integer" use="required"/>  
                        <xsd:attribute name="SetupHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="MachineHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LaborHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LotSize" type="xsd:decimal" use="optional"/>  
                    </xsd:complexType>  
                </xsd:element>  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>' ;  
GO  
-- Verify - list of collections in the database.  
SELECT *  
FROM sys.xml_schema_collections;  
-- Verify - list of namespaces in the database.  
SELECT name  
FROM sys.xml_schema_namespaces;  
  
-- Use it. Create a typed xml variable. Note collection name specified.  
DECLARE @x xml (ManuInstructionsSchemaCollection);  
GO  
--Or create a typed xml column.  
CREATE TABLE T (  
        i int primary key,   
        x xml (ManuInstructionsSchemaCollection));  
GO  
-- Clean up  
DROP TABLE T;  
GO  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
Go  
USE master;  
GO  
DROP DATABASE SampleDB;  
```  
  
 Vous pouvez également affecter la collection de schémas à une variable et spécifier celle-ci dans l'instruction `CREATE XML SCHEMA COLLECTION` comme suit :  
  
```  
DECLARE @MySchemaCollection nvarchar(max)  
Set @MySchemaCollection  = N' copy the schema collection here'  
CREATE XML SCHEMA COLLECTION MyCollection AS @MySchemaCollection   
```  
  
 La variable de l'exemple est de type `nvarchar(max)`. Elle peut être également de type **xml**, auquel cas elle est implicitement convertie en chaîne de caractères.  
  
 Pour plus d’informations, consultez [Afficher une collection de schémas XML stockée](../../relational-databases/xml/view-a-stored-xml-schema-collection.md).  
  
 Vous pouvez stocker des collections de schémas dans une colonne de type **xml**. Dans ce cas, procédez comme suit pour créer la collection de schémas XML :  
  
1.  Extrayez la collection de schémas de la colonne à l’aide d’une instruction SELECT et affectez-la à une variable de type **xml** ou **varchar**.  
  
2.  Spécifiez le nom de la variable dans l'instruction CREATE XML SCHEMA COLLECTION.  
  
 CREATE XML SCHEMA COLLECTION stocke uniquement les composants de schéma que SQL Server comprend ; tous les éléments du schéma XML ne sont pas stockés dans la base de données. Par conséquent, si vous voulez récupérer la collection de schémas XML exactement comme elle a été fournie, il est recommandé d'enregistrer vos schémas XML dans une colonne de la base de données ou dans un autre dossier de votre ordinateur.  
  
### <a name="b-specifying-multiple-schema-namespaces-in-a-schema-collection"></a>B. Spécification de plusieurs espaces de noms de schémas dans une collection de schémas  
 Vous pouvez spécifier plusieurs schémas XML lorsque vous créez une collection de schémas XML. Par exemple :  
  
```sql  
CREATE XML SCHEMA COLLECTION MyCollection AS N'  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!-- Contents of schema here -->    
</xsd:schema>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!-- Contents of schema here -->  
</xsd:schema>';  
```  
  
 Le code exemple suivant crée la collection de schémas XML `ProductDescriptionSchemaCollection` qui inclut deux espaces de noms de schémas XML.  
  
```sql  
CREATE XML SCHEMA COLLECTION ProductDescriptionSchemaCollection AS   
'<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"  
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
    elementFormDefault="qualified"   
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
    <xsd:element name="Warranty"  >  
        <xsd:complexType>  
            <xsd:sequence>  
                <xsd:element name="WarrantyPeriod" type="xsd:string"  />  
                <xsd:element name="Description" type="xsd:string"  />  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>  
 <xs:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    elementFormDefault="qualified"   
    xmlns:mstns="https://tempuri.org/XMLSchema.xsd"   
    xmlns:xs="http://www.w3.org/2001/XMLSchema"  
    xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" >  
    <xs:import   
namespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" />  
    <xs:element name="ProductDescription" type="ProductDescription" />  
        <xs:complexType name="ProductDescription">  
            <xs:sequence>  
                <xs:element name="Summary" type="Summary" minOccurs="0" />  
            </xs:sequence>  
            <xs:attribute name="ProductModelID" type="xs:string" />  
            <xs:attribute name="ProductModelName" type="xs:string" />  
        </xs:complexType>  
        <xs:complexType name="Summary" mixed="true" >  
            <xs:sequence>  
                <xs:any processContents="skip" namespace="http://www.w3.org/1999/xhtml" minOccurs="0" maxOccurs="unbounded" />  
            </xs:sequence>  
        </xs:complexType>  
</xs:schema>'  
;  
GO -- Clean up  
DROP XML SCHEMA COLLECTION ProductDescriptionSchemaCollection;  
GO  
```  
  
### <a name="c-importing-a-schema-that-does-not-specify-a-target-namespace"></a>C. Importation d'un schéma qui ne spécifie aucun espace de noms cible  
 Si un schéma qui ne contient pas d’attribut **targetNamespace** est importé dans une collection, ses composants sont associés à l’espace de noms cible composé d’une chaîne de caractères vide (voir l’exemple ci-dessous). Notez que si vous n'associez pas au moins un schéma importé dans la collection, plusieurs composants de schéma (éventuellement non liés entre eux) seront associés à l'espace de noms de la chaîne vide par défaut.  
  
```sql  
-- Create a collection that contains a schema with no target namespace.  
CREATE XML SCHEMA COLLECTION MySampleCollection AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  xmlns:ns="http://ns">  
<element name="e" type="dateTime"/>  
</schema>';  
go  
-- Query will return the names of all the collections that   
--contain a schema with no target namespace.  
SELECT sys.xml_schema_collections.name   
FROM   sys.xml_schema_collections   
JOIN   sys.xml_schema_namespaces   
ON     sys.xml_schema_collections.xml_collection_id =   
       sys.xml_schema_namespaces.xml_collection_id   
WHERE  sys.xml_schema_namespaces.name='';  
```  
  
### <a name="d-using-an-xml-schema-collection-and-batches"></a>D. Utilisation d'une collection de schémas et de traitements XML  
 Une collection de schémas ne peut pas être référencée dans le traitement où elle a été créée. Si vous essayez de faire référence à une collection dans le traitement où celle-ci a été créée, vous obtenez un message d'erreur indiquant que la collection n'existe pas. L'exemple suivant fonctionne ; vous obtiendrez toutefois une erreur si vous supprimez `GO` et essayez par la suite de faire référence à la collection de schémas XML pour entrer une variable `xml` dans le même traitement.  
  
```sql  
CREATE XML SCHEMA COLLECTION mySC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="string"/>  
</schema>  
';  
GO  
CREATE TABLE T (Col1 xml (mySC));  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md)   
 [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [Spécifications et limitations relatives aux collections de schémas XML sur le serveur](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
