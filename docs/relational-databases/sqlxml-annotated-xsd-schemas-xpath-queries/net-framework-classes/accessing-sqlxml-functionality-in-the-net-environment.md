---
title: Accéder aux fonctionnalités SQLXML dans l’environnement .NET
description: Découvrez comment utiliser les classes managées SQLXML pour accéder à l’environnement .NET Framework.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- Managed Classes [SQLXML], accessing SQLXML functionality
- SQLXML Managed Classes, accessing SQLXML functionality
- DiffGrams [SQLXML], accessing SQLXML functionality
- .NET Framework [SQLXML], accessing SQLXML functionality
ms.assetid: 74744535-2945-414d-9a5b-7e8cc363953a
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4557f0e19b3515c7a8a652ba06bde82e6bdd0685
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97431394"
---
# <a name="accessing-sqlxml-functionality-in-the-net-environment"></a>Accès aux fonctionnalités SQLXML dans l'environnement .NET
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Dans cet exemple, vous pouvez voir les informations suivantes :  
  
-   Comment utiliser [!INCLUDE[msCoName](../../../includes/msconame-md.md)] les classes managées SQLXML (Microsoft. Data. SQLXML) pour accéder à Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans l' [!INCLUDE[msCoName](../../../includes/msconame-md.md)] environnement .NET Framework.  
  
-   Comment les DiffGrams générés dans l'environnement .NET Framework peuvent appliquer les mises à jour des données aux tables [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Dans cette application, une requête XPath est exécutée sur un schéma XSD. L’exécution de la requête XPath retourne un document XML constitué de données de contact (**FirstName**, **LastName**). L'application charge le document XML dans le dataset de l'environnement .NET Framework. Les données du dataset sont modifiées : le prénom du contact est changé en « Susan » pour le premier contact du dataset. Le DiffGram est généré à partir du dataset, puis la mise à jour spécifiée dans le DiffGram (modification du prénom de l'employé) est appliquée à la table Person.Contact.  
  
> [!NOTE]  
>  Dans le code, vous devez fournir le nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans la chaîne de connexion.  
  
```  
using System;  
using System.Data;  
using Microsoft.Data.SqlXml;  
using System.IO;  
class Test  
{  
   static string ConnString = "Provider=SQLOLEDB;Server=SqlServerName;database=AdventureWorks;Integrated Security=SSPI;";  
   public static int testParams()  
   {  
      DataRow row;  
      SqlXmlAdapter ad;  
      //need a memory stream to hold diff gram temporarily  
      MemoryStream ms = new MemoryStream();  
      SqlXmlCommand cmd = new SqlXmlCommand(ConnString);  
      cmd.RootTag = "ROOT";  
      cmd.CommandText = "Con";  
      cmd.CommandType = SqlXmlCommandType.XPath;  
      cmd.SchemaPath = "MySchema.xml";  
      //load data set  
      DataSet ds = new DataSet();  
      ad = new SqlXmlAdapter(cmd);  
      ad.Fill(ds);  
      row = ds.Tables["Con"].Rows[0];  
      row["FName"] = "Susan";  
      ad.Update(ds);  
      return 0;  
   }  
   public static int Main(String[] args)  
   {  
      testParams();  
      return 0;  
   }  
}  
```  
  
 **Pour tester l'exemple :**  
  
 Pour tester cet exemple, le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework doit être installé sur votre ordinateur.  
  
1.  Enregistrez ce schéma XSD (MySchema.xml) dans un dossier :  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="Con" sql:relation="Person.Contact" >  
       <xsd:complexType>  
         <xsd:sequence>  
            <xsd:element name="FName"    
                         sql:field="FirstName"   
                         type="xsd:string" />   
            <xsd:element name="LName"    
                         sql:field="LastName"    
                         type="xsd:string" />  
         </xsd:sequence>  
         <xsd:attribute name="ContactID" type="xsd:integer" />  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
2.  Enregistrez le code C# (DocSample.cs) fourni dans cet exemple dans le même dossier que celui du schéma. (Si vous stockez les fichiers dans un dossier différent, vous devrez modifier le code et spécifier le chemin d'accès approprié au répertoire pour le schéma de mappage.)  
  
3.  Compilez le code. Pour compiler le code à l'invite de commandes, utilisez :  
  
    ```  
    csc /reference:Microsoft.Data.SqlXML.dll DocSample.cs  
    ```  
  
     Un fichier exécutable (DocSample.exe) est alors créé.  
  
 À l'invite de commandes, exécutez DocSample.exe.  
  
  
