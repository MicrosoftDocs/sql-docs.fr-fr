---
title: Notes de publication de SQL Server 2016 | Microsoft Docs
description: Ce document Notes de publication décrit les problèmes connus dont vous devez prendre connaissance avant d’installer ou de dépanner les versions de Microsoft SQL Server 2016.
ms.date: 04/25/2018
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- build notes
- release issues
ms.assetid: c64077a2-bec8-4c87-9def-3dbfb1ea1fb6
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: dad7665ffa825a80aa6bcb3050b04a96060c3f65
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076041"
---
# <a name="sql-server-2016-release-notes"></a>Notes de publication de SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  
  Cet article décrit les limitations et les problèmes des versions SQL Server 2016, notamment les Service Packs. Pour plus d’informations sur les nouveautés, consultez [Nouveautés de SQL Server 2016](./what-s-new-in-sql-server-2016.md).

- [![Télécharger à partir du Centre d’évaluation](../includes/media/download2.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)  Télécharger SQL Server 2016 à partir du **[Centre d’évaluation](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)**
- [![Machine virtuelle Azure de petite taille](../includes/media/azure-vm.png)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview) Vous avez un compte Azure ?  Cliquez **[ici](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)** pour lancer une machine virtuelle avec SQL Server 2016 SP1 déjà installé.
- [![Télécharger SSMS](../includes/media/download2.png)](../ssms/download-sql-server-management-studio-ssms.md) Pour obtenir la dernière version de SQL Server Management Studio, consultez **[Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)** .

## <a name="sql-server-2016-service-pack-2-sp2"></a><a name="bkmk_2016sp2"></a>SQL Server 2016 Service Pack 2 (SP2)

![info_tip](../sql-server/media/info-tip.png) SQL Server 2016 SP2 inclut toutes les mises à jour cumulatives publiées après 2016 SP1, jusqu’à CU8 inclus.

- [![Centre de téléchargement Microsoft](../includes/media/download2.png)](https://www.microsoft.com/download/details.aspx?id=56836) [Télécharger SQL Server 2016 Service Pack 2 (SP2)](https://www.microsoft.com/download/details.aspx?id=56836)
- Pour une liste complète des mises à jour, consultez [Informations de version de SQL Server 2016 Service Pack 2](https://support.microsoft.com/help/4052908/sql-server-2016-service-pack-2-release-information)

Il peut s’avérer nécessaire de redémarrer le système après l’installation de SQL Server 2016 SP2. Nous vous recommandons de planifier et d’effectuer un redémarrage après l’installation de SQL Server 2016 SP2.

Améliorations relatives aux performances et à la scalabilité incluses dans SQL Server 2016 SP2.

|Fonctionnalité|Description|Informations complémentaires|
|---|---|---|
|Procédure améliorée de nettoyage de base de données de distribution   |   Une table de base de données de distribution de grande taille causait une situation de blocage. Cette amélioration vise à éviter certains scénarios de blocage ou d’interblocage.   |   [KB4040276](https://support.microsoft.com/help/4040276/fix-indirect-checkpoints-on-the-tempdb-database-cause-non-yielding)   |
|Nettoyage du suivi des modifications   |   Amélioration des performances et de l’efficacité du nettoyage du suivi des modifications pour les tables Change Tracking.   |   [KB4052129](https://support.microsoft.com//help/4052129/update-for-manual-change-tracking-cleanup-procedure-in-sql-server-2016)   |
|Utiliser l’expiration du temps processeur pour annuler une requête Resource Governor   |   Améliore la gestion des demandes de requête en annulant réellement la requête, si les seuils du processeur pour une requête ont été atteints. Ce comportement est activé sous l’indicateur de trace 2422.   |   [KB4038419](https://support.microsoft.com/help/4038419/add-cpu-timeout-to-resource-governor-request-max-cpu-time-sec)   |
|SELECT INTO pour créer une table cible dans un groupe de fichiers   |   À partir de SQL Server 2016 SP2, la syntaxe T-SQL SELECT INTO prend en charge le chargement d’une table dans un groupe de fichiers autre qu’un groupe de fichiers par défaut de l’utilisateur, à l’aide du mot clé ON <Filegroup name> dans la syntaxe T-SQL.   |      |
|Point de contrôle indirect amélioré pour TempDB   |   Le point de contrôle indirect pour TempDB est amélioré pour minimiser la contention de verrouillage tournant sur DPLists. Cette amélioration permet à la charge de travail TempDB sur SQL Server 2016 de bénéficier immédiatement d’un scale-out si le point de contrôle indirect est défini sur ON pour TempDB.   |   [KB4040276](https://support.microsoft.com/help/4040276)   |
|Performances de sauvegarde de base de données améliorées sur les machines à mémoire volumineuse   |   SQL Server 2016 SP2 optimise la façon dont nous drainons les E/S continues durant la sauvegarde, entraînant des gains significatifs en termes de performances de sauvegarde pour les bases de données petites à moyennes. Nous avons constaté une amélioration supérieure à 100 fois lors de sauvegardes de bases de données système sur une machine de 2 To. Le gain de performances diminue avec l’augmentation de la taille de la base de données, car les pages à sauvegarder et les E/S de la sauvegarde prennent plus de temps que les itérations du pool de mémoires tampons. Cette modification permet d’améliorer les performances de sauvegarde pour les clients hébergeant plusieurs bases de données de petite taille sur des serveurs haut de gamme volumineux dotés d’une grande capacité de mémoire.   |      |
|Prise en charge de la compression de sauvegarde VDI pour les bases de données compatibles TDE   |   SQL Server 2016 SP2 ajoute la prise en charge de VDI pour permettre aux solutions de sauvegarde VDI de tirer parti de la compression pour les bases de données compatibles TDE. Avec cette amélioration, un nouveau format de sauvegarde a été introduit pour prendre en charge la compression de sauvegarde pour les bases de données compatibles TDE. Le moteur SQL Server gère en toute fluidité les formats de sauvegarde nouveaux et anciens pour restaurer les sauvegardes.   |      |
|Chargement dynamique des paramètres de profil d’agent de réplication   |   Cette nouvelle amélioration permet le chargement dynamique des paramètres des agents de réplication sans avoir à redémarrer l’agent. Ce changement s’applique uniquement aux paramètres de profil d’agent les plus couramment utilisés.   |      |
|Prise en charge de l’option MAXDOP pour la création/mise à jour des statistiques   |    Cette amélioration permet de spécifier l’option MAXDOP pour une instruction CREATE/UPDATE relative aux statistiques. Elle permet aussi de s’assurer que le paramètre MAXDOP correct est utilisé lorsque des statistiques sont mises à jour dans le cadre d’une opération de création ou de regénération de tous types d’index (si l’option MAXDOP est présente)   |   [KB4041809](https://support.microsoft.com/help/4041809)   |
|Mise à jour des statistiques automatique améliorée pour les statistiques incrémentielles   |    Dans certains scénarios, lorsque plusieurs modifications des données ont eu lieu sur plusieurs partitions d’une table entraînant le compteur de modifications totales pour les statistiques incrémentielles à dépasser le seuil de mise à jour automatique, mais sans que les partitions individuelles ne dépassent le seuil de mise à jour automatique, la mise à jour des statistiques peut être retardée jusqu’à ce que davantage de modifications aient lieu dans la table. Ce comportement est corrigé sous l’indicateur de trace 11024.   |      |

Améliorations relatives à la prise en charge et aux diagnostics incluses dans SQL Server 2016 SP2.

|Fonctionnalité|Description|Informations complémentaires|
|---|---|---|
|Prise en charge complète de DTC pour les bases de données dans un groupe de disponibilité   |   Les transactions entre bases de données faisant partie d’un groupe de disponibilité ne sont actuellement pas prises en charge pour SQL Server 2016. Avec SQL Server 2016 SP2, nous présentons la prise en charge complète des transactions distribuées avec des bases de données de groupes de disponibilité.   |      |
|Mise à jour de la colonne is_encrypted de sys.databases pour refléter de manière correcte l’état du chiffrement pour TempDB   |   La valeur de la colonne is_encryptedcolumn dans sys.databases est égale à 1 pour TempDB, même après la désactivation du chiffrement pour toutes les bases de données utilisateur et le redémarrage de SQL Server. Le comportement attendu est une valeur de 0, car TempDB n’est plus chiffré dans ce cas. À partir de SQL Server 2016 SP2, sys.databases.is_encrypted reflète maintenant de manière correcte l’état du chiffrement pour TempDB.   |      |
|Nouvelles options DBCC CLONEDATABASE pour générer un clone et une sauvegarde vérifiés   |   Avec SQL Server 2016 SP2, DBCC CLONEDATABASE offre deux nouvelles options : créer un clone vérifié ou créer une sauvegarde vérifiée. Lorsqu’une base de données clone est créée avec l’option WITH VERIFY_CLONEDB, un clone de base de données cohérent est créé et vérifié. Celui-ci est pris en charge par Microsoft pour une utilisation en production. Une nouvelle propriété est ajoutée pour valider que le clone est vérifié SELECT DATABASEPROPERTYEX('clone_database_name', 'IsVerifiedClone'). Lorsqu’un clone est créé avec l’option BACKUP_CLONEDB, une sauvegarde est générée dans le même dossier que le fichier de données afin de faciliter, pour les utilisateurs, le déplacement du clone vers un autre serveur ou son envoi au Support technique Microsoft à des fins de résolution des problèmes.   |      |
|Prise en charge de Service Broker (SSB) pour DBCC CLONEDATABASE   |   Commande DBCC CLONEDATABASE améliorée pour autoriser le script d’objets SSB.   |      |
|Nouvelle vue de gestion dynamique (DMV) pour surveiller l’utilisation de l’espace du magasin de versions TempDB   |   Une nouvelle DMV sys.dm_tran_version_store_space_usage est présentée dans SQL Server 2016 SP2 pour permettre le monitoring de l’utilisation du magasin de versions par TempDB. Les Administrateurs de base de données peuvent désormais planifier de manière proactive le dimensionnement de TempDB en fonction des exigences d’utilisation du magasin de versions par base de données, sans surcharge des performances en cas d’exécution sur des serveurs de production.   |      |
|Prise en charge complète des vidages (dumps) pour les agents de réplication | Actuellement, si les agents de réplication rencontrent une exception non gérée, le comportement par défaut est de créer un vidage minimal des symptômes de l’exception. Cela rend la résolution des problèmes d’une exception non gérée très difficile. Via cette modification, nous introduisons une nouvelle clé de registre, qui permet de créer un vidage complet pour les Agents de réplication.   |      |
|Amélioration des événements étendus pour la lecture de l’échec de routage pour un groupe de disponibilité   |   Auparavant, xEvent read_only_rout_fail se déclenchait si une liste de routage existait, mais qu’aucun des serveurs dans la liste de routage n’était disponible pour les connexions. SQL Server 2016 SP2 inclut des informations supplémentaires pour aider à résoudre ce type de problème. Il étend également les points de code où cet événement XEvent peut être déclenché.   |      |
|Nouvelle vue de gestion dynamique (DMV) pour analyser le journal des transactions |   Ajout d’une nouvelle DMV sys.dm_db_log_stats qui retourne des attributs de niveau résumé et des informations sur les fichiers journaux des transactions des bases de données.   |      |
|Nouvelle vue de gestion dynamique (DMV) pour surveiller les informations du fichier journal virtuel |   Une nouvelle vue de gestion dynamique, sys.dm_db_log_info, est introduite dans SQL Server 2016 SP2 pour exposer les informations de fichier journal virtuel similaires à DBCC LOGINFO, afin de surveiller, alerter et éviter les problèmes T-Log potentiels rencontrés par les clients.   |      |
|Informations de processeur dans sys.dm_os_sys_info|   De nouvelles colonnes ont été ajoutées à la DMV sys.dm_os_sys_info pour exposer les informations relatives au processeur, comme socket_count et cores_per_numa.   |      |
|Informations d’extension modifiée dans sys.dm_db_file_space_usage|   Une nouvelle colonne a été ajoutée à sys.dm_db_file_space_usage pour suivre le nombre d’extensions modifiées depuis la dernière sauvegarde complète.   |      |
|Informations de segment dans sys.dm_exec_query_stats   |   De nouvelles colonnes ont été ajoutées à sys.dm_exec_query_stats pour effectuer le suivi du nombre de segments columnstore ignorés et lus, comme total_columnstore_segment_reads et total_columnstore_segment_skips.   |   [KB4051358](https://support.microsoft.com/help/4051358)   |
|Définition du niveau approprié de compatibilité pour une base de données de distribution   |   Après l’installation du Service Pack, le niveau de compatibilité de la base de données de distribution passe à 90. Ceci était dû à un chemin d’accès de code dans la procédure stockée sp_vupgrade_replication. Cette procédure stockée a été modifiée afin de définir le niveau de compatibilité correct pour la base de données de distribution.   |      |
|Exposer les dernières informations DBCC CHECKDB correctes connues   |   Une nouvelle option de base de données a été ajoutée pour retourner par programme la dernière exécution de DBCC CHECKDB réussie. Les utilisateurs peuvent désormais interroger DATABASEPROPERTYEX([database], 'lastgoodcheckdbtime') pour obtenir une valeur unique représentant la date/l’heure de la dernière exécution réussie de DBCC CHECKDB sur la base de données spécifiée.   |      |
|Améliorations de Showplan XML|   [Informations sur les statistiques utilisées pour compiler le plan de requête](/archive/blogs/sql_server_team/sql-server-2017-showplan-enhancements), notamment, le nom des statistiques, le compteur de modifications, le pourcentage d’échantillonnage et la dernière mise à jour des statistiques. Remarque : ceci a été ajouté pour les modèles CE 120 et ultérieurs uniquement. Par exemple, non pris en charge pour CE 70.| |
| |Un nouvel attribut [EstimateRowsWithoutRowgoal](/archive/blogs/sql_server_team/more-showplan-enhancements-row-goal) est ajouté à showplan XML si l’Optimiseur de requête utilise la logique « row goal ».| |
| |Nouveaux attributs du runtime [UdfCpuTime et UdfElapsedTime](/archive/blogs/sql_server_team/more-showplan-enhancements-udfs) dans showplan XML, pour effectuer le suivi du temps passé dans UDF (User-Defined Functions) scalaire.| |
| |Type d’attente CXPACKET ajouté à la [liste des 10 principales attentes possible](/archive/blogs/sql_server_team/new-showplan-enhancements) dans showplan XML - L’exécution de requête parallèle implique souvent des attentes CXPACKET, mais ce type d’attente ne répondait pas dans showplan XML.   |      |
| |Extension de l’avertissement de dépassement du runtime pour consigner le nombre de pages écrites dans TempDB durant un dépassement opérateur parallélisme.| |
|Prise en charge de la réplication pour les bases de données avec classements de caractères supplémentaires   |   La réplication peut désormais être prise en charge sur les bases de données qui utilisent les classements de caractères supplémentaires.   |      |
|Traitement correct de Service Broker avec basculement du groupe de disponibilité   |   Dans la mise en œuvre actuelle, quand Service Broker est activé sur des bases de données du groupe de disponibilité et qu’il y a un basculement du groupe de disponibilité, toutes les connexions Service Broker créées à partir du réplica principal restent ouvertes. Cette amélioration vise à fermer toutes les connexions ouvertes pendant un basculement du groupe de disponibilité.   |      |
|Résolution des problèmes améliorée des attentes de parallélisme   |   avec l’ajout d’une nouvelle attente [CXCONSUMER](/archive/blogs/sql_server_team/making-parallelism-waits-actionable).   |      |
|Cohérence améliorée entre les vues de gestion dynamiques (DMV) pour les mêmes informations   |   La DMV sys.dm_exec_session_wait_stats effectue désormais le suivi des attentes CXPACKET et CXCONSUMER de manière cohérente avec la DMV sys.dm_os_wait_stats.   |      |
|Résolution des problèmes améliorée des blocages de parallélisme intra-requête | Nouvel événement étendu exchange_spill pour consigner le nombre de pages écrites dans TempDB lors d’un dépassement opérateur parallélisme, dans le nom de champ xEvent worktable_physical_writes.| |
| |Les colonnes de dépassement dans les DMV sys.dm_exec_query_stats, sys.dm_exec_procedure_stats et sys.dm_exec_trigger_stats (comme total_spills) inclut aussi les données propagées par les opérateurs de parallélisme.| |
| |Le graphe de blocage XML est amélioré pour les scénarios de blocage de parallélisme, avec davantage d’attributs ajoutés à la ressource exchangeEvent.| |
| |Le graphe de blocage XML est amélioré pour les blocages impliquant des opérateurs en mode lot, avec davantage d’attributs ajoutés à la ressource SyncPoint.| |
|Rechargement dynamique de certains paramètres de profil d’agent de réplication   |   Dans la mise en œuvre actuelle des agents de réplication, toute modification dans le paramètre de profil d’agent requiert l’arrêt et le redémarrage de l’agent. Ces améliorations permettent le rechargement dynamique des paramètres sans avoir à redémarrer l’agent de réplication.   |      |

![Capture d’écran d’une barre horizontale.](media/horizontal-bar.png)

## <a name="sql-server-2016-service-pack-1-sp1"></a><a name="bkmk_2016sp1"></a>SQL Server 2016 Service Pack 1 (SP1)
![info_tip](../sql-server/media/info-tip.png) SQL Server 2016 SP1 inclut toutes les mises à jour cumulatives jusqu’à SQL Server 2016 RTM CU3, notamment la mise à jour de sécurité MS16-136. Elle contient un récapitulatif des solutions fournies dans les mises à jour cumulatives de SQL Server 2016 jusqu’à la dernière mise à jour cumulative - CU3 (incluse) et la mise à jour de sécurité MS16-136 publiée le 8 novembre 2016.

Les fonctionnalités suivantes sont disponibles dans les éditions Standard, Web, Express et Base de données locale de SQL Server SP1 (sauf indication contraire) :
- Always Encrypted
- Capture des changements de données (non disponible dans l’édition Express)
- columnstore
- Compression
- Masquage dynamique des données
- Audit à granularité fine
- OLTP en mémoire (non disponible dans l’édition Base de données locale)
- Plusieurs conteneurs Filestream (non disponible dans l’édition Base de données locale)
- Partitionnement
- PolyBase
- Sécurité au niveau des lignes

Le tableau suivant récapitule les principales améliorations fournies dans SQL Server 2016 SP1.

|Fonctionnalité|Description|Informations complémentaires|
|---|---|---|
|Insertion en bloc dans des segments de mémoire avec un TABLOCK automatique sous TF 715| L’indicateur de trace 715 active le verrou de table pour les opérations de chargement en masse dans un segment de mémoire sans index non-cluster.|[Migrating SAP workloads to SQL Server just got 2.5x faster](/archive/blogs/sql_server_team/migrating-sap-workloads-to-sql-server-just-got-2-5x-faster)|
|CREATE ou ALTER|Déployer des objets tels que des procédures stockées, des déclencheurs, des fonctions définies par l’utilisateur et des vues.|[Blog relatif au moteur de base de données SQL Server](/archive/blogs/sqlserverstorageengine/create-or-alter-another-great-language-enhancement-in-sql-server-2016-sp1)|
|Prise en charge de DROP TABLE pour la réplication|Prise en charge de la DLL TABLE DROP pour la réplication afin de permettre la suppression d’articles de réplication.|[KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactiona)|
|Signature du pilote Filestream RsFx|Le pilote Filestream RsFx est signé et certifié à l’aide du portail du tableau de bord du centre de développement du matériel Windows (portail de développement), ce qui permet d’installer le pilote Filestream RsFx SQL Server 2016 SP1 sur Windows Server 2016 et Windows 10 sans aucun problème.|[Migrating SAP workloads to SQL Server just got 2.5x faster](/archive/blogs/sql_server_team/migrating-sap-workloads-to-sql-server-just-got-2-5x-faster)|
|LPIM sur un compte de service SQL - identification par programmation|Permettre aux administrateurs de base de données d’identifier par programmation si le privilège LPIM (Verrouiller les pages en mémoire) est en vigueur au moment du démarrage du service.|[Developers Choice: Programmatically identify LPIM and IFI privileges in SQL Server](/archive/blogs/sql_server_team/developers-choice-programmatically-identify-lpim-and-ifi-privileges-in-sql-server)|
|Nettoyage manuel du suivi des modifications|Une nouvelle procédure stockée nettoie à la demande la table interne de suivi des modifications.| [KB 3173157](https://support.microsoft.com/help/3173157/adds-a-stored-procedure-for-the-manual-cleanup-of-the-change-tracking)|
|Changements de Parallel INSERT..SELECT pour les tables temporaires locales|Nouvelle fonctionnalité Parallel INSERT dans les opérations INSERT..SELECT.|[SQL Server Customer Advisory Team](/archive/blogs/sqlcat/real-world-parallel-insert-what-else-you-need-to-know)|
|Showplan XML|Diagnostics étendus incluant un avertissement d’allocation et une mémoire maximale activés pour une requête, des indicateurs de trace activés, ainsi que d’autres informations de diagnostic. | [KB 3190761](https://support.microsoft.com/help/3190761/update-to-improve-diagnostics-by-expose-data-type-of-the-parameters-fo)|
|Mémoire de classe de stockage|Améliore le traitement des transactions à l’aide de la mémoire de classe de stockage dans Windows Server 2016, ce qui permet d’accélérer les temps de validation des transactions par ordre de grandeur.|[Blog relatif au moteur de base de données SQL Server](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)|
|USE HINT|Utilisez l’option de requête `OPTION(USE HINT('<option>'))` pour modifier le comportement de l’optimiseur de requête à l’aide d’indicateurs de niveau requête pris en charge. Contrairement à QUERYTRACEON, l’option USE HINT ne nécessite pas de privilèges sysadmin.|[Developers Choice: USE HINT query hints](/archive/blogs/sql_server_team/developers-choice-use-hint-query-hints)|
|Ajouts d’événements XEvent|Nouvelles fonctionnalités de diagnostics XEvent et Perfmon améliorent la résolution des problèmes de latence.|[Événements étendus](../relational-databases/extended-events/extended-events.md)|

De plus, notez les correctifs suivants :
- En fonction des commentaires des administrateurs de base de données et de la Communauté SQL, à compter de SQL 2016 SP1, les messages de journalisation Hekaton sont réduits au minimum.
- Passez en revue les nouveaux [indicateurs de trace](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
- Les versions complètes des exemples de bases de données WideWorldImporters fonctionnent maintenant avec les éditions Standard et Express, à compter de SQL Server 2016 SP1, et sont disponibles sur [GitHub]( https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0). Aucun changement n’est nécessaire dans l’exemple. Les sauvegardes de base de données créées dans la version finale de l’édition Entreprise avec Standard et Express SP1.

Il peut s’avérer nécessaire de redémarrer le système après l’installation de SQL Server 2016 SP1. Nous vous recommandons de planifier et d’effectuer un redémarrage après l’installation de SQL Server 2016 SP1.

### <a name="download-pages-and-more-information"></a>Pages de téléchargement et informations supplémentaires

- [Télécharger le Service Pack 1 de Microsoft SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=54276)
- [SQL Server 2016 Service Pack 1 (SP1) Released](/archive/blogs/sqlreleaseservices/sql-server-2016-service-pack-1-sp1-released)
- [Informations de version de SQL Server 2016 Service Pack 1](https://support.microsoft.com/kb/3182545)
- ![info_tip](../sql-server/media/info-tip.png) [Centre de mise à jour SQL Server](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) pour obtenir des liens et des informations sur toutes les versions prises en charge, notamment les Service Packs de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)]

![Autre capture d’écran d’une barre horizontale.](media/horizontal-bar.png)

##  <a name="sql-server-2016-release---general-availability-ga"></a><a name="bkmk_2016_ga"></a> SQL Server 2016 Release - General Availability (GA)
-   [Moteur de base de données (DG)](#bkmk_ga_instalpatch)
-   [Stretch Database (DG)](#bkmk_ga_stretch)
-   [Magasin de requêtes (DG)](#bkmk_ga_query_store)
-   [Documentation du produit (DG)](#bkmk_ga_docs)

### <a name="repl_icon_warn--install-patch-requirement-ga"></a>![repl_icon_warn](../database-engine/availability-groups/windows/media/repl-icon-warn.gif) <a name="bkmk_ga_instalpatch"></a> Installer le correctif obligatoire (GA)
**Problème et impact sur le client :** Microsoft a identifié un problème qui affecte les fichiers binaires Microsoft VC ++ 2013 Runtime qui sont installés en tant que prérequis par SQL Server 2016. Une mise à jour est disponible pour résoudre ce problème. Si cette mise à jour des fichiers binaires du runtime VC n’est pas installée, SQL Server 2016 risque de rencontrer des problèmes de stabilité dans certains scénarios. Avant d’installer SQL Server 2016, vérifiez si l’ordinateur a besoin du correctif décrit dans l’ [article 3164398 de la Base de connaissances](https://support.microsoft.com/kb/3164398). Le correctif est également inclus dans [Package de mises à jour cumulatives 1 (CU1) pour SQL Server 2016 RTM](https://www.microsoft.com/download/details.aspx?id=53338).

**Résolution :** Utilisez l’une des solutions suivantes :

- Installez la [Mise à jour pour Visual C++ 2013 et de Visual C++ Redistributable Package (KB 3138367)](https://support.microsoft.com/kb/3138367). Le recours à l’article de la Base de connaissances est la méthode de résolution recommandée. Vous pouvez installer cette mise à jour avant ou après avoir installé SQL Server 2016.

    Si SQL Server 2016 est déjà installé, exécutez les étapes suivantes dans l’ordre :

    1.  Téléchargez la version appropriée de *vcredist_\*exe*.
    1.  Arrêtez le service SQL Server pour toutes les instances du moteur de base de données.
    1.  Installez **KB 3138367**.
    1.  Redémarrez l'ordinateur.


 - Installez  [KB 3164398 – Mise à jour critique pour les composants requis MSVCRT de SQL Server 2016](https://support.microsoft.com/kb/3164398).

    Si vous utilisez **KB 3164398**, vous pouvez l’installer en même temps que SQL Server, via Microsoft Update ou à partir du Centre de téléchargement Microsoft.

    - **Pendant l’installation de SQL Server 2016 :** si l’ordinateur exécutant le programme d’installation de SQL Server a accès à Internet, le programme d’installation de SQL Server recherche la mise à jour pendant l’installation générale de SQL Server. Si vous acceptez la mise à jour, le programme d’installation télécharge et met à jour les fichiers binaires pendant l’installation.

    - **Microsoft Update :** la mise à jour est disponible auprès de Microsoft Update en tant que mise à jour critique de SQL Server 2016 non liée à la sécurité. Une installation via Microsoft Update après SQL Server 2016 nécessite le redémarrage du serveur à la suite de la mise à jour.

    - **Centre de téléchargement :** enfin, la mise à jour est accessible à partir du Centre de téléchargement Microsoft. Vous pouvez télécharger le logiciel nécessaire à la mise à jour et l’installer sur les serveurs équipés de SQL Server 2016.


### <a name="stretch-database"></a><a name="bkmk_ga_stretch"></a>Stretch Database

#### <a name="problem-with-a-specific-character-in-a-database-or-table-name"></a>Problème lié à la présence d’un caractère spécifique dans le nom d’une base de données ou d’une table

**Problème et impact sur le client :** la tentative d’activation de Stretch Database sur une base de données ou une table échoue avec une erreur. Ce problème se produit quand le nom de l’objet comprend un caractère qui est traité comme un caractère différent quand il est converti de minuscule en majuscule. Le caractère « ƒ » (créé en tapant Alt+159) est un exemple de caractère provoquant ce problème.

**Solution de contournement :** si vous voulez activer Stretch Database sur la base de données ou la table, la seule solution est de renommer l’objet et de supprimer le caractère qui pose problème.

#### <a name="problem-with-an-index-that-uses-the-include-keyword"></a>Problème lié à un index qui utilise le mot clé INCLUDE

**Problème et impact sur le client :** la tentative d’activation de Stretch Database sur une table dont un index utilise le mot clé INCLUDE pour inclure des colonnes supplémentaires dans l’index échoue avec une erreur.

**Solution de contournement :** supprimez l’index qui utilise le mot clé INCLUDE, activez Stretch Database sur la table, puis recréez l’index. Dans ce cas, veillez à suivre les pratiques et stratégies de maintenance de votre organisation pour limiter autant que possible ou éviter tout impact sur les utilisateurs de la table concernée.

### <a name="query-store"></a><a name="bkmk_ga_query_store"></a>Query Store

#### <a name="problem-with-automatic-data-cleanup-on-editions-other-than-enterprise-and-developer"></a>Problème de nettoyage de données automatique sur les éditions autres que Enterprise et Developer

 **Problème et impact sur le client :** le nettoyage de données automatique échoue sur les éditions qui ne sont pas Entreprise et Developer.
Par voie de conséquence, dans la mesure où les données ne sont pas purgées manuellement, l’espace utilisé par le Magasin des requêtes croît au fil du temps jusqu’à atteindre la limite configurée. Si ce problème n’est pas corrigé, l’espace disque alloué pour les journaux d’erreurs se remplit également, car chaque tentative de nettoyage génère un fichier de vidage. La période d’activation du nettoyage dépend de la fréquence de la charge de travail, mais elle ne dépasse pas 15 minutes.

 **Solution de contournement :** si vous prévoyez d’utiliser le Magasin des requêtes sur des éditions autres que les éditions Entreprise et Developer, vous devez désactiver explicitement les stratégies de nettoyage. Cela peut être fait à partir de SQL Server Management Studio (page Propriétés de la base de données) ou via un script Transact-SQL :

```ALTER DATABASE <database name> SET QUERY_STORE (OPERATION_MODE = READ_WRITE, CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 0), SIZE_BASED_CLEANUP_MODE = OFF)```

En outre, envisagez des options de nettoyage manuel pour empêcher le magasin de requêtes de passer en mode lecture seule. Par exemple, exécutez la requête suivante pour nettoyer périodiquement un espace de données dans son intégralité :

```ALTER DATABASE <database name> SET QUERY_STORE CLEAR```

De même, exécutez les procédures stockées ci-dessous du magasin de requêtes pour nettoyer les statistiques d’exécution, des requêtes ou des plans spécifiques :

- `sp_query_store_reset_exec_stats`

- `sp_query_store_remove_plan`

- `sp_query_store_remove_query`


###  <a name="product-documentation-ga"></a><a name="bkmk_ga_docs"></a> Documentation du produit (DG)
 **Problème et impact sur le client :** la version téléchargeable de la documentation de SQL Server 2016 n’est pas encore disponible. Quand vous utilisez le Gestionnaire de bibliothèque d’aide pour **installer du contenu à partir d’une source en ligne**, vous voyez la documentation de SQL Server 2012 et SQL Server 2014, mais il n’existe aucune option pour la documentation de SQL Server 2016.

 **Solution de contournement :** utilisez l’une des solutions de contournement suivantes :

 ![Gérer les paramètres de l’aide pour SQL Server](../sql-server/media/docs-sql2016-managehelpsettings.png "Gérer les paramètres de l’aide pour SQL Server")

-   Utilisez l’option **Choisir l’aide en ligne ou locale** , puis sélectionnez « Utiliser l’aide en ligne ».

-   Utilisez l’option **Installer du contenu à partir d’une source en ligne** , puis téléchargez le contenu SQL Server 2014.

 **Aide (F1) :** par défaut, quand vous appuyez sur F1 dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], la version en ligne de l’article d’aide F1 s’affiche dans le navigateur. Le problème vient de l’aide basée sur le navigateur même si vous avez configuré et installé l’aide locale.

**Mise à jour de contenu :** Dans SQL Server Management Studio et Visual Studio, l’application Visionneuse d’aide peut cesser de répondre pendant l’ajout de la documentation. Pour résoudre ce problème, effectuez les étapes ci-dessous. Pour plus d’informations sur ce problème, consultez [La visionneuse d’aide Visual Studio se fige sur l’écran de démarrage](/previous-versions/mt654096(v=vs.140)).

* Ouvrez le fichier %LOCALAPPDATA%\Microsoft\HelpViewer2.2\HlpViewer_SSMS16_en-US.settings | HlpViewer_VisualStudio14_en-US.settings dans le Bloc-notes et remplacez la date dans le code ci-dessous par une date future.

```
     Cache LastRefreshed="12/31/2017 00:00:00"
```

## <a name="additional-information"></a>Informations supplémentaires
+ [Installation de SQL Server 2016](../database-engine/install-windows/install-sql-server.md)
+ [Centre de mise à jour SQL Server - liens et informations pour toutes les versions prises en charge](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png "MS_Logo_X-Small")
