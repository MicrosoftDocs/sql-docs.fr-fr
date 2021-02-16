---
description: Codage d'un gestionnaire de connexions personnalisé
title: Codage d’un gestionnaire de connexions personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom connection managers [Integration Services], coding
ms.assetid: b12b6778-1f01-4a7d-984d-73f2f7630aa5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3ad9aa72bb972204bc762ff69692c82c4dda5963
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348929"
---
# <a name="coding-a-custom-connection-manager"></a>Codage d'un gestionnaire de connexions personnalisé

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir créé une classe qui hérite de la classe de base <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>, puis appliqué l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute> à cette classe, vous devez substituer l'implémentation des propriétés et des méthodes de la classe de base afin de fournir vos fonctionnalités personnalisées.  
  
 Pour obtenir des exemples de gestionnaires de connexions personnalisés, consultez [Développement d’une interface utilisateur pour un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md). Les exemples de code présentés dans cette rubrique sont tirés de l'exemple de gestionnaire de connexions personnalisé SQL Server.  
  
> [!NOTE]  
>  Une grande partie des tâches, sources et destinations intégrées à [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilisent uniquement des types spécifiques de gestionnaires de connexions intégrés. Par conséquent, ces exemples ne peuvent pas être testés avec les tâches et composants intégrés.  
  
## <a name="configuring-the-connection-manager"></a>Configuration du gestionnaire de connexions  
  
### <a name="setting-the-connectionstring-property"></a>Définition de la propriété ConnectionString  
 La propriété <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> est une propriété importante et la seule propriété propre à un gestionnaire de connexions personnalisé. Le gestionnaire de connexions utilise la valeur de cette propriété pour se connecter à la source de données externe. Si vous combinez plusieurs autres propriétés, telles que le nom du serveur et le nom de la base de données, pour créer la chaîne de connexion, vous pouvez utiliser une fonction d'assistance pour assembler la chaîne en remplaçant certaines valeurs dans un modèle de chaîne de connexion par la nouvelle valeur fournie par l'utilisateur. L'exemple de code suivant présente une implémentation de la propriété <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> qui compte sur une fonction d'assistance pour assembler la chaîne.  
  
```vb  
' Default values.  
Private _serverName As String = "(local)"  
Private _databaseName As String = "AdventureWorks"  
Private _connectionString As String = String.Empty  
  
Private Const CONNECTIONSTRING_TEMPLATE As String = _  
  "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=SSPI"  
  
Public Property ServerName() As String  
  Get  
    Return _serverName  
  End Get  
  Set(ByVal value As String)  
    _serverName = value  
  End Set  
End Property  
  
Public Property DatabaseName() As String  
  Get  
    Return _databaseName  
  End Get  
  Set(ByVal value As String)  
    _databaseName = value  
  End Set  
End Property  
  
Public Overrides Property ConnectionString() As String  
  Get  
    UpdateConnectionString()  
    Return _connectionString  
  End Get  
  Set(ByVal value As String)  
    _connectionString = value  
  End Set  
End Property  
  
Private Sub UpdateConnectionString()  
  
  Dim temporaryString As String = CONNECTIONSTRING_TEMPLATE  
  
  If Not String.IsNullOrEmpty(_serverName) Then  
    temporaryString = temporaryString.Replace("<servername>", _serverName)  
  End If  
  If Not String.IsNullOrEmpty(_databaseName) Then  
    temporaryString = temporaryString.Replace("<databasename>", _databaseName)  
  End If  
  
  _connectionString = temporaryString  
  
End Sub  
```  
  
```csharp  
// Default values.  
private string _serverName = "(local)";  
private string _databaseName = "AdventureWorks";  
private string _connectionString = String.Empty;  
  
private const string CONNECTIONSTRING_TEMPLATE = "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=SSPI";  
  
public string ServerName  
{  
  get  
  {  
    return _serverName;  
  }  
  set  
  {  
    _serverName = value;  
  }  
}  
  
public string DatabaseName  
{  
  get  
  {  
    return _databaseName;  
  }  
  set  
  {  
    _databaseName = value;  
  }  
}  
  
public override string ConnectionString  
{  
  get  
  {  
    UpdateConnectionString();  
    return _connectionString;  
  }  
  set  
  {  
    _connectionString = value;  
  }  
}  
  
private void UpdateConnectionString()  
{  
  
  string temporaryString = CONNECTIONSTRING_TEMPLATE;  
  
  if (!String.IsNullOrEmpty(_serverName))  
  {  
    temporaryString = temporaryString.Replace("<servername>", _serverName);  
  }  
  
  if (!String.IsNullOrEmpty(_databaseName))  
  {  
    temporaryString = temporaryString.Replace("<databasename>", _databaseName);  
  }  
  
  _connectionString = temporaryString;  
  
}  
```  
  
### <a name="validating-the-connection-manager"></a>Validation du gestionnaire de connexions  
 Vous remplacez la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A> pour vous assurer que le gestionnaire de connexions a été configuré correctement. Au minimum, vous devez valider le format de la chaîne de connexion et vous assurer que les valeurs ont été fournies pour tous les arguments. L'exécution ne peut pas se poursuivre tant que le gestionnaire de connexions ne renvoie pas <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> à partir de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A>.  
  
 L'exemple de code suivant présente une implémentation de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A> qui permet de s'assurer que l'utilisateur a spécifié un nom de serveur pour la connexion.  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents) As Microsoft.SqlServer.Dts.Runtime.DTSExecResult  
  
  If String.IsNullOrEmpty(_serverName) Then  
    infoEvents.FireError(0, "SqlConnectionManager", "No server name specified", String.Empty, 0)  
    Return DTSExecResult.Failure  
  Else  
    Return DTSExecResult.Success  
  End If  
  
End Function  
```  
  
```csharp  
public override Microsoft.SqlServer.Dts.Runtime.DTSExecResult Validate(Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents infoEvents)  
{  
  
  if (String.IsNullOrEmpty(_serverName))  
  {  
    infoEvents.FireError(0, "SqlConnectionManager", "No server name specified", String.Empty, 0);  
    return DTSExecResult.Failure;  
  }  
  else  
  {  
    return DTSExecResult.Success;  
  }  
  
}  
```  
  
### <a name="persisting-the-connection-manager"></a>Persistance du gestionnaire de connexions  
 En général, vous n'avez pas à implémenter de persistance personnalisée pour un gestionnaire de connexions. Une persistance personnalisée est uniquement requise lorsque les propriétés d'un objet utilisent des types de données complexes. Pour plus d’informations, consultez [Développement d’objets personnalisés pour Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md).  
  
## <a name="working-with-the-external-data-source"></a>Utilisation de la source de données externe  
 Les méthodes qui prennent en charge la connexion à une source de données externe sont les méthodes les plus importantes d'un gestionnaire de connexions personnalisé. Les méthodes <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A> sont appelées à différents instants à la fois au moment de la conception et au moment de l'exécution.  
  
### <a name="acquiring-the-connection"></a>Acquisition de la connexion  
 Vous devez déterminer quel type d'objet il convient que la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> renvoie à partir de votre gestionnaire de connexions personnalisé. Par exemple, un gestionnaire de connexions de fichiers renvoie uniquement une chaîne qui contient un chemin d'accès et un nom de fichier, alors qu'un gestionnaire de connexions ADO.NET renvoie un objet de connexion managée qui est déjà ouvert. Un gestionnaire de connexions OLE DB renvoie un objet de connexion OLE DB natif qui ne peut pas être utilisé à partir de code managé. Le gestionnaire de connexions SQL Server personnalisé, duquel proviennent les extraits de code de cette rubrique, renvoie un objet **SqlConnection** ouvert.  
  
 Les utilisateurs de votre gestionnaire de connexions ont besoin de savoir à l'avance quel type d'objet attendre, afin de pouvoir convertir l'objet retourné en type approprié et d'accéder à ses méthodes et propriétés.  
  
```vb  
Public Overrides Function AcquireConnection(ByVal txn As Object) As Object  
  
  Dim sqlConnection As New SqlConnection  
  
  UpdateConnectionString()  
  
  With sqlConnection  
    .ConnectionString = _connectionString  
    .Open()  
  End With  
  
  Return sqlConnection  
  
End Function  
```  
  
```csharp  
public override object AcquireConnection(object txn)  
{  
  
  SqlConnection sqlConnection = new SqlConnection();  
  
  UpdateConnectionString();  
  
  {  
    sqlConnection.ConnectionString = _connectionString;  
    sqlConnection.Open();  
  }  
  
  return sqlConnection;  
  
}  
```  
  
### <a name="releasing-the-connection"></a>Libération de la connexion  
 La mesure que vous prenez dans la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A> dépend du type d'objet que vous avez retourné de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>. S'il existe un objet de connexion ouvert, vous devez le fermer et libérer toutes les ressources qu'il utilise. Si la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> a retourné uniquement une valeur de chaîne, aucune action ne doit être entreprise.  
  
```vb  
Public Overrides Sub ReleaseConnection(ByVal connection As Object)  
  
  Dim sqlConnection As SqlConnection  
  
  sqlConnection = DirectCast(connection, SqlConnection)  
  
  If sqlConnection.State <> ConnectionState.Closed Then  
    sqlConnection.Close()  
  End If  
  
End Sub  
```  
  
```csharp  
public override void ReleaseConnection(object connection)  
{  
  SqlConnection sqlConnection;  
  sqlConnection = (SqlConnection)connection;  
  if (sqlConnection.State != ConnectionState.Closed)  
    sqlConnection.Close();  
}  
```  
 
## <a name="see-also"></a>Voir aussi  
 [Création d’un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)   
 [Développement d'une interface utilisateur pour un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
  
  
