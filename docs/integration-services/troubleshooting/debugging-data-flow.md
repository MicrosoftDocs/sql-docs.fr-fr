---
description: Débogage d'un flux de données
title: Débogage d’un flux de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- progress reporting [Integration Services]
- data viewers [Integration Services]
- data flow [Integration Services], debugging
- debugging [Integration Services], data flow
- counting rows
ms.assetid: 1c574f1b-54f7-4c05-8e42-8620e2c1df0f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c69bed5fbec76fab380f91515827630fa60938a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342164"
---
# <a name="debugging-data-flow"></a>Débogage d'un flux de données

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] incluent des fonctionnalités et des outils permettant de résoudre les problèmes des flux de données d’un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] - Le concepteur SSIS fournit des visionneuses de données.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] - Le concepteur SSIS et les transformations [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournissent des nombres de lignes.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] - Le concepteur SSIS fournit des rapports de progression au moment de l’exécution.  
  
## <a name="data-viewers"></a>Visionneuses de données  
 Les visionneuses de données affichent les données entre deux composants d'un flux de données. Elles permettent d'afficher les données lorsque celles-ci sont extraites d'une source de données et intègrent pour la première fois un flux de données, avant et après la mise à jour des données par une transformation et avant le chargement des données dans leur destination.  
  
 Pour afficher les données, vous devez attacher des visionneuses au chemin d'accès qui connecte deux composants de flux de données. Le fait de pouvoir afficher les données entre deux composants de flux de données facilite l'identification des valeurs de données inattendues, permet de voir les modifications apportées par une transformation aux valeurs des colonnes et permet de découvrir la raison pour laquelle une transformation échoue. Par exemple, si vous découvrez qu'une recherche dans une table de référence échoue et que vous souhaitez corriger cette erreur, vous voudrez peut-être ajouter une transformation qui fournit des données par défaut pour les colonnes vides.  
  
 Une visionneuse de données peut afficher des données dans une grille. Dans une grille, vous sélectionnez les colonnes à afficher. Les valeurs des colonnes sélectionnées s'affichent sous forme de tableau.  
  
 Vous pouvez également inclure plusieurs visionneuses de données dans un chemin d'accès. Vous pouvez afficher les mêmes données dans différents formats ; par exemple créer un graphique et une grille des données, ou créer des visionneuses de données distinctes pour différentes colonnes de données.  
  
 Quand vous ajoutez une visionneuse de données à un chemin, le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ajoute une icône de visionneuse de données sur l’aire de conception de l’onglet **Flux de données** , en regard du chemin. Les transformations acceptant les sorties multiples (telles que la transformation de fractionnement conditionnel) peuvent inclure une visionneuse de données sur chaque chemin d'accès.  
  
 Au moment de l’exécution, une fenêtre **Visionneuse de données** s’ouvre et affiche les informations spécifiées par le format de la visionneuse de données. Par exemple, une visionneuse de données qui utilise le format de grille affiche les données des colonnes sélectionnées, le nombre de lignes de sortie transmises au composant du flux de données et le nombre de lignes affichées. Ces informations s'affichent tampon par tampon et, selon la largeur des lignes dans le flux de données, un tampon peut contenir plus ou moins de lignes.  
  
 Dans la boîte de dialogue **Visionneuse de données** , vous pouvez copier les données dans le Presse-papiers, effacer toutes les données de la table, reconfigurer la visionneuse de données, reprendre le flux de données, ainsi que détacher ou attacher la visionneuse de données.  
  
#### <a name="to-add-a-data-viewer"></a>Pour ajouter une visionneuse de données  
  
-   [Ajouter une visionneuse de données à un flux de données](#add_viewer)  
  
## <a name="row-counts"></a>Nombres de lignes  
 Le nombre de lignes transférées via un chemin est affiché sur l’aire de conception de l’onglet **Flux de données** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] en regard du chemin. Ce nombre est mis à jour régulièrement quand les données empruntent le chemin.  
  
 Vous pouvez également ajouter une transformation de nombre de lignes au flux de données pour capturer le nombre de lignes final dans une variable. Pour plus d’informations, voir [Row Count Transformation](../../integration-services/data-flow/transformations/row-count-transformation.md).  
  
## <a name="progress-reporting"></a>Rapport de progression  
 Quand vous exécutez un package, le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] indique la progression sur l’aire de conception de l’onglet **Flux de données** en affichant chaque composant du flux de données dans une couleur qui indique son état. Lorsque les composants commencent à effectuer leur travail, ils passent à la couleur jaune et une fois terminés, ils passent à la couleur verte. Une couleur rouge indique que le composant a échoué.  
  
 Le tableau suivant décrit les codes de couleur.  
  
|Couleur|Description|  
|-----------|-----------------|  
|Aucune couleur|En attente d'être appelé par le moteur de flux de données.|  
|Jaune|Exécution d'une transformation, extraction de données ou chargement de données en cours.|  
|Vert|Exécuté avec succès.|  
|rouge|Exécuté avec des erreurs.|  

## <a name="analysis-of-data-flow"></a>Analyse des flux de données
  Vous pouvez utiliser la vue de base de données [catalog.execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) **SSISDB** pour analyser le flux de données des packages. Cette vue affiche une ligne à chaque fois qu'un composant de flux de données envoie des données à un composant en aval. Les informations peuvent être utilisées pour mieux comprendre les lignes envoyées à chaque composant.  
  
> [!NOTE]  
>  Le niveau de journalisation doit avoir la valeur **Commentaires** afin de capturer des informations avec la vue catalog.execution_data_statistics.  
  
 L'exemple suivant affiche le nombre de lignes transmises entre les composants d'un package.  
  
```sql
use SSISDB  
select package_name, task_name, source_component_name, destination_component_name, rows_sent  
from catalog.execution_data_statistics  
where execution_id = 132  
order by source_component_name, destination_component_name   
```  
  
 L'exemple suivant calcule le nombre de lignes par milliseconde envoyées par chaque composant pour une exécution spécifique. Les valeurs calculées sont les suivantes :  
  
-   **total_rows** - Somme de toutes les lignes envoyées par le composant  
  
-   **wall_clock_time_ms** - Durée d’exécution écoulée totale en millisecondes pour chaque composant  
  
-   **num_rows_per_millisecond** - Nombre de lignes par milliseconde envoyées par chaque composant  
  
 La clause **HAVING** est utilisée pour éviter une erreur de division par zéro dans les calculs.  
  
```sql  
use SSISDB  
select source_component_name, destination_component_name,  
    sum(rows_sent) as total_rows,  
    DATEDIFF(ms,min(created_time),max(created_time)) as wall_clock_time_ms,  
    ((0.0+sum(rows_sent)) / (datediff(ms,min(created_time),max(created_time)))) as [num_rows_per_millisecond]  
from [catalog].[execution_data_statistics]  
where execution_id = 132  
group by source_component_name, destination_component_name  
having (datediff(ms,min(created_time),max(created_time))) > 0  
order by source_component_name desc  
```  

## <a name="configure-an-error-output-in-a-data-flow-component"></a>Configurer une sortie d'erreur dans un composant de flux de données
  De nombreux composants de flux de données prennent en charge les sorties d’erreur, et en fonction du composant, le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] offre différentes manières de configurer une sortie d’erreur. En plus de configurer une sortie d'erreur, vous pouvez également configurer les colonnes d'une sortie d'erreur. Cela inclut la configuration des colonnes **ErrorCode** et **ErrorColumn** ajoutées par le composant.  
  
### <a name="configuring-an-error-output"></a>Configuration d'une sortie d'erreur  
 Pour configurer une sortie d'erreur, vous avez deux options :  
  
-   Utilisez la boîte de dialogue **Configurer la sortie d’erreur** . Cette boîte de dialogue vous permet de configurer une sortie d'erreur sur n'importe quel composant de flux de données prenant en charge les sorties d'erreur.  
  
-   Utilisez la boîte de dialogue de l'éditeur pour le composant. Certains composants vous permettent de configurer directement des sorties d'erreur dans la boîte de dialogue de l'éditeur. Toutefois, vous ne pouvez pas configurer des sorties d’erreur dans la boîte de dialogue de l’éditeur pour la source ADO NET, la transformation d’importation de colonne, la transformation de commande OLE DB ou la destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 Les procédures suivantes décrivent l'utilisation de ces boîtes de dialogue pour configurer des sorties d'erreur.  
  
#### <a name="to-configure-an-error-output-using-the-configure-error-output-dialog-box"></a>Pour configurer une sortie d'erreur à l'aide de la boîte de dialogue Configurer la sortie d'erreur  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l’onglet **Flux de données** .  
  
4.  Faites glisser la sortie d'erreur, représentée par la flèche rouge, du composant qui est la source des erreurs vers un autre composant du flux de données.  
  
5.  Dans la boîte de dialogue **Configurer la sortie d’erreur** , sélectionnez une action dans les colonnes **Erreur** et **Troncation** pour chaque colonne de sortie du composant.  
  
6.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  
  
#### <a name="to-add-an-error-output-using-the-editor-dialog-box-for-the-component"></a>Pour ajouter une sortie d'erreur à l'aide de la boîte de dialogue de l'éditeur du composant  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l’onglet **Flux de données** .  
  
4.  Double-cliquez sur les composants de flux de données pour lesquels vous voulez configurer une sortie d'erreur, et en fonction du composant, procédez de l'une des manières suivantes :  
  
    -   Cliquez sur **Configurer la sortie d’erreur**.  
  
    -   Cliquez sur **Sortie d’erreur**.  
  
5.  Définissez l’option **Erreur** pour chaque colonne.  
  
6.  Définissez l’option **Troncation** pour chaque colonne.  
  
7.  Cliquez sur **OK**.  
  
8.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  
  
### <a name="configuring-error-output-columns"></a>Configuration des colonnes de sortie d'erreur  
 Pour configurer les colonnes de sortie d’erreur, vous devez utiliser l’onglet **Propriétés d’entrée et de sortie** dans la boîte de dialogue **Éditeur avancé** .  
  
#### <a name="to-configure-error-output-columns"></a>Pour configurer des colonnes de sortie d'erreur  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l’onglet **Flux de données** .  
  
4.  Cliquez avec le bouton droit sur le composant dont vous voulez configurer les colonnes de sortie d’erreur et cliquez sur **Afficher l’éditeur avancé**.  
  
5.  Cliquez sur l’onglet **Propriétés d’entrée et de sortie** et développez **Sortie d’erreur de \<component name>** , puis **Colonnes de sortie**.  
  
6.  Cliquez sur une colonne et mettez à jour ses propriétés.  
  
    > [!NOTE]  
    >  La liste des colonnes comprend les colonnes de l’entrée du composant, les colonnes **ErrorCode** et **ErrorColumn** ajoutées par des sorties d'erreur précédentes et les colonnes **ErrorCode** et **ErrorColumn** ajoutées par ce composant.  
  
7.  Cliquez sur **OK**.  
  
8.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  

## <a name="add-a-data-viewer-to-a-data-flow"></a><a name="add_viewer"></a> Ajouter une visionneuse de données à un flux de données
  Cette rubrique explique comment ajouter et configurer une visionneuse de données dans un flux de données. Une visionneuse de données affiche des données déplacées entre deux composants de flux de données Par exemple, une visionneuse de données peut afficher les données extraites d'une source de données avant qu'une transformation dans le flux de données modifie les données.  
  
 Un chemin d'accès connecte des composants d'un flux de données en reliant la sortie d'un composant à l'entrée d'un autre composant.  
  
 Avant que vous puissiez ajouter des visionneuses de données à un package, celui-ci doit inclure une tâche de flux de données et au moins deux composants de flux de données connectés.  
  
 Associe une visionneuse de données à une sortie d’erreur pour afficher la description de l’erreur et le nom de la colonne dans laquelle l’erreur s’est produite. Par défaut, la sortie d’erreur inclut uniquement des identificateurs numériques pour l’erreur et la colonne.  
  
### <a name="to-add-a-data-viewer-to-a-data-flow"></a>Pour ajouter une visionneuse de données à un flux de données  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de contrôle** , s'il n'est pas déjà sélectionné.  
  
4.  Cliquez sur la tâche de flux de données au flux de données de laquelle vous voulez joindre une visionneuse de données, puis cliquez sur l'onglet **Flux de données** .  
  
5.  Cliquez avec le bouton droit sur un chemin entre deux composants de flux de données, puis cliquez sur **Modifier**.  
  
6.  Dans la page **Général** , vous pouvez afficher et modifier les propriétés du chemin d'accès. Par exemple, dans la liste déroulante **PathAnnotation** , vous pouvez sélectionner l’annotation qui apparaît en regard du chemin.  
  
7.  Dans la page **Métadonnées** , vous pouvez afficher les métadonnées de la colonne et les copier dans le presse-papiers.  
  
8.  Dans la page **Visionneuse de données** , cliquez sur **Activer la visionneuse de données**.  
  
9. Dans la zone Colonnes à afficher, sélectionnez les colonnes à afficher dans la vue de source de données. Par défaut, toutes les colonnes disponibles sont sélectionnées et figurent dans la liste **Colonnes affichées** . Déplacez les colonnes que vous ne voulez pas utiliser dans la liste **Colonnes inutilisées** en les sélectionnant et en cliquant sur la flèche gauche.  
  
    > [!NOTE]  
    >  Dans la grille, les valeurs qui représentent les types de données DT_DATE, DT_DBTIME2, DT_FILETIME, DT_DBTIMESTAMP, DT_DBTIMESTAMP2 et DT_DBTIMESTAMPOFFSET apparaissent sous forme de chaînes au format ISO 8601 et un espace de séparation remplace le séparateur **T** . Les valeurs qui représentent les types de données DT_DATE et DT_FILETIME incluent sept chiffres pour les fractions de seconde. Étant donné que le type de données DT_FILETIME stocke uniquement trois chiffres pour les fractions de seconde, la grille affiche des zéros pour les quatre chiffres restants. Les valeurs qui représentent le type de données DT_DBTIMESTAMP incluent trois chiffres pour les fractions de seconde. Pour les valeurs qui représentent les types de données DT_DBTIME2, DT_DBTIMESTAMP2 et DT_DBTIMESTAMPOFFSET, le nombre de chiffres pour les fractions de seconde correspond à l'échelle spécifiée pour le type de données de la colonne. Pour plus d'informations sur les formats ISO 8601, consultez [Date and Time Formats](../data-flow/parsing-data.md). Pour plus d'informations sur les types de données, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
10. Cliquez sur **OK**.  

## <a name="data-flow-taps"></a>Drainage des flux de données
 Vous pouvez ajouter un drainage des données au niveau du chemin d’un flux de données d’un package au moment de l’exécution et diriger la sortie à partir du drainage des données vers un fichier externe. Pour utiliser cette fonctionnalité, vous devez déployer votre projet SSIS à l'aide du modèle de déploiement de projet sur un serveur SSIS. Après avoir déployé le package sur le serveur, vous devez exécuter des scripts T-SQL sur la base de données SSISDB pour ajouter des drainages de données avant d'exécuter le package. Voici un exemple de scénario :  
  
1.  Créez une instance d’exécution d’un package à l’aide de la procédure stockée [catalog.create_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
2.  Ajoutez un drainage de données à l’aide d’une procédure stockée [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md) ou [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md) .  
  
3.  Démarrez l’instance d’exécution du package à l’aide de [catalog.start_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md).  
  
 Voici un exemple de script SQL qui exécute les étapes décrites dans le scénario ci-dessus :  
  
```sql
Declare @execid bigint  
EXEC [SSISDB].[catalog].[create_execution] @folder_name=N'ETL Folder', @project_name=N'ETL Project', @package_name=N'Package.dtsx', @execution_id=@execid OUTPUT  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt'  
EXEC [SSISDB].[catalog].[start_execution] @execid  
```  
  
 Les paramètres folder_name, project_name et package_name de la procédure stockée create_execution correspondent au dossier, au projet et au package dans le catalogue Integration Services. Vous pouvez obtenir les noms de dossier, projet et package à utiliser dans l'appel de create_execution dans SQL Server Management Studio comme le montre l'image suivante. Si le projet SSIS ne s'affiche pas ici, vous n'avez peut-être pas encore déployé le projet sur le serveur SSIS. Cliquez avec le bouton droit sur le projet SSIS dans Visual Studio, puis cliquez sur Déployer pour déployer le projet sur le serveur SSIS prévu.  
  
 Au lieu de taper des instructions SQL, générez le script d'exécution de package en procédant comme suit :  
  
1.  Cliquez avec le bouton droit sur **Package.dtsx** , puis sélectionnez **Exécuter**.  
  
2.  Cliquez sur le bouton de la barre d'outils **Script** pour générer le script.  
  
3.  Ajoutez l'instruction add_data_tap avant l'appel de start_execution.  
  
 Le paramètre task_package_path de la procédure stockée add_data_tap correspond à la propriété PackagePath de la tâche de flux de données dans Visual Studio. Dans Visual Studio, cliquez avec le bouton droit sur **Tâche de flux de données**, puis cliquez sur **Propriétés** pour lancer la fenêtre Propriétés.  Notez la valeur de la propriété **PackagePath** pour l’utiliser comme valeur du paramètre task_package_path de l’appel de procédure stockée add_data_tap.  
  
 Le paramètre dataflow_path_id_string de la procédure stockée add_data_tap correspond à la propriété IdentificationString du chemin d'accès de flux de données auquel vous voulez ajouter un drainage de données. Pour obtenir dataflow_path_id_string, cliquez sur le chemin de flux de données (flèche entre les tâches du flux de données), et notez la valeur de la propriété **IdentificationString** dans la fenêtre Propriétés.  
  
 Quand vous exécutez le script, le fichier de sortie est stocké dans \<Program Files>\Microsoft SQL Server\110\DTS\DataDumps. S'il existe déjà un fichier du même nom, un nouveau fichier avec un suffixe (par exemple : output[1].txt) est créé.  
  
 Comme mentionné précédemment, vous pouvez également utiliser la procédure stockée [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)plutôt que add_data_tap. Cette procédure stockée accepte l'ID de tâche de flux de données comme paramètre au lieu de task_package_path. L'ID de tâche de flux de données est disponible dans la fenêtre Propriétés de Visual Studio.  
  
### <a name="removing-a-data-tap"></a>Suppression d'un drainage de données  
 Vous pouvez supprimer un drainage de données avant de lancer l’exécution à l’aide de la procédure stockée [catalog.remove_data_tap](../../integration-services/system-stored-procedures/catalog-remove-data-tap.md) . Cette procédure stockée accepte l'ID de drainage de données comme paramètre, que vous pouvez obtenir en tant que résultat de la procédure stockée add_data_tap.  
  
```sql
DECLARE @tap_id bigint  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt' @data_tap_id=@tap_id OUTPUT  
EXEC [SSISDB].[catalog].remove_data_tap @tap_id  
```  
  
### <a name="listing-all-data-taps"></a>Création de la liste de tous les drainages de données  
 Vous pouvez également afficher la liste de tous les drainages de données à l'aide de l'affichage catalog.execution_data_taps. Cet exemple extrait les drainages de données pour une instance d'exécution de spécification (ID : 54).  
  
```sql 
select * from [SSISDB].[catalog].execution_data_taps where execution_id=@execid  
```  
  
### <a name="performance-consideration"></a>Observation relative aux performances  
 Le fait d'activer le niveau de journalisation détaillée et d'ajouter des drainages de données augmente les opérations d'E/S effectuées par votre solution d'intégration de données. Par conséquent, il est recommandé d'ajouter des drainages de données uniquement à des fins de dépannage.  
  
### <a name="video"></a>Vidéo  
 Cette [vidéo sur TechNet](../../sql-server/index.yml) montre comment ajouter/utiliser des drainages de données dans le catalogue SQL Server 2012 SSISDB qui permettent de déboguer des packages par programmation et de capturer les résultats partiels au moment de l’exécution. Elle explique également comment répertorier/supprimer ces drainages de données et les meilleures pratiques pour l'utilisation des drainages de données dans des packages SSIS.  
 
## <a name="see-also"></a>Voir aussi  
 [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md)  
  
