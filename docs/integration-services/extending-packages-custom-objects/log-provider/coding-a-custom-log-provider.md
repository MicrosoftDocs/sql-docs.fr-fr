---
description: Codage d'un module fournisseur d'informations personnalisé
title: Codage d’un module fournisseur d’informations personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom log providers [Integration Services], coding
ms.assetid: 979a29ca-956e-4fdd-ab47-f06e84cead7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 93ddf518405b44b5abb6956ee5da063b98ece1e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348894"
---
# <a name="coding-a-custom-log-provider"></a>Codage d'un module fournisseur d'informations personnalisé

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir créé une classe qui hérite de la classe de base <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>, puis appliqué l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> à cette classe, vous devez substituer l'implémentation des propriétés et des méthodes de la classe de base afin de fournir vos fonctionnalités personnalisées.  
  
 Pour obtenir des exemples fonctionnels de modules personnalisés, consultez [Développement d’une interface utilisateur pour un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md).  
  
## <a name="configuring-the-log-provider"></a>Configuration du module fournisseur d’informations  
  
### <a name="initializing-the-log-provider"></a>Initialisation du module fournisseur d'informations  
 Vous remplacez la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.InitializeLogProvider%2A> pour mettre en cache des références à la collection de connexions et à l'interface d'événements. Vous pouvez utiliser ces références mises en cache ultérieurement dans d'autres méthodes du module fournisseur d'informations.  
  
### <a name="using-the-configstring-property"></a>Utilisation de la propriété ConfigString  
 Au moment de la conception, un module fournisseur d’informations reçoit des informations de configuration de la colonne **Configuration**. Ces informations de configuration correspondent à la propriété <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> du module fournisseur d'informations. Par défaut, cette colonne contient une zone de texte à partir de laquelle vous pouvez extraire des informations de chaîne. La plupart des modules fournisseurs d'informations inclus dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilisent cette propriété pour stocker le nom du gestionnaire de connexions que le fournisseur utilise pour se connecter à une source de données externe. Si votre module fournisseur d’informations utilise la propriété <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>, utilisez la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> pour valider cette propriété et vous assurer que la propriété est correctement définie.  
  
### <a name="validating-the-log-provider"></a>Validation du module fournisseur d'informations  
 Vous remplacez la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> pour vous assurer que le fournisseur a été configuré correctement et qu'il est prêt pour l'exécution. En général, un niveau minimum de validation consiste à s'assurer que la propriété <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> est correctement définie. L'exécution ne peut pas continuer tant que le module fournisseur d'informations ne renvoie pas <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> à partir de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A>.  
  
 L'exemple de code suivant illustre une implémentation de <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> qui s'assure que le nom d'un gestionnaire de connexions est spécifié, que ce gestionnaire de connexions existe dans le package et qu'il renvoie un nom de fichier dans la propriété <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>.  
  
```csharp  
public override DTSExecResult Validate(IDTSInfoEvents infoEvents)  
{  
    if (this.ConfigString.Length == 0 || connections.Contains(ConfigString) == false)  
    {  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
        return DTSExecResult.Failure;  
    }  
    else  
    {  
        string fileName = connections[ConfigString].AcquireConnection(null) as string;  
  
        if (fileName == null || fileName.Length == 0)  
        {  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
            return DTSExecResult.Failure;  
        }  
    }  
    return DTSExecResult.Success;  
}  
```  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As IDTSInfoEvents) As DTSExecResult  
    If Me.ConfigString.Length = 0 Or connections.Contains(ConfigString) = False Then  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
        Return DTSExecResult.Failure  
    Else   
        Dim fileName As String =  connections(ConfigString).AcquireConnectionCType(as string, Nothing)  
  
        If fileName = Nothing Or fileName.Length = 0 Then  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
            Return DTSExecResult.Failure  
        End If  
    End If  
    Return DTSExecResult.Success  
End Function  
```  
  
### <a name="persisting-the-log-provider"></a>Persistance du module fournisseur d'informations  
 D'ordinaire, vous n'avez pas à implémenter de persistance personnalisée pour un gestionnaire de connexions. Une persistance personnalisée est uniquement requise lorsque les propriétés d'un objet utilisent des types de données complexes. Pour plus d’informations, consultez [Développement d’objets personnalisés pour Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md).  
  
## <a name="logging-with-the-log-provider"></a>Journalisation avec le module fournisseur d'informations  
 Il existe trois méthodes d'exécution qui doivent être remplacées par tous les modules fournisseurs d'informations : <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>.  
  
> [!IMPORTANT]  
>  Pendant la validation et l'exécution d'un package unique, les méthodes <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> sont appelées plusieurs fois. Assurez-vous que votre code personnalisé n'engendre pas l'écrasement des entrées de journal précédentes lors de l'ouverture et de la fermeture suivantes du journal. Si vous avez choisi de journaliser les événements de validation dans votre package test, le premier événement journalisé que vous devez voir est OnPreValidate ; si le premier événement journalisé est PackageStart, les événements de validation initiaux ont été écrasés.  
  
### <a name="opening-the-log"></a>Ouverture du journal  
 La plupart des modules fournisseurs d'informations se connectent à une source de données externe, telle qu'un fichier ou une base de données, pour stocker les informations d'événements collectées pendant l'exécution du package. Comme avec tout autre objet inclus dans l'exécution, la connexion à la source de données externe est en général établie en utilisant des objets de gestionnaire de connexions.  
  
 La méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> est appelée au début de l'exécution du package. Remplacez cette méthode pour établir une connexion à la source de données externe.  
  
 L'exemple de code suivant représente un module fournisseur d'informations qui ouvre un fichier texte à des fins d'écriture pendant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>. Il ouvre le fichier en appelant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> du gestionnaire de connexions qui a été spécifié dans la propriété <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>.  
  
```csharp  
public override void OpenLog()  
{  
    if(!this.connections.Contains(this.ConfigString))  
        throw new Exception("The ConnectionManager " + this.ConfigString + " does not exist in the Connections collection.");  
  
    this.connectionManager = connections[ConfigString];  
    string filePath = this.connectionManager.AcquireConnection(null) as string;  
  
    if(filePath == null || filePath.Length == 0)  
        throw new Exception("The ConnectionManager " + this.ConfigString + " is not a valid FILE ConnectionManager");  
  
    //  Create a StreamWriter to append to.  
    sw = new StreamWriter(filePath,true);  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString());  
}  
```  
  
```vb  
Public Overrides  Sub OpenLog()  
    If Not Me.connections.Contains(Me.ConfigString) Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " does not exist in the Connections collection.")  
    End If  
  
    Me.connectionManager = connections(ConfigString)  
    Dim filePath As String =  Me.connectionManager.AcquireConnectionCType(as string, Nothing)  
  
    If filePath = Nothing Or filePath.Length = 0 Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " is not a valid FILE ConnectionManager")  
    End If  
  
    '  Create a StreamWriter to append to.  
    sw = New StreamWriter(filePath,True)  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString())  
End Sub  
```  
  
### <a name="writing-log-entries"></a>Écriture des entrées du journal  
 La méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> est appelée chaque fois qu’un objet compris dans le package déclenche un événement en appelant une méthode Fire\<event> sur l’une des interfaces d’événements. Chaque événement est déclenché avec des informations sur son contexte et habituellement un message explicatif. Toutefois, tous les appels de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> n'incluent pas d'informations sur chaque paramètre de méthode. Par exemple, certains événements standard dont les noms sont évidents ne fournissent pas MessageText, et DataCode et DataBytes sont prévus pour des informations supplémentaires facultatives.  
  
 L'exemple de code suivant implémente la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> et écrit les événements dans le flux qui a été ouvert dans la section précédente.  
  
```csharp  
public override void Log(string logEntryName, string computerName, string operatorName, string sourceName, string sourceID, string executionID, string messageText, DateTime startTime, DateTime endTime, int dataCode, byte[] dataBytes)  
{  
    sw.Write(logEntryName + ",");  
    sw.Write(computerName + ",");  
    sw.Write(operatorName + ",");  
    sw.Write(sourceName + ",");  
    sw.Write(sourceID + ",");  
    sw.Write(messageText + ",");  
    sw.Write(dataBytes + ",");  
    sw.WriteLine("");  
}  
```  
  
```vb  
Public Overrides  Sub Log(ByVal logEnTryName As String, ByVal computerName As String, ByVal operatorName As String, ByVal sourceName As String, ByVal sourceID As String, ByVal executionID As String, ByVal messageText As String, ByVal startTime As DateTime, ByVal endTime As DateTime, ByVal dataCode As Integer, ByVal dataBytes() As Byte)  
    sw.Write(logEnTryName + ",")  
    sw.Write(computerName + ",")  
    sw.Write(operatorName + ",")  
    sw.Write(sourceName + ",")  
    sw.Write(sourceID + ",")  
    sw.Write(messageText + ",")  
    sw.Write(dataBytes + ",")  
    sw.WriteLine("")  
End Sub  
```  
  
### <a name="closing-the-log"></a>Fermeture du journal  
 La méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> est appelée à la fin de l'exécution du package, une fois que tous les objets compris dans le package ont achevé leur exécution ou lorsque le package s'arrête en raison d'erreurs.  
  
 L'exemple de code suivant montre une implémentation de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> qui ferme le flux de fichiers qui a été ouvert pendant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>.  
  
```csharp  
public override void CloseLog()  
{  
    if (sw != null)  
    {  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString());  
        sw.Close();  
    }  
}  
```  
  
```vb  
Public Overrides  Sub CloseLog()  
    If Not sw Is Nothing Then  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString())  
        sw.Close()  
    End If  
End Sub  
```  
 
## <a name="see-also"></a>Voir aussi  
 [Création d’un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)   
 [Développement d’une interface utilisateur pour un module fournisseur d’informations personnalisé](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
