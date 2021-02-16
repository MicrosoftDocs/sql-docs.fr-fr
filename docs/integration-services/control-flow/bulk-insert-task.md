---
description: tâche d'insertion en bloc
title: Insertion en bloc, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.bulkinserttask.f1
- sql13.dts.designer.bulkinserttask.connection.f1
- sql13.dts.designer.bulkinserttask.general.f1
- sql13.dts.designer.bulkinserttask.options.f1
helpviewer_keywords:
- Bulk Insert task
- copying data [Integration Services]
ms.assetid: c5166156-6b4c-4369-81ed-27c4ce7040ae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 739086916d6b7bdea630e8327aa0ede0fb899c87
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348971"
---
# <a name="bulk-insert-task"></a>tâche d'insertion en bloc

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche d'insertion en bloc est un moyen efficace pour copier de gros volumes de données dans une table ou une vue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par exemple, supposons que votre entreprise stocke la liste de ses produits d’un million de lignes sur un mainframe, mais que son système d’e-commerce utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour fournir des données à des pages web. Vous devez mettre à jour la table des produits [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] toutes les nuits, à l'aide de la liste principale des produits depuis le gros ordinateur. Pour mettre à jour la table, vous enregistrez la liste des produits dans un fichier au format délimité par des tabulations, puis vous utilisez la tâche d'insertion en bloc pour copier les données directement dans la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour garantir une copie de données à haute vitesse, les transformations ne peuvent s'effectuer sur les données lors de leur déplacement entre le fichier source et la table ou la vue.  
  
## <a name="usage-considerations"></a>Considérations sur l'utilisation  
 Avant d'utiliser la tâche d'insertion en bloc, tenez compte des éléments suivants :  
  
-   La tâche d'insertion en bloc ne peut transférer des données que depuis un fichier texte vers une table ou une vue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour utiliser la tâche d’insertion en bloc afin de transférer des données depuis d’autres systèmes de gestion de base de données (SGBD), vous devez exporter les données de la source vers un fichier texte, puis les importer depuis celui-ci vers une table ou une vue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   La destination doit être une table ou une vue d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si la table ou la vue de destination contient déjà des données, les nouvelles données sont ajoutées à la table existante lors de l'exécution de la tâche d'insertion en bloc. Si vous souhaitez remplacer les données, avant de lancer la tâche d'insertion en bloc, utilisez une tâche d'exécution SQL qui applique une instruction DELETE ou TRUNCATE. Pour plus d'informations, consultez [Execute SQL Task](../../integration-services/control-flow/execute-sql-task.md).  
  
-   Vous pouvez utiliser un fichier de format dans l'objet tâche d'insertion en bloc. Si vous avez créé un fichier de format à l'aide de l'utilitaire **bcp** , vous pouvez spécifier son chemin d'accès dans la tâche d'insertion en bloc. La tâche d'insertion en bloc prend en charge les fichiers de format XML et non-XML. Pour plus d’informations sur les fichiers de format, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
-   Seuls les membres du rôle serveur fixe sysadmin peuvent exécuter un package contenant une tâche d'insertion en bloc.  
  
## <a name="bulk-insert-task-with-transactions"></a>Tâche d'insertion en bloc avec des transactions  
 Si aucune taille de traitement n'est définie, la copie par bloc complète est traitée comme une transaction. Une taille de traitement égale à **0** indique que les données sont insérées en un seul traitement. Si une taille de traitement est définie, chaque traitement représente une transaction validée à la fin de l'exécution de ce traitement.  
  
 Le fait que la tâche d'insertion en bloc se joigne ou non à la transaction sur package détermine son comportement vis-à-vis des transactions de package. Si la tâche d'insertion en bloc ne se joint pas à la transaction de package, chaque traitement exempt d'erreurs est validé comme une unité, avant que le traitement suivant soit traité. Si la tâche d'insertion en bloc se joint à la transaction de package, les traitements d'instructions exempts d'erreurs restent dans la transaction à la fin de la tâche. Ces traitements sont soumis à la validation ou à l'annulation du package.  
  
 Un échec de la tâche d'insertion en bloc ne provoque pas automatiquement l'annulation des traitements correctement chargés ; de même, si la tâche réussit, les traitements ne sont pas automatiquement validés. La validation et l'annulation ne se produisent qu'en fonction des paramètres de propriétés du package et du flux de travail.  
  
## <a name="source-and-destination"></a>Source et destination  
 Lorsque vous spécifiez l'emplacement du fichier texte source, tenez compte des éléments suivants :  
  
-   Le serveur doit être autorisé à accéder au fichier et à la base de données de destination.  
  
-   Le serveur exécute la tâche d'insertion en bloc. Par conséquent, tout fichier de format utilisé par la tâche doit se trouver sur le serveur.  
  
-   Le fichier source chargé par la tâche d'insertion en bloc peut se trouver sur un serveur distant ou sur le même serveur que la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle les données sont insérées. Si le fichier se situe sur un serveur distant, vous devez spécifier son nom en indiquant le nom UNC (Universal Naming Convention) dans le chemin d'accès.  
  
## <a name="performance-optimization"></a>Optimisation des performances  
 Pour optimiser les performances, tenez compte des éléments suivants :  
  
-   Si le fichier texte est situé sur le même ordinateur que la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle les données sont insérées, la copie a lieu à une vitesse plus rapide puisque les données n'ont pas à transiter par le réseau.  
  
-   La tâche d'insertion en bloc n'enregistre pas les lignes à l'origine d'erreurs. Si vous devez capturer ces informations, utilisez les sorties d'erreur des composants de flux de données afin de récupérer dans un fichier d'exception les lignes à l'origine des erreurs.  
  
## <a name="custom-log-entries-available-on-the-bulk-insert-task"></a>Entrées de journal personnalisées disponibles dans la tâche d'insertion en bloc  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche d'insertion en bloc. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**BulkInsertTaskBegin**|Indique que l'insertion en bloc a commencé.|  
|**BulkInsertTaskEnd**|Indique que l'insertion en bloc est terminée.|  
|**BulkInsertTaskInfos**|Fournit des informations détaillées concernant la tâche.|  
  
## <a name="bulk-insert-task-configuration"></a>Configuration de la tâche d'insertion en bloc  
 Vous pouvez configurer la tâche d'insertion en bloc comme suit :  
  
-   Spécifiez le gestionnaire de connexions OLE DB à utiliser pour établir la connexion à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destination et la table ou vue dans laquelle les données sont insérées. La tâche d'insertion en bloc prend uniquement en charge les connexions OLE DB de la base de données de destination.  
  
-   Indiquez le gestionnaire de connexions de fichiers ou de fichiers plats pour accéder au fichier source. La tâche d'insertion en bloc utilise le gestionnaire de connexions uniquement pour l'emplacement du fichier source. La tâche ignore les autres options que vous sélectionnez dans l'éditeur de gestionnaire de connexions.  
  
-   Définissez le format adopté par la tâche d'insertion en bloc, soit en utilisant un fichier de format, soit en configurant les séparateurs de colonnes et de lignes des données source. Si vous utilisez un fichier de format, indiquez le gestionnaire de connexions de fichiers permettant d'y accéder.  
  
-   Spécifiez les actions à réaliser dans la table ou la vue de destination au moment où la tâche insère les données. Les options comprennent la vérification éventuelle des contraintes, l'activation des insertions d'identité, la conservation des valeurs NULL, l'exécution des déclencheurs ou le verrouillage de la table.  
  
-   Indiquez les informations relatives au traitement de données à insérer, telles que la taille du traitement, les première et dernière lignes à insérer depuis le fichier, le nombre d'erreurs d'insertion pouvant se produire avant que la tâche arrête d'insérer des lignes et les noms des colonnes à trier.  
  
 Si la tâche d'insertion en bloc se sert d'un gestionnaire de connexions de fichiers plats pour accéder au fichier source, elle n'utilise pas le format spécifié dans ce gestionnaire. Elle utilise à la place le format spécifié dans un fichier de format, ou les valeurs des propriétés RowDelimiter et ColumnDelimiter de la tâche.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="programmatic-configuration-of-the-bulk-insert-task"></a>Configuration par programme de la tâche d'insertion en bloc  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>  
  
## <a name="related-tasks"></a>Tâches associées  
 [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="related-content"></a>Contenu associé  
  
-   Article technique, [You may get "Unable to prepare the SSIS bulk insert for data insertion" error on UAC enabled systems](https://go.microsoft.com/fwlink/?LinkId=233693), sur support.microsoft.com.  
  
-   Article technique, [Guide des performances de chargement des données](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100)), sur le site msdn.microsoft.com.  
  
-   Article technique, [Using SQL Server Integration Services to Bulk Load Data](https://go.microsoft.com/fwlink/?LinkId=233701), sur le site simple-talk.com.  
  
## <a name="bulk-insert-task-editor-connection-page"></a>Éditeur de tâche d'insertion en bloc (page Connexion)
  Utilisez la page **Connexion** de la boîte de dialogue **Éditeur de tâche d'insertion en bloc** pour définir la source et la destination de l'opération d'insertion en bloc et le format à utiliser.  
  
 Pour en savoir plus sur l’utilisation des insertions en bloc, consultez [Tâche d’insertion en bloc](../../integration-services/control-flow/bulk-insert-task.md) et [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
### <a name="options"></a>Options  
 **Connection**  
 Sélectionnez un gestionnaire de connexions OLE DB dans la liste ou cliquez sur \<**New connection...**> pour créer une connexion.  
  
 **Rubriques connexes :** [Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **Table de destination**  
 Tapez le nom de la table de destination ou affichez ou sélectionnez une table ou une vue dans la liste.  
  
 **Format**  
 Sélectionnez la source du format de l'insertion en bloc. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Utiliser un fichier**|Sélectionnez un fichier contenant la spécification de format. Cette option affiche l'option dynamique **FormatFile**.|  
|**Spécifier**|Spécifiez le format. Cette option affiche les options dynamiques **RowDelimiter** et **ColumnDelimiter**.|  
  
 **File**  
 Sélectionnez un gestionnaire de connexions de fichiers ou de fichiers plats dans la liste, ou cliquez sur \<**New connection...**> pour créer une connexion.  
  
 L'emplacement du fichier dépend du moteur de base de données SQL Server spécifié dans le gestionnaire de connexions pour cette tâche. Le fichier texte doit être accessible au moteur de base de données SQL Server situé sur un disque dur local du serveur ou via un partage ou un lecteur mappé à SQL Server. Le fichier n'est pas accessible au runtime SSIS.  
  
 Si vous accédez au fichier source en utilisant un gestionnaire de connexions de fichiers plats, la tâche d'insertion en bloc n'utilise pas le format défini dans le gestionnaire de connexions de fichiers plats. Elle utilise à la place le format spécifié dans un fichier de format, ou les valeurs des propriétés RowDelimiter et ColumnDelimiter de la tâche.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Gestionnaire de connexions de fichiers plats](../../integration-services/connection-manager/flat-file-connection-manager.md) 
  
 **Actualiser les tables**  
 Actualise la liste des tables et des vues.  
  
### <a name="format-dynamic-options"></a>Options dynamiques de format  
  
#### <a name="format--use-file"></a>Format = Utiliser un fichier  
 **FormatFile**  
 Tapez le chemin du fichier de format ou cliquez sur le bouton avec des points de suspension **(...)** pour rechercher le fichier de format.  
  
#### <a name="format--specify"></a>Format = Spécifier  
 **RowDelimiter**  
 Spécifiez le délimiteur de ligne dans le fichier source. La valeur par défaut est **{CR}{LF}** .  
  
 **ColumnDelimiter**  
 Spécifiez le délimiteur de colonne dans le fichier source. La valeur par défaut est **Tab**.  
  
## <a name="bulk-insert-task-editor-general-page"></a>Éditeur de tâche d'insertion en bloc (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche d'insertion en bloc** afin d'attribuer un nom et décrire la tâche d'insertion en bloc.  
  
### <a name="options"></a>Options  
 **Nom**  
 Permet d'attribuer un nom unique à la tâche d'insertion en bloc. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Permet de fournir une description à la tâche d'insertion en bloc.  
 
## <a name="bulk-insert-task-editor-options-page"></a>Éditeur de tâche d'insertion en bloc (page Options)
  Utilisez la page **Options** de la boîte de dialogue **Éditeur de tâche d'insertion en bloc** afin de définir les propriétés de l'opération d'insertion en bloc. La tâche d’insertion en bloc copie des volumes importants de données dans une table ou une vue [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour en savoir plus sur l’utilisation des insertions en bloc, consultez [Tâche d’insertion en bloc](../../integration-services/control-flow/bulk-insert-task.md) et [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
### <a name="options"></a>Options  
 **CodePage**  
 Permet d'indiquer la page de codes des données dans le fichier de données.  
  
 **DataFileType**  
 Permet d'indiquer la valeur correspondant au type de données à utiliser lors d'une opération de chargement.  
  
 **BatchSize**  
 Permet d'indiquer le nombre de lignes contenues dans un traitement. La valeur par défaut correspond à la totalité du fichier de données. Si vous attribuez à **BatchSize** la valeur zéro, les données sont chargées dans un traitement unique.  
  
 **LastRow**  
 Permet de spécifier la dernière ligne à copier.  
  
 **FirstRow**  
 Permet de spécifier la première ligne à partir de laquelle la copie doit commencer.  
  
 **Options**  
 |Terme|Définition|  
|----------|----------------|  
|**Contraintes de validation**|Permet de vérifier les contraintes s'appliquant à la table et aux colonnes.|  
|**Conserver les valeurs NULL**|Permet de conserver les valeurs Null pendant l'opération d'insertion en bloc au lieu d'insérer des valeurs par défaut dans les colonnes vides.|  
|**Activer l’insertion d’identité**|Permet d'insérer des valeurs existantes dans une colonne d'identité.|  
|**Verrou de table**|Permet de verrouiller la table lors de l'opération d'insertion en bloc.|  
|**Exécuter les déclencheurs**|Lance tout déclencheur d'insertion, de mise à jour ou de suppression sur la table.|  
  
 **SortedData**  
 Implique l'ajout de la clause ORDER BY dans l'instruction d'insertion en bloc. Le nom de colonne que vous fournissez doit être celui d'une colonne valide pour la table de destination. La valeur par défaut est **false**. En d'autres termes, les données ne sont pas triées par une clause ORDER BY.  
  
 **MaxErrors**  
 Permet de spécifier le nombre maximal d'erreurs tolérées avant l'annulation de l'opération d'insertion en bloc. La valeur 0 indique qu'un nombre illimité d'erreurs est autorisé.  
  
> [!NOTE]  
>  Chaque ligne ne pouvant pas être importée par l'opération de chargement en masse est comptée comme une erreur.  
