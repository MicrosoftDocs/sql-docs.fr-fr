---
description: Codage et débogage de la tâche de script
title: Codage et débogage de la tâche de script | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], debugging
- SSIS Script task, development environment
- SSIS Script task, debugging
- Script task [Integration Services], development environment
- Script task [Integration Services], coding
- debugging [Integration Services], scripts
- VSTA
- SSIS Script task, coding
ms.assetid: 687c262f-fcab-42e8-92ae-e956f3d92d69
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f5cc9132e7a8f67d2c4636eeaeaa0cb7db5ca0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351584"
---
# <a name="coding-and-debugging-the-script-task"></a>Codage et débogage de la tâche de script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir configuré la tâche de script dans l’**éditeur de tâche de script**, vous pouvez écrire votre code personnalisé dans l’environnement de développement de tâche de script.  
  
## <a name="script-task-development-environment"></a>Environnement de développement de tâche de script  
 La tâche de script utilise [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) comme environnement de développement du script proprement dit.  
  
 Le code de script est écrit dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C#. Vous pouvez spécifier le langage de script en définissant la propriété **ScriptLanguage** dans l’**éditeur de tâche de script**. Si vous préférez utiliser un autre langage de programmation, vous pouvez développer un assembly personnalisé dans le langage de votre choix et appeler ses fonctionnalités à partir du code inclus dans la de script.  
  
 Le script que vous créez dans la tâche de script est stocké dans la définition du package. Aucun fichier de script distinct n'est créé. Par conséquent, l'utilisation de la tâche de script n'affecte pas le déploiement de package.  
  
> [!NOTE]  
>  Lorsque vous concevez le package et déboguez le script, le code de script est écrit temporairement dans un fichier projet. Étant donné que le stockage d'informations sensibles dans un fichier représente un risque potentiel en termes de sécurité, nous vous recommandons de ne pas inclure d'informations sensibles, telles que des mots de passe, dans le code de script.  
  
 Par défaut, **Option Strict** est désactivé dans l’environnement de développement intégré.  
  
## <a name="script-task-project-structure"></a>Structure du projet de tâche de script  
 Lorsque vous créez ou modifiez le script contenu dans une tâche de script, VSTA ouvre un nouveau projet vide ou rouvre le projet existant. La création de ce projet VSTA n'affecte pas le déploiement du package étant donné que le projet est enregistré à l'intérieur du fichier de package ; la tâche de script ne crée pas de fichiers supplémentaires.  
  
### <a name="project-items-and-classes-in-the-script-task-project"></a>Éléments et classes du projet de tâche de script  
 Par défaut, le projet de tâche de script affiché dans la fenêtre Explorateur de projets VSTA contient un seul élément, **ScriptMain**. L’élément **ScriptMain** contient à son tour une classe unique, également nommé **ScriptMain**. Les éléments de code inclus dans la classe varient selon le langage de programmation sélectionné pour la tâche de script :  
  
-   Lorsque la tâche de script est configurée pour le langage de programmation [!INCLUDE[vb_orcas_long](../../../includes/vb-orcas-long-md.md)], la classe **ScriptMain** possède une sous-routine publique, **Main**. La sous-routine **ScriptMain.Main** est la méthode appelée par le runtime lorsque vous exécutez votre tâche de script.  
  
     Par défaut, le seul code figurant dans la sous-routine **Main** d’un nouveau script est la ligne `Dts.TaskResult = ScriptResults.Success`. Cette ligne informe le runtime que la tâche a été menée à bien. La propriété **Dts.TaskResult** est décrite dans [Retour de résultats de la tâche de script](../../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md).  
  
-   Lorsque la tâche de script est configurée pour le langage de programmation Visual C#, la classe **ScriptMain** possède une méthode publique, **Main**. La méthode est appelée lors de l'exécution de la tâche de script.  
  
     Par défaut, la méthode **Main** inclut la ligne `Dts.TaskResult = (int)ScriptResults.Success`. Cette ligne informe le runtime que la tâche a été menée à bien.  
  
 L’élément **ScriptMain** peut contenir d’autres classes que **ScriptMain**. La tâche de script ne peut accéder qu'aux classes qu'elle héberge.  
  
 Par défaut, l’élément de projet **ScriptMain** contient le code généré automatiquement suivant. Le modèle de code fournit également une vue d'ensemble de la tâche de script, ainsi que des informations supplémentaires sur la récupération et la manipulation des objets SSIS, tels que les variables, les événements et les connexions.  
  
```vb  
' Microsoft SQL Server Integration Services Script Task  
' Write scripts using Microsoft Visual Basic 2008.  
' The ScriptMain is the entry point class of the script.  
  
Imports System  
Imports System.Data  
Imports System.Math  
Imports Microsoft.SqlServer.Dts.Runtime.VSTAProxy  
  
<System.AddIn.AddIn("ScriptMain", Version:="1.0", Publisher:="", Description:="")> _  
Partial Class ScriptMain  
  
Private Sub ScriptMain_Startup(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Startup  
  
End Sub  
  
Private Sub ScriptMain_Shutdown(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Shutdown  
Try  
' Unlock variables from the read-only and read-write variable collection properties  
If (Dts.Variables.Count <> 0) Then  
Dts.Variables.Unlock()  
End If  
Catch ex As Exception  
        End Try  
End Sub  
  
Enum ScriptResults  
Success = DTSExecResult.Success  
Failure = DTSExecResult.Failure  
End Enum  
  
' The execution engine calls this method when the task executes.  
' To access the object model, use the Dts property. Connections, variables, events,  
' and logging features are available as members of the Dts property as shown in the following examples.  
'  
' To reference a variable, call Dts.Variables("MyCaseSensitiveVariableName").Value  
' To post a log entry, call Dts.Log("This is my log text", 999, Nothing)  
' To fire an event, call Dts.Events.FireInformation(99, "test", "hit the help message", "", 0, True)  
'  
' To use the connections collection use something like the following:  
' ConnectionManager cm = Dts.Connections.Add("OLEDB")  
' cm.ConnectionString = "Data Source=localhost;Initial Catalog=AdventureWorks;Provider=SQLNCLI10;Integrated Security=SSPI;Auto Translate=False;"  
'  
' Before returning from this method, set the value of Dts.TaskResult to indicate success or failure.  
'   
' To open Help, press F1.  
  
Public Sub Main()  
'  
' Add your code here  
'  
Dts.TaskResult = ScriptResults.Success  
End Sub  
  
End Class  
```  
  
```csharp  
/*  
   Microsoft SQL Server Integration Services Script Task  
   Write scripts using Microsoft Visual C# 2008.  
   The ScriptMain is the entry point class of the script.  
*/  
  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime.VSTAProxy;  
using System.Windows.Forms;  
  
namespace ST_1bcfdbad36d94f8ba9f23a10375abe53.csproj  
{  
    [System.AddIn.AddIn("ScriptMain", Version = "1.0", Publisher = "", Description = "")]  
    public partial class ScriptMain  
    {  
        private void ScriptMain_Startup(object sender, EventArgs e)  
        {  
  
        }  
  
        private void ScriptMain_Shutdown(object sender, EventArgs e)  
        {  
            try  
            {  
                // Unlock variables from the read-only and read-write variable collection properties  
                if (Dts.Variables.Count != 0)  
                {  
                    Dts.Variables.Unlock();  
                }  
            }  
            catch  
            {  
            }  
        }  
  
        #region VSTA generated code  
        private void InternalStartup()  
        {  
            this.Startup += new System.EventHandler(ScriptMain_Startup);  
            this.Shutdown += new System.EventHandler(ScriptMain_Shutdown);  
        }  
        enum ScriptResults  
        {  
            Success = DTSExecResult.Success,  
            Failure = DTSExecResult.Failure  
        };  
  
        #endregion  
  
        /*  
The execution engine calls this method when the task executes.  
To access the object model, use the Dts property. Connections, variables, events,  
and logging features are available as members of the Dts property as shown in the following examples.  
  
To reference a variable, call Dts.Variables["MyCaseSensitiveVariableName"].Value;  
To post a log entry, call Dts.Log("This is my log text", 999, null);  
To fire an event, call Dts.Events.FireInformation(99, "test", "hit the help message", "", 0, true);  
  
To use the connections collection use something like the following:  
ConnectionManager cm = Dts.Connections.Add("OLEDB");  
cm.ConnectionString = "Data Source=localhost;Initial Catalog=AdventureWorks;Provider=SQLNCLI10;Integrated Security=SSPI;Auto Translate=False;";  
  
Before returning from this method, set the value of Dts.TaskResult to indicate success or failure.  
  
To open Help, press F1.  
*/  
  
        public void Main()  
        {  
            // TODO: Add your code here  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
    }  
```  
  
### <a name="additional-project-items-in-the-script-task-project"></a>Autres éléments du projet de tâche de script  
 Le projet de tâche de script peut inclure d’autres éléments que l’élément **ScriptMain** par défaut. Vous pouvez ajouter des classes, des modules et des fichiers de code au projet. Vous pouvez également utiliser des dossiers pour organiser des groupes d'éléments. Tous les éléments que vous ajoutez sont rendus persistants à l'intérieur du package.  
  
### <a name="references-in-the-script-task-project"></a>Références dans le projet de tâche de script  
 Vous pouvez ajouter des références aux assemblys managés en cliquant avec le bouton droit sur le projet de tâche de script dans l’**Explorateur de projets**, puis en cliquant sur **Ajouter une référence**. Pour plus d’informations, consultez [Référencement d’autres assemblys dans les solutions de script](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md).  
  
> [!NOTE]  
>  Vous pouvez consulter des références de projet dans l’environnement de développement intégré VSTA dans l’**Affichage de classes** ou l’**Explorateur de projets**. Vous ouvrez l’une ou l’autre de ces fenêtres à partir du menu **Affichage**. Vous pouvez ajouter une nouvelle référence à partir du menu **Projet**, de l’**Explorateur de projets** ou d’**Affichage de classes**.  
  
## <a name="interacting-with-the-package-in-the-script-task"></a>Interaction avec le package dans la tâche de script  
 La tâche de script utilise l’objet **Dts** global, qui est une instance de la classe <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel>, ainsi que ses membres, pour interagir avec le package qui la contient et avec le runtime [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 Le tableau suivant répertorie les principaux membres publics de la classe <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel>, qui est exposée au code de la tâche de script par le biais de l’objet **Dts** global. Les rubriques de cette section examinent en détail l'utilisation de ces membres.  
  
|Membre|Objectif|  
|------------|-------------|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Connections%2A>|Permet d'accéder aux gestionnaires de connexion définis dans le package.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>|Fournit une interface d'événements pour permettre à la tâche de script de déclencher des erreurs, des avertissements et des messages d'information.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.ExecutionValue%2A>|Offre un moyen simple de retourner un objet unique au runtime (en plus de **TaskResult**) qui peut également être utilisé pour la création de branche de flux de travail.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>|Enregistre des informations, telles que la progression et le résultat d'une tâche, à des modules fournisseurs d'informations activés.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.TaskResult%2A>|Signale le succès ou l'échec de la tâche.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Transaction%2A>|Fournit la transaction, le cas échéant, dans laquelle le conteneur de la tâche s'exécute.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>|Permet d’accéder aux variables répertoriées dans les propriétés de tâche **ReadOnlyVariables** et **ReadWriteVariables** à utiliser dans le script.|  
  
 La classe <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel> contient également quelques membres publics que vous n'utiliserez probablement pas.  
  
|Membre|Description|  
|------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>|La propriété <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> fournit un accès plus pratique aux variables. Vous pouvez utiliser <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>, mais vous devez alors appeler des méthodes de manière explicite pour verrouiller et déverrouiller l'accès en lecture ou en écriture aux variables. La tâche de script se charge de gérer la sémantique de verrouillage lorsque vous utilisez la propriété <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>.|  
  
## <a name="debugging-the-script-task"></a>Débogage de la tâche de script  
 Pour déboguer le code dans votre tâche de script, définissez au moins un point d'arrêt dans le code, puis fermez l'environnement de développement intégré VSTA pour exécuter le package dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. Lorsque l'exécution du package entre la tâche de script, l'environnement de développement intégré s'ouvre à nouveau et affiche votre code en lecture seule. Lorsque l'exécution atteint le point d'arrêt, vous pouvez examiner les valeurs des variables et exécuter pas à pas le code restant.  
  
> [!WARNING]  
>  Vous ne pouvez pas déboguer la tâche de script lorsque vous exécutez le package en mode 64 bits.  
  
> [!NOTE]  
>  Vous devez exécuter le package à déboguer dans votre tâche de script. Si vous exécutez uniquement la tâche, les points d'arrêt figurant dans le code de tâche de script sont ignorés.  
  
> [!NOTE]  
>  Vous ne pouvez pas déboguer une tâche de script si vous l'exécutez dans le cadre d'un package enfant exécuté à partir d'une tâche d'exécution de package. Dans ce cas, les points d'arrêt définis dans la tâche de script dans le package enfant sont ignorés. Vous pouvez déboguer normalement le package enfant en l'exécutant séparément.  
  
> [!NOTE]  
>  Lorsque vous déboguez un package qui contient plusieurs tâches Script, le débogueur n'en débogue qu'une seule. Le système peut déboguer une autre tâche Script si le débogueur a terminé, comme dans le cas d'une boucle Foreach ou du conteneur de boucles For.  
  
## <a name="external-resources"></a>Ressources externes  
  
-   Entrée de blog, [VSTA setup and configuration troubles for SSIS 2008 and R2 installations](/archive/blogs/jason_howell/vsta-setup-and-configuration-troubles-for-ssis-2008-and-r2-installations), sur blogs.msdn.com.  
  
## <a name="see-also"></a>Voir aussi  
 [Référencement d’autres assemblys dans les solutions de script](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md)   
 [Configuration de la tâche de script dans l'éditeur de tâche de script](../../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md)  
  
