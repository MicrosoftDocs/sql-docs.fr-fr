---
title: dta (utilitaire)
description: L’utilitaire dta est une version d’invite de commandes de l’Assistant Paramétrage du moteur de base de données qui vous permet d’utiliser la fonctionnalité dans les applications et les scripts.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- physical design structures [SQL Server]
- command prompt utilities [SQL Server], dta
- dta utility
- tuning databases [SQL Server], Database Engine Tuning Advisor
- workloads [SQL Server], analyzing
- dta utility, about dta utility
- performance [SQL Server], Database Engine Tuning Advisor
- Database Engine Tuning Advisor [SQL Server], command prompt
- optimizing databases [SQL Server]
ms.assetid: a0b210ce-9b58-4709-80cb-9363b68a1f5a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/09/2017
ms.openlocfilehash: b05279df08153efab4257a74a8389ea33b38b966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338746"
---
# <a name="dta-utility"></a>dta (utilitaire)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'utilitaire **dta** constitue la version d'invite de commandes de l'Assistant Paramétrage du moteur de base de données. L'utilitaire **dta** est conçu pour permettre l'utilisation de l'Assistant Paramétrage du moteur de base de données dans des applications et des scripts.  

À l'instar de l'Assistant Paramétrage du moteur de base de données, l'utilitaire **dta** analyse une charge de travail et recommande des PDS (Physical Design Structures) pour améliorer les performances du serveur pour cette charge de travail. La charge de travail peut être un cache du plan, un fichier de trace ou une table du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , ou un script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les PDS incluent des index, des vues indexées et un partitionnement. Après l'analyse d'une charge de travail, l'utilitaire **dta** produit une recommandation pour la conception physique des bases de données et peut générer le script nécessaire pour la mise en œuvre de la recommandation. Des charges de travail peuvent être spécifiées à partir de l’invite de commandes avec l’argument **-if** ou **-it** . Vous pouvez aussi spécifier un fichier d’entrée XML à partir de l’invite de commandes avec l’argument **-ix** . Dans ce cas, la charge de travail est spécifiée dans le fichier d'entrée XML.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
dta  
[ -? ] |  
[  
      [ -S server_name[ \instance ] ]  
      { { -U login_id [-P password ] } | -E  }  
      { -D database_name [ ,...n ] }  
      [ -d database_name ]   
      [ -Tl table_list | -Tf table_list_file ]  
      { -if workload_file | -it workload_trace_table_name  |   
        -ip | -iq }  
      { -ssession_name | -IDsession_ID }  
      [ -F ]  
      [ -of output_script_file_name ]  
      [ -or output_xml_report_file_name ]  
      [ -ox output_XML_file_name ]  
      [ -rl analysis_report_list [ ,...n ] ]  
      [ -ix input_XML_file_name ]  
      [ -A time_for_tuning_in_minutes ]  
      [ -n number_of_events ]
      [ -I time_window_in_hours ]  
      [ -m minimum_improvement ]  
      [ -fa physical_design_structures_to_add ]  
      [ -fi filtered_indexes]  
      [ -fc columnstore_indexes]  
      [ -fp partitioning_strategy ]  
      [ -fk keep_existing_option ]  
      [ -fx drop_only_mode ]  
      [ -B storage_size ]  
      [ -c max_key_columns_in_index ]  
      [ -C max_columns_in_index ]  
      [ -e | -e tuning_log_name ]  
      [ -N online_option]  
      [ -q ]  
      [ -u ]  
      [ -x ]  
      [ -a ]  
]  
```  
  
## <a name="arguments"></a>Arguments  
 **-?**  
 Affiche des informations sur l’utilisation.  
  
 **-A** _time_for_tuning_in_minutes_  
 Spécifie la limite de durée de réglage en minutes. **dta** utilise la limite de temps spécifiée pour régler la charge de travail et générer un script avec les modifications de conception physique recommandées. Par défaut, **dta** utilise une durée de réglage de 8 heures. Si la valeur 0 est spécifiée, la durée de réglage est illimitée. **dta** pourra éventuellement achever le réglage de toute la charge de travail avant l'expiration de la limite de durée. Cependant, pour vous assurer que toute la charge de travail est réglée, nous vous recommandons de spécifier une durée de réglage illimitée (-A 0).  
  
 **-a**  
 Règle la charge de travail et applique la recommandation sans vous demander de confirmation.  
  
 **-B** _storage_size_  
 Spécifie l'espace maximal (en mégaoctets) pouvant être consommé par l'index et le partitionnement recommandés. Lorsque plusieurs bases de données sont paramétrées, les recommandations pour toutes les bases de données sont prises en compte pour le calcul de l'espace. Par défaut, **dta** utilise la plus petite des tailles de stockage suivantes :  
  
-   Le triple de la taille actuelle des données brutes, qui inclut la taille totale des segments et les index cluster des tables de la base de données.  
  
-   L'espace libre disponible sur tous les lecteurs de disques connectés, plus la taille des données brutes.  
  
 La taille de stockage par défaut ne comprend pas les index non-cluster et les vues indexées.  
  
 **-C** _max_columns_in_index_  
 Spécifie le nombre maximal de colonnes dans les index proposés par **dta** . La valeur maximale est 1024. Par défaut, cet argument a la valeur 16.  
  
 **-c** _max_key_columns_in_index_  
 Spécifie le nombre maximal de colonnes clés dans les index proposés par **dta** . La valeur par défaut est 16 (valeur maximale autorisée). **dta** peut également créer des index avec des colonnes incluses. Les index recommandés avec des colonnes incluses peuvent dépasser le nombre de colonnes spécifiées dans cet argument.  
  
 **-D** _database_name_  
 Spécifie le nom de chaque base de données à paramétrer. La première base de données est la base de données par défaut. Vous pouvez spécifier plusieurs bases de données en séparant les noms de bases de données avec des virgules, par exemple :  
  
```  
dta -D database_name1, database_name2...  
```  
  
 Vous pouvez également spécifier plusieurs bases de données en utilisant l'argument **-D** pour chaque nom de base de données, par exemple :  
  
```  
dta -D database_name1 -D database_name2... n  
```  
  
 L’argument **-D** est obligatoire. Si l’argument **-d** n’a pas été spécifié, **dta** se connecte initialement à la base de données qui a été spécifiée avec la première clause `USE database_name` dans la charge de travail. Si la charge de travail ne comporte pas de clause `USE database_name` explicite, vous devez utiliser l’argument **-d** .  
  
 Par exemple, si vous avez une charge de travail qui ne contient pas de clause `USE database_name` explicite et si vous utilisez la commande **dta** suivante, aucune recommandation n'est générée :  
  
```  
dta -D db_name1, db_name2...  
```  
  
 Mais si vous utilisez la même charge de travail et employez la commande **dta** suivante qui utilise l’argument **-d** , une recommandation est générée :  
  
```  
dta -D db_name1, db_name2 -d db_name1  
```  
  
 **-d** _database_name_  
 Spécifie la première base de données à laquelle **dta** se connecte lors du paramétrage d’une charge de travail. Une seule base de données peut être spécifiée pour cet argument. Par exemple :  
  
```  
dta -d AdventureWorks2012 ...  
```  
  
 Si plusieurs noms de bases de données sont spécifiés, **dta** retourne une erreur. L’argument **-d** est facultatif.  
  
 Si vous utilisez un fichier d'entrée XML, vous pouvez spécifier la première base de données à laquelle **dta** se connecte en utilisant l'élément **DatabaseToConnect** qui se trouve sous l'élément **TuningOptions** . Pour plus d'informations, consultez [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md).  
  
 Si vous ne réglez qu’une seule base de données, l’argument **-d** fournit une fonctionnalité similaire à l’argument **-d** de l’utilitaire **sqlcmd** , mais il n’exécute pas l’instruction USE *database_name* . Pour plus d'informations, consultez [sqlcmd Utility](../../tools/sqlcmd-utility.md).  
  
 **-E**  
 Utilise une connexion approuvée au lieu de demander un mot de passe. L’argument **-E** ou l’argument **-U** , qui spécifie un ID de connexion, doit être utilisé.  
  
 **-e** _tuning_log_name_  
 Spécifie le nom de la table ou du fichier où **dta** enregistre les événements qu’il n’a pas pu régler. La table est créée sur le serveur où le réglage est effectué.  
  
 Si une table est employée, spécifiez son nom selon le format : *[database_name].[owner_name].table_name*. Le tableau suivant indique les valeurs par défaut de chaque paramètre :  
  
|Paramètre|Valeur par défaut|Détails|  
|---------------|-------------------|-------------|  
|*database_name*|*database_name* spécifié avec l’option **-D**||  
|*owner_name*|**dbo**|*owner_name* doit avoir la valeur **dbo**. Si une autre valeur est spécifiée, l'exécution de **dta** échoue et il retourne une erreur.|  
|*table_name*|None||  
  
 Si un fichier est utilisé, spécifiez .xml comme extension. Par exemple, TuningLog.xml.  
  
> [!NOTE]  
>  L’utilitaire **dta** ne supprime pas le contenu des tables du journal de paramétrage définies par l’utilisateur si la session est supprimée. Il est recommandé de spécifier une table pour le journal de paramétrage quand vous réglez des charges de travail très importantes. Le paramétrage de charges de travail importantes pouvant engendrer des journaux de paramétrage volumineux, il est plus rapide d'effacer les sessions lorsqu'une table est utilisée.  
  
 **-F**  
 Permet à **dta** de remplacer un fichier de sortie existant. Si un fichier de sortie de même nom existe déjà et si **-F** n’est pas spécifié, **dta** retourne une erreur. Vous pouvez utiliser **-F** avec **-of**, **-or** ou **-ox**.  
  
 **-fa** _physical_design_structures_to_add_  
 Spécifie les types de PDS (Physical Design Structures) que **dta** doit inclure dans la recommandation. Le tableau suivant répertorie et décrit les valeurs pouvant être spécifiées pour cet argument. Si aucune valeur n’est spécifiée, **dta** utilise la valeur par défaut **-fa IDX**.  
  
|Valeur|Description|  
|-----------|-----------------|  
|IDX_IV|Index et vues indexées.|  
|IDX|Index uniquement.|  
|IV|Vues indexées uniquement.|  
|NCL_IDX|Index non-cluster uniquement.|  
  
 **-fi**  
 Spécifie que les index filtrés soient considérés pour de nouvelles recommandations. Pour plus d'informations, consultez [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
**-fc**  
 Spécifie que les index columnstore doivent être considérés pour les nouvelles recommandations. DTA va considérer à la fois les index columnstore cluster et non cluster. Pour plus d'informations, consultez la rubrique    
[Recommandations relatives aux index columnstore dans l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md).
 ||  
|-|  
|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  

  
 **-fk** _keep_existing_option_  
 Spécifie les structures PDS (Physical Design Structures) que **dta** doit conserver lors de la génération de sa recommandation. Le tableau suivant répertorie et décrit les valeurs pouvant être spécifiées pour cet argument :  
  
|Valeur|Description|  
|-----------|-----------------|  
|Aucune|Aucune structure existante|  
|ALL|Toutes les structures existantes|  
|ALIGNED|Toutes les structures alignées sur les partitions.|  
|CL_IDX|Tous les index cluster sur les tables|  
|IDX|Tous les index cluster et non-cluster sur les tables|  
  
 **-fp** _partitioning_strategy_  
 Spécifie si les nouvelles PDS (Physical Design Structures) (index et vues indexées) que **dta** propose doivent être partitionnées et de quelle manière. Le tableau suivant répertorie et décrit les valeurs pouvant être spécifiées pour cet argument :  
  
|Valeur|Description|  
|-----------|-----------------|  
|Aucune|Aucun partitionnement|  
|FULL|Partitionnement complet (choisissez cette option pour améliorer les performances)|  
|ALIGNED|Partitionnement aligné seulement (choisissez cette valeur pour simplifier la gestion)|  
  
 ALIGNED signifie que dans la recommandation générée par **dta** , chaque index proposé est partitionné exactement de la même manière que la table sous-jacente pour laquelle l'index est défini. Les index non-cluster sur une vue indexée sont alignés avec la vue indexée. Une seule valeur peut être spécifiée pour cet argument. La valeur par défaut est **-fp NONE**.  
  
 **-fx** _drop_only_mode_  
 Spécifie que **dta** considère uniquement la suppression des PDS (Physical Design Structures) existantes. Aucune nouvelle PDS n'est considérée. Lorsque cette option est spécifiée, **dta** évalue l'utilité des PDS (Physical Design Structures) existantes et recommande la suppression des structures rarement utilisées. Cet argument ne prend aucune valeur. Il ne peut pas être utilisé avec les arguments **-fa**, **-fp** ou **-fk ALL** .  
  
 **-ID** _session_ID_  
 Spécifie un identificateur numérique pour la session de réglage. Si ce paramètre n'est pas spécifié, **dta** génère un numéro d'identification. Vous pouvez utiliser cet identificateur pour afficher des informations pour des sessions de réglage existantes. Si vous ne spécifiez pas de valeur pour **-ID**, un nom de session doit être spécifié avec **-s**.  
  
 **-ip**  
 Spécifie que le cache du plan est utilisé comme charge de travail. Les 1 000 premiers événements du cache du plan pour les bases de données explicitement sélectionnées sont analysés. Cette valeur peut être modifiée à l'aide de l'option **-n**.  

**-iq**  
 Spécifie que le Magasin des requêtes est utilisé comme charge de travail. Les 1 000 premiers événements du Magasin des requêtes pour les bases de données explicitement sélectionnées sont analysés. Cette valeur peut être modifiée à l'aide de l'option **-n**.  Pour plus d’informations, consultez [Magasin de requêtes](../../relational-databases/performance/how-query-store-collects-data.md) et [Paramétrage de base de données à l’aide des charges de travail du Magasin de requêtes](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).
 ||  
|-|  
|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
     
 **-if** _workload_file_  
 Spécifie le chemin d'accès et le nom du fichier de charge de travail à utiliser comme entrée pour le réglage. Le fichier doit être dans l'un de ces formats : .trc (fichier trace SQL Server Profiler), .sql (fichier SQL) ou .log (fichier de trace [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Un fichier de charge de travail ou une table de charge travail doit être spécifié.  
  
 **-it** _workload_trace_table_name_  
 Spécifie le nom de la table contenant la trace de charge de travail pour le réglage. Le nom est spécifié selon le format : [*database_name*] **.** [*owner_name*] **.** _table_name_.  
  
 Le tableau suivant indique les valeurs par défaut de chaque paramètre :  
  
|Paramètre|Valeur par défaut|  
|---------------|-------------------|  
|*database_name*|*database_name* spécifié avec l’option **-D** .|  
|*owner_name*|**dbo**.|  
|*table_name*|Aucun.|  
  
> [!NOTE]  
>  *owner_name* doit avoir la valeur **dbo**. Si toute autre valeur est spécifiée, l'exécution de **dta** échoue et une erreur est retournée. Notez également qu'une table de charge de travail ou un fichier de charge de travail doit être spécifié.  
  
 **-ix** _input_XML_file_name_  
 Spécifie le nom du fichier XML contenant les informations d’entrée **dta** . Ce doit être un document XML valide, conforme à DTASchema.xsd. Les arguments conflictuels spécifiés à partir de l'invite de commandes pour les options de réglage remplacent la valeur correspondante dans ce fichier XML. Une seule exception : configuration spécifiée par l'utilisateur entrée dans le mode évaluation dans le fichier d'entrée XML. Par exemple, si une configuration est entrée dans l'élément **Configuration** du fichier d'entrée XML et si un élément **EvaluateConfiguration** est également spécifié comme l'une des options de réglage, les options de réglage spécifiées dans le fichier d'entrée XML sont prioritaires par rapport à toute option de réglage entrée à partir de l'invite de commandes.  

 **-k** _maxtotalindexes_  
 Nombre maximal d'index dans la recommandation.  

 **-K** _maxtotalindexes_  
 Nombre maximal d'index par table.

 **-m** _minimum_improvement_  
 Spécifie le pourcentage minimal d'amélioration que la configuration recommandée doit satisfaire.  
  
 **-N** _online_option_  
 Spécifie si des PDS (Physical Design Structures) sont créées en ligne. Le tableau suivant répertorie et décrit les valeurs pouvant être spécifiées pour cet argument :  
  
|Valeur|Description|  
|-----------|-----------------|  
|OFF|Aucune PDS ne peut être créée en ligne.|  
|ACTIVÉ|Toutes les PDS recommandées peuvent être créées en ligne.|  
|MIXED|L'Assistant Paramétrage du moteur de base de données tente de recommander des PDS pouvant être créées en ligne lorsque cela est possible.|  
  
 Si des index sont créés en ligne, ONLINE = ON est ajouté à sa définition d'objet.  
  
 **-n** _number_of_events_  
 Spécifie le nombre d’événements dans la charge de travail que **dta** doit régler. Si cet argument est spécifié et si la charge de travail est un fichier de trace qui contient des informations de durée, **dta** règle les événements en ordre décroissant de durée. Cet argument est utile pour comparer deux configurations de PDS. Pour comparer deux configurations, spécifiez le même nombre d'événements à régler pour les deux configurations, puis spécifiez une durée de réglage illimitée pour les deux de la manière suivante :  
  
```  
dta -n number_of_events -A 0  
```  
  
 Dans ce cas, il est important de spécifier une durée de réglage illimitée (`-A 0`). Sinon, l'Assistant Paramétrage du moteur de base de données utilise par défaut une durée de réglage de 8 heures.
 
 **-I** _time_window_in_hours_   
   Spécifie la fenêtre de temps (en heures) pendant laquelle une requête doit avoir été exécutée pour être prise en compte par DTA pour le paramétrage lors de l’utilisation de l’option **-iq** (charge de travail à partir du Magasin des requêtes). 
```  
dta -iq -I 48  
```  
Dans ce cas, DTA utilise le Magasin des requêtes comme source de la charge de travail et ne tient compte que des requêtes qui ont été exécutées au cours des 48 dernières heures.  
  ||  
|-|  
|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  


  
 **-of** _output_script_file_name_  
 Spécifie que **dta** écrit la recommandation sous forme de script [!INCLUDE[tsql](../../includes/tsql-md.md)] sous le nom de fichier et à la destination spécifiés.  
  
 Vous pouvez utiliser **-F** avec cette option. Vérifiez que le nom de fichier est unique, surtout si vous utilisez aussi **-or** et **-ox**.  
  
 **-or** _output_xml_report_file_name_  
 Spécifie que **dta** écrit la recommandation dans un rapport de sortie XML. Si un nom de fichier est fourni, les recommandations sont écrites dans cette destination. Sinon, **dta** utilise le nom de session pour générer le nom de fichier et l'écrit dans le répertoire en cours.  
  
 Vous pouvez utiliser **-F** avec cette option. Vérifiez que le nom de fichier est unique, surtout si vous utilisez aussi **-of** et **-ox**.  
  
 **-ox** _output_XML_file_name_  
 Spécifie que **dta** écrit la recommandation sous forme de fichier XML sous le nom de fichier et à la destination fournis. Vérifiez que l'Assistant Paramétrage du moteur de base de données possède des autorisations d'écriture dans le répertoire de destination.  
  
 Vous pouvez utiliser **-F** avec cette option. Vérifiez que le nom de fichier est unique, surtout si vous utilisez aussi **-of** et **-or**.  
  
 **-P** _password_  
 Spécifie le mot de passe de l’ID de connexion. Si cette option est omise, **dta** demande d'entrer un mot de passe.  
  
 **-q**  
 Active le mode silencieux. Aucune information n'est écrite sur la console, notamment les informations de progression et d'en-tête.  
  
 **-rl** _analysis_report_list_  
 Spécifie la liste des rapports d'analyse à générer. Le tableau suivant répertorie les valeurs pouvant être spécifiées pour cet argument :  
  
|Valeur|Rapport|  
|-----------|------------|  
|ALL|Tous les rapports d'analyse|  
|STMT_COST|Rapport de coût d'instruction|  
|EVT_FREQ|Rapport de fréquence d'événement|  
|STMT_DET|Rapport détaillé d'instruction|  
|CUR_STMT_IDX|Rapport de relation instruction-index (configuration actuelle)|  
|REC_STMT_IDX|Rapport de relation instruction-index (configuration recommandée)|  
|STMT_COSTRANGE|Rapport de fourchette de coûts d'instruction|  
|CUR_IDX_USAGE|Rapport d'utilisation de l'index (configuration actuelle)|  
|REC_IDX_USAGE|Rapport d'utilisation de l'index (configuration recommandée)|  
|CUR_IDX_DET|Rapport détaillé d'index (configuration actuelle)|  
|REC_IDX_DET|Rapport détaillé d'index (configuration recommandée)|  
|VIW_TAB|Rapport de relation vue-table|  
|WKLD_ANL|Rapport d'analyse de la charge de travail|  
|DB_ACCESS|Rapport d'accès à la base de données|  
|TAB_ACCESS|Rapport d'accès à la table|  
|COL_ACCESS|Rapports d'accès à la colonne|  
  
 Spécifiez plusieurs rapports en séparant les valeurs avec des virgules, par exemple :  
  
```  
... -rl EVT_FREQ, VIW_TAB, WKLD_ANL ...  
```  
  
 **-S** _server_name_[ *\instance*]  
 Spécifie le nom de l'ordinateur et de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auxquels la connexion doit être établie. Si la valeur de *server_name* n’est pas spécifiée, **dta** se connecte à l’instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l’ordinateur local. Cette option est requise lors de la connexion à une instance nommée ou de l'exécution de **dta** à partir d'un ordinateur distant sur le réseau.  
  
 **-s** _session_name_  
 Spécifie le nom de la session de réglage. Ce paramètre est obligatoire si **-ID** n’est pas spécifié.  
  
 **-Tf** _table_list_file_  
 Spécifie le nom du fichier contenant la liste des tables à paramétrer. Chaque table listée à l'intérieur du fichier doit commencer sur une nouvelle ligne. Les noms de table doivent être complets avec une dénomination en trois parties, par exemple, **AdventureWorks2012.HumanResources.Department**. Éventuellement, pour invoquer la fonctionnalité de dimensionnement d'une table, le nom d'une table existante peut être suivi par un nombre indiquant le nombre de lignes prévues pour cette table. L'Assistant Paramétrage du moteur de base de données tient compte du nombre prévu de lignes lors du réglage ou de l'évaluation des instructions dans la charge de travail faisant référence à ces tables. Notez qu’un ou plusieurs espaces peuvent figurer entre le nombre *number_of_rows* et le nom *table_name*.  
  
 Il s’agit du format de fichier pour *table_list_file*:  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 Cet argument peut remplacer l’entrée d’une liste de tables à l’invite de commandes ( **-TI**). N’utilisez pas un fichier de liste de tables ( **-Tf**) si vous employez **-TI**. Si les deux arguments sont employés, **dta** échoue et retourne une erreur.  
  
 Si les arguments **-Tf** et **-Tl** sont omis, toutes les tables utilisateur dans les bases de données spécifiées sont prises en compte pour le réglage.  
  
 **-Tl** _table_list_  
 Spécifie à l'invite de commandes une liste de tables à régler. Placez des virgules entre les noms de table pour les séparer. Si une seule base de données est spécifiée avec l’argument **-D** , les noms de table n’ont pas besoin d’être qualifiés avec un nom de base de données. Sinon, le nom complet au format : *database_name.schema_name.table_name* est obligatoire pour chaque table.  
  
 Cet argument constitue une alternative à l’utilisation d’un fichier de liste de tables ( **-Tf**). Si les deux arguments **-Tl** et **-Tf** sont employés, **dta** échoue et retourne une erreur.  
  
 **-U** _login_id_  
 Spécifie l'ID de connexion utilisé pour une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **-u**  
 Lance l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Tous les paramètres sont traités comme les paramètres initiaux de l'interface utilisateur.  
  
 **-x**  
 Démarre une session de réglage et quitte.  
  
## <a name="remarks"></a>Notes  
 Appuyez sur Ctrl+C pour arrêter la session de réglage et générer des recommandations basées sur l’analyse que **dta** a effectuée jusqu’à ce point. Un message vous demande de déterminer si vous souhaitez générer ou non des recommandations. Appuyez de nouveau sur CTRL+C pour arrêter la session de réglage sans générer de recommandations.  
  
## <a name="examples"></a>Exemples  
 **A. Paramétrer une charge de travail incluant des index et des vues indexées dans sa recommandation**  
  
 Cet exemple utilise une connexion sécurisée (`-E`) pour se connecter à la base de données **tpcd1G** sur MyServer afin d’analyser une charge de travail et créer des recommandations. Il écrit la sortie dans un fichier de script nommé script.sql. Si script.sql existe déjà, **dta** remplace le fichier, car l'argument `-F` a été spécifié. La session de réglage se poursuit pendant une durée illimitée pour garantir l'analyse complète de la charge de travail (`-A 0`). La recommandation doit fournir une amélioration minimale de 5 % (`-m 5`). **dta** doit inclure des index et des vues indexées dans sa recommandation finale (`-fa IDX_IV`).  
  
```  
dta -S MyServer -E -D tpcd1G -if tpcd_22.sql -F -of script.sql -A 0 -m 5 -fa IDX_IV  
```  
  
 **B. Limiter l’utilisation du disque**  
  
 Cet exemple limite la taille de base de données totale, qui inclut les données brutes et les index supplémentaires, à 3 gigaoctets (Go) (`-B 3000`) et dirige la sortie vers d:\result_dir\script1.sql. Il ne s'exécute pas plus d'une heure (`-A 60`).  
  
```  
dta -D tpcd1G -if tpcd_22.sql -B 3000 -of "d:\result_dir\script1.sql" -A 60  
```  
  
 **C. Limiter le nombre de requêtes optimisées**  
  
 Cet exemple limite le nombre de requêtes lues du fichier orders_wkld.sql à un maximum de 10 (`-n 10`) et s'exécute pendant 15 minutes (`-A 15`), selon l'événement se produisant en premier. Pour être certain que toutes les 10 requêtes sont réglées, spécifiez une durée de réglage illimitée avec `-A 0`. Si le temps est important, spécifiez une limite de temps appropriée en spécifiant le nombre de minutes disponibles pour le réglage avec l'argument `-A` comme l'illustre cet exemple.  
  
```  
dta -D orders -if orders_wkld.sql -of script.sql -A 15 -n 10  
```  
  
 **D. Optimiser des tables spécifiques répertoriées dans un fichier**  

 Cet exemple illustre l’utilisation de *table_list_file* (l’argument **-Tf** ). Le contenu du fichier table_list.txt est le suivant :  

```
AdventureWorks2012.Sales.Customer  100000  
AdventureWorks2012.Sales.Store  
AdventureWorks2012.Production.Product  2000000  
```

Le contenu de table_list.txt spécifie que :  

- Seules les tables **Customer**, **Store** et **Product** dans la base de données doivent être réglées.  
  
- Le nombre de lignes dans les tables **Customer** et **Product** sont par défaut de 100 000 et de 2 000 000, respectivement.  
  
- Le nombre de lignes dans **Store** doit être évalué au nombre actuel de lignes dans la table.  

    Notez qu’un ou plusieurs espaces peuvent figurer entre le nombre de lignes et le nom de la table précédente dans *table_list_file*.  
    
    La durée de réglage est de 2 heures (`-A 120`) et la sortie est écrite dans un fichier XML (`-ox XMLTune.xml`).  

``` 
dta -D pubs -if pubs_wkld.sql -ox XMLTune.xml -A 120 -Tf table_list.txt  
``` 

## <a name="see-also"></a>Voir aussi

- [Référence de l’utilitaire d’invite de commandes &#40;moteur de base de données&#41;](../../tools/command-prompt-utility-reference-database-engine.md)
- [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)