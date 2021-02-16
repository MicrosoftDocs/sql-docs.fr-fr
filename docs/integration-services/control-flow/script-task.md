---
description: Tâche de script
title: Script, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.scripttask.f1
- sql13.dts.designer.scripttask.general.f1
- sql13.dts.designer.scripttask.script.f1
helpviewer_keywords:
- scripts [Integration Services], tasks
- Script task [Integration Services], about Script task
- Script task [Integration Services]
ms.assetid: f6cce7df-4bd6-4b75-9f89-6c37b4bb5558
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 10be8411e2de29c514c3d9aed50c550a0e86ba91
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348271"
---
# <a name="script-task"></a>Tâche de script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche de script fournit du code permettant d’exécuter des fonctions qui ne sont pas disponibles dans les tâches et transformations intégrées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. La tâche de script peut également combiner des fonctions dans un même script au lieu d'utiliser plusieurs tâches et transformations. Utilisez la tâche de script pour le travail devant être effectué une fois dans un package (ou une fois par objet énuméré), et non une fois par ligne de données.  
  
 Vous pouvez utiliser la tâche de script aux fins suivantes :  
  
-   Accéder aux données à l'aide d'autres technologies non prises en charge par les types de connexion intégrés. Par exemple, un script peut utiliser des interfaces ADSI (Active Directory Service Interfaces) pour accéder aux noms d'utilisateur et les extraire d'Active Directory.  
  
-   Créer un compteur de performances spécifique au package. Par exemple, un script peut créer un compteur de performances mis à jour pendant l'exécution d'une tâche complexe ou peu performante.  
  
-   Déterminez si les fichiers spécifiés sont vides ou combien de lignes ils contiennent, puis en fonction de ces informations, affectez le flux de contrôle dans un package. Par exemple, si un fichier ne contient aucune ligne, la valeur 0 d'une variable et une contrainte de précédence qui évalue la valeur empêchent une tâche de système de fichiers de copier le fichier.  
  
 Si vous devez utiliser le script afin d'effectuer le même travail pour chaque ligne de données d'un ensemble, vous devriez utiliser le composant Script à la place de la tâche de script. Par exemple, si vous souhaitez évaluer un montant de frais de port raisonnable et ignorer les lignes de données présentant des montants extrêmement élevés ou bas, il convient d'utiliser un composant Script. Pour plus d’informations, voir [Script Component](../../integration-services/data-flow/transformations/script-component.md).  
  
 Si plusieurs packages utilisent un script, envisagez l'écriture d'une tâche personnalisée plutôt que de recourir à la tâche de script. Pour plus d’informations, consultez [Développement d’une tâche personnalisée](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
 Si vous choisissez la tâche de script pour votre package, vous devez développer le script utilisé par la tâche et configurer cette dernière.  
  
## <a name="writing-and-running-the-script-that-the-task-uses"></a>Écriture et exécution du script utilisé par la tâche  
 La tâche de script utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) comme environnement d’écriture des scripts et moteur d’exécution de ces derniers.  
  
 VSTA fournit l’ensemble des fonctionnalités standard de l’environnement [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] , telles que l’éditeur [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] à code de couleur, IntelliSense et **l’Explorateur d’objets**. VSTA utilise également le même débogueur que les autres outils de développement [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Les points d'arrêt dans le script fonctionnent de façon transparente avec ceux des tâches et des conteneurs [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . VSTA prend en charge les langages de programmation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic et [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#.  
  
 Vous ne pouvez lancer un script que si VSTA est installé sur l'ordinateur où le package est exécuté. Lorsque le package s'exécute, la tâche charge le moteur de script puis exécute le script. Vous pouvez accéder aux assemblys .NET externes dans les scripts en ajoutant des références aux assemblys du projet. Actuellement, nous ne prenons pas en charge les références d’assembly standard .NET Core et .NET.  
  
> [!NOTE]  
>  Contrairement aux versions antérieures pour lesquelles il était possible d’indiquer si les scripts étaient précompilés, tous les scripts de [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] et des versions ultérieures sont précompilés. Lorsqu'un script est précompilé, le moteur de langage n'est pas chargé au moment de l'exécution et le package s'exécute plus rapidement. Toutefois, les fichiers binaires précompilés occupent un espace disque important.  
  
## <a name="configuring-the-script-task"></a>Configuration de la tâche de script  
 Vous pouvez configurer la tâche de script comme suit :  
  
-   Fournissez le script personnalisé que la tâche exécute.  
  
-   Spécifiez la méthode dans le projet VSTA utilisée au moment de l’exécution de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comme point d’entrée du code de tâche de script.  
  
-   Spécifiez le langage de script  
  
-   Éventuellement, fournissez les listes des variables en lecture seule et en lecture/écriture à utiliser dans le script.  
  
 Vous pouvez définir ces propriétés via le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programme.  
  
### <a name="configuring-the-script-task-in-the-designer"></a>Configuration de la tâche de script dans le concepteur  
 Le tableau suivant décrit l’événement **ScriptTaskLogEntry** pouvant être consigné pour la tâche de script. L’événement **ScriptTaskLogEntry** est sélectionné pour se connecter à l’onglet **Détails** de la boîte de dialogue **Configurer les journaux SSIS** . Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|Indique les résultats de l'implémentation de la journalisation dans le script. La tâche écrit une entrée de journal pour chaque appel de la méthode **Log** de l’objet **Dts** . La tâche écrit ces entrées lorsque le code est exécuté. Pour plus d’informations, consultez [Journalisation dans la tâche de script](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md).|  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez les rubriques suivantes :  
  
-   [Éditeur de tâche de script &#40;page Général&#41;]()  
  
-   [Éditeur de tâche de script &#40;page Script&#41;]()  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d’informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="configuring-the-script-task-programmatically"></a>Configuration par programme de la tâche de script  
 Pour plus d’informations sur la définition par programmation de ces propriétés, consultez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask>  
  
## <a name="script-task-editor-general-page"></a>Éditeur de tâche de script (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de script** pour donner un nom et une description à la tâche de script.  
  
 Pour en savoir plus sur la tâche de script, consultez [Script Task](../../integration-services/control-flow/script-task.md) et [Configuration de la tâche de script dans l'éditeur de tâche de script](../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md). Pour en savoir plus sur la programmation de la tâche de script, consultez [Extension du package à l'aide de la tâche de script](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md).  
  
### <a name="options"></a>Options  
 **Nom**  
 Attribuez un nom unique à la tâche de script. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Tapez la description de la tâche de script.  
  
## <a name="script-task-editor-script-page"></a>Éditeur de tâche de script (page Script)
  Utilisez la page **Script** de la boîte de dialogue **Éditeur de tâche de script** pour définir les propriétés du script et vérifier les variables auxquelles le script peut accéder.  
  
> [!NOTE]  
>  Dans [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] et les versions ultérieures, tous les scripts sont précompilés. Dans les versions antérieures, vous définissez une propriété **PrecompileScriptIntoBinaryCode** pour spécifier que le script a été précompilé.  
  
 Pour en savoir plus sur la tâche de script, consultez [Script Task](../../integration-services/control-flow/script-task.md) et [Configuration de la tâche de script dans l'éditeur de tâche de script](../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md). Pour en savoir plus sur la programmation de la tâche de script, consultez [Extension du package à l'aide de la tâche de script](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md).  
  
### <a name="options"></a>Options  
 **ScriptLanguage**  
 Sélectionnez le langage de script pour la tâche, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#.  
  
 Après avoir créé un script pour la tâche, vous ne pouvez pas modifier la valeur de la propriété **ScriptLanguage** .  
  
 Pour définir le langage de script par défaut pour la tâche de script, utilisez l'option **Langage de script** dans la page **Général** de la boîte de dialogue **Options** . Pour plus d'informations, consultez [General Page]().  
  
 **EntryPoint**  
 Spécifiez la méthode que le runtime [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] appelle comme point d’entrée dans le code de la tâche de script. La méthode spécifiée doit se trouver dans la classe ScriptMain (classe par défaut générée par les modèles de script) du projet [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA).  
  
 Si vous modifiez le nom de la méthode dans le projet VSTA, vous devez modifier la valeur de la propriété **EntryPoint** .  
  
 **ReadOnlyVariables**  
 Tapez une liste séparée par des virgules des variables en lecture seule accessibles au script ou cliquez sur le bouton de sélection (**...**) et sélectionnez les variables dans la boîte de dialogue **Sélectionner des variables** .  
  
> [!NOTE]  
>  Les noms des variables tiennent compte de la casse.  
  
 **ReadWriteVariables**  
 Tapez une liste séparée par des virgules des variables en lecture/écriture accessibles au script ou cliquez sur le bouton de sélection (**...**) et sélectionnez les variables dans la boîte de dialogue **Sélectionner des variables** .  
  
> [!NOTE]  
>  Les noms des variables tiennent compte de la casse.  
  
 **Modifier le script**  
 Ouvre l'environnement de développement intégré VSTA dans lequel vous pouvez créer ou modifier le script.  
  
## <a name="related-content"></a>Contenu associé  
  
-   Article technique [Procédure d'envoi de courrier électronique avec notification de remise en C#](https://go.microsoft.com/fwlink/?LinkId=237625)(Procédure d’envoi d’e-mail avec notification de remise en C#) sur shareourideas.com  
  
