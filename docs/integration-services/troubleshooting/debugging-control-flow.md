---
description: Débogage du flux de contrôle
title: Débogage du flux de contrôle | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.setbreakpoints.f1
helpviewer_keywords:
- progress reporting [Integration Services]
- breakpoints [Integration Services]
- debugging [Integration Services], control flow
- control flow [Integration Services], debugging
- color-coded progress reporting [Integration Services]
- Set Breakpoints dialog box
ms.assetid: 54a458cc-9f4f-4b48-8cf2-db2e0fa7756c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 53fc539f46751cf6073aee0d5cd3aefae1971283
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338405"
---
# <a name="debugging-control-flow"></a>Débogage du flux de contrôle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluent des fonctionnalités et des outils permettant de résoudre les problèmes du flux de contrôle d’un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend en charge les points d’arrêt sur les conteneurs et les tâches.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] - Le concepteur SSIS fournit des rapports de progression au moment de l’exécution.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] propose des fenêtres de débogage.  
  
## <a name="breakpoints"></a>Points d’arrêt  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] Le concepteur propose la boîte de dialogue **Définir des points d’arrêt** dans laquelle vous pouvez définir des points d’arrêt en activant des conditions d’arrêt et en spécifiant le nombre d’occurrences d’un point d’arrêt avant la suspension de l’exécution du package. Les points d'arrêt peuvent être activés au niveau du package ou au niveau du composant. Si des conditions d’arrêt sont activées au niveau de la tâche ou du conteneur, l’icône de point d’arrêt apparaît en regard de la tâche ou du conteneur sur la surface de dessin de l’onglet **Flux de contrôle** . Si les conditions d’arrêt sont activées au niveau du package, l’icône de point d’arrêt apparaît sur l’étiquette de l’onglet **Flux de contrôle** .  
  
 Lorsqu'un point d'arrêt est atteint, l'icône de point d'arrêt se transforme pour vous aider à identifier la source du point d'arrêt. Vous pouvez ajouter, supprimer et modifier des points d'arrêt au cours de l'exécution du package.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] propose dix conditions d’arrêt que vous pouvez activer sur toutes les tâches et tous les conteneurs. Dans la boîte de dialogue **Définir des points d’arrêt** , vous pouvez activer des points d’arrêt pour les conditions suivantes :  
  
|Condition d'arrêt|Description|  
|---------------------|-----------------|  
|Quand la tâche ou le conteneur reçoit l’événement **OnPreExecute** .|Appelée lorsqu'une tâche est sur le point de s'exécuter. Cet événement est déclenché par une tâche ou un conteneur immédiatement avant son exécution.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnPostExecute** .|Appelée immédiatement après la fin de la logique d'exécution de la tâche. Cet événement est déclenché par une tâche ou un conteneur immédiatement après son exécution.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnError** .|Appelée par une tâche ou un conteneur lorsqu'une erreur se produit.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnWarning** .|Appelée lorsque la tâche est dans un état qui ne justifie pas une erreur, mais garantit un avertissement.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnInformation** .|Appelée lorsque la tâche doit fournir des informations.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnTaskFailed** .|Appelée par l'hôte de la tâche lorsqu'il échoue.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnProgress** .|Appelée pour mettre à jour la progression de l'exécution de la tâche.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnQueryCancel** .|Appelée à tout moment du traitement de la tâche lorsque vous pouvez annuler l'exécution de la tâche.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnVariableValueChanged** .|Appelée par le runtime [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] lorsque la valeur d'une variable change. L’événement RaiseChangeEvent de la variable doit avoir la valeur **true** pour déclencher cet événement.<br /><br /> **&#42;&#42; Avertissement &#42;&#42;** La variable associée à ce point d’arrêt doit être définie dans l’étendue du **conteneur**. Si la variable est définie dans l'étendue du package, le point d'arrêt n'obtient pas de correspondance.|  
|Quand la tâche ou le conteneur reçoit l’événement **OnCustomEvent** .|Appelée par les tâches pour déclencher des événements personnalisés définis par la tâche.|  
  
 Outre les conditions d'arrêt disponibles pour toutes les tâches et tous les conteneurs, certaines tâches et certains conteneurs proposent des conditions d'arrêt spéciales permettant de définir des points d'arrêt. Vous pouvez ainsi activer une condition d'arrêt sur le conteneur de boucles For définissant un point d'arrêt qui suspend l'exécution au début de chaque itération de la boucle.  
  
 Pour le rendre plus flexible et plus puissant, vous pouvez modifier le comportement d'un point d'arrêt en spécifiant les options suivantes :  
  
-   Le nombre d'accès, ou le nombre maximal d'occurrences d'une condition d'arrêt avant suspension de l'exécution.  
  
-   Le type du nombre d'accès, ou la règle spécifiant à quel moment la condition d'arrêt déclenche le point d'arrêt.  
  
 Les types du nombre d'accès, à l'exception du type Toujours, sont qualifiés de manière plus approfondie par le nombre d'accès. Par exemple, si le type est « Égal au nombre d'accès » et si le nombre d'accès est 5, l'exécution est suspendue à la sixième occurrence de la condition d'arrêt.  
  
 Le tableau suivant décrit les types de nombre d'accès.  
  
|Type du nombre d'accès|Description|  
|--------------------|-----------------|  
|Toujours|L'exécution est toujours suspendue lorsque le point d'arrêt est atteint.|  
|Égal au nombre d'accès|L'exécution est suspendue lorsque le nombre de fois où s'est produit le point d'arrêt est égal au nombre d'accès.|  
|Supérieur ou égal au nombre d'accès|L'exécution est suspendue lorsque le nombre de fois où s'est produit le point d'arrêt est supérieur ou égal au nombre d'accès.|  
|Multiple du nombre d'accès|L'exécution est suspendue lorsqu'un multiple du nombre d'accès est atteint. Par exemple, si vous définissez cette option sur 5, l'exécution est suspendue une fois toutes les cinq fois.|  
  
#### <a name="to-set-breakpoints"></a>Pour définir des points d'arrêt  
  
-   [Déboguer un package en définissant des points d’arrêt sur une tâche ou un conteneur](#debug)  
  
## <a name="progress-reporting"></a>Rapport de progression  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] Le concepteur propose deux types de rapports de progression : les codes de couleur sur l’aire de conception de l’onglet **Flux de contrôle** et les messages de progression sous l’onglet **Progression** .  
  
 Lorsque vous exécutez un package, le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] indique la progression de l'exécution en affichant chaque tâche ou conteneur dans une couleur qui indique l'état de l'exécution. En fonction de la couleur, vous pouvez déterminer si l'élément est en attente d'exécution, s'il est en cours d'exécution, s'il s'est terminé avec succès ou s'il s'est terminé avec des erreurs. Les codes de couleur disparaissent dès que vous arrêtez l'exécution du package.  
  
 Le tableau suivant indique les couleurs utilisées pour décrire l'état de l'exécution.  
  
|Couleur|État de l'exécution|  
|-----------|----------------------|  
|Gris|En attente d'exécution|  
|Jaune|Exécution en cours|  
|Vert|Exécuté avec succès|  
|mis en surbrillance|Exécuté avec des erreurs|  
  
 L’onglet **Progression** énumère les tâches et les conteneurs dans l’ordre d’exécution et indique les heures de début et de fin, les avertissements et les messages d’erreur. À la fin de l’exécution du package, les informations de progression restent disponibles sous l’onglet **Résultats d’exécution** .  
  
> [!NOTE]  
>  Pour activer ou désactiver l'affichage de messages sous l'onglet **Progression** , basculez l'option **Création de rapports de progression de débogage** dans le menu **SSIS** .  
  
 Le diagramme qui suit représente l’onglet **Progression** .  
  
 ![Onglet Progression du Concepteur SSIS](../../integration-services/troubleshooting/media/mw-dtsflow04.gif "Onglet Progression du Concepteur SSIS")  
  
## <a name="debug-windows"></a>Fenêtres de débogage  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] propose de nombreuses fenêtres que vous pouvez utiliser pour travailler avec les points d’arrêt et déboguer les packages qui contiennent des points d’arrêt. Pour en savoir plus sur chacune des fenêtres, ouvrez-les et appuyez sur F1 pour afficher l'aide.  
  
 Pour ouvrir ces fenêtres dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur le menu **Déboguer** , pointez sur **Fenêtres**, puis cliquez sur **Points d’arrêt**, **Sortie** ou **Immédiat**.  
  
 Le tableau qui suit décrit ces fenêtres.  
  
|Fenêtre|Description|  
|------------|-----------------|  
|Points d’arrêt|Énumère les points d'arrêt d'un package et donne accès aux options permettant d'activer ou de supprimer des points d'arrêt.|  
|Output|Affiche les messages d’état relatifs aux fonctionnalités de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|  
|Immédiat|Utilisée pour déboguer et évaluer des expressions, et imprimer des valeurs variables.|  

## <a name="debug-a-package-by-setting-breakpoints-on-a-task-or-a-container"></a><a name="debug"></a> Déboguer un package en définissant des points d'arrêt sur une tâche ou un conteneur
  Cette section décrit la procédure de définition des points d'arrêt dans un package, une tâche, un conteneur de boucle For ou Foreach ou un conteneur de séquences.  
  
### <a name="to-set-breakpoints-in-a-package-a-task-or-a-container"></a>Pour définir des points d'arrêt dans un package, une tâche ou un conteneur  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Double-cliquez sur le package dans lequel vous souhaitez définir des points d'arrêt.  
  
3.  Dans le concepteur SSIS, effectuez les opérations suivantes :  
  
    -   Pour définir des points d’arrêt dans l’objet package, cliquez sur l’onglet **Flux de contrôle** , placez le curseur n’importe où sur l’arrière-plan de l’aire de conception, cliquez avec le bouton droit, puis cliquez sur **Modifier les points d’arrêt**.  
  
    -   Pour définir des points d’arrêt dans un flux de contrôle de package, cliquez sur l’onglet **Flux de contrôle** , cliquez avec le bouton droit sur une tâche, un conteneur de boucle For ou Foreach ou un conteneur de séquences, puis cliquez sur **Modifier les points d’arrêt**.  
  
    -   Pour définir des points d’arrêt dans un gestionnaire d’événements, cliquez sur l’onglet **Gestionnaire d’événements** , cliquez avec le bouton droit sur une tâche, un conteneur de boucle For ou Foreach ou un conteneur de séquences, puis cliquez sur **Modifier les points d’arrêt**.  
  
4.  Dans la boîte de dialogue **Définir les points d’arrêt \<container name>** , sélectionnez les points d’arrêt à activer.  
  
5.  Vous pouvez également modifier le type du nombre d'accès et la valeur du nombre d'accès pour chaque point d'arrêt.  
  
6.  Pour enregistrer le package, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

## <a name="set-breakpoints"></a>Définir des points d’arrêt
  Utilisez la boîte de dialogue **Définir des points d'arrêt** pour spécifier les événements pour lesquels activer des points d'arrêt et pour gérer le contrôle du point d'arrêt.  
  
### <a name="options"></a>Options  
 **Activé**  
 Sélectionnez cette option pour activer un point d'arrêt sur un événement.  
  
 **Break Condition**  
 Affichez la liste des événements disponibles sur lesquels définir des points d'arrêt.  
  
 **Hit Count Type**  
 Spécifiez le moment où le point d'arrêt entre en vigueur.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Toujours**|L'exécution est toujours suspendue lorsque le point d'arrêt est atteint.|  
|**Égal au nombre d'accès**|L'exécution est suspendue lorsque le nombre de fois où s'est produit le point d'arrêt est égal au nombre d'accès.|  
|**Supérieur ou égal au nombre d'accès**|L'exécution est suspendue lorsque le nombre de fois où s'est produit le point d'arrêt est supérieur ou égal au nombre d'accès.|  
|**Multiple du nombre d'accès**|L'exécution est suspendue lorsqu'un multiple du nombre d'accès est atteint. Par exemple, si vous définissez cette option sur 5, l'exécution est suspendue une fois toutes les cinq fois.|  
  
 **Nombre d'accès**  
 Spécifiez le nombre d'accès à partir duquel déclencher un arrêt. Cette option n'est pas disponible si le point d'arrêt est constamment en vigueur.  
  
## <a name="see-also"></a>Voir aussi  
 [Outils de dépannage pour le développement des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)  
 [Déboguer un script en définissant des points d’arrêt dans une tâche de script et un composant de script](../../integration-services/extending-packages-scripting/debug-a-script-by-setting-breakpoints-in-a-script-task-and-script-component.md)   
