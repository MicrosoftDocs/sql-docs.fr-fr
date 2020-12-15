---
title: Exécuter des requêtes XPath avec des espaces de noms (SQLXMLOLEDB)
description: Découvrez comment spécifier des espaces de noms dans SQLXML 4,0 lors de l’exécution de requêtes XPath avec le fournisseur SQLXMLOLEDB.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, executing XPath queries
- namespaces property
- queries [SQLXML], SQLXMLOLEDB Provider
- XPath queries [SQLXML], namespaces
- XPath queries [SQLXML], SQLXMLOLEDB Provider
- namespaces [SQLXML], XPath queries
ms.assetid: 024a4b7d-435d-47ba-9e80-2c2f640108f5
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ee24d16dce8ae60ccf51a866a274b15b2737ca89
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462910"
---
# <a name="executing-xpath-queries-with-namespaces-sqlxmloledb-provider"></a>Exécution de requêtes XPath avec des espaces de noms (fournisseur SQLXMLOLEDB)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Les requêtes XPath peuvent inclure des espaces de noms. Si les éléments de schéma sont qualifiés par un espace de noms (autrement dit, s'ils incluent un espace de noms cible), les requêtes XPath contre le schéma doivent spécifier cet espace de noms.  
  
 L'utilisation du caractère générique (*) n'étant pas prise en charge dans SQLXML 4.0, vous devez spécifier la requête XPath en utilisant un préfixe d'espace de noms. Pour résoudre ce préfixe, utilisez la propriété Namespaces pour spécifier la liaison d’espace de noms.  
  
 Dans l’exemple suivant, la requête XPath spécifie des espaces de noms à l’aide du caractère générique ( \* ) et des fonctions XPath local name () et namespace-URI (). Cette requête XPath retourne tous les éléments dont le nom local est **contact** et l’URI de l’espace de noms est **urn : MySchema : contacts**.  
  
```  
/*[local-name() = 'Contact' and namespace-uri() = 'urn:myschema:Contacts']  
```  
  
 Dans SQLXML 4.0, cette requête XPath doit être spécifiée avec un préfixe d'espace de noms. Par exemple, **x :contact**, où **x** est le préfixe d’espace de noms. Prenons le schéma XSD suivant :  
  
```  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:con="urn:myschema:Contacts"  
            targetNamespace="urn:myschema:Contacts">  
<complexType name="ContactType">  
  <attribute name="CID" sql:field="ContactID" type="ID"/>  
  <attribute name="FName" sql:field="FirstName" type="string"/>  
  <attribute name="LName" sql:field="LastName"/>   
</complexType>  
<element name="Contact" type="con:ContactType" sql:relation="Person.Contact"/>  
</schema>  
```  
  
 Étant donné que ce schéma définit l'espace de noms cible, une requête Xpath (telle que « Employee ») contre le schéma doit inclure l'espace de noms.  
  
 Voici un exemple d'application [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic qui exécute une requête XPath (x:Employee) contre le schéma XSD précédent. Pour résoudre le préfixe, la liaison d’espace de noms est spécifiée à l’aide de la propriété Namespaces.  
  
> [!NOTE]  
>  Dans le code, vous devez fournir le nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans la chaîne de connexion. En outre, cet exemple spécifie l'utilisation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) comme fournisseur de données, ce qui requiert l'installation d'un logiciel client réseau supplémentaire. Pour plus d’informations, consultez [Configuration système requise pour SQL Server Native Client](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md).  
  
```  
Option Explicit  
Private Sub Form_Load()  
    Dim con As New ADODB.Connection  
    Dim cmd As New ADODB.Command  
    Dim stm As New ADODB.Stream  
    con.Open "provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Data Source=SqlServerName;Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
    Set cmd.ActiveConnection = con  
    stm.Open  
    cmd.Properties("Output Stream").Value = stm  
    cmd.Properties("Output Encoding") = "utf-8"  
    cmd.Properties("Mapping schema") = "C:\DirectoryPath\con-ex.xml"  
    cmd.Properties("namespaces") = "xmlns:x='urn:myschema:Contacts'"  
    '  Debug.Print "Set Command Dialect to DBGUID_XPATH"  
    cmd.Dialect = "{ec2a4293-e898-11d2-b1b7-00c04f680c56}"  
    cmd.CommandText = "x:Contact"  
    cmd.Execute , , adExecuteStream   
    stm.Position = 0  
    Debug.Print stm.ReadText(adReadAll)  
End Sub  
```  
  
### <a name="to-test-this-application"></a>Pour tester cette application  
  
1.  Enregistrez l'exemple de schéma XSD dans un dossier.  
  
2.  Créez un projet exécutable Visual Basic et copiez-y le code. Modifiez le chemin d'accès au répertoire spécifié comme il se doit.  
  
3.  Ajoutez la référence de projet suivante :  
  
    ```  
    "Microsoft ActiveX Data Objects 2.8 Library"  
    ```  
  
4.  Exécutez l'application.  

 Voici le résultat partiel :  
  
```  
<y0:Employee xmlns:y0="urn:myschema:Contacts"   
             LName="Achong" CID="1" FName="Gustavo"/>  
<y0:Employee xmlns:y0="urn:myschema:Employees"   
             LName="Abel" CID="2" FName="Catherine"/>  
```  
  
 Les préfixes générés dans le document XML sont arbitraires, mais ils mappent au même espace de noms.  
  
  
