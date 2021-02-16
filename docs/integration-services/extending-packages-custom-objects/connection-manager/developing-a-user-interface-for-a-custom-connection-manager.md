---
description: Développement d'une interface utilisateur pour un gestionnaire de connexions personnalisé
title: Développement d’une interface utilisateur pour un gestionnaire de connexions personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom connection managers [Integration Services], developing user interface
- custom user interface [Integration Services], custom connection manager
ms.assetid: 908bf2ac-fc84-4af8-a869-1cb43573d2df
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a1c127fde2a5e0b19f890b2b84a411b3f8e98d7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348903"
---
# <a name="developing-a-user-interface-for-a-custom-connection-manager"></a>Développement d'une interface utilisateur pour un gestionnaire de connexions personnalisé

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir remplacé l'implémentation des propriétés et méthodes de la classe de base afin de fournir vos fonctionnalités personnalisées, vous pouvez créer une interface utilisateur personnalisée pour votre gestionnaire de connexions. Si vous ne créez pas d'interface utilisateur personnalisée, les utilisateurs peuvent configurer votre gestionnaire de connexions uniquement en utilisant la fenêtre Propriétés.  
  
 Dans un projet d’interface utilisateur ou d’assembly personnalisé, vous avez normalement deux classes : une classe qui implémente l’objet <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI> et le formulaire Windows qu’il affiche pour collecter des informations auprès de l’utilisateur.  
  
> [!IMPORTANT]  
>  Après avoir signé et généré votre interface utilisateur personnalisée, puis l’avoir installée dans le Global Assembly Cache comme décrit dans [Codage d’un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md), n’oubliez pas de fournir le nom complet de cette classe dans la propriété <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute.UITypeName%2A> de la classe <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute>.  
  
> [!NOTE]  
>  Une grande partie des tâches, sources et destinations intégrées à [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilisent uniquement des types spécifiques de gestionnaires de connexions intégrés. Par conséquent, ces exemples ne peuvent pas être testés avec les tâches et composants intégrés.  
  
## <a name="coding-the-user-interface-class"></a>Codage de la classe d'interface utilisateur  
 L'interface <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI> comporte quatre méthodes : <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Initialize%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.New%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Edit%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Delete%2A>. Les sections suivantes décrivent ces quatre méthodes.  
  
> [!NOTE]  
>  Il se peut que vous n'ayez pas à écrire de code pour la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Delete%2A> si aucun nettoyage n'est requis lorsque l'utilisateur supprime une instance du gestionnaire de connexions.  
  
### <a name="initializing-the-user-interface"></a>Initialisation de l'interface utilisateur  
 Dans la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Initialize%2A>, le concepteur fournit une référence au gestionnaire de connexions en cours de configuration afin que la classe d'interface utilisateur puisse modifier les propriétés du gestionnaire de connexions. Comme indiqué dans le code suivant, votre code doit mettre en cache la référence au gestionnaire de connexions en vue d’une utilisation ultérieure.  
  
```vb  
Public Sub Initialize(ByVal connectionManager As Microsoft.SqlServer.Dts.Runtime.ConnectionManager, ByVal serviceProvider As System.IServiceProvider) Implements Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Initialize  
  
    _connectionManager = connectionManager  
    _serviceProvider = serviceProvider  
  
  End Sub  
```  
  
```csharp  
public void Initialize(Microsoft.SqlServer.Dts.Runtime.ConnectionManager connectionManager, System.IServiceProvider serviceProvider)  
{  
  
  _connectionManager = connectionManager;  
  _serviceProvider = serviceProvider;  
  
}  
```  
  
### <a name="creating-a-new-instance-of-the-user-interface"></a>Création d'une instance de l'interface utilisateur  
 La méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.New%2A>, qui n'est pas un constructeur, est appelée après la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Initialize%2A> lorsque l'utilisateur crée une nouvelle instance du gestionnaire de connexions. Dans la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.New%2A>, vous souhaitez généralement afficher le formulaire à modifier, à moins que l'utilisateur ait copié et collé un gestionnaire de connexions existant. Le code suivant illustre une implémentation de cette méthode.  
  
```vb  
Public Function [New](ByVal parentWindow As System.Windows.Forms.IWin32Window, ByVal connections As Microsoft.SqlServer.Dts.Runtime.Connections, ByVal connectionUIArgs As Microsoft.SqlServer.Dts.Runtime.Design.ConnectionManagerUIArgs) As Boolean Implements Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.New  
  
  Dim clipboardService As IDtsClipboardService  
  
  clipboardService = _  
    DirectCast(_serviceProvider.GetService(GetType(IDtsClipboardService)), IDtsClipboardService)  
  If Not clipboardService Is Nothing Then  
    ' If the connection manager has been copied and pasted, take no action.  
    If clipboardService.IsPasteActive Then  
      Return True  
    End If  
  End If  
  
  Return EditSqlConnection(parentWindow)  
  
End Function  
```  
  
```csharp  
public bool New(System.Windows.Forms.IWin32Window parentWindow, Microsoft.SqlServer.Dts.Runtime.Connections connections, Microsoft.SqlServer.Dts.Runtime.Design.ConnectionManagerUIArgs connectionUIArgs)  
  {  
    IDtsClipboardService clipboardService;  
  
    clipboardService = (IDtsClipboardService)_serviceProvider.GetService(typeof(IDtsClipboardService));  
    if (clipboardService != null)  
    // If connection manager has been copied and pasted, take no action.  
    {  
      if (clipboardService.IsPasteActive)  
      {  
        return true;  
      }  
    }  
  
    return EditSqlConnection(parentWindow);  
  }  
```  
  
### <a name="editing-the-connection-manager"></a>Modification du gestionnaire de connexions  
 Étant donné que le formulaire de modification est appelé à la fois à partir des méthodes <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.New%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Edit%2A>, il est plus pratique d'utiliser une fonction d'assistance pour encapsuler le code qui permet d'afficher le formulaire. Le code suivant illustre une implémentation de cette fonction d'assistance.  
  
```vb  
Private Function EditSqlConnection(ByVal parentWindow As IWin32Window) As Boolean  
  
  Dim sqlCMUIForm As New SqlConnMgrUIFormVB  
  
  sqlCMUIForm.Initialize(_connectionManager, _serviceProvider)  
  If sqlCMUIForm.ShowDialog(parentWindow) = DialogResult.OK Then  
    Return True  
  Else  
    Return False  
  End If  
  
End Function  
```  
  
```csharp  
private bool EditSqlConnection(IWin32Window parentWindow)  
 {  
  
   SqlConnMgrUIFormCS sqlCMUIForm = new SqlConnMgrUIFormCS();  
  
   sqlCMUIForm.Initialize(_connectionManager, _serviceProvider);  
   if (sqlCMUIForm.ShowDialog(parentWindow) == DialogResult.OK)  
   {  
     return true;  
   }  
   else  
   {  
     return false;  
   }  
  
 }  
```  
  
 Dans la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Edit%2A>, vous devez simplement afficher le formulaire à modifier. Le code suivant illustre une implémentation de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Edit%2A> qui utilise une fonction d'assistance pour encapsuler le code pour le formulaire.  
  
```vb  
Public Function Edit(ByVal parentWindow As System.Windows.Forms.IWin32Window, ByVal connections As Microsoft.SqlServer.Dts.Runtime.Connections, ByVal connectionUIArg As Microsoft.SqlServer.Dts.Runtime.Design.ConnectionManagerUIArgs) As Boolean Implements Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI.Edit  
  
  Return EditSqlConnection(parentWindow)  
  
End Function  
```  
  
```csharp  
public bool Edit(System.Windows.Forms.IWin32Window parentWindow, Microsoft.SqlServer.Dts.Runtime.Connections connections, Microsoft.SqlServer.Dts.Runtime.Design.ConnectionManagerUIArgs connectionUIArg)  
{  
  
  return EditSqlConnection(parentWindow);  
  
}  
```  
  
## <a name="coding-the-user-interface-form"></a>Codage du formulaire d'interface utilisateur  
 Après avoir créé la classe d'interface utilisateur qui implémente les méthodes de l'interface <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI>, vous devez créer un formulaire Windows où l'utilisateur peut configurer les propriétés de votre gestionnaire de connexions.  
  
### <a name="initializing-the-user-interface-form"></a>Initialisation du formulaire d'interface utilisateur  
 Lorsque vous affichez votre formulaire personnalisé à modifier, vous pouvez passer une référence au gestionnaire de connexions modifié. Vous pouvez passer cette référence soit en utilisant un constructeur personnalisé pour la classe Form, soit en créant votre propre méthode **Initialize** comme illustré ici.  
  
```vb  
Public Sub Initialize(ByVal connectionManager As ConnectionManager, ByVal serviceProvider As IServiceProvider)  
  
  _connectionManager = connectionManager  
  _serviceProvider = serviceProvider  
  ConfigureControlsFromConnectionManager()  
  EnableControls()  
  
End Sub  
```  
  
```csharp  
public void Initialize(ConnectionManager connectionManager, IServiceProvider serviceProvider)  
 {  
  
   _connectionManager = connectionManager;  
   _serviceProvider = serviceProvider;  
   ConfigureControlsFromConnectionManager();  
   EnableControls();  
  
 }  
```  
  
### <a name="setting-properties-on-the-user-interface-form"></a>Définition de propriétés sur le formulaire d'interface utilisateur  
 Enfin, votre classe Form a besoin d'une fonction d'assistance qui permet de remplir les contrôles sur le formulaire lorsqu'il est tout d'abord chargé avec les valeurs existantes ou par défaut des propriétés du gestionnaire de connexions. Votre classe Form a également besoin d'une fonction similaire qui définit les valeurs des propriétés sur les valeurs entrées par l'utilisateur lorsqu'il clique sur OK et ferme le formulaire.  
  
```vb  
Private Const CONNECTIONNAME_BASE As String = "SqlConnectionManager"  
  
Private Sub ConfigureControlsFromConnectionManager()  
  
  Dim tempName As String  
  Dim tempServerName As String  
  Dim tempDatabaseName As String  
  
  With _connectionManager  
  
    tempName = .Properties("Name").GetValue(_connectionManager).ToString  
    If Not String.IsNullOrEmpty(tempName) Then  
      _connectionName = tempName  
    Else  
      _connectionName = CONNECTIONNAME_BASE  
    End If  
  
    tempServerName = .Properties("ServerName").GetValue(_connectionManager).ToString  
    If Not String.IsNullOrEmpty(tempServerName) Then  
      _serverName = tempServerName  
      txtServerName.Text = _serverName  
    End If  
  
    tempDatabaseName = .Properties("DatabaseName").GetValue(_connectionManager).ToString  
    If Not String.IsNullOrEmpty(tempDatabaseName) Then  
      _databaseName = tempDatabaseName  
      txtDatabaseName.Text = _databaseName  
    End If  
  
  End With  
  
End Sub  
  
Private Sub ConfigureConnectionManagerFromControls()  
  
  With _connectionManager  
    .Properties("Name").SetValue(_connectionManager, _connectionName)  
    .Properties("ServerName").SetValue(_connectionManager, _serverName)  
    .Properties("DatabaseName").SetValue(_connectionManager, _databaseName)  
  End With  
  
End Sub  
```  
  
```csharp  
private const string CONNECTIONNAME_BASE = "SqlConnectionManager";  
  
private void ConfigureControlsFromConnectionManager()  
 {  
  
   string tempName;  
   string tempServerName;  
   string tempDatabaseName;  
  
   {  
     tempName = _connectionManager.Properties["Name"].GetValue(_connectionManager).ToString();  
     if (!String.IsNullOrEmpty(tempName))  
     {  
       _connectionName = tempName;  
     }  
     else  
     {  
       _connectionName = CONNECTIONNAME_BASE;  
     }  
  
     tempServerName = _connectionManager.Properties["ServerName"].GetValue(_connectionManager).ToString();  
     if (!String.IsNullOrEmpty(tempServerName))  
     {  
       _serverName = tempServerName;  
       txtServerName.Text = _serverName;  
     }  
  
     tempDatabaseName = _connectionManager.Properties["DatabaseName"].GetValue(_connectionManager).ToString();  
     if (!String.IsNullOrEmpty(tempDatabaseName))  
     {  
       _databaseName = tempDatabaseName;  
       txtDatabaseName.Text = _databaseName;  
     }  
  
   }  
  
 }  
  
 private void ConfigureConnectionManagerFromControls()  
 {  
  
   {  
     _connectionManager.Properties["Name"].SetValue(_connectionManager, _connectionName);  
     _connectionManager.Properties["ServerName"].SetValue(_connectionManager, _serverName);  
     _connectionManager.Properties["DatabaseName"].SetValue(_connectionManager, _databaseName);  
   }  
  
 }  
```
  
## <a name="see-also"></a>Voir aussi  
 [Création d’un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)   
 [Codage d'un gestionnaire de connexions personnalisé](../../../integration-services/extending-packages-custom-objects/connection-manager/coding-a-custom-connection-manager.md)  
  
  
