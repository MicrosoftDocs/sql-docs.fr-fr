---
title: Afficher et analyser des traces
titleSuffix: SQL Server Profiler
description: Découvrez comment utiliser SQL Server Profiler pour afficher les données de trace, rechercher des événements spécifiques, afficher des noms d’objets et résoudre des problèmes.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 17e821ca-a12e-4192-acc1-96765d9ae266
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: b95380a4a99e525cc97588b2051d1812c6e2d34a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338643"
---
# <a name="view-and-analyze-traces-with-sql-server-profiler"></a>Afficher et analyser des traces avec SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Utilisez [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour afficher les données d'événement capturées dans une trace. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche les données selon les propriétés de trace définies. L’une des façons d'analyser les données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consiste à les copier dans un autre programme, par exemple [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage peut utiliser un fichier de trace qui contient des événements de traitement SQL et d’appels de procédures distantes si la colonne de données **Text** est présente dans la trace. Pour vous assurer que les colonnes et les événements nécessaires sont bien présents pour être utilisés avec l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , utilisez le modèle de paramétrage prédéfini fourni avec [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
 Si vous ouvrez une trace en utilisant le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], il n'est pas nécessaire que le fichier de trace porte l'extension .trc si le fichier a été créé par le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ou des procédures stockées du système de trace SQL.  
  
> [!NOTE]  
>  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] peut également lire les fichiers .log de trace SQL et les fichiers de script SQL génériques. Si vous ouvrez un fichier de trace SQL qui ne porte pas l’extension .log, par exemple le fichier trace.txt, spécifiez **SQLTrace_Log** comme format de fichier.  
  
 Vous pouvez configurer le format d'horodatage du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour vous aider dans l'analyse des traces.  
  
## <a name="troubleshooting-data"></a>Dépannage de problèmes liés aux données  
 Le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]vous permet de résoudre des problèmes de données en regroupant les traces ou les fichiers de traces dans quatre colonnes : **Duration**, **CPU**, **Reads** et **Writes** . Le genre de données que vous pourriez être amené à réparer pourrait être une requête qui fonctionne mal ou qui a un nombre exceptionnellement élevé de lectures logiques.  
  
 Vous pouvez trouver des informations complémentaires en enregistrant les traces dans des tables et en utilisant [!INCLUDE[tsql](../../includes/tsql-md.md)] pour interroger les données d'événement. Par exemple, pour déterminer quels événements **SQL:BatchCompleted** ont présenté des délais d'attente excessifs, exécutez :  
  
```  
SELECT  TextData, Duration, CPU  
FROM    trace_table_name  
WHERE   EventClass = 12 -- SQL:BatchCompleted events  
AND     CPU < (Duration * 1000)  
```  
  
> [!NOTE]  
>  Le serveur signale la durée d’un événement en microsecondes (10^-6 secondes) et le temps UC utilisé par l’événement en millisecondes (10^-3 secondes). L'interface utilisateur graphique de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche par défaut la colonne **Durée** en millisecondes. Cependant, quand la trace est enregistrée dans un fichier ou une table de base de données, la valeur de la colonne **Durée** est mentionnée en microsecondes.  
  
## <a name="displaying-object-names-when-viewing-traces"></a>Affichage de noms d'objets durant l'affichage des traces  
 Pour afficher le nom d’un objet au lieu de son identificateur (**Object ID**), vous devez capturer les colonnes **Server Name** et **Database ID** en plus de la colonne **Object Name** .  
  
 Si vous choisissez de regrouper les traces par **Object ID** , assurez-vous de les regrouper d’abord par **Server Name** et par **Database ID** , puis par **Object ID** . De même, si vous choisissez de regrouper les traces par **Index ID** , assurez-vous de les regrouper d’abord par **Server Name**, **Database ID** et **Object ID** , puis par **Index ID** . Cet ordre doit être respecté parce que les ID d'objet et d'index ne sont pas les mêmes sur tous les serveurs et dans toutes les bases de données (et un objet n'a pas toujours le même ID d'index).  
  
## <a name="finding-specific-events-within-a-trace"></a>Détection d'événements spécifiques dans une trace  
 Pour rechercher des événements dans une trace et les regrouper, exécutez les étapes suivantes :  
  
1.  Créez votre trace.  
  
    -   Lorsque vous définissez la trace, capturez les colonnes de données **Event Class**, **ClientProcessID** et **Start Time** en plus des autres colonnes de données que vous souhaitez capturer. Pour plus d’informations, consultez [Créer une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md).  
  
    -   Regroupez les données capturées sur la colonne **Event Class**, puis capturez la trace dans un fichier ou une table. Pour regrouper les données capturées, cliquez sur **Organiser les colonnes** dans l'onglet **Sélection des événements** de la boîte de dialogue Propriétés de la trace. Pour plus d’informations, consultez [Organiser les colonnes affichées dans une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/organize-columns-displayed-in-a-trace-sql-server-profiler.md).  
  
    -   Démarrez la trace et arrêtez-la une fois que la durée spécifiée est écoulée ou que les événements nécessaires ont été capturés.  
  
2.  Trouvez les événements cibles.  
  
    -   Ouvrez le fichier ou la table de trace et développez le nœud de la classe d'événements de votre choix, par exemple **Deadlock Chain**. Pour plus d’informations, consultez [Ouvrir un fichier de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) ou l'Assistant Paramétrage du [Ouvrir une table de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md).  
  
    -   Parcourez la trace jusqu’à ce que vous trouviez les événements que vous recherchez (pour vous aider, utilisez l’option **Rechercher** du menu **Modifier** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ). Notez les valeurs contenues dans les colonnes de données **ClientProcessID** et **Start Time** des événements de votre choix.  
  
3.  Affichez les événements en contexte.  
  
    -   Affichez les propriétés de la trace, et regroupez-les par **ClientProcessID** plutôt que par **Event Class** .  
  
    -   Développez les nœuds de chaque ID de processus client que vous souhaitez afficher. Parcourez la trace manuellement ou utilisez l’option **Rechercher** jusqu’à ce que vous trouviez les valeurs **Start Time** précédemment notées pour les événements cibles. Les événements sont affichés dans l'ordre chronologique avec les autres événements relatifs à chaque ID de processus client sélectionné. Par exemple, les événements **Interblocage** et **Interblocage Chain**, capturés dans la trace, se trouveront immédiatement après les événements **SQL:BatchStarting** events within the expeted client process ID.  
  
 La même technique peut être utilisée pour retrouver des événements regroupés. Une fois que vous avez trouvé les événements recherchés, regroupez-les par **ClientProcessID**, **ApplicationName**, ou autre classe d’événements pour afficher les activités qui y sont liées dans l’ordre chronologique.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher une trace enregistrée &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/view-a-saved-trace-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [Afficher des informations de filtre &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/view-filter-information-sql-server-profiler.md)   
 [Afficher des informations de filtre &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/view-filter-information-transact-sql.md)   
 [Ouvrir un fichier de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)   
 [Ouvrir une table de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)  
  
  
