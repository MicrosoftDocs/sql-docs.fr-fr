---
title: Exécuter des fichiers de modèle avec la propriété CommandText
description: Affichez un exemple d’utilisation de la propriété SQLXML CommandText pour spécifier le nom d’un fichier de modèle contenant des requêtes SQL ou XPath.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- Managed Classes [SQLXML], executing template files
- SQLXML Managed Classes, executing template files
- templates [SQLXML], SQLXML Managed Classes
- executing template files [SQLXML]
- CommandText property
ms.assetid: f1b1278d-252d-4a06-836e-4ef77f338ef9
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 86cf0f5133adf3bbf1e8efa8ff0e93ff4bc98c66
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97414242"
---
# <a name="executing-template-files-by-using-the-commandtext-property"></a>Exécution de fichiers modèles à l'aide de la propriété CommandText
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Cet exemple montre comment vous pouvez spécifier les fichiers modèles qui se composent de requêtes SQL ou XPath à l’aide de la propriété CommandText. Au lieu de spécifier la requête SQL ou XPath comme valeur de CommandText, vous pouvez spécifier un nom de fichier comme valeur. Dans l’exemple suivant, la propriété CommandType est spécifiée en tant que SqlXmlCommandType. TemplateFile.  
  
 L'exemple d'application exécute ce modèle :  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query>  
    SELECT TOP 2 ContactID, FirstName, LastName   
    FROM   Person.Contact  
    FOR XML AUTO  
  </sql:query>  
</ROOT>  
```  
  
 Il s’agit de l’exemple d’application C#. Pour tester l'application, enregistrez le modèle (TemplateFile.xml),  puis exécutez l'application.  
  
> [!NOTE]  
>  Dans le code, vous devez fournir le nom de l'instance de Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans la chaîne de connexion.  
  
```  
using System;  
using Microsoft.Data.SqlXml;  
using System.IO;  
class Test  
{  
      static string ConnString = "Provider=SQLOLEDB;Server=(local);database=AdventureWorks;Integrated Security=SSPI";  
  
      public static int testParams()  
      {  
         //Stream strm;  
         SqlXmlCommand cmd = new SqlXmlCommand(ConnString);  
         cmd.CommandType = SqlXmlCommandType.TemplateFile;  
         cmd.CommandText = "TemplateFile.xml";  
         using (Stream strm = cmd.ExecuteStream()){  
            using (StreamReader sr = new StreamReader(strm)){  
                Console.WriteLine(sr.ReadToEnd());  
            }  
         }  
  
         return 0;        
      }  
      public static int Main(String[] args)  
      {  
         testParams();     
         return 0;  
      }  
   }  
```  
  
### <a name="to-test-the-application"></a>Pour tester l'application  
  
1.  Assurez-vous que le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework est installé sur votre ordinateur.  
  
2.  Enregistrez le modèle XML (TemplateFile.xml) fourni dans cet exemple dans un dossier.  
  
3.  Enregistrez le code C# (DocSample.cs) fourni dans cet exemple dans le même dossier que celui dans lequel le schéma est stocké. (Si vous stockez les fichiers dans un dossier différent, vous devrez modifier le code et spécifier le chemin d'accès approprié au répertoire pour le schéma de mappage.)  
  
4.  Compilez le code. Pour compiler le code à l'invite de commandes, utilisez :  
  
    ```  
    csc /reference:Microsoft.Data.SqlXML.dll DocSample.cs  
    ```  
  
     Un fichier exécutable (DocSample.exe) est alors créé.  
  
5.  À l'invite de commandes, exécutez DocSample.exe.  

 Si vous transmettez un paramètre à un modèle, le nom du paramètre doit commencer par arobase (@); par exemple, p.Name = " \@ ContactID", où p est un objet SqlXmlParameter.  
  
 Voici le modèle mis à jour qui prend un paramètre.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:header>  
     <sql:param name='ContactID'>1</sql:param>    
  </sql:header>  
  <sql:query>  
    SELECT ContactID, FirstName, LastName  
    FROM   Person.Contact  
    WHERE  ContactID=@ContactID  
    FOR XML AUTO  
  </sql:query>  
</ROOT>  
```  
  
 Voici le code mis à jour dans lequel un paramètre est passé pour exécuter le modèle.  
  
```  
public static int testParams()  
{  
  
   Stream strm;  
   SqlXmlParameter p;  
  
   SqlXmlCommand cmd = new SqlXmlCommand(ConnString);  
   cmd.CommandType = SqlXmlCommandType.TemplateFile;  
   cmd.CommandText = "TemplateFile.xml";  
   p = cmd.CreateParameter();  
   p.Name="@ContactID";  
   p.Value = "1";  
   strm = cmd.ExecuteStream();  
   StreamReader sw = new StreamReader(strm);  
   Console.WriteLine(sw.ReadToEnd());  
   return 0;        
}  
```  
  
  
