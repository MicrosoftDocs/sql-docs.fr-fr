---
description: ALTER XML SCHEMA COLLECTION (Transact-SQL)
title: ALTER XML SCHEMA COLLECTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_XML_SCHEMA_COLLECTION_TSQL
- ALTER XML SCHEMA COLLECTION
dev_langs:
- TSQL
helpviewer_keywords:
- schema collections [SQL Server], altering
- xml_schema_namespace function
- adding schema components
- ALTER XML SCHEMA COLLECTION statement
- XML schemas [SQL Server], adding
- XML schema collections [SQL Server], modifying
- schema collections [SQL Server], adding components
- XML schema collections [SQL Server], adding components
- importing schemas
- XML schema collections [SQL Server], altering
- schema collections [SQL Server], modifying
- multiple schema namespaces
ms.assetid: e311c425-742a-4b0d-b847-8b974bf66d53
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f10572e2ceec916d930d8efd9cd2f2402294c926
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098489"
---
# <a name="alter-xml-schema-collection-transact-sql"></a>ALTER XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute de nouveaux composants de schéma à une collection de schémas XML existante.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER XML SCHEMA COLLECTION [ relational_schema. ]sql_identifier ADD 'Schema Component'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *relational_schema*  
 Identifie le nom du schéma relationnel. Si cet argument n'est pas spécifié, le schéma relationnel par défaut est utilisé.  
  
 *sql_identifier*  
 ID SQL de la collection de schémas XML.  
  
 **'** *Schema Component* **'**  
 Composant de schéma à insérer.  
  
## <a name="remarks"></a>Notes  
 Utilisez ALTER XML SCHEMA COLLECTION pour ajouter de nouveaux schémas XML dont les espaces de noms ne se trouvent pas déjà dans la collection de schémas XML ou pour ajouter de nouveaux composants aux espaces de noms existants dans la collection.  
  
 Le code exemple suivant ajoute un nouvel \<element> à l’espace de noms existant `https://MySchema/test_xml_schema` dans la collection `MyColl`.  
  
```sql  
-- First create an XML schema collection.  
CREATE XML SCHEMA COLLECTION MyColl AS '  
   <schema   
    xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="https://MySchema/test_xml_schema">  
      <element name="root" type="string"/>   
  </schema>'  
-- Modify the collection.   
ALTER XML SCHEMA COLLECTION MyColl ADD '  
  <schema xmlns="http://www.w3.org/2001/XMLSchema"   
         targetNamespace="https://MySchema/test_xml_schema">   
     <element name="anotherElement" type="byte"/>   
 </schema>';  
```  
  
 `ALTER XML SCHEMA` ajoute l'élément `<anotherElement>` à l'espace de noms précédemment défini `https://MySchema/test_xml_schema`.  
  
 Remarquez que si certains composants que vous voulez ajouter à la collection font référence à des composants déjà présents dans cette collection, vous devez utiliser `<import namespace="referenced_component_namespace" />`. Il n'est cependant pas accepté d'utiliser l'espace de noms du schéma actif dans `<xsd:import>` ; par conséquent, les composants du même espace de noms cible que l'espace de noms du schéma actif sont automatiquement importés.  
  
 Pour supprimer des collections, utilisez [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md).  
  
 Si la collection de schémas contient déjà un caractère générique de validation lax ou un élément de type **xs:anyType**, l’ajout d’un nouvel élément global, d’un nouveau type ou d’une nouvelle déclaration d’attribut à la collection de schémas entraîne la revalidation de toutes les données stockées qui sont conditionnées par la collection de schémas.  
  
## <a name="permissions"></a>Autorisations  
 La modification d'une collection de schémas XML nécessite l'autorisation ALTER sur la collection.  
  
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
  
-- Use it. Create a typed xml variable. Note the collection name   
-- that is specified.  
DECLARE @x xml (ManuInstructionsSchemaCollection);  
GO  
--Or create a typed xml column.  
CREATE TABLE T (  
        i int primary key,   
        x xml (ManuInstructionsSchemaCollection));  
GO  
-- Clean up.  
DROP TABLE T;  
GO  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
Go  
USE master;  
GO  
DROP DATABASE SampleDB;  
```  
  
 Vous pouvez également affecter la collection de schémas à une variable et spécifier celle-ci dans l'instruction `CREATE XML SCHEMA COLLECTION` comme suit :  
  
```sql  
DECLARE @MySchemaCollection nvarchar(max);  
SET @MySchemaCollection  = N' copy the schema collection here';  
CREATE XML SCHEMA COLLECTION AS @MySchemaCollection;   
```  
  
 La variable de l'exemple est de type `nvarchar(max)`. Elle peut être également de type **xml**, auquel cas elle est implicitement convertie en chaîne de caractères.  
  
 Pour plus d’informations, consultez [Afficher une collection de schémas XML stockée](../../relational-databases/xml/view-a-stored-xml-schema-collection.md).  
  
 Vous pouvez stocker les collections de schémas dans une colonne de type **xml**. Dans ce cas, pour créer une collection de schémas XML, procédez comme suit :  
  
1.  Extrayez la collection de schémas de la colonne à l’aide d’une instruction SELECT et affectez-la à une variable de type **xml** ou **varchar**.  
  
2.  Spécifiez le nom de la variable dans l'instruction CREATE XML SCHEMA COLLECTION.  
  
 L'instruction CREATE XML SCHEMA COLLECTION stocke uniquement les composants de schéma que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut interpréter ; tous les éléments du schéma XML ne sont pas stockés dans la base de données. Par conséquent, si vous voulez récupérer la collection de schémas XML exactement comme elle a été fournie, il est recommandé d'enregistrer vos schémas XML dans une colonne de la base de données ou dans un autre dossier de votre ordinateur.  
  
### <a name="b-specifying-multiple-schema-namespaces-in-a-schema-collection"></a>B. Spécification de plusieurs espaces de noms de schémas dans une collection de schémas  
 Vous pouvez spécifier plusieurs schémas XML lorsque vous créez une collection de schémas XML. Par exemple :  
  
```sql  
CREATE XML SCHEMA COLLECTION N'  
<xsd:schema>....</xsd:schema>  
<xsd:schema>...</xsd:schema>';  
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
GO   
-- Clean up  
DROP XML SCHEMA COLLECTION ProductDescriptionSchemaCollection;  
GO  
```  
  
### <a name="c-importing-a-schema-that-does-not-specify-a-target-namespace"></a>C. Importation d'un schéma qui ne spécifie aucun espace de noms cible  
 Si un schéma qui ne contient pas d’attribut **targetNamespace** est importé dans une collection, ses composants sont associés à l’espace de noms cible composé d’une chaîne de caractères vide (voir l’exemple ci-dessous). Remarquez que si vous n'associez pas un ou plusieurs schémas importés dans la collection, plusieurs composants de schéma (potentiellement sans relation) sont associés à l'espace de noms composé d'une chaîne de caractères vide.  
  
```sql  
-- Create a collection that contains a schema with no target namespace.  
CREATE XML SCHEMA COLLECTION MySampleCollection AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  xmlns:ns="http://ns">  
<element name="e" type="dateTime"/>  
</schema>';  
GO  
-- query will return the names of all the collections that   
--contain a schema with no target namespace  
SELECT sys.xml_schema_collections.name   
FROM   sys.xml_schema_collections   
JOIN   sys.xml_schema_namespaces   
ON     sys.xml_schema_collections.xml_collection_id =   
       sys.xml_schema_namespaces.xml_collection_id   
WHERE  sys.xml_schema_namespaces.name='';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Spécifications et limitations relatives aux collections de schémas XML sur le serveur](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
