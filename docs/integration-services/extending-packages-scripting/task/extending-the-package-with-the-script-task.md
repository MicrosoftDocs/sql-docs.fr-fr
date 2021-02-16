---
description: Extension du package à l'aide de la tâche de script
title: Extension du package à l’aide de la tâche de script | Microsoft Docs
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
- scripts [Integration Services]
- SSIS Script task
- tasks [Integration Services], scripts
- Script task [Integration Services], about Script task
- scripts [Integration Services], about Script task with packages
- SSIS Script task, about Script task
ms.assetid: 911e6d26-a6fd-4fc3-a111-bf5f048e9bff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2795546fe71c84bdc9780cc61680748ed8c72949
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351566"
---
# <a name="extending-the-package-with-the-script-task"></a>Extension du package à l'aide de la tâche de script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche de script étend les fonctionnalités d’exécution des packages [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] à l’aide de code personnalisé écrit dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# qui est compilé et exécuté au moment de l’exécution des packages. La tâche de script simplifie le développement d'une tâche d'exécution personnalisée lorsque les tâches incluses dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] ne répondent pas complètement à vos besoins. La tâche de script écrit tout le code d'infrastructure requis à votre place, ce qui vous permet de vous concentrer exclusivement sur le code requis pour votre traitement personnalisé.  
  
 Une tâche de script interagit avec le package conteneur via l’objet **Dts** global, une instance de la classe <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel> qui est exposée dans l’environnement de script. Vous pouvez écrire le code dans une tâche de script qui modifie les valeurs stockées dans les variables [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] ; le package peut ensuite utiliser ces valeurs mises à jour pour déterminer le chemin d'accès de son flux de travail. La tâche de script peut également utiliser l'espace de noms [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] et la bibliothèque de classes [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], ainsi que des assemblys personnalisés, pour implémenter des fonctionnalités personnalisées.  
  
 La tâche de script et le code d'infrastructure qu'elle génère simplifient considérablement le processus qui consiste à développer une tâche personnalisée. Toutefois, pour comprendre le fonctionnement de la tâche de script, il peut s’avérer utile de lire la section [Développement d’une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md) afin de maîtriser les étapes du développement d’une tâche personnalisée.  
  
 Si vous créez une tâche que vous projetez de réutiliser dans plusieurs packages, vous devez envisager de développer une tâche personnalisée au lieu d'utiliser la tâche de script. Pour plus d’informations, consultez [Comparaison des solutions de script et des objets personnalisés](../../../integration-services/extending-packages-scripting/comparing-scripting-solutions-and-custom-objects.md).  
  
## <a name="in-this-section"></a>Dans cette section  
 Les rubriques suivantes fournissent des informations supplémentaires sur la tâche de script.  
  
 [Configuration de la tâche de script dans l'éditeur de tâche de script](../../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md)  
 Explique de quelle manière les propriétés que vous configurez dans l’**éditeur de tâche de script** affectent les fonctionnalités et les performances du code de la tâche de script.  
  
 [Codage et débogage de la tâche de script](../../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md)  
 Explique comment utiliser [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) pour développer les scripts contenus dans la tâche de script.  
  
 [Utilisation de variables dans la tâche de script](../../../integration-services/extending-packages-scripting/task/using-variables-in-the-script-task.md)  
 Explique comment utiliser des variables via la propriété <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>.  
  
 [Connexion à des sources de données dans la tâche de script](../../../integration-services/extending-packages-scripting/task/connecting-to-data-sources-in-the-script-task.md)  
 Explique comment utiliser des connexions via la propriété <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Connections%2A>.  
  
 [Déclenchement d'événements dans la tâche de script](../../../integration-services/extending-packages-scripting/task/raising-events-in-the-script-task.md)  
 Explique comment déclencher des événements via la propriété <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>.  
  
 [Journalisation dans la tâche de script](../../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md)  
 Explique comment journaliser des informations via la méthode <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>.  
  
 [Retour de résultats de la tâche de script](../../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md)  
 Explique comment renvoyer des résultats via la propriété <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.TaskResult%2A> et la propriété <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.ExecutionValue%2A>.  
  
 [Exemples de tâche de script](../../../integration-services/extending-packages-scripting-task-examples/script-task-examples.md)  
 Fournit des exemples simples qui montrent plusieurs utilisations possibles d'une tâche de script.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâche de script](../../../integration-services/control-flow/script-task.md)   
 [Comparaison de la tâche de script et du composant Script](../../../integration-services/extending-packages-scripting/comparing-the-script-task-and-the-script-component.md)  
  
  
