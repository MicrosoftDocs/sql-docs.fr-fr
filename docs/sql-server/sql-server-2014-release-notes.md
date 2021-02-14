---
title: Notes de publication de SQL Server 2014 | Microsoft Docs
description: Ce document Notes de publication décrit les problèmes connus dont vous devez prendre connaissance avant d’installer ou de dépanner les versions de Microsoft SQL Server 2014 (12.x).
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: bf4c4922-80b3-4be3-bf71-228247f97004
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 5881a0c5d3e4a9ce17a2f8ead0656ce549d41064
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081131"
---
# <a name="sql-server-2014-release-notes"></a>Notes de publication de SQL Server 2014
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]
Cet article décrit les problèmes connus avec les versions de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], dont les Service Packs associés.

## <a name="sql-server-2014-service-pack-2-sp2"></a>SQL Server 2014 Service Pack 2 (SP2)

SQL Server 2014 SP2 contient des cumuls de correctifs logiciels publiés pour SQL Server 2014 SP1 CU7. Il contient des améliorations centrées sur les performances, la scalabilité et les diagnostics basés sur les commentaires des clients et de la Communauté SQL.

### <a name="performance-and-scalability-improvements-in-sp2"></a>Améliorations des performances et de la scalabilité dans SP2

|Fonctionnalité|Description|Informations supplémentaires|
|---|---|---|
|Partitionnement du NUMA logiciel automatique|Vous pouvez configurer automatiquement le NUMA logiciel sur les systèmes ayant 8 UC ou plus par nœud NUMA.|[Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md)|
|Buffer Pool Extension|Permet au pool de mémoires tampons SQL Server de monter en charge au-delà 8 To.|[Extension du pool de mémoires tampons](../database-engine/configure-windows/buffer-pool-extension.md)|
|Mise à l’échelle dynamique des objets mémoire| Les objets mémoire sont partitionnés de façon dynamique en fonction du nombre de nœuds et de cœurs. Cette amélioration rend inutile l’indicateur de trace 8048 après SQL 2014 SP2.|[Mise à l’échelle dynamique des objets mémoire](/archive/blogs/sql_server_team/dynamic-memory-object-scaling)|
|Indicateur MAXDOP pour les commandes DBCC CHECK*|Cette amélioration est utile pour exécuter DBCC CHECKDB avec un paramètre MAXDOP autre que la valeur sp_configure.|[Indicateurs (Transact-SQL) - Requête](../t-sql/queries/hints-transact-sql-query.md)|
|Amélioration du verrouillage tournant SOS_RWLock|Supprime la nécessité du verrouillage tournant pour SOS_RWLock et utilise à la place des techniques sans verrou similaires à OLTP en mémoire. |[Nouvelle conception de SOS_RWLock](/archive/blogs/psssql/sql-2016-it-just-runs-faster-sos_rwlock-redesign)|
|Implémentation native spatiale|Amélioration significative des performances des requêtes spatiales.|[Amélioration des performances spatiales dans SQL Server 2012 et 2014](https://support.microsoft.com/help/3107399/spatial-performance-improvements-in-sql-server-2012-and-2014)

### <a name="supportability-and-diagnostics-improvements-in-sp2"></a>Améliorations de la prise en charge et des diagnostics dans SP2

|Fonctionnalité|Description|Informations supplémentaires|
|---|---|---|
|Journalisation du dépassement du délai d’attente d’AlwaysON|Ajout de la nouvelle fonctionnalité de journalisation pour les messages de délai d’expiration du bail afin que l’heure actuelle et les heures de renouvellement attendues soient enregistrées. |[Amélioration des diagnostics de délai d’expiration du bail des groupes de disponibilité AlwaysOn](/archive/blogs/alwaysonpro/improved-alwayson-availability-group-lease-timeout-diagnostics)
|Événements XEvent et compteurs de performances AlwaysON|Nouveaux événements XEvent et compteurs de performances AlwaysON pour améliorer les diagnostics lors de la résolution des problèmes de latence avec AlwaysON. |[KB 3107172](https://support.microsoft.com/help/3107172/improve-tempdb-spill-diagnostics-by-using-extended-events-in-sql-serve) et [KB 3107400](https://support.microsoft.com/help/3107400/improved-tempdb-spill-diagnostics-in-showplan-xml-schema-in-sql-server)
|Nettoyage du suivi des modifications|Une nouvelle procédure stockée sp_flush_CT_internal_table_on_demand nettoie à la demande les tables internes de suivi des modifications.|[KB 3173157](https://support.microsoft.com/help/3173157/adds-a-stored-procedure-for-the-manual-cleanup-of-the-change-tracking)
|Clonage de base de données|Utilisez la nouvelle commande DBCC pour résoudre les problèmes rencontrés avec des bases de données de production existantes en clonant le schéma, les métadonnées et les statistiques de clonage, mais sans inclure les données. Les bases de données clonées ne sont pas destinées à être utilisées dans les environnements de production.|[KB 3177838](https://support.microsoft.com/help/3177838/how-to-use-dbcc-clonedatabase-to-generate-a-schema-and-statistics-only)
|Ajouts de fonctions de gestion dynamique (DMF)|Les nouvelles DMF sys.dm_db_incremental_stats_properties exposent des informations par partition pour les statistiques incrémentielles.|[KB 3170114](https://support.microsoft.com/help/3170114/update-to-add-dmf-sys-dm-db-incremental-stats-properties-in-sql-server)
|DMF pour la récupération de la mémoire tampon d’entrée dans SQL Server|Une nouvelle DMF pour la récupération de la mémoire tampon d’entrée pour une session/requête (sys.dm_exec_input_buffer) est maintenant disponible. Il s’agit d’une fonctionnalité équivalente à DBCC INPUTBUFFER.|[sys.dm_exec_input_buffer](../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md)
|Prise en charge de DROP DDL pour la réplication|Permet à une table incluse sous la forme d’un article dans une publication de réplication transactionnelle d’être supprimée de la base de données et de la publication.|[KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactiona)
|Privilège IFI pour un compte de service SQL|Déterminer si l’initialisation instantanée de fichiers (IFI, Instant File Initialization) est active lors du démarrage du service SQL Server.|[Initialisation des fichiers de base de données](../relational-databases/databases/database-instant-file-initialization.md)
|Allocations de mémoire - Gestion des problèmes|Vous pouvez tirer parti des indicateurs de diagnostic lors de l’exécution de requêtes en limitant les allocations de mémoire pour éviter la contention de mémoire.|[KB 3107401](https://support.microsoft.com/help/3107401/new-query-memory-grant-options-are-available-min-grant-percent-and-max)
|Profilage par opérateur léger de l’exécution des requêtes |Optimise la collecte de statistiques d’exécution des requêtes par opérateur, comme le nombre réel de lignes.|[Developers Choice: Query progress - anytime, anywhere](/archive/blogs/sql_server_team/query-progress-anytime-anywhere)
|Diagnostics de l’exécution des requêtes|Les lignes réelles lues sont maintenant signalées dans les plans d’exécution de requête pour contribuer à améliorer la résolution des problèmes liés aux performances des requêtes.|[KB 3107397](https://support.microsoft.com/help/3107397/improved-diagnostics-for-query-execution-plans-that-involve-residual-p)
|Diagnostics de l’exécution des requêtes pour le dépassement dans tempdb|Les classes Hash Warning et Sort Warnings comprennent maintenant des colonnes supplémentaires pour suivre les statistiques d’E/S physiques, la mémoire utilisée et les lignes affectées. |[Améliorer les diagnostics de dépassement dans tempdb](https://support.microsoft.com/help/3107172/improve-tempdb-spill-diagnostics-by-using-extended-events-in-sql-serve)
|Prise en charge de tempdb |Utilisez un nouveau message de journal des erreurs pour le nombre de fichiers tempdb et les changements de fichiers de données tempdb au démarrage du serveur.|[KB 2963384](https://support.microsoft.com/help/2963384/fix-sql-server-crashes-when-the-log-file-of-tempdb-database-is-full-in)


De plus, notez les correctifs suivants :
- La pile des appels Xevent inclut maintenant des noms de modules et un décalage à la place des adresses absolues.
- Amélioration de la corrélation entre les diagnostics XE et DMV : utilisation de query_hash et de query_plan_hash pour identifier une requête de façon unique. DMV les définit comme des champs varbinary(8), tandis que XEvent les définit comme des champs UINT64. Étant donné que SQL Server n’utilise pas de valeurs bigint non signées, le cast ne fonctionne pas toujours. Cette amélioration introduit de nouvelles colonnes d’action/de filtre XEvent équivalentes à query_hash et à query_plan_hash, excepté quand elles sont définies comme des colonnes INT64. Ce correctif permet une meilleure corrélation des requêtes entre XE et DMV.
- Prise en charge du codage UTF-8 dans l’instruction BULK INSERT et BCP : la prise en charge de l’exportation et de l’importation de données encodées dans le jeu de caractères UTF-8 est maintenant activée dans l’instruction BULK INSERT et BCP.

### <a name="download-pages-and-more-information-for-sp2"></a>Pages de téléchargement et informations supplémentaires pour SP2

- [Télécharger le Service Pack 2 de Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=53168)
- [SQL Server 2014 Service Pack 2 est maintenant disponible](https://www.microsoft.com/download/details.aspx?id=53167)
- [SQL Server Express 2012 SP2](https://www.microsoft.com/download/details.aspx?id=53167)
- [SQL Server 2014 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=53164)
- [SQL Server 2014 SP2 Report Builder](https://www.microsoft.com/download/details.aspx?id=53163)
- [Complément SQL Server 2014 SP2 Reporting Services pour Microsoft SharePoint](https://www.microsoft.com/download/details.aspx?id=53162)
- [SQL Server 2014 SP2 Semantic Language Statistics ](https://www.microsoft.com/download/details.aspx?id=53165)
- [Informations de version de SQL Server 2014 Service Pack 2](https://support.microsoft.com/help/3171021/sql-server-2014-service-pack-2-release-information)

## <a name="sql-server-2014-service-pack-1-sp1"></a>SQL Server 2014 Service Pack 1 (SP1)

SQL Server 2014 SP1 contient des correctifs fournis dans SQL Server 2014 CU 1 jusqu’à la mise à jour cumulative CU 5 (incluse), ainsi que d’un cumul de correctifs déjà fournis dans SQL Server 2012 SP2.

> [!NOTE]
> Si le catalogue SSISDB est activé pour votre instance de SQL Server et qu’une erreur d’installation s’affiche quand vous effectuez une mise à niveau vers SP1, suivez les instructions décrites concernant ce problème dans [Erreur 912 ou 3417 lorsque vous installez SQL Server 2014 SP1](https://support.microsoft.com/topic/kb3018269-error-912-or-3417-when-you-install-sql-server-2014-sp1-build-12-0-4050-0-7f34525c-d517-6399-a31b-e7033db13706).

### <a name="download-pages-and-more-information-for-sp1"></a>Pages de téléchargement et informations supplémentaires pour SP1

- [Télécharger le Service Pack 1 de Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=46694)
- [SQL Server 2014 Service Pack 1 has released - Updated](/archive/blogs/sqlreleaseservices/sql-server-2014-service-pack-1-has-released-updated)
- [Microsoft SQL Server 2014 SP1 Express](https://www.microsoft.com/download/details.aspx?id=42299)
- [Microsoft SQL Server 2014 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=46694)


## <a name="before-you-install-sql-server-2014-rtm"></a>Avant d’installer SQL Server 2014 RTM

### <a name="limitations-and-restrictions-in-sql-server-2014-rtm"></a>Limitations et restrictions dans SQL Server 2014 RTM

1.  La mise à niveau de SQL Server 2014 CTP 1 vers SQL Server 2014 RTM N'est PAS prise en charge.  
2.  L'installation de SQL Server 2014 CTP 1 côte à côte avec SQL Server 2014 RTM N'est PAS prise en charge.  
3.  L'attachement ou la restauration d'une base de données SQL Server 2014 CTP 1 vers SQL Server 2014 RTM N'est PAS prise en charge.  

**Solution de contournement :** Aucun.

#### <a name="upgrading-from-sql-server-2014-ctp-2-to-sql-server-rtm"></a>Mise à niveau de SQL Server 2014 CTP 2 vers SQL Server RTM
La mise à niveau est entièrement prise en charge. Plus précisément, vous pouvez :

1.  Attacher une base de données SQL Server 2014 CTP 2 à une instance de SQL Server 2014 RTM.    
2.  Restaurer une sauvegarde de base de données prise sur SQL Server 2014 CTP 2 sur une instance de SQL Server 2014 RTM.    
3.  Mettre à niveau sur place vers SQL Server 2014 RTM.
4.  Mettre à niveau de façon propagée vers SQL Server 2014 RTM. Vous devez passer en mode de basculement manuel avant d'initialiser la mise à niveau propagée. Pour plus d’informations, reportez-vous à [Mise à niveau et mise à jour des serveurs d’un groupe de disponibilité avec un temps mort et une perte de données minimaux](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).    
5.  Les données collectées par les jeux d'éléments de collecte des performances des transactions installés dans SQL Server 2014 CTP 2 ne peuvent pas être affichées par SQL Server Management Studio dans SQL Server 2014 RTM, et vice versa.
  
#### <a name="downgrading-from-sql-server-2014-rtm-to-sql-server-2014-ctp-2"></a>Rétrogradation de SQL Server 2014 RTM vers SQL Server 2014 CTP 2  
Cette action n’est pas prise en charge.  
  
**Solution de contournement :** Il n’existe pas de solution de contournement pour le passage à une version antérieure. Nous vous recommandons de sauvegarder la base de données avant d’effectuer une mise à niveau vers SQL Server 2014 RTM.  
  
#### <a name="incorrect-version-of-streaminsight-client-on-sql-server-2014-mediaisocab"></a>Version incorrecte du client StreamInsight sur le média/ISO/CAB SQL Server 2014  
La version incorrecte de StreamInsight.msi et StreamInsightClient.msi se trouve dans le chemin d'accès suivant sur le média/ISO/CAB SQL Server (StreamInsight\\\<Architecture\>\\\<Language ID\>).  
  
**Solution de contournement :** Téléchargez et installez la version correcte à partir de la [page de téléchargement de SQL Server 2014 Feature Pack](https://www.microsoft.com/download/details.aspx?id=57474).  
  
### <a name="product-documentation-rtm"></a><a name="ProdDoc"></a>Version finale de la documentation du produit
  
Le contenu du Générateur de rapports et de PowerPivot n’est pas disponible dans toutes les langues. 

**Problème :** Le contenu du Générateur de rapports n’est pas disponible dans les langues suivantes :  
  
-   Grec (el-GR)  
-   Norvégien (Bokmal) (nb-NO)  
-   Finnois (fi Fi)  
-   Danois (da-DK)  
  
Dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], ce contenu était disponible dans un fichier CHM fourni avec le produit et disponible dans ces langues. Les fichiers CHM ne sont plus fournis avec le produit et le contenu du Générateur de rapports est uniquement disponible sur MSDN. MSDN ne prend pas en charge ces langues. Le Générateur de rapports a également été supprimé de TechNet, et n'est donc plus disponible dans les langues prises en charge.  
  
**Solution de contournement :** Aucun.  
  
**Problème :** Le contenu Power Pivot n’est pas disponible dans les langues suivantes :
  
-   Grec (el-GR)  
-   Norvégien (Bokmal) (nb-NO)  
-   Finnois (fi Fi)  
-   Danois (da-DK)  
-   Tchèque (cs-CZ)  
-   Hongrois (hu-HU)  
-   Néerlandais (Pays-Bas) (nl-NL)  
-   Polonais (pl-PL)  
-   Suédois (sv-SE)  
-   Turc (tr-TR)  
-   Portugais (Portugal) (pt-PT)  
  
Dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], ce contenu était disponible sur TechNet et dans ces langues. Ce contenu a été supprimé de TechNet, et n'est plus disponible dans les langues prises en charge.  
  
**Solution de contournement :** Aucun.  
  
### <a name="database-engine-rtm"></a><a name="DBEngine"></a>Moteur de base de données (version finale)
  
#### <a name="changes-made-for-standard-edition-in-sql-server-2014-rtm"></a>Changements apportés à l’édition Standard dans SQL Server 2014 RTM  
L'édition SQL Server 2014 Standard comprend les modifications suivantes :  
  
-   La fonctionnalité d'extension du pool de mémoires tampons autorise l'utilisation d'une taille maximale allant jusqu'à 4 fois la mémoire configurée.    
-   La mémoire maximale est passée de 64 Go à 128 Go.  
 
#### <a name="memory-optimization-advisor-flags-default-constraints-as-incompatible"></a>Le Conseiller d’optimisation de la mémoire signale les contraintes par défaut comme incompatibles  
**Problème :** Le Conseiller d’optimisation de la mémoire dans SQL Server Management Studio signale toutes les contraintes par défaut comme étant incompatibles. Certaines contraintes par défaut ne sont pas prises en charge dans une table mémoire optimisée ; le Conseiller ne fait pas de distinction entre les types de contraintes par défaut prises en charge et non prises en charge. Les contraintes par défaut prises en charge incluent toutes les constantes, expressions et fonctions intégrées prises en charge dans les procédures stockées compilées en mode natif. Pour afficher la liste des fonctions prises en charge dans les procédures stockées compilées en mode natif, consultez [Constructions prises en charge dans les procédures stockées compilées en mode natif](../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md?viewFallbackFrom=sql-server-2014).  
  
**Solution de contournement :** Si vous voulez utiliser le conseiller pour identifier les bloqueurs, ignorez les contraintes par défaut compatibles. Pour utiliser le Conseiller d'optimisation de la mémoire pour migrer des tables ayant des contraintes par défaut compatibles, mais aucun autre bloqueur, suivez ces étapes :  
  
1.  Supprimez les contraintes par défaut de la définition de table.    
2.  Utilisez le Conseiller pour obtenir un script de migration sur la table.    
3.  Rajoutez les contraintes par défaut dans le script de migration.    
4.  Exécutez le script de migration.  
  
#### <a name="informational-message-file-access-denied-incorrectly-reported-as-an-error-in-the-sql-server-2014-error-log"></a>Le message d’information « Accès au fichier refusé » est signalé à tort comme erreur dans le journal des erreurs de SQL Server 2014  
**Problème :** Lors du redémarrage d’un serveur qui a des bases de données contenant des tables à mémoire optimisée, vous pouvez voir le type de message d’erreur suivant dans le journal des erreurs SQL Server 2014 :  
  
```  
[ERROR]Unable to delete file C:\Program Files\Microsoft SQL   
Server\....old.dll. This error may be due to a previous failure to unload   
memory-optimized table DLLs.  
```  
Ce message est en fait fourni à titre d’information. Aucune action n’est requise de la part de l’utilisateur.  
  
**Solution de contournement :** Aucun. Ce message est fourni à titre d'information.  
  
#### <a name="missing-index-details-incorrectly-report-included-columns-for-memory-optimized-table"></a>Les détails sur l’index manquant signalent de façon erronée des colonnes incluses pour la table à mémoire optimisée  
**Problème :** Si SQL Server 2014 détecte un index manquant pour une requête sur une table à mémoire optimisée, il signale un index absent dans SHOWPLAN_XML, ainsi que dans les vues de gestion dynamique de l’index manquant, par exemple sys.dm_db_missing_index_details. Dans certains cas, les détails de l'index absent contiendront des colonnes incluses. Bien que toutes les colonnes soient implicitement incluses avec tous les index des tables mémoire optimisées, il n'est pas possible de spécifier explicitement les colonnes incluses avec des index mémoire optimisés.  
  
**Solution de contournement :** Ne spécifiez pas la clause INCLUDE pour des index de tables à mémoire optimisée.  
  
#### <a name="missing-index-details-omit-missing-indexes-when-a-hash-index-exists-but-is-not-suitable-for-the-query"></a>Les détails sur l’index manquant omettent les index manquants quand un index de hachage existe mais qu’il n’est pas approprié pour la requête  
**Problème :** Si un index HASH sur les colonnes d’une table à mémoire optimisée est référencé dans une requête mais que l’index ne peut pas être utilisé pour la requête, SQL Server 2014 ne signale pas toujours un index manquant dans SHOWPLAN_XML et dans la vue de gestion dynamique sys.dm_db_missing_index_details.  
  
Notamment, si une requête contient des prédicats d'égalité qui impliquent un sous-ensemble de colonnes clés d'index ou si elle contient des prédicats d'inégalité qui impliquent des colonnes clés d'index, l'index HASH ne peut pas être utilisé tel quel, et un autre index est requis pour exécuter la requête efficacement.  
  
**Solution de contournement :** Si vous utilisez des index de hachage, inspectez les requêtes et les plans de requête pour déterminer si les requêtes peuvent tirer parti des opérations de recherche d’index sur un sous-ensemble de la clé d’index ou des opérations de recherche d’index sur les prédicats d’inégalité. Si vous devez effectuer une recherche sur un sous-ensemble de la clé d'index, utilisez un index NONCLUSTERED, ou un index HASH uniquement sur les colonnes qui font l'objet de votre recherche. Si vous devez effectuer une recherche sur un prédicat d'inégalité, utilisez un index NONCLUSTERED au lieu d'un index HASH.  
  
#### <a name="failure-when-using-a-memory-optimized-table-and-memory-optimized-table-variable-in-the-same-query-if-the-database-option-read_committed_snapshot-is-set-to-on"></a>Échec lors de l’utilisation d’une table à mémoire optimisée et d’une variable de table à mémoire optimisée dans la même requête, si l’option de base de données READ_COMMITTED_SNAPSHOT a la valeur ON  
**Problème :** Si l’option de base de données READ_COMMITTED_SNAPSHOT a la valeur ON et que vous accédez à une table à mémoire optimisée et à une variable de table à mémoire optimisée dans la même instruction en dehors du contexte d’une transaction utilisateur, ce message d’erreur peut s’afficher :  
  
```  
Msg 41359  
A query that accesses memory optimized tables using the READ COMMITTED  
isolation level, cannot access disk based tables when the database option  
READ_COMMITTED_SNAPSHOT is set to ON. Provide a supported isolation level  
for the memory optimized table using a table hint, such as WITH (SNAPSHOT).  
```  
  
**Solution de contournement :** Utilisez l’indicateur de table WITH (SNAPSHOT) avec la variable de table ou affectez la valeur ON à l’option de base de données MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT à l’aide de l’instruction suivante :  
  
```  
ALTER DATABASE CURRENT   
SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
```  
  
#### <a name="procedure-and-query-execution-statistics-for-natively-compiled-stored-procedures-record-worker-time-in-multiples-of-1000"></a>Les statistiques d’exécution des procédures et des requêtes pour les procédures stockées compilées en mode natif enregistrent le temps de travail par multiples de 1000  
**Problème :** Après l’activation de la collection de procédures ou de la collection de statistiques d’exécution de requête pour les procédures stockées compilées en mode natif avec sp_xtp_control_proc_exec_stats ou sp_xtp_control_query_exec_stats, vous verrez que le *_worker_time est indiqué par multiples de 1 000 dans les vues de gestion dynamique sys.dm_exec_procedure_stats et sys.dm_exec_query_stats. Les exécutions de requête dont le temps de travail est inférieur à 500 microsecondes seront indiquées avec un worker_time de 0.  
  
**Solution de contournement :** Aucun. Ne comptez pas sur le worker_time indiqué dans les vues de gestion dynamique des statistiques pour les requêtes à exécution courte dans les procédures stockées compilées en mode natif.  
  
#### <a name="error-with-showplan_xml-for-natively-compiled-stored-procedures-that-contain-long-expressions"></a>Erreur avec SHOWPLAN_XML pour les procédures stockées compilées en mode natif contenant des expressions longues  
**Problème :** Si une procédure stockée compilée en mode natif contient une expression longue, l’obtention du SHOWPLAN_XML pour la procédure, à l’aide de l’option T-SQL SHOWPLAN_XML SET ON ou de l’option « Afficher le plan d’exécution estimé » dans Management Studio, peut entraîner l’erreur suivante :  
  
```  
Msg 41322. MAT/PIT export/import encountered a failure for memory  
optimized table or natively compiled stored procedure with object ID  
278292051 in database ID 6. The error code was  
0xc00cee81.  
```  
  
**Solution de contournement :** Il existe deux solutions de contournement :  
  
1.  Ajoutez des parenthèses à l'expression, comme dans l'exemple suivant :  
  
    À la place de :  
  
    ```  
    SELECT @v0 + @v1 + @v2 + ... + @v199  
    ```  
  
    Écriture :  
  
    ```  
    SELECT((@v0 + ... + @v49) + (@v50 + ... + @v99)) + ((@v100 + ... + @v149) + (@v150 + ... + @v199))  
    ```  
  
2.  Créez une seconde procédure avec une expression légèrement simplifiée, pour le plan d'exécution de requêtes. La forme générale du plan doit être identique. Par exemple, à la place de :  
  
    ```  
    SELECT @v0 +@v1 +@v2 +...+@v199  
    ```  
  
    Écriture :  
  
    ```  
    SELECT @v0 +@v1  
    ```  
  
#### <a name="using-a-string-parameter-or-variable-with-datepart-and-related-functions-in-a-natively-compiled-stored-procedure-results-in-an-error"></a>L’utilisation d’un paramètre ou d’une variable avec DATEPART et des fonctions liées dans une procédure stockée compilée en mode natif génère une erreur  
**Problème :** Quand vous utilisez une procédure stockée compilée en mode natif qui utilise un paramètre ou une variable de chaîne avec les fonctions intégrées DATEPART, DAY, MONTH et YEAR, un message d’erreur indique que le datetimeoffset n’est pas pris en charge avec les procédures stockées compilées en mode natif.  
  
**Solution de contournement :** Attribuez le paramètre ou la variable de chaîne à une nouvelle variable de type datetime2, puis utilisez cette variable dans la fonction DATEPART, DAY, MONTH ou YEAR. Par exemple :  
  
```  
DECLARE @d datetime2 = @string  
DATEPART(weekday, @d)  
```  
  
#### <a name="native-compilation-advisor-flags-delete-from-clauses-incorrectly"></a>Le Conseiller de compilation native signale les clauses DELETE FROM de façon incorrecte  
**Problème :** Le Conseiller de compilation native indique les clauses DELETE FROM d’une procédure stockée comme étant incompatibles, ce qui est incorrect.  
  
**Solution de contournement :** Aucun.  
  
#### <a name="register-through-ssms-adds-dac-meta-data-with-mismatched-instance-ids"></a>L’inscription par le biais de SSMS ajoute des métadonnées DAC avec des ID d’instance incompatibles  
**Problème :** Lors de l’inscription ou de la suppression d’un package d’application de la couche Données (.dacpac) par le biais de SQL Server Management Studio, les tables sysdac* ne sont pas mises à jour correctement pour permettre à un utilisateur d’interroger l’historique dacpac pour la base de données.  L’instance_id pour sysdac_history_internal et pour sysdac_instances_internal ne correspondent pas pour permettre une jointure.  
  
**Solution de contournement :** Ce problème est résolu avec la redistribution du Feature Pack de [Data-Tier Application Framework](https://www.microsoft.com/download/details.aspx?id=100297).  Une fois la mise à jour appliquée, toutes les nouvelles entrées d’historique utilisent la valeur répertoriée pour l’instance_id dans la table sysdac_instances_internal.  
  
Si vous avez déjà le problème avec des valeurs d’instance_id non correspondantes, la seule façon de corriger les valeurs qui ne correspondent pas est de se connecter au serveur en tant qu’utilisateur disposant de privilèges pour écrire dans la base de données MSDB et mettre à jour les valeurs d’instance_id pour les faire correspondre.  Si vous obtenez plusieurs événements d’inscription et de désinscription de la même base de données, il peut être nécessaire d’examiner la date/l’heure pour voir quels enregistrements correspondent à la valeur instance_id actuelle.  
  
1.  Connectez-vous au serveur dans SQL Server Management Studio avec une connexion qui possède des autorisations de mise à jour sur MSDB.    
2.  Ouvrez une nouvelle requête utilisant la base de données MSDB.    
3.  Exécutez cette requête pour afficher toutes vos instances DAC actives.  Recherchez l’instance que vous voulez corriger et notez l’instance_id :  
  
    `select * from` sysdac_instances_internal  
  
4.  Exécutez cette requête pour voir toutes les entrées d'historique :  
  
    `select * from` sysdac_history_internal  
  
5.  Identifiez les lignes qui doivent correspondre à l’instance que vous résolvez. 
6.  Mettez à jour la valeur sysdac_history_internal.instance_id avec la valeur que vous avez notée à l'étape 3 (dans la table sysdac_instances_internal) :  
  
    `update` sysdac_history_internal `set` instance_id = '\<value from step 3\>' `where` \<expression that matches the rows you want to update\>  
  
### <a name="reporting-services-rtm"></a><a name="SSRS"></a>Reporting Services (RTM)
  
#### <a name="the-sql-server-2012-reporting-services-native-mode-report-server-cannot-run-side-by-side-with-sql-server-2014-reporting-services-sharepoint-components"></a>Le serveur de rapports SQL Server 2012 Reporting Services en mode natif ne peut pas fonctionner côte à côte avec les composants SQL Server 2014 Reporting Services SharePoint  
**Problème :** Le service Windows en mode natif [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] « SQL Server Reporting Services » (ReportingServicesService.exe) ne démarre pas quand des composants [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint sont installés sur le même serveur.  
  
**Solution de contournement :** Désinstallez les composants [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint et redémarrez le service Windows Microsoft SQL Server 2012 Reporting Services.  
  
**Informations supplémentaires :**  
  
[!INCLUDE[ssSQL11](../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] Le mode natif ne peut pas fonctionner côte à côte dans l’une ou l’autre des conditions suivantes :  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] Complément pour les produits SharePoint    
-   Service partagé [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint  
  
L’installation côte à côte empêche le service Windows [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] en mode natif de démarrer. Des messages d’erreur, semblables à ceux mentionnés ici, s’affichent dans le journal des événements Windows :  
  
```  
Log Name:   Application  
Source:          Report Server (<SQL instance ID>)  
Event ID:        117  
Task Category:   Startup/Shutdown  
Level:           Error  
Keywords:        Classic  
Description:     The report server database is an invalid version.  
  
Log Name:      Application  
Source:        Report Server (<SQL instance ID>)  
Event ID:      107  
Task Category: Management  
Level:         Error  
Keywords:      Classic  
Description:   Report Server (DENALI) cannot connect to the report server database.  
```  
  
Pour plus d'informations, consultez [Conseils, astuces et dépannage pour SQL Server 2014 Reporting Services](https://go.microsoft.com/fwlink/?LinkID=391254).  
  
#### <a name="required-upgrade-order-for-multi-node-sharepoint-farm-to-sql-server-2014-reporting-services"></a>Ordre de mise à niveau nécessaire pour la batterie de serveurs SharePoint à plusieurs nœuds sur SQL Server 2014 Reporting Services  
**Problème :** La génération de rapport dans une batterie de serveurs à plusieurs nœuds échoue si les instances du service partagé [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint sont mises à niveau avant toutes les instances du complément [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] pour les produits SharePoint.  
  
**Solution de contournement :** Dans une batterie de serveurs SharePoint à plusieurs nœuds :  
  
1.  Mettez d'abord à niveau toutes les instances du complément [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] pour les produits SharePoint.    
2.  Mettez ensuite à niveau toutes les instances du service partagé [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint.  
  
Pour plus d'informations, consultez [Conseils, astuces et dépannage pour SQL Server 2014 Reporting Services](https://go.microsoft.com/fwlink/?LinkID=391254).  
  
### <a name="sql-server-2014-rtm-on-azure-virtual-machines"></a><a name="AzureVM"></a>SQL Server 2014 RTM sur des machines virtuelles Azure  
  
#### <a name="the-add-azure-replica-wizard-returns-an-error-when-configuring-an-availability-group-listener-in-azure"></a>L’Assistant Ajouter un réplica Azure retourne une erreur lors de la configuration d’un écouteur de groupe de disponibilité dans Azure  
**Problème :** Si un groupe de disponibilité a un écouteur, l’Assistant Ajouter un réplica Azure retourne une erreur quand vous tentez de configurer l’écouteur dans Azure.  
  
Ce problème est dû au fait que les écouteurs de groupe de disponibilité exigent l’affectation d’une adresse IP dans chaque sous-réseau qui héberge des réplicas de groupe de disponibilité, dont le sous-réseau Azure.  
  
**Solution de contournement :**  
  
1.  Dans la page de l'écouteur, attribuez une adresse IP statique libre dans le sous-réseau Azure qui hébergera le réplica de groupe de disponibilité à l'écouteur du groupe de disponibilité.  
  
    Cette solution de contournement permet à l’Assistant d’effectuer l’ajout du réplica dans Azure.  
  
2.  Une fois l’Assistant terminé, vous devez achever la configuration de l’écouteur dans Azure comme expliqué dans la rubrique [Configuration de l’écouteur pour les groupes de disponibilité AlwaysOn dans Azure](/previous-versions/azure/dn376546(v=azure.100))  
  
### <a name="analysis-services-rtm"></a><a name="SSAS"></a>Analysis Services (RTM)
  
#### <a name="msolap5-must-be-downloaded-installed-and-registered-for-a-sharepoint-2010-new-farm-configured-with-sql-server-2014"></a>MSOLAP.5 doit être téléchargé, installé et inscrit pour une nouvelle batterie de serveurs SharePoint 2010 configurée avec SQL Server 2014  
**Problème :**  
  
-   Pour SharePoint 2010, MSOLAP.5 doit être téléchargé, installé et inscrit pour une nouvelle batterie de serveurs SharePoint 2013 configurée avec un déploiement SQL Server 2014 RTM. Les classeurs PowerPivot ne peuvent pas se connecter aux modèles de données car le fournisseur référencé dans la chaîne de connexion n’est pas installé.  
  
**Solution de contournement :**  
  
1.  Téléchargez le fournisseur MSOLAP.5 à partir du Feature Pack [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] . Installez le fournisseur sur les serveurs d'applications exécutant Excel Services. Pour plus d’informations, consultez la section « Fournisseur Microsoft Analysis Services OLE DB pour Microsoft SQL Server 2012 SP1 » [Feature Pack de Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Inscrivez MSOLAP.5 en tant que fournisseur approuvé dans SharePoint Excel Services. Pour plus d'informations, consultez [Ajouter MSOLAP.5 en tant que fournisseur de données approuvé dans Excel Services](/analysis-services/power-pivot-for-sharepoint-ssas?viewFallbackFrom=sql-server-ver15).  
  
**Informations supplémentaires :**  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] contient MSOLAP.6. [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../includes/sssql14-md.md)][!INCLUDE[ssGemini](../includes/ssgemini-md.md)] utilisent MSOLAP.5. Si MSOLAP.5 n'est pas installé sur l'ordinateur exécutant Excel Services, Excel Services ne peut pas charger les modèles de données.  
  
#### <a name="msolap5-must-be-downloaded-installed-and-registered-for-a-sharepoint-2013-new-farm-configured-with-sql-server-2014"></a>MSOLAP.5 doit être téléchargé, installé et inscrit pour une nouvelle batterie de serveurs SharePoint 2013 configurée avec SQL Server 2014  
**Problème :**  
  
-   Pour une batterie de serveurs SharePoint 2013 configurée avec un déploiement [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] , les classeurs Excel référençant le fournisseur MSOLAP.5 ne peuvent pas se connecter aux modèles de données tabulaires, car le fournisseur référencé dans la chaîne de connexion n'est pas installé.  
  
**Solution de contournement :**  
  
1.  Téléchargez le fournisseur MSOLAP.5 à partir du Feature Pack [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] . Installez le fournisseur sur les serveurs d'applications exécutant Excel Services. Pour plus d’informations, consultez la section « Fournisseur Microsoft Analysis Services OLE DB pour Microsoft SQL Server 2012 SP1 » [Feature Pack de Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Inscrivez MSOLAP.5 en tant que fournisseur approuvé dans SharePoint Excel Services. Pour plus d'informations, consultez [Ajouter MSOLAP.5 en tant que fournisseur de données approuvé dans Excel Services](/analysis-services/power-pivot-for-sharepoint-ssas?viewFallbackFrom=sql-server-ver15).  
  
**Informations supplémentaires :**  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] contient MSOLAP.6. mais les classeurs PowerPivot SQL Server 2014 utilisent MSOLAP.5. Si MSOLAP.5 n'est pas installé sur l'ordinateur exécutant Excel Services, Excel Services ne peut pas charger les modèles de données.  
  
#### <a name="corrupt-data-refresh-schedules-rtm"></a>Planification d’actualisation des données corrompue (version finale)
**Problème :**  
  
-   Vous mettez jour une planification de l'actualisation et la planification est endommagée et inutilisable.  
  
**Solution de contournement :**  
  
1.  Dans Microsoft Excel, supprimez les propriétés avancées personnalisées. Consultez la section « Solution de contournement » de l’article [2927748 Ko](https://support.microsoft.com/kb/2927748) de la Base de connaissances.  
  
**Informations supplémentaires :**  
  
-    Si la longueur sérialisée de la planification d’actualisation est inférieure à la planification d’origine, quand vous mettez à jour une planification d’actualisation des données d’un classeur, la taille du tampon n’est pas correctement mise à jour et les nouvelles informations de planification sont fusionnées avec les anciennes informations de planification, ce qui endommage la planification.  
  
### <a name="data-quality-services-rtm"></a><a name="DQS"></a>Data Quality Services (RTM)
  
#### <a name="no-cross-version-support-for-data-quality-services-in-master-data-services"></a>Pas de prise en charge des inter-versions pour Data Quality Services dans Master Data Services  
**Problème :** Les scénarios suivants ne sont pas pris en charge :  
  
-   Master Data Services 2014 hébergé dans une base de données du moteur de base de données SQL Server dans SQL Server 2012 avec Data Quality Services 2012.  
  
-   Master Data Services 2012 hébergé dans une base de données du moteur de base de données SQL Server dans SQL Server 2014 avec Data Quality Services 2014 installé.  
  
**Solution de contournement :** Utilisez la même version de Master Data Services que la base de données du moteur de base de données et Data Quality Services.  
  
### <a name="upgrade-advisor-issues-rtm"></a><a name="UA"></a>Problèmes relatifs au Conseiller de mise à niveau (version finale)
  
#### <a name="sql-server-2014-upgrade-advisor-reports-irrelevant-upgrade-issues-for-sql-server-reporting-services"></a>Le Conseiller de mise à niveau SQL Server 2014 signale des problèmes de mise à niveau non pertinents pour SQL Server Reporting Services  
**Problème :** le Conseiller de mise à niveau de SQL Server (SSUA) fourni avec le média SQL Server 2014 signale incorrectement plusieurs erreurs lors de l’analyse du serveur SQL Server Reporting Services.  
  
**Solution de contournement :** Ce problème est résolu dans le Conseiller de mise à niveau fourni dans [SQL Server 2014 Feature Pack pour SSUA](https://www.microsoft.com/download/details.aspx?id=57474).  
  
#### <a name="sql-server-2014-upgrade-advisor-reports-an-error-when-analyzing-sql-server-integration-services-server"></a>Le Conseiller de mise à niveau SQL Server 2014 signale une erreur lors de l’analyse du serveur SQL Server Integration Services  
**Problème :** Le Conseiller de mise à niveau SQL Server (SSUA) fourni avec le support de SQL Server 2014 signale une erreur lors de l’analyse du serveur SQL Server Integration Services.  L’erreur affichée à l’utilisateur est la suivante :  
  
```  
The installed version of Integration Services does not support Upgrade Advisor.   
The assembly information is "Microsoft.SqlServer.ManagedDTS, Version=11.0.0.0,   
Culture=neutral, PublicKeyToken=89845dcd8080cc91  
```  
  
**Solution de contournement :** Ce problème est résolu dans le Conseiller de mise à niveau fourni dans [SQL Server 2014 Feature Pack pour SSUA](https://www.microsoft.com/download/details.aspx?id=57474).  
  
[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
