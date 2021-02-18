---
title: Enregistrer des traces et de modèles de trace
titleSuffix: SQL Server Profiler
description: Découvrez comment enregistrer des données d’événement capturées dans un fichier de trace ou une table de base de données dans SQL Server Profiler et comment enregistrer des modèles de trace définis par l’utilisateur.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 957e6bf8-e7a3-4a59-a1cd-0a41538a8158
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: c75f865ed31e43f2fbdec96cfc65fcbd9619c1d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345770"
---
# <a name="save-traces-and-trace-templates"></a>Enregistrer des traces et de modèles de trace

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Il est important de faire la distinction entre l'enregistrement de fichiers de trace et l'enregistrement de modèles de trace. L'enregistrement d'un fichier de trace suppose l'enregistrement des données d'événement capturées à un emplacement précis. L'enregistrement d'un modèle de trace suppose l'enregistrement de la définition de la trace, par exemple les colonnes de données, les classes d'événements ou les filtres spécifiés.  
  
## <a name="saving-traces"></a>enregistrement des traces  
 Enregistrez les données d'événement capturées dans un fichier ou une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque vous devez analyser ou relire ultérieurement les données capturées. Un fichier de trace permet d'effectuer les opérations suivantes :  
  
-   utilisez un fichier de trace ou une table de trace pour créer une charge de travail employée comme entrée pour l'Assistant Paramétrage du moteur de base de données ;  
  
-   utilisez un fichier de trace pour capturer des événements et envoyer le fichier de trace au fournisseur de prise en charge pour analyse ;  
  
-   utilisez les outils de traitement de requête dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour accéder aux données ou les afficher dans le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Cependant, seuls les membres du rôle de serveur fixe **sysadmin** ou le créateur de la table peuvent accéder directement à la table de trace.  
  
> [!NOTE]  
>  La capture des données de trace dans une table est une opération plus lente que la capture dans un fichier. Une autre solution consiste à capturer des données de trace dans un fichier, à ouvrir le fichier de trace, puis à enregistrer le fichier de trace en tant que table de trace.  
  
 Lors de l’utilisation d’un fichier de trace, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] enregistre les données d’événement capturées (et non les définitions de trace) dans un fichier de trace [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] (\*.trc). L'extension est automatiquement ajoutée à la fin du fichier de trace au moment de l'enregistrement, quelle que soit l'extension spécifiée par ailleurs. Par exemple, si vous spécifiez un fichier de trace **Trace.dat**, le fichier créé est nommé **Trace.dat.trc**.  
  
> [!IMPORTANT]  
>  Les utilisateurs qui disposent des autorisations SHOWPLAN, ALTER TRACE ou VIEW SERVER STATE peuvent afficher les requêtes capturées dans une sortie Showplan. Ces requêtes peuvent contenir des informations critiques telles que les mots de passe. C’est pourquoi, il est recommandé de n’accorder ces autorisations qu’aux utilisateurs qui sont autorisés à afficher les informations critiques, tels que les membres du rôle de base de données fixe **db_owner** ou les membres du rôle serveur fixe **sysadmin** . Il est également recommandé d'enregistrer les fichiers Showplan ou de trace qui contiennent des événements Showplan uniquement sur un emplacement qui utilise le système de fichiers NTFS et que vous limitiez l'accès aux utilisateurs qui sont autorisés à afficher les informations critiques.  
  
## <a name="saving-templates"></a>Enregistrement de modèles  
 La définition de modèle d'une trace inclut les classes d'événements, les colonnes de données, les filtres et toutes les autres propriétés (à l'exception des données d'événement capturées) utilisées pour créer une trace. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] fournit des modèles système prédéfinis pour les tâches de suivi courantes et pour certaines tâches spécifiques, telles que la création d’une charge de travail que l’Assistant Paramétrage du moteur de base de données peut utiliser pour affiner la conception physique de la base de données. Vous pouvez également créer et enregistrer des modèles définis par l'utilisateur.  
  
### <a name="importing-and-exporting-templates"></a>Importation et exportation de modèles  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] vous permet d’importer et d’exporter des modèles d’un serveur à un autre. L'exportation d'un modèle transfère une copie d'un modèle existant vers un répertoire que vous spécifiez. L'importation d'un modèle crée une copie d'un modèle que vous spécifiez. Lorsque ces modèles sont affichés dans le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], vous pouvez les distinguer des modèles système grâce au terme « (utilisateur) » ajouté à la suite du nom du modèle. Vous ne pouvez ni remplacer, ni modifier directement un modèle système prédéfini.  
  
### <a name="analyzing-performance-with-templates"></a>Analyse des performances avec des modèles  
 Si vous analysez fréquemment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez des modèles pour analyser les performances. Les modèles capturent les mêmes données d'événement chaque fois et utilisent la même définition de trace pour analyser les mêmes événements. Vous n'avez pas besoin de définir les classes d'événements et les colonnes de données chaque fois que vous créez une trace. En outre, un modèle peut être donné à un autre utilisateur pour analyser des événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifiques. Par exemple, un fournisseur de support technique peut fournir un modèle à un client. Le client utilise le modèle pour capturer les données d'événement requises qui sont ensuite envoyées au fournisseur de support technique pour analyse.  
  
 **Pour enregistrer une trace dans un fichier**  
  
 [Enregistrer des résultats d’une trace dans un fichier &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)  
  
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Enregistrer des résultats d’une trace dans une table &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)   
 [Créer un modèle de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)   
 [Dériver un modèle à partir d’une trace en cours d’exécution &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)   
 [Dériver un modèle à partir d’un fichier de trace ou d’une table de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)   
 [Exporter un modèle de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)   
 [Importer un modèle de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)  
  
  
