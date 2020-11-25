---
title: Fonctions de Table-Valued CLR | Microsoft Docs
description: Une fonction table retourne une table. Dans SQL Server intégration du CLR, vous pouvez écrire des fonctions table dans du code managé.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
dev_langs:
- TSQL
- VB
- CSharp
helpviewer_keywords:
- Transact-SQL table-valued functions
- table-valued functions [CLR integration]
- TVFs [CLR integration]
ms.assetid: 9a6133ea-36e9-45bf-b572-1c0df3d6c194
author: rothja
ms.author: jroth
ms.openlocfilehash: 4295ca970e503ad1785846d63e5ed479923f4303
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125289"
---
# <a name="clr-table-valued-functions"></a>Fonctions table CLR
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Une fonction table est une fonction définie par l'utilisateur qui retourne une table.  
  
 Depuis [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] étend les fonctionnalités des fonctions table en vous permettant de définir une fonction table dans n'importe quel langage managé. Les données sont retournées à partir d’une fonction table par le biais d’un objet **IEnumerable** ou **IEnumerator** .  
  
> [!NOTE]  
>  Pour les fonctions table, les colonnes du type de table de retour ne peuvent pas inclure de colonnes timestamp ou de colonnes de type de données de chaîne non-Unicode (telles que **char**, **varchar** et **Text**). La contrainte NOT NULL n'est pas prise en charge.  
  
 Pour plus d’informations sur les fonctions de Table-Valued du CLR, consultez [Introduction à SQL Server fonctions table CLR !](https://www.mssqltips.com/sqlservertip/2582/introduction-to-sql-server-clr-table-valued-functions/)  
  
## <a name="differences-between-transact-sql-and-clr-table-valued-functions"></a>Différences entre les fonctions table Transact-SQL et CLR  
 Les fonctions table [!INCLUDE[tsql](../../includes/tsql-md.md)] matérialisent les résultats de l'appel de la fonction dans une table intermédiaire. Dans la mesure où elles utilisent une table intermédiaire, elles peuvent prendre en charge des contraintes et des index uniques sur les résultats. Ces fonctionnalités peuvent être extrêmement utiles lorsque des résultats sont retournés en grande quantité.  
  
 Par opposition, les fonctions table CLR représentent une solution d'accès en continu. Il n'est pas obligatoire que le jeu de résultats entier soit matérialisé dans une table unique. L’objet **IEnumerable** retourné par la fonction managée est appelé directement par le plan d’exécution de la requête qui appelle la fonction table, et les résultats sont consommés de manière incrémentielle. Ce modèle d'accès en continu garantit que les résultats peuvent être consommés dès que la première ligne est disponible, au lieu d'attendre le remplissage de la table entière. Il s'agit également d'une solution plus appropriée si de très nombreuses lignes sont retournées, car elles n'ont pas à être matérialisées en totalité en mémoire. Par exemple, une fonction table managée peut être utilisée pour analyser un fichier texte et retourner chaque ligne de texte sous forme de ligne de table.  
  
## <a name="implementing-table-valued-functions"></a>Implémentation de fonctions table  
 Vous devez implémenter les fonctions table en tant que méthodes d'une classe dans un assembly [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework. Votre code de fonction table doit implémenter l’interface **IEnumerable** . L’interface **IEnumerable** est définie dans le .NET Framework. Les types représentant des tableaux et des collections dans le .NET Framework implémentent déjà l’interface **IEnumerable** . Cela facilite l'écriture de fonctions table qui convertissent une collection ou un tableau en un jeu de résultats.  
  
## <a name="table-valued-parameters"></a>Paramètres table  
 Les paramètres table sont des types de tables définis par l'utilisateur et passés à une procédure ou une fonction, et qui offrent un moyen efficace pour passer plusieurs lignes de données au serveur. Ils procurent une fonctionnalité semblable aux tableaux de paramètres, mais offrent une meilleure souplesse et une intégration plus étroite à [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ils sont également susceptibles de générer de meilleures performances. Les paramètres table permettent également de réduire le nombre d’allers-retours au serveur. Au lieu d’envoyer plusieurs demandes au serveur, comme avec une liste de paramètres scalaires, les données peuvent être envoyées au serveur en tant que paramètre table. Un type de table défini par l'utilisateur ne peut pas être passé en tant que paramètre table à une fonction ou à une procédure stockée managée s'exécutant dans le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ni être retourné à partir de ces dernières. Pour plus d’informations sur les paramètres table, consultez [Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
## <a name="output-parameters-and-table-valued-functions"></a>Paramètres de sortie et fonctions table  
 Des informations peuvent être retournées à partir de fonctions table via des paramètres de sortie. Le paramètre correspondant de la fonction table du code d'implémentation doit utiliser un paramètre passé par référence en guise d'argument. Notez que Visual Basic ne prend pas en charge les paramètres de sortie de la même manière que Visual C#. Vous devez spécifier le paramètre par référence et appliquer l' \<Out()> attribut pour représenter un paramètre de sortie, comme suit :  
  
```vb  
Imports System.Runtime.InteropServices  
...  
Public Shared Sub FillRow ( <Out()> ByRef value As SqlInt32)  
```  
  
### <a name="defining-a-table-valued-function-in-transact-sql"></a>Définition d'une fonction table dans Transact-SQL  
 La syntaxe permettant de définir une fonction table CLR est semblable à celle d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] fonction table, avec l’ajout de la clause **External Name** . Par exemple :  
  
```  
CREATE FUNCTION GetEmpFirstLastNames()  
RETURNS TABLE (FirstName NVARCHAR(4000), LastName NVARCHAR(4000))  
EXTERNAL NAME MyDotNETAssembly.[MyNamespace.MyClassname]. GetEmpFirstLastNames;  
```  
  
 Les fonctions table sont utilisées pour représenter les données sous forme relationnelle pour un traitement supplémentaire dans les requêtes, par exemple :  
  
```  
select * from function();  
select * from tbl join function() f on tbl.col = f.col;  
select * from table t cross apply function(t.column);  
```  
  
 Les fonctions table peuvent retourner une table dans les circonstances suivantes :  
  
-   Les fonctions table sont créées à partir d'arguments d'entrée scalaires. Par exemple, une fonction table peut accepter une chaîne de nombres délimités par des virgules et retourner ces derniers sous forme de table.  
  
-   Les fonctions table sont générées à partir de données externes. Par exemple, une fonction table peut lire le journal des événements et l'exposer sous forme de table.  
  
 **Remarque** Une fonction table peut uniquement effectuer l’accès aux données par le biais d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] requête dans la méthode **InitMethod** , et non dans la méthode **FillRow** . Le **InitMethod** doit être marqué avec la propriété d’attribut **SqlFunction. DataAccess. Read** si une [!INCLUDE[tsql](../../includes/tsql-md.md)] requête est exécutée.  
  
## <a name="a-sample-table-valued-function"></a>Exemple de fonction table  
 La fonction table suivante retourne des informations du journal des événements système. La fonction accepte un seul argument de chaîne contenant le nom du journal des événements à lire.  
  
###### <a name="sample-code"></a>Exemple de code  
  
```csharp  
using System;  
using System.Data.Sql;  
using Microsoft.SqlServer.Server;  
using System.Collections;  
using System.Data.SqlTypes;  
using System.Diagnostics;  
  
public class TabularEventLog  
{  
    [SqlFunction(FillRowMethodName = "FillRow")]  
    public static IEnumerable InitMethod(String logname)  
    {  
        return new EventLog(logname).Entries;
    }  
  
    public static void FillRow(Object obj, out SqlDateTime timeWritten, out SqlChars message, out SqlChars category, out long instanceId)  
    {  
        EventLogEntry eventLogEntry = (EventLogEntry)obj;  
        timeWritten = new SqlDateTime(eventLogEntry.TimeWritten);  
        message = new SqlChars(eventLogEntry.Message);  
        category = new SqlChars(eventLogEntry.Category);  
        instanceId = eventLogEntry.InstanceId;  
    }  
}  
```  
  
```vb  
Imports System  
Imports System.Data.Sql  
Imports Microsoft.SqlServer.Server  
Imports System.Collections  
Imports System.Data.SqlTypes  
Imports System.Diagnostics  
Imports System.Runtime.InteropServices  
  
Public Class TabularEventLog  
    <SqlFunction(FillRowMethodName:="FillRow")> _  
    Public Shared Function InitMethod(ByVal logname As String) As IEnumerable  
        Return New EventLog(logname).Entries  
    End Function  
  
    Public Shared Sub FillRow(ByVal obj As Object, <Out()> ByRef timeWritten As SqlDateTime, <Out()> ByRef message As SqlChars, <Out()> ByRef category As SqlChars, <Out()> ByRef instanceId As Long)  
        Dim eventLogEnTry As EventLogEntry = CType(obj, EventLogEntry)  
        timeWritten = New SqlDateTime(eventLogEnTry.TimeWritten)  
        message = New SqlChars(eventLogEnTry.Message)  
        category = New SqlChars(eventLogEnTry.Category)  
        instanceId = eventLogEnTry.InstanceId  
    End Sub  
End Class  
```  
  
###### <a name="declaring-and-using-the-sample-table-valued-function"></a>Déclaration et utilisation de l'exemple de fonction table  
 Une fois l'exemple de fonction table compilé, il peut être déclaré en [!INCLUDE[tsql](../../includes/tsql-md.md)] comme suit :  
  
```  
use master;  
-- Replace SQL_Server_logon with your SQL Server user credentials.  
GRANT EXTERNAL ACCESS ASSEMBLY TO [SQL_Server_logon];   
-- Modify the following line to specify a different database.  
ALTER DATABASE master SET TRUSTWORTHY ON;  
  
-- Modify the next line to use the appropriate database.  
CREATE ASSEMBLY tvfEventLog   
FROM 'D:\assemblies\tvfEventLog\tvfeventlog.dll'   
WITH PERMISSION_SET = EXTERNAL_ACCESS;  
GO  
CREATE FUNCTION ReadEventLog(@logname nvarchar(100))  
RETURNS TABLE   
(logTime datetime,Message nvarchar(4000),Category nvarchar(4000),InstanceId bigint)  
AS   
EXTERNAL NAME tvfEventLog.TabularEventLog.InitMethod;  
GO  
```  
  
 L'exécution des objets de base de données Visual C++ compilés avec /clr:pure n'est pas prise en charge sur [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Par exemple, de tels objets de base de données incluent des fonctions table.  
  
 Pour tester l'exemple, essayez le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant :  
  
```  
-- Select the top 100 events,  
SELECT TOP 100 *  
FROM dbo.ReadEventLog(N'Security') as T;  
go  
  
-- Select the last 10 login events.  
SELECT TOP 10 T.logTime, T.Message, T.InstanceId   
FROM dbo.ReadEventLog(N'Security') as T  
WHERE T.Category = N'Logon/Logoff';  
go  
```  
  
## <a name="sample-returning-the-results-of-a-sql-server-query"></a>Exemple : retour des résultats d'une requête SQL Server  
 L'exemple suivant illustre une fonction table qui interroge une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cet exemple utilise la base de données AdventureWorks Light de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]. [https://www.codeplex.com/sqlserversamples](https://go.microsoft.com/fwlink/?LinkId=87843)Pour plus d’informations sur le téléchargement d’AdventureWorks, consultez.  
  
 Attribuez à votre fichier de code source le nom FindInvalidEmails.cs ou FindInvalidEmails.vb.  
  
```csharp  
using System;  
using System.Collections;  
using System.Data;  
using System.Data.SqlClient;  
using System.Data.SqlTypes;  
  
using Microsoft.SqlServer.Server;  
  
public partial class UserDefinedFunctions {  
   private class EmailResult {  
      public SqlInt32 CustomerId;  
      public SqlString EmailAdress;  
  
      public EmailResult(SqlInt32 customerId, SqlString emailAdress) {  
         CustomerId = customerId;  
         EmailAdress = emailAdress;  
      }  
   }  
  
   public static bool ValidateEmail(SqlString emailAddress) {  
      if (emailAddress.IsNull)  
         return false;  
  
      if (!emailAddress.Value.EndsWith("@adventure-works.com"))  
         return false;  
  
      // Validate the address. Put any more rules here.  
      return true;  
   }  
  
   [SqlFunction(  
       DataAccess = DataAccessKind.Read,  
       FillRowMethodName = "FindInvalidEmails_FillRow",  
       TableDefinition="CustomerId int, EmailAddress nvarchar(4000)")]  
   public static IEnumerable FindInvalidEmails(SqlDateTime modifiedSince) {  
      ArrayList resultCollection = new ArrayList();  
  
      using (SqlConnection connection = new SqlConnection("context connection=true")) {  
         connection.Open();  
  
         using (SqlCommand selectEmails = new SqlCommand(  
             "SELECT " +  
             "[CustomerID], [EmailAddress] " +  
             "FROM [AdventureWorksLT2008].[SalesLT].[Customer] " +  
             "WHERE [ModifiedDate] >= @modifiedSince",  
             connection)) {  
            SqlParameter modifiedSinceParam = selectEmails.Parameters.Add(  
                "@modifiedSince",  
                SqlDbType.DateTime);  
            modifiedSinceParam.Value = modifiedSince;  
  
            using (SqlDataReader emailsReader = selectEmails.ExecuteReader()) {  
               while (emailsReader.Read()) {  
                  SqlString emailAddress = emailsReader.GetSqlString(1);  
                  if (ValidateEmail(emailAddress)) {  
                     resultCollection.Add(new EmailResult(  
                         emailsReader.GetSqlInt32(0),  
                         emailAddress));  
                  }  
               }  
            }  
         }  
      }  
  
      return resultCollection;  
   }  
  
   public static void FindInvalidEmails_FillRow(  
       object emailResultObj,  
       out SqlInt32 customerId,  
       out SqlString emailAdress) {  
      EmailResult emailResult = (EmailResult)emailResultObj;  
  
      customerId = emailResult.CustomerId;  
      emailAdress = emailResult.EmailAdress;  
   }  
};  
```  
  
```vb  
Imports System.Collections  
Imports System.Data  
Imports System.Data.SqlClient  
Imports System.Data.SqlTypes  
Imports Microsoft.SqlServer.Server  
  
Public Partial Class UserDefinedFunctions  
   Private Class EmailResult  
      Public CustomerId As SqlInt32  
      Public EmailAdress As SqlString  
  
      Public Sub New(customerId__1 As SqlInt32, emailAdress__2 As SqlString)  
         CustomerId = customerId__1  
         EmailAdress = emailAdress__2  
      End Sub  
   End Class  
  
   Public Shared Function ValidateEmail(emailAddress As SqlString) As Boolean  
      If emailAddress.IsNull Then  
         Return False  
      End If  
  
      If Not emailAddress.Value.EndsWith("@adventure-works.com") Then  
         Return False  
      End If  
  
      ' Validate the address. Put any more rules here.  
      Return True  
   End Function  
  
   <SqlFunction(DataAccess := DataAccessKind.Read, FillRowMethodName := "FindInvalidEmails_FillRow", TableDefinition := "CustomerId int, EmailAddress nvarchar(4000)")> _  
   Public Shared Function FindInvalidEmails(modifiedSince As SqlDateTime) As IEnumerable  
      Dim resultCollection As New ArrayList()  
  
      Using connection As New SqlConnection("context connection=true")  
         connection.Open()  
  
         Using selectEmails As New SqlCommand("SELECT " & "[CustomerID], [EmailAddress] " & "FROM [AdventureWorksLT2008].[SalesLT].[Customer] " & "WHERE [ModifiedDate] >= @modifiedSince", connection)  
            Dim modifiedSinceParam As SqlParameter = selectEmails.Parameters.Add("@modifiedSince", SqlDbType.DateTime)  
            modifiedSinceParam.Value = modifiedSince  
  
            Using emailsReader As SqlDataReader = selectEmails.ExecuteReader()  
               While emailsReader.Read()  
                  Dim emailAddress As SqlString = emailsReader.GetSqlString(1)  
                  If ValidateEmail(emailAddress) Then  
                     resultCollection.Add(New EmailResult(emailsReader.GetSqlInt32(0), emailAddress))  
                  End If  
               End While  
            End Using  
         End Using  
      End Using  
  
      Return resultCollection  
   End Function  
  
   Public Shared Sub FindInvalidEmails_FillRow(emailResultObj As Object, ByRef customerId As SqlInt32, ByRef emailAdress As SqlString)  
      Dim emailResult As EmailResult = DirectCast(emailResultObj, EmailResult)  
  
      customerId = emailResult.CustomerId  
      emailAdress = emailResult.EmailAdress  
   End Sub  
End ClassImports System.Collections  
Imports System.Data  
Imports System.Data.SqlClient  
Imports System.Data.SqlTypes  
Imports Microsoft.SqlServer.Server  
  
Public Partial Class UserDefinedFunctions  
   Private Class EmailResult  
      Public CustomerId As SqlInt32  
      Public EmailAdress As SqlString  
  
      Public Sub New(customerId__1 As SqlInt32, emailAdress__2 As SqlString)  
         CustomerId = customerId__1  
         EmailAdress = emailAdress__2  
      End Sub  
   End Class  
  
   Public Shared Function ValidateEmail(emailAddress As SqlString) As Boolean  
      If emailAddress.IsNull Then  
         Return False  
      End If  
  
      If Not emailAddress.Value.EndsWith("@adventure-works.com") Then  
         Return False  
      End If  
  
      ' Validate the address. Put any more rules here.  
      Return True  
   End Function  
  
   <SqlFunction(DataAccess := DataAccessKind.Read, FillRowMethodName := "FindInvalidEmails_FillRow", TableDefinition := "CustomerId int, EmailAddress nvarchar(4000)")> _  
   Public Shared Function FindInvalidEmails(modifiedSince As SqlDateTime) As IEnumerable  
      Dim resultCollection As New ArrayList()  
  
      Using connection As New SqlConnection("context connection=true")  
         connection.Open()  
  
         Using selectEmails As New SqlCommand("SELECT " & "[CustomerID], [EmailAddress] " & "FROM [AdventureWorksLT2008].[SalesLT].[Customer] " & "WHERE [ModifiedDate] >= @modifiedSince", connection)  
            Dim modifiedSinceParam As SqlParameter = selectEmails.Parameters.Add("@modifiedSince", SqlDbType.DateTime)  
            modifiedSinceParam.Value = modifiedSince  
  
            Using emailsReader As SqlDataReader = selectEmails.ExecuteReader()  
               While emailsReader.Read()  
                  Dim emailAddress As SqlString = emailsReader.GetSqlString(1)  
                  If ValidateEmail(emailAddress) Then  
                     resultCollection.Add(New EmailResult(emailsReader.GetSqlInt32(0), emailAddress))  
                  End If  
               End While  
            End Using  
         End Using  
      End Using  
  
      Return resultCollection  
   End Function  
  
   Public Shared Sub FindInvalidEmails_FillRow(emailResultObj As Object, customerId As SqlInt32, emailAdress As SqlString)  
      Dim emailResult As EmailResult = DirectCast(emailResultObj, EmailResult)  
  
      customerId = emailResult.CustomerId  
      emailAdress = emailResult.EmailAdress  
   End Sub  
End Class  
```  
  
 Compilez le code source dans une DLL et copiez la DLL dans le répertoire racine de votre lecteur C.  Exécutez ensuite la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante.  
  
```  
use AdventureWorksLT2008;  
go  
  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'FindInvalidEmails')  
   DROP FUNCTION FindInvalidEmails;  
go  
  
IF EXISTS (SELECT name FROM sys.assemblies WHERE name = 'MyClrCode')  
   DROP ASSEMBLY MyClrCode;  
go  
  
CREATE ASSEMBLY MyClrCode FROM 'C:\FindInvalidEmails.dll'  
WITH PERMISSION_SET = SAFE -- EXTERNAL_ACCESS;  
GO  
  
CREATE FUNCTION FindInvalidEmails(@ModifiedSince datetime)   
RETURNS TABLE (  
   CustomerId int,  
   EmailAddress nvarchar(4000)  
)  
AS EXTERNAL NAME MyClrCode.UserDefinedFunctions.[FindInvalidEmails];  
go  
  
SELECT * FROM FindInvalidEmails('2000-01-01');  
go  
```  
  
  
