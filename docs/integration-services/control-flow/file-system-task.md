---
description: Tâches du système de fichiers
title: Système de fichiers, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.filesystemtask.f1
- sql13.dts.designer.filesystemtask.general.f1
helpviewer_keywords:
- File System task [Integration Services]
ms.assetid: 7dd79a6a-e066-4028-a385-1d40f31056f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 51646edb5b2937d04d4915e2a7d4a66327343a6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345714"
---
# <a name="file-system-task"></a>Tâches du système de fichiers

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche de système de fichiers effectue des opérations sur les fichiers et les répertoires du système de fichiers. Par exemple, à l'aide de la tâche de système de fichiers, un package peut créer, déplacer ou supprimer des répertoires et des fichiers. Vous pouvez également utiliser la tâche de système de fichiers pour définir les attributs des fichiers et des répertoires. Par exemple, la tâche de système de fichiers peut rendre les fichiers cachés ou accessibles en lecture seule.  
  
 Toutes les opérations de la tâche de système de fichiers utilisent une source, qui peut être un fichier ou un répertoire. Par exemple, le fichier que la tâche copie, ou le répertoire qu'elle supprime, est une source. La source peut être spécifiée à l'aide d'un gestionnaire de connexions de fichiers qui pointe vers le répertoire ou vers le fichier, ou à l'aide du nom d'une variable qui contient le chemin d'accès source. Pour plus d’informations, consultez [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md) et [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
 Les opérations qui copient et déplacent les fichiers et les répertoires, et qui renomment les fichiers, utilisent une destination et une source. La destination est spécifiée à l'aide d'un gestionnaire de connexions de fichiers ou d'une variable. Les opérations de la tâche de système de fichiers peuvent être configurées afin de permettre le remplacement des fichiers et des répertoires de destination. L'opération de création d'un nouveau répertoire peut être configurée de manière à utiliser un répertoire existant portant le nom spécifié au lieu d'échouer si le répertoire existe déjà.  
  
## <a name="predefined-file-system-operations"></a>Opérations prédéfinies sur le système de fichiers  
 La tâche de système de fichiers comprend un ensemble prédéfini d'opérations. Le tableau suivant décrit ces opérations.  
  
|Opération|Description|  
|---------------|-----------------|  
|Copier le répertoire|Copie un dossier d'un emplacement à l'autre.|  
|Copier le fichier|Copie un fichier d'un emplacement à l'autre.|  
|Créer un répertoire|Crée un dossier à un emplacement spécifié.|  
|Supprimer le répertoire|Supprime un dossier à un emplacement spécifié.|  
|Supprimer le contenu du répertoire|Supprime la totalité des fichiers et des dossiers d'un dossier.|  
|Supprimer le fichier|Supprime un fichier à un emplacement spécifié.|  
|Déplacer le répertoire|Déplace un dossier d'un emplacement à l'autre.|  
|Déplacer le fichier|Déplace un fichier d'un emplacement à l'autre.|  
|Renommer le fichier|Renomme un fichier à un emplacement spécifié.|  
|Définir des attributs|Définit les attributs des fichiers et des dossiers. Les attributs sont Archive, Caché, Normal, Lecture seule et Système. L'option Normal indique l'absence d'attributs et ne peut pas être combinée avec les autres attributs. Vous pouvez combiner tous les autres attributs.|  
  
 La tâche de système de fichiers s'exécute sur un seul fichier ou répertoire. Cette tâche ne prend donc pas en charge l'utilisation de caractères génériques pour effectuer la même opération sur plusieurs fichiers. Pour que la tâche de système de fichiers répète une même opération sur plusieurs fichiers ou répertoires, mettez la tâche de système de fichiers dans un conteneur de boucles Foreach, comme décrit dans les étapes suivantes :  
  
-   **Configuration du conteneur de boucles Foreach** : dans la page **Collection** de l’Éditeur de boucle Foreach, définissez l’énumérateur sur **Énumérateur Foreach File** et entrez l’expression générique comme configuration d’énumérateur pour **Fichiers**. Dans la page **Mappage de variables** de l’Éditeur de boucle Foreach, mappez la variable que vous voulez utiliser pour transmettre un par un les noms de fichiers à la tâche de système de fichiers.  
  
-   **Ajout et configuration d’une tâche de système de fichiers** : ajoutez une tâche de système de fichiers au conteneur de boucles Foreach. Dans la page **Général** de l’Éditeur de tâche de système de fichiers, affectez à la propriété **SourceVariable** ou **DestinationVariable** la variable que vous avez définie dans le conteneur de boucles Foreach.  
  
## <a name="custom-log-entries-available-on-the-file-system-task"></a>Entrées de journal personnalisées disponibles dans la tâche de système de fichiers  
 Le tableau suivant décrit l'entrée de journal personnalisée pour la tâche de système de fichiers. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**FileSystemOperation**|Indique l'opération que la tâche effectue. L'entrée de journal est écrite au démarrage de l'opération du système de fichiers et inclut des informations sur la source et la destination.|  
  
## <a name="configuring-the-file-system-task"></a>Configuration de la tâche de système de fichiers  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez les rubriques suivantes :  
  
-   [Éditeur de tâche de système de fichiers &#40;page Général&#41;]()  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d’informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
 Pour plus d'informations sur la définition par programmation de ces propriétés, consultez la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask>  
  
## <a name="related-tasks"></a>Tâches associées  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend une tâche qui télécharge et charge les fichiers de données et gère les répertoires sur les serveurs. Pour plus d’informations, consultez [Tâche FTP](../../integration-services/control-flow/ftp-task.md).  
  
## <a name="file-system-task-editor-general-page"></a>Éditeur de tâche de système de fichiers (page Général)
  Utilisez la page **Général** de l' **Éditeur de tâche de système de fichiers** pour configurer l'opération de système de fichiers qu'exécute la tâche.  
  
 Vous devez spécifier un gestionnaire de connexions source et de destination en définissant les propriétés SourceConnection et DestinationConnection. Vous pouvez fournir les noms de gestionnaires de connexions de fichiers qui pointent sur les fichiers utilisés par la tâche comme source ou destination. Si les chemins de fichiers sont stockés dans des variables, vous pouvez également fournir le nom des variables. Pour utiliser des variables afin de stocker les chemins des fichiers, vous devez d’abord définir l’option IsSourcePathVariable de la connexion source et l’option IsDestinationPatheVariable de la connexion de destination sur **True**. Ensuite, vous pouvez créer de nouvelles variables ou choisir les variables existantes, système ou définies par l'utilisateur, à utiliser. Dans la boîte de dialogue **Ajouter une variable** , vous pouvez configurer et spécifier l'étendue des variables. L'étendue doit être la tâche de système de fichiers ou un conteneur parent. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](../integration-services-ssis-variables.md).  
  
> [!NOTE]  
>  Pour remplacer les variables sélectionnées pour les propriétés **SourceConnection** et **DestinationConnection** , entrez une expression pour les propriétés **Source** et **Destination** . Vous entrez les expressions sur la page **Expressions** de l' **Éditeur de tâche de système de fichiers**. Par exemple, pour définir le chemin d'accès des fichiers utilisés comme destination par la tâche, vous pouvez utiliser la variable A dans certaines conditions et la variables B dans d'autres conditions.  
  
> [!NOTE]  
>  La tâche de système de fichiers s'exécute sur un seul fichier ou répertoire. Cette tâche ne prend donc pas en charge l'utilisation de caractères génériques pour effectuer la même opération sur plusieurs fichiers ou répertoires. Pour que la tâche de système de fichiers répète une même opération sur plusieurs fichiers ou répertoires, placez-la dans un conteneur de boucles Foreach. Pour plus d’informations, consultez [File System Task](../../integration-services/control-flow/file-system-task.md).  
  
 Vous pouvez utiliser des expressions pour recourir à différentes variables.  
  
### <a name="options"></a>Options  
 **IsDestinationPathVariable**  
 Indique si le chemin d'accès de destination est stocké dans une variable. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Le chemin d'accès de destination est stocké dans une variable. Cette valeur affiche l'option dynamique **DestinationVariable**.|  
|**False**|Le chemin d'accès de destination est défini dans un gestionnaire de connexions de fichiers. Cette valeur affiche l'option dynamique **DestinationConnection**.|  
  
 **OverwriteDestination**  
 Indiquez si l'opération peut remplacer les fichiers dans le répertoire de destination.  
  
 **Nom**  
 Fournissez un nom unique pour la tâche de système de fichiers. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Tapez la description de la tâche de système de fichiers.  
  
 **opération**  
 Sélectionnez l'opération de système de fichiers à exécuter. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Copier le répertoire**|Copier un répertoire. Cette valeur affiche les options dynamiques d'une source et d'une destination.|  
|**Copier un fichier**|Copier un fichier. Cette valeur affiche les options dynamiques d'une source et d'une destination.|  
|**Créer un répertoire**|Créer un répertoire Cette valeur affiche les options dynamiques d'un répertoire source et de destination.|  
|**Supprimer le répertoire**|Supprimer un répertoire. Cette valeur affiche les options dynamiques d'une source.|  
|**Supprimer le contenu du répertoire**|Supprimer le contenu d'un répertoire. Cette valeur affiche les options dynamiques d'une source.|  
|**Supprimer un fichier**|Supprimer un fichier. Cette valeur affiche les options dynamiques d'une source.|  
|**Déplacer le répertoire**|Déplacer un répertoire. Cette valeur affiche les options dynamiques d'une source et d'une destination.|  
|**Déplacer le fichier**|Déplacer un fichier. Cette valeur affiche les options dynamiques d'une source et d'une destination. Lorsque vous déplacez un fichier, n'incluez pas de nom de fichier dans le chemin d'accès au répertoire que vous fournissez comme destination.|  
|**Renommer le fichier**|Renommer un fichier. Cette valeur affiche les options dynamiques d'une source et d'une destination. Lorsque vous renommez un fichier, incluez le nouveau nom de fichier dans le chemin d'accès au répertoire que vous fournissez comme destination.|  
|**Définir les attributs**|Définir les attributs d'un fichier ou d'un répertoire. Cette valeur affiche les options dynamiques d'une source et d'une opération.|  
  
 **IsSourcePathVariable**  
 Indique si le chemin d'accès de destination est stocké dans une variable. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur||  
|-----------|-|  
|**True**|Le chemin d'accès de destination est stocké dans une variable. Sélectionnez cette valeur pour afficher l'option dynamique **SourceVariable**.|  
|**False**|Le chemin d'accès de destination est défini dans un gestionnaire de connexions de fichiers. Cette valeur affiche l'option dynamique **DestinationVariable**.|  
  
### <a name="isdestinationpathvariable-dynamic-options"></a>Options dynamiques IsDestinationPathVariable  
  
#### <a name="isdestinationpathvariable--true"></a>IsDestinationPathVariable = True  
 **DestinationVariable**  
 Sélectionnez le nom de variable dans la liste ou cliquez sur \<**New variable...**> pour créer une variable.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
#### <a name="isdestinationpathvariable--false"></a>IsDestinationPathVariable = False  
 **DestinationConnection**  
 Sélectionnez un gestionnaire de connexions de fichiers dans la liste ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
### <a name="issourcepathvariable-dynamic-options"></a>Options dynamiques IsSourcePathVariable  
  
#### <a name="issourcepathvariable--true"></a>IsSourcePathVariable = True  
 **SourceVariable**  
 Sélectionnez le nom de variable dans la liste ou cliquez sur \<**New variable...**> pour créer une variable.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
#### <a name="issourcepathvariable--false"></a>IsSourcePathVariable = False  
 **SourceConnection**  
 Sélectionnez un gestionnaire de connexions de fichiers dans la liste ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md)  
  
### <a name="operation-dynamic-options"></a>Options dynamiques d'opération  
  
#### <a name="operation--set-attributes"></a>Opération = Définir les attributs  
 **Hidden**  
 Indique si le fichier ou le répertoire est visible.  
  
 **Lecture seule**  
 Indique si le fichier est en lecture seule.  
  
 **Archive**  
 Indique si le fichier ou le répertoire est prêt pour l'archivage.  
  
 **Système**  
 Indique si le fichier est un fichier de système d'exploitation.  
  
#### <a name="operation--create-directory"></a>Opération = Créer un répertoire  
 **UseDirectoryIfExists**  
 Indique si l’option **Créer un répertoire** utilise un répertoire existant portant le nom spécifié au lieu de créer un nouveau répertoire.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
