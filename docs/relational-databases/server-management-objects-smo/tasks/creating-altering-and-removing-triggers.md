---
description: Création, modification et suppression de déclencheurs
title: Création, modification et suppression de déclencheurs
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- triggers [SMO]
ms.assetid: 8ddbe23b-6e31-4f8e-8a70-17bd5072413e
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 977fcad2724e6f16d447df6d580f187b525c2d57
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97431952"
---
# <a name="creating-altering-and-removing-triggers"></a>Création, modification et suppression de déclencheurs
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]
  Dans SMO, les déclencheurs sont représentés à l'aide de l'objet <xref:Microsoft.SqlServer.Management.Smo.Trigger>. Le [!INCLUDE[tsql](../../../includes/tsql-md.md)] code qui s’exécute lorsque le déclencheur est déclenché est défini par la <xref:Microsoft.SqlServer.Management.Smo.Trigger.TextBody%2A> propriété de l’objet déclencheur. Le type de déclencheur est défini à l'aide d'autres propriétés de l'objet <xref:Microsoft.SqlServer.Management.Smo.Trigger>, par exemple la propriété <xref:Microsoft.SqlServer.Management.Smo.Trigger.Update%2A>. Il s'agit d'une propriété booléenne qui spécifie si le déclencheur est activé par une **UPDATE** des enregistrements sur la table parente.  
  
 L'objet <xref:Microsoft.SqlServer.Management.Smo.Trigger> représente des déclencheurs traditionnels du langage de manipulation de données (DML). Dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] et les versions ultérieures, les déclencheurs DDL sont également pris en charge. Les déclencheurs DDL sont représentés par l'objet <xref:Microsoft.SqlServer.Management.Smo.DatabaseDdlTrigger> et l'objet <xref:Microsoft.SqlServer.Management.Smo.ServerDdlTrigger>.  
  
## <a name="example"></a>Exemple  
Pour utiliser un exemple de code qui est fourni, vous devrez choisir l'environnement de programmation, le modèle de programmation et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un projet Visual C&#35; Smo dans Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
 
  
## <a name="creating-altering-and-removing-a-trigger-in-visual-basic"></a>Création, modification et suppression d'un déclencheur en Visual Basic  
 Cet exemple de code montre comment créer et insérer un déclencheur de mise à jour sur une table existante, nommée `Sales`, dans la base de données [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . Le déclencheur envoie un message de rappel lorsque la table est mise à jour ou qu'un nouvel enregistrement est inséré.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim mysrv As Server
mysrv = New Server
'Reference the AdventureWorks2012 2008R2 database.
Dim mydb As Database
mydb = mysrv.Databases("AdventureWorks2012")
'Declare a Table object variable and reference the Customer table.
Dim mytab As Table
mytab = mydb.Tables("Customer", "Sales")
'Define a Trigger object variable by supplying the parent table, schema ,and name in the constructor.
Dim tr As Trigger
tr = New Trigger(mytab, "Sales")
'Set TextMode property to False, then set other properties to define the trigger.
tr.TextMode = False
tr.Insert = True
tr.Update = True
tr.InsertOrder = Agent.ActivationOrder.First
Dim stmt As String
stmt = " RAISERROR('Notify Customer Relations',16,10) "
tr.TextBody = stmt
tr.ImplementationType = ImplementationType.TransactSql
'Create the trigger on the instance of SQL Server.
tr.Create()
'Remove the trigger.
tr.Drop()
``` 
  
## <a name="creating-altering-and-removing-a-trigger-in-visual-c"></a>Création, modification et suppression d'un déclencheur en Visual C#  
 Cet exemple de code montre comment créer et insérer un déclencheur de mise à jour sur une table existante, nommée `Sales`, dans la base de données [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . Le déclencheur envoie un message de rappel lorsque la table est mise à jour ou qu'un nouvel enregistrement est inséré.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            Server mysrv;  
            mysrv = new Server();  
            //Reference the AdventureWorks2012 database.   
            Database mydb;  
            mydb = mysrv.Databases["AdventureWorks2012"];  
            //Declare a Table object variable and reference the Customer table.   
            Table mytab;  
            mytab = mydb.Tables["Customer", "Sales"];  
            //Define a Trigger object variable by supplying the parent table, schema ,and name in the constructor.   
            Trigger tr;  
            tr = new Trigger(mytab, "Sales");  
            //Set TextMode property to False, then set other properties to define the trigger.   
            tr.TextMode = false;  
            tr.Insert = true;  
            tr.Update = true;  
            tr.InsertOrder = ActivationOrder.First;  
            string stmt;  
            stmt = " RAISERROR('Notify Customer Relations',16,10) ";  
            tr.TextBody = stmt;  
            tr.ImplementationType = ImplementationType.TransactSql;  
            //Create the trigger on the instance of SQL Server.   
            tr.Create();  
            //Remove the trigger.   
            tr.Drop();  
        }  
```  
  
## <a name="creating-altering-and-removing-a-trigger-in-powershell"></a>Création, modification et suppression d'un déclencheur dans PowerShell  
 Cet exemple de code montre comment créer et insérer un déclencheur de mise à jour sur une table existante, nommée `Sales`, dans la base de données [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . Le déclencheur envoie un message de rappel lorsque la table est mise à jour ou qu'un nouvel enregistrement est inséré.  
  
```powershell  
# Set the path context to the local, default instance of SQL Server and to the  
#database tables in Adventureworks2012  
CD \sql\localhost\default\databases\AdventureWorks2012\Tables\  
  
#Get reference to the trigger's target table  
$mytab = get-item Sales.Customer  
  
# Define a Trigger object variable by supplying the parent table, schema ,and name in the constructor.  
$tr  = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Trigger `  
-argumentlist $mytab, "Sales"  
  
# Set TextMode property to False, then set other properties to define the trigger.   
$tr.TextMode = $false  
$tr.Insert = $true  
$tr.Update = $true  
$tr.InsertOrder = [Microsoft.SqlServer.Management.SMO.Agent.ActivationOrder]::First  
$tr.TextBody = " RAISERROR('Notify Customer Relations',16,10) "  
$tr.ImplementationType = [Microsoft.SqlServer.Management.SMO.ImplementationType]::TransactSql  
  
# Create the trigger on the instance of SQL Server.   
$tr.Create()  
  
#Remove the trigger.   
$tr.Drop()  
```  
  
  
