---
description: Création, modification et suppression de règles
title: Création, modification et suppression de règles
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- rules [SMO]
ms.assetid: 16981459-524e-4b39-a899-4370eaf763cc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f3fcf4dca6ec2cc56ddab8a847afd87c96811e96
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88420243"
---
# <a name="creating-altering-and-removing-rules"></a>Création, modification et suppression de règles
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Dans SMO, les règles sont représentées par l'objet <xref:Microsoft.SqlServer.Management.Smo.Rule>. La règle est définie par la propriété <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.TextBody%2A>, qui est une chaîne de texte contenant une expression de condition qui utilise des opérateurs ou des prédicats, par exemple IN, LIKE ou BETWEEN. Elle ne peut pas faire référence à des colonnes ou à d'autres objets de base de données. Vous pouvez y inclure des fonctions intégrées qui ne font pas référence à des objets de base de données.  
  
 La définition dans la propriété <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.TextBody%2A> doit contenir une variable qui fait référence à la valeur de données entrée. Vous pouvez utiliser n’importe quel nom ou symbole pour représenter la valeur lors de la création de la règle, mais le premier caractère doit être le \@ symbole.  
  
## <a name="example"></a>Exemple  
 Pour utiliser un exemple de code qui est fourni, vous devrez choisir l'environnement de programmation, le modèle de programmation et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un projet Visual C&#35; Smo dans Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-altering-and-removing-a-rule-in-visual-basic"></a>Création, modification et suppression d'une règle en Visual Basic  
 Cet exemple de code montre comment créer une règle, l'attacher à une colonne, modifier des propriétés de l'objet <xref:Microsoft.SqlServer.Management.Smo.Rule>, la détacher de la colonne, puis la supprimer.  
  
 L’instruction **Dim** pour l' <xref:Microsoft.SqlServer.Management.Smo.Rule> objet est spécifiée avec le chemin d’accès complet de l’assembly pour éviter toute ambiguïté avec un <xref:Microsoft.SqlServer.Management.Smo.Rule> objet dans l’assembly System. Data.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Declare a Table object variable and reference the Product table.
Dim tb As Table
tb = db.Tables("Product", "Production")
'Define a Rule object variable by supplying the parent database, name and schema in the constructor. 
'Note that the full namespace must be given for the Rule type to differentiate it from other Rule types.
Dim ru As Microsoft.SqlServer.Management.Smo.Rule
ru = New Rule(db, "TestRule", "Production")
'Set the TextHeader and TextBody properties to define the rule.
ru.TextHeader = "CREATE RULE [Production].[TestRule] AS"
ru.TextBody = "@value BETWEEN GETDATE() AND DATEADD(year,4,GETDATE())"
'Create the rule on the instance of SQL Server.
ru.Create()
'Bind the rule to a column in the Product table by supplying the table, schema, and 
'column as arguments in the BindToColumn method.
ru.BindToColumn("Product", "SellEndDate", "Production")
'Unbind from the column before removing the rule from the database.
ru.UnbindFromColumn("Product", "SellEndDate", "Production")
ru.Drop()
```
  
## <a name="creating-altering-and-removing-a-rule-in-visual-c"></a>Création, modification et suppression d'une règle en Visual C#  
 Cet exemple de code montre comment créer une règle, l'attacher à une colonne, modifier des propriétés de l'objet <xref:Microsoft.SqlServer.Management.Smo.Rule>, la détacher de la colonne, puis la supprimer.  
  
 L’instruction **Dim** pour l' <xref:Microsoft.SqlServer.Management.Smo.Rule> objet est spécifiée avec le chemin d’accès complet de l’assembly pour éviter toute ambiguïté avec un <xref:Microsoft.SqlServer.Management.Smo.Rule> objet dans l’assembly System. Data.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            Server srv;  
            srv = new Server();  
            //Reference the AdventureWorks2012 database.   
            Database db;  
            db = srv.Databases["AdventureWorks2012"];  
  
            //Define a Rule object variable by supplying the parent database, name and schema in the constructor.   
            //Note that the full namespace must be given for the Rule type to differentiate it from other Rule types.   
            Microsoft.SqlServer.Management.Smo.Rule ru;  
            ru = new Rule(db, "TestRule", "Production");  
            //Set the TextHeader and TextBody properties to define the rule.   
            ru.TextHeader = "CREATE RULE [Production].[TestRule] AS";  
            ru.TextBody = "@value BETWEEN GETDATE() AND DATEADD(year,4,GETDATE())";  
            //Create the rule on the instance of SQL Server.   
            ru.Create();  
            //Bind the rule to a column in the Product table by supplying the table, schema, and   
            //column as arguments in the BindToColumn method.   
            ru.BindToColumn("Product", "SellEndDate", "Production");  
            //Unbind from the column before removing the rule from the database.   
            ru.UnbindFromColumn("Product", "SellEndDate", "Production");  
            ru.Drop();  
  
        }  
```  
  
## <a name="creating-altering-and-removing-a-rule-in-powershell"></a>Création, modification et suppression d'une règle dans PowerShell  
 Cet exemple de code montre comment créer une règle, l'attacher à une colonne, modifier des propriétés de l'objet <xref:Microsoft.SqlServer.Management.Smo.Rule>, la détacher de la colonne, puis la supprimer.  
  
 L’instruction **Dim** pour l' <xref:Microsoft.SqlServer.Management.Smo.Rule> objet est spécifiée avec le chemin d’accès complet de l’assembly pour éviter toute ambiguïté avec un <xref:Microsoft.SqlServer.Management.Smo.Rule> objet dans l’assembly System. Data.  
  
```powershell   
# Set the path context to the local, default instance of SQL Server and get a reference to AdventureWorks2012  
CD \sql\localhost\default\databases  
$db = get-item Adventureworks2012  
  
# Define a Rule object variable by supplying the parent database, name and schema in the constructor.   
$ru = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Rule `  
-argumentlist $db, "TestRule", "Production"  
  
#Set the TextHeader and TextBody properties to define the rule.   
$ru.TextHeader = "CREATE RULE [Production].[TestRule] AS"  
$ru.TextBody = "@value BETWEEN GETDATE() AND DATEADD(year,4,GETDATE())"  
  
#Create the rule on the instance of SQL Server.   
$ru.Create()  
  
# Bind the rule to a column in the Product table by supplying the table, schema, and   
# column as arguments in the BindToColumn method.   
$ru.BindToColumn("Product", "SellEndDate", "Production")  
  
#Unbind from the column before removing the rule from the database.   
$ru.UnbindFromColumn("Product", "SellEndDate", "Production")  
$ru.Drop()  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.SqlServer.Management.Smo.Rule>  
  
  
