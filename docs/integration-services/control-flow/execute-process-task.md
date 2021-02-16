---
description: Tâche d'exécution de processus
title: Exécuter le processus, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executeprocesstask.f1
- sql13.dts.designer.executeprocesstask.general.f1
- sql13.dts.designer.executeprocesstask.process.f1
helpviewer_keywords:
- Execute Process task [Integration Services]
ms.assetid: aca5a0b5-34a9-45bc-a234-8e63ea51a1ee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 98409c2a75cc76589a705579ff02f1be34c9c7b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352113"
---
# <a name="execute-process-task"></a>Tâche d'exécution de processus

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche d’exécution de processus exécute une application ou un fichier de commandes dans le cadre d’un flux de travail de package [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Bien qu’il soit possible d’utiliser la tâche d’exécution de processus pour ouvrir des applications standard telles que [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] ou [!INCLUDE[ofprword](../../includes/ofprword-md.md)], il est courant de l’utiliser pour exécuter des applications de gestion ou des fichiers de commandes fonctionnant sur une source de données. Par exemple, vous pouvez utiliser la tâche d'exécution de processus pour développer un fichier texte compressé. Ensuite, le package peut utiliser le fichier texte comme source de données pour le flux de données de ce package. Vous pouvez aussi utiliser la tâche d'exécution de processus pour démarrer une application [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] personnalisée qui génère quotidiennement un état des ventes. Ensuite, vous pouvez associer le rapport à une tâche Envoyer un message pour le transmettre à une liste de distribution.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend d’autres tâches qui effectuent des opérations de flux de travail, telles que l’exécution de packages. Pour plus d’informations, consultez [Tâche d’exécution de package](../../integration-services/control-flow/execute-package-task.md).  
  
## <a name="custom-log-entries-available-on-the-execute-process-task"></a>Entrées de journal personnalisées disponibles dans la tâche d'exécution de processus  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche d'exécution de processus. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|Fournit des informations sur le processus que la tâche est chargée d'exécuter.<br /><br /> Deux entrées de journal sont écrites. La première contient des informations sur le nom et l'emplacement de l'exécutable que la tâche exécute ; la deuxième enregistre la sortie de l'exécutable.|  
|**ExecuteProcessVariableRouting**|Fournit des informations sur les variables qui doivent être acheminées vers l'entrée et les sorties de l'exécutable. Les entrées du journal sont écrites pour stdin (l'entrée), stdout (la sortie) et stderr (la sortie des erreurs).|  
  
## <a name="configuration-of-the-execute-process-task"></a>Configuration de la tâche d’exécution de processus  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="property-settings"></a>Paramètres de propriété  
 Lorsque la tâche d'exécution de processus exécute une application personnalisée, elle fournit l'entrée à l'application via l'une des méthodes suivantes, ou les deux :  
  
-   Une variable que vous spécifiez dans le paramètre de propriété **StandardInputVariable** . Pour plus d’informations sur les variables, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](../integration-services-ssis-variables.md).  
  
-   Un argument que vous spécifiez dans le paramètre de propriété **Arguments** . (Par exemple, si la tâche ouvre un document dans Word, l'argument peut nommer le fichier .doc.)  
  
 Pour passer plusieurs arguments à une application personnalisée dans une tâche d'exécution de processus, utilisez des espaces pour délimiter les arguments. Un argument ne peut pas inclure d'espace ; sinon, la tâche ne s'exécutera pas. Vous pouvez utiliser une expression pour passer une valeur variable comme argument. Dans l'exemple suivant, l'expression passe deux valeurs variables comme arguments et utilise un espace pour délimiter les arguments :  
  
 `@variable1 + " " + @variable2`  
  
 Vous pouvez utiliser une expression pour définir différentes propriétés de tâche d'exécution de processus.  
  
 Quand vous configurez la tâche d’exécution de processus à l’aide de la propriété **StandardInputVariable** pour fournir l’entrée, appelez la méthode **Console.ReadLine** de l’application pour lire l’entrée. Pour plus d’informations, consultez la rubrique [Console.ReadLine, méthode](/dotnet/api/system.console.readline) dans la bibliothèque de classes [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 Quand vous configurez la tâche d’exécution de processus à l’aide de la propriété **Arguments** pour fournir l’entrée, effectuez l’une des étapes suivantes pour obtenir les arguments :  
  
-   Si vous utilisez Microsoft Visual Basic pour écrire l’application, définissez la propriété **My.Application.CommandLineArgs** . L’exemple suivant définit la propriété **My.Application.CommandLineArgs** pour extraire deux arguments :  
  
    ```vb  
    Dim variable1 As String = My.Application.CommandLineArgs.Item(0)  
    Dim variable2 As String = My.Application.CommandLineArgs.Item(1)   
    ```  
  
     Pour plus d’informations, consultez la rubrique [My.Application.CommandLineArgs, propriété](https://go.microsoft.com/fwlink/?LinkId=129200)dans la Référence [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .  
  
-   Si vous utilisez Microsoft Visual C# pour écrire l’application, utilisez la méthode **Main**.  
  
     Pour plus d’informations, consultez la rubrique [Arguments de ligne de commande (Guide de programmation C#)](/dotnet/csharp/programming-guide/main-and-command-args/command-line-arguments)du Guide de programmation C#.  
  
 La tâche d’exécution de processus comprend également les propriétés **StandardOutputVariable** et **StandardErrorVariable** à l’aide desquelles vous pouvez spécifier les variables qui exploitent la sortie et la sortie d’erreur standard de l’application, respectivement.  
  
 En outre, vous pouvez configurer la tâche d'exécution de processus de manière à spécifier un répertoire de travail, un délai d'attente ou une valeur pour indiquer que l'exécutable s'est correctement exécuté. La tâche peut également être configurée de manière à échouer si l'exécutable est introuvable à l'emplacement spécifié ou que le code de retour de l'exécutable ne correspond pas à la valeur indiquant la réussite.  
  
## <a name="programmatic-configuration-of-the-execute-process-task"></a>Configuration par programmation de la tâche d'exécution de processus  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ExecuteProcess.ExecuteProcess>  
  
## <a name="execute-process-task-editor-general-page"></a>Éditeur de tâche d'exécution de processus (page Général)
  La page **Général** de la boîte de dialogue **Éditeur de tâche d’exécution de processus** permet de nommer et de décrire la tâche d’exécution de processus.  
  
### <a name="options"></a>Options  
 **Nom**  
 Fournissez un nom unique pour la tâche d'exécution de processus. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche d'exécution de processus.  
  
## <a name="execute-process-task-editor-process-page"></a>Execute Process Task Editor (Process Page)
  Utilisez la page **Traiter** de la boîte de dialogue **Éditeur de tâche d'exécution de processus** pour configurer les options qui exécutent le processus. Ces options comprennent l'exécutable à lancer, son emplacement, les arguments de l'invite de commandes et les variables qui fournissent l'entrée et la sortie de capture.  
  
### <a name="options"></a>Options  
 **RequireFullFileName**  
 Indiquez si la tâche doit échouer si l'exécutable est introuvable à l'emplacement spécifié.  
  
 **Exécutable**  
 Tapez le nom de l'exécutable à lancer.  
  
 **Arguments**  
 Fournissez les arguments de l'invite de commandes.  
  
 **WorkingDirectory**  
 Tapez le chemin du dossier qui contient l’exécutable ou cliquez sur le bouton Parcourir **(...)** , puis recherchez le dossier.  
  
 **StandardInputVariable**  
 Sélectionnez une variable pour fournir l’entrée au processus ou cliquez sur \<**New variable...**> pour créer une variable.  
  
 **Rubriques connexes :** [Ajouter une variable](../integration-services-ssis-variables.md)  
  
 **StandardOutputVariable**  
 Sélectionnez une variable pour capturer la sortie du processus ou cliquez sur \<**New variable...**> pour créer une variable.  
  
 **StandardErrorVariable**  
 Sélectionnez une variable pour capturer la sortie d’erreur du processus ou cliquez sur \<**New variable...**> pour créer une variable.  
  
 **FailTaskIfReturnCodeIsNotSuccessValue**  
 Indiquez si la tâche échoue quand le code de sortie du processus est différent de la valeur spécifiée dans **SuccessValue**.  
  
 **SuccessValue**  
 Spécifiez la valeur retournée par l'exécutable pour indiquer le succès de l'opération. Cette valeur est définie par défaut sur **0**.  
  
 **TimeOut**  
 Spécifiez la durée en secondes de l'exécution du processus. Une valeur **0** indique qu’aucun délai d’attente n’est utilisé et que l’exécution du processus dure tant que ce dernier n’est pas terminé ou tant qu’aucune erreur ne se produit.  
  
 **TerminateProcessAfterTimeOut**  
 Indiquez si le processus doit se terminer après le délai d’attente spécifié par l’option **TimeOut** . Cette option est uniquement disponible si la valeur de l'option **TimeOut** n'est pas **0**.  
  
 **WindowStyle**  
 Spécifiez le style de la fenêtre dans lequel le processus est exécuté.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
