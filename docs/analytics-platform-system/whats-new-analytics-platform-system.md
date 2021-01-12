---
title: Nouveautés
description: Découvrez les nouveautés de Microsoft Analytics Platform System, une appliance locale avec montée en puissance parallèle qui héberge des Data Warehouses en parallèle MPP SQL Server.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 004550d95ca69bae45c518b8054886e635e829df
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091782"
---
# <a name="whats-new-in-analytics-platform-system-a-scale-out-mpp-data-warehouse"></a>Nouveautés d’Analytics Platform System, un entrepôt de données MPP avec montée en puissance parallèle
Découvrez les nouveautés des dernières mises à jour d’appliance pour Microsoft Analytics Platform System (APS). APS est un appareil local avec montée en puissance parallèle qui héberge les Data Warehouses MPP SQL Server parallèles. 

::: moniker range=">= aps-pdw-2016-au7 "
<a name="h2-aps-cu7.7"></a>
## <a name="aps-cu77"></a>APS CU7.7
Date de publication-2020 novembre

### <a name="scvmm2016"></a>SCVMM2016
Le logiciel APS CU 7.7 met à niveau les machines virtuelles VMM vers Windows Server 2016 et installe SCVMM2016. SCVMM 2012 R2 en cours d’utilisation a une date de fin de vie du 2022 juillet. La version la plus récente de SCVMM est nécessaire pour prendre en charge la mise à niveau obligatoire de CU 7.7. Les clients sont invités à effectuer la mise à niveau vers CU 7.7 dès que possible.

### <a name="ssis-destination-adapter-for-sql-server-2019-as-target"></a>Adaptateur de destination SSIS pour SQL Server 2019 en tant que cible
La nouvelle carte de destination SSIS APS qui prend en charge SQL Server 2019 en tant que cible de déploiement peut être téléchargée à partir du [site de téléchargement](https://www.microsoft.com/download/details.aspx?id=57472).

<a name="h2-aps-cu7.6"></a>
## <a name="aps-cu76"></a>APS CU7.6
Date de publication-2020 avril

### <a name="rename-column"></a>Renommer une colonne
Après la mise à niveau vers CU 7.6, les clients peuvent renommer une colonne d’une table créée par l’utilisateur. Consultez [Renommer (Transact-SQL)](../t-sql/statements/rename-transact-sql.md) pour connaître la syntaxe, des exemples, des limitations et plus d’informations.

### <a name="alter-view"></a>ALTER VIEW
Les clients pourront désormais modifier les vues. Pour plus d’informations, consultez [ALTER VIEW (Transact-SQL)](../t-sql/statements/alter-view-transact-sql.md) .

<a name="h2-aps-cu7.5"></a>
## <a name="aps-cu75"></a>APS CU7.5
Date de publication : septembre 2019

### <a name="alter-external-data-source"></a>Modifier la source de données externe
Les clients seront en mesure de modifier la définition de la source de données externe avec la mise à jour CU 7.5. Les clients avec une haute disponibilité de nœud de nom Hadoop peuvent désormais modifier la source de données pour modifier les arguments en cas de basculement. Pour les APS, seuls l’emplacement, les RESOURCE_MANAGER_LOCATION et les informations d’identification peuvent être modifiés. Pour plus d’informations, consultez [ALTER external data source](../t-sql/statements/alter-external-data-source-transact-sql.md?view=sql-server-2017&preserve-view=true) .

### <a name="cdh-515-and-516-support-with-polybase"></a>Prise en charge de CDH 5,15 et 5,16 avec Polybase
Polybase sur APS avec mise à jour CU 7.5 prend désormais en charge les versions CDH 5,15 et 5,16 de la distribution Hadoop à partir de Cloudera. Utilisez l’option 6 pour les versions CDH 5. x. 

### <a name="try_convert-and-try_cast-support"></a>Prise en charge des Try_Convert et des Try_Cast
CU 7.5 les APS prennent désormais en charge les fonctions TSQL [TRY_CAST](../t-sql/functions/try-cast-transact-sql.md?view=sql-server-2017&preserve-view=true) et [TRY_CONVERT](../t-sql/functions/try-convert-transact-sql.md?view=sql-server-2017&preserve-view=true) . Ces deux fonctions retournent une valeur convertie dans le type de données spécifié si la conversion réussit ; Sinon, retourne la valeur null.

<a name="h2-aps-cu7.4"></a>
## <a name="aps-cu74"></a>APS CU7.4
Date de publication-mai 2019

### <a name="loading-large-rows-with-dwloader"></a>Chargement de lignes volumineuses avec dwloader
Depuis APS CU 7.4, les clients peuvent utiliser un nouveau dwloader pour charger des lignes dans des tables dont la taille est supérieure à 32 Ko (32 768 octets). Le nouveau dwloader prend en charge le commutateur-l qui prend une valeur entière comprise entre 32768 et 33554432 (en octets) pour charger des lignes d’une taille supérieure à 32 Ko. Utilisez cette option uniquement lors du chargement de lignes volumineuses (supérieures à 32 Ko), car ce commutateur alloue plus de mémoire sur le client et le serveur et peut ralentir les charges. Vous pouvez télécharger le nouveau dwloader à partir du [site de téléchargement](https://www.microsoft.com/download/details.aspx?id=57472).  

### <a name="hdp-30-and-31-support-with-polybase"></a>Prise en charge de HDP 3,0 et 3,1 avec Polybase
Polybase sur APS prend désormais en charge HDP 3,0 et 3,1 avec cette mise à jour. Utilisez l’option 7 pour les versions HDP 3. x. Pour plus d’informations, consultez la page [connectivité Polybase](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) .

### <a name="utf16-file-support-with-polybase"></a>Prise en charge des fichiers UTF16 avec Polybase
Polybase prend désormais en charge la lecture des fichiers texte délimités qui se trouvent dans l’encodage UTF16 (LE). Pour plus d’informations, consultez [créer un format de fichier externe](../t-sql/statements/create-external-file-format-transact-sql.md) . 

<a name="h2-aps-cu7.3"></a>
## <a name="aps-cu73"></a>APS CU7.3
Date de publication-décembre 2018

### <a name="common-subexpression-elimination"></a>Élimination des sous-expressions communes
APS CU 7.3 améliore les performances des requêtes avec l’élimination de sous-expression commune dans l’optimiseur de requête SQL. L’amélioration améliore les requêtes de deux manières. Le premier avantage est la possibilité d’identifier et d’éliminer ces expressions pour réduire le temps de compilation SQL. Le deuxième avantage et le plus important est que les opérations de déplacement de données pour ces sous-expressions redondantes sont éliminées, de sorte que la durée d’exécution des requêtes est plus rapide. Vous trouverez une explication détaillée de cette fonctionnalité [ici](common-sub-expression-elimination.md).

### <a name="aps-informatica-connector-for-informatica-1020-published"></a>Connecteur Informatica APS pour Informatica 10.2.0 publié
Nous avons publié une nouvelle version des connecteurs Informatica pour APS qui fonctionne avec Informatica version 10.2.0 et 10.2.0 Hotfix 1. Les nouveaux connecteurs peuvent être téléchargés à partir du [site de téléchargement](https://www.microsoft.com/download/details.aspx?id=57472).
> [!NOTE]
> Le connecteur Informatica APS pour Informatica 10.2.0 ou 10.2.0 Hotfix 1 ne fonctionne pas sur un protocole TLS 1.2 strict et requiert TLS 1.0 et 1,1 pour être entièrement fonctionnel.

#### <a name="supported-versions"></a>Versions prises en charge

| Version APS | Informatica PowerCenter | Pilote |
|:---|:---|:---|
| APS 2016 | 9.6.1 | SQL Server Native Client 11. x |
| APS 2016 et versions ultérieures | 10.2.0, 10.2.0 Hotfix 1 | SQL Server Native Client 11. x |

<a name="h2-aps-cu7.2"></a>
## <a name="aps-cu72"></a>APS CU7.2
Date de publication : octobre 2018

### <a name="support-for-tls-12"></a>Prise en charge de TLS 1.2
APS CU 7.2 prend en charge TLS 1,2. La communication entre les ordinateurs clients et les APS et les communications intra-nœud APS peut désormais être configurée pour communiquer uniquement sur TLS 1.2. Les outils tels que SSDT, SSIS et Dwloader installés sur les ordinateurs clients qui sont configurés pour communiquer uniquement sur TLS 1,2 peuvent désormais se connecter aux points d’accès à l’aide de TLS 1,2. Par défaut, APS prend en charge toutes les versions TLS (1,0, 1,1 et 1,2) pour la compatibilité descendante. Si vous souhaitez configurer votre appliance APS de manière à ce qu’elle utilise strictement TLS 1,2, vous pouvez le faire en modifiant les paramètres du Registre. 

Pour plus d’informations, consultez [configuration de TLS 1.2 sur APS](configure-tls12-aps.md).

### <a name="hadoop-encryption-zone-support-for-polybase"></a>Prise en charge de la zone de chiffrement Hadoop pour Polybase
Polybase peut désormais communiquer avec les zones de chiffrement Hadoop. Consultez modifications de configuration APS nécessaires à la [configuration de la sécurité Hadoop](polybase-configure-hadoop-security.md#encryptionzone).

### <a name="insert-select-maxdop-options"></a>Insert-Select les options MAXDOP
Nous avons ajouté un [commutateur de fonctionnalité](appliance-feature-switch.md) qui vous permet de choisir des paramètres MAXDOP supérieurs à 1 pour les opérations Insert-Select. Vous pouvez maintenant définir le paramètre MAXDOP sur 0, 1, 2 ou 4. La valeur par défaut est 1.

> [!IMPORTANT]  
> L’extension de MAXDOP peut parfois entraîner des erreurs d’exploitation ou d’interblocage plus lentes. Si cela se produit, remplacez le paramètre par MAXDOP 1 et recommencez l’opération.

### <a name="columnstore-index-health-dmv"></a>DMV d’intégrité de l’index ColumnStore
Vous pouvez afficher les informations sur l’intégrité de l’index ColumnStore à l’aide de **dm_pdw_nodes_db_column_store_row_group_physical_stats** DMV. Utilisez la vue suivante pour déterminer la fragmentation et décider quand reconstruire ou réorganiser un index ColumnStore.

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[pdw_node_id]    = nt.[pdw_node_id]                                          
)
select *
from cte;
```

### <a name="polybase-date-range-increase-for-orc-and-parquet-files"></a>Augmentation de la plage de dates Polybase pour les fichiers ORC et parquet
La lecture, l’importation et l’exportation des types de données de date à l’aide de Polybase prennent désormais en charge les dates antérieures à 1970-01-01 et après 2038-01-20 pour les types de fichiers ORC et parquet.

### <a name="ssis-destination-adapter-for-sql-server-2017-as-target"></a>Adaptateur de destination SSIS pour SQL Server 2017 en tant que cible
La nouvelle carte de destination SSIS APS qui prend en charge SQL Server 2017 en tant que cible de déploiement peut être téléchargée à partir du [site de téléchargement](https://www.microsoft.com/download/details.aspx?id=57472).

<a name="h2-aps-cu7.1"></a>
## <a name="aps-cu71"></a>APS CU7.1
Date de publication : juillet 2018

### <a name="dbcc-commands-do-not-consume-concurrency-slots-behavior-change"></a>Les commandes DBCC ne consomment pas d’emplacements de concurrence (changement de comportement)
APS prend en charge un sous-ensemble des [commandes DBCC](../t-sql/database-console-commands/dbcc-transact-sql.md) T-SQL telles que [DBCC DROPCLEANBUFFERS](../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md). Auparavant, ces commandes consommaient un [emplacement d’accès concurrentiel](./workload-management.md?view=aps-pdw-2016-au7&preserve-view=true&#concurrency-slots), réduisant ainsi le nombre de chargements/requêtes utilisateur qui pouvaient être exécutés. Les `DBCC` commandes sont maintenant exécutées dans une file d’attente locale qui ne consomme pas un emplacement d’accès concurrentiel utilisateur pour améliorer les performances globales d’exécution des requêtes.

### <a name="replaces-some-metadata-calls-with-catalog-objects"></a>Remplace certains appels de métadonnées par des objets de catalogue
L’utilisation d’objets de catalogue pour les appels de métadonnées au lieu d’utiliser SMO a montré l’amélioration des performances dans APS. À partir de CU 7.1, certains de ces appels de métadonnées utilisent désormais des objets de catalogue par défaut. Ce comportement peut être désactivé par le [commutateur de fonctionnalité](appliance-feature-switch.md) si les clients utilisant des requêtes de métadonnées s’exécutent en cas de problème.

### <a name="bug-fixes"></a>Résolution des bogues
Nous avons effectué la mise à niveau vers SQL Server 2016 SP2 CU2 avec APS CU 7.1. La mise à niveau corrige certains problèmes décrits ci-dessous.

| Intitulé | Description |
|:---|:---|
| **Blocage potentiel du moteur de Tuple** |La mise à niveau résout un long risque d’interblocage dans une transaction distribuée et un thread d’arrière-plan du moteur de Tuple. Après l’installation de CU 7.1, les clients qui ont utilisé TF634 pour arrêter le moteur de tuple comme SQL Server paramètre de démarrage ou l’indicateur de trace global peuvent le supprimer en toute sécurité. | 
| **Échec de certaines requêtes de décalage/Lead** |Certaines requêtes sur les tables ICC avec des fonctions de décalage/Lead imbriquées qui génèrent des erreurs sont désormais résolues avec cette mise à niveau. | 


<a name="h2-aps-au7"></a>
## <a name="aps-au7"></a>APS AU7
Date de publication-mai 2018

APS 2016 est un composant requis pour la mise à niveau vers AU7. Les nouvelles fonctionnalités d’APS AU7 sont les suivantes :

### <a name="auto-create-and-auto-update-statistics"></a>Création automatique et mise à jour automatique des statistiques
APS AU7 crée et met à jour automatiquement les statistiques, par défaut. Pour mettre à jour les paramètres de statistiques, les administrateurs peuvent utiliser un nouvel élément de menu commutateur de fonctionnalité dans la [Configuration Manager](appliance-configuration.md#CMTasks). Le [commutateur de fonctionnalité](appliance-feature-switch.md) contrôle la création automatique, la mise à jour automatique et le comportement de mise à jour asynchrone des statistiques. Vous pouvez également mettre à jour les paramètres de statistiques à l’aide de l’instruction [ALTER DATABASE (Parallel Data Warehouse)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) .

### <a name="t-sql"></a>T-SQL
SELECT @var est désormais pris en charge. Pour plus d’informations, consultez [Sélectionner une variable locale](../t-sql/language-elements/select-local-variable-transact-sql.md) 

Les indicateurs de requête sont désormais pris en charge. Pour plus d’informations, consultez [indicateurs (Transact-SQL)-requête](../t-sql/queries/hints-transact-sql-query.md)

### <a name="feature-switch"></a>Commutateur de fonctionnalité
APS AU7 présente le commutateur des fonctionnalités dans [Configuration Manager](launch-the-configuration-manager.md). AutoStatsEnabled et DmsProcessStopMessageTimeoutInSeconds sont désormais des options configurables qui peuvent être modifiées par les administrateurs.

### <a name="known-issues"></a>Problèmes connus
Avec APS AU7 Software, une mise à jour du BIOS Intel est fournie, qui résout un problème décrit en tant qu' *attaques par canal latéral d’exécution spéculative*. Les attaques visent à exploiter les *vulnérabilités appelées spectre et Meltdown*. Bien qu’elles soient empaquetées avec APS, la mise à jour du BIOS est installée manuellement, et non dans le cadre de l’installation du logiciel APS AU7.

Microsoft conseille à tous les clients d’installer le BIOS mis à jour. Microsoft a mesuré l’effet de l’occultation des adresses virtuelles (KVAS), de l’indirection de la table de pages du noyau (KPTI) et de l’atténuation des prédictions de branche indirecte (IBP) sur diverses charges de travail SQL dans différents environnements. Les mesures ont trouvé une dégradation significative sur certaines charges de travail. En fonction des résultats, il est recommandé de tester l’impact sur les performances de la mise à jour du BIOS avant de les déployer dans un environnement de production. Consultez les conseils SQL Server [ici](https://support.microsoft.com/help/4073225/guidance-protect-sql-server-against-spectre-meltdown).

::: moniker-end
::: moniker range=">= aps-pdw-2016 "
<a name="h2-aps-au6"></a>
## <a name="aps-2016"></a>APS 2016
Cette section a décrit les nouvelles fonctionnalités pour APS 2016-AU6.

### <a name="sql-server-2016"></a>SQL Server 2016

APS AU6 s’exécute sur la dernière version de SQL Server 2016 et utilise le niveau de compatibilité de base de données par défaut 130. SQL Server 2016 permet la prise en charge de nouvelles fonctionnalités telles que :

- Index secondaires pour les index ColumnStore en cluster.
- Kerberos pour Polybase.

### <a name="t-sql"></a>T-SQL
APS AU6 prend en charge ces améliorations de compatibilité T-SQL.  Ces éléments de langage supplémentaires facilitent la migration à partir de SQL Server et d’autres sources de données. 

- Les [classements SQL au niveau des colonnes][] sont désormais pris en charge, en plus des classements Windows.
- Les [index non cluster sur les index ColumnStore cluster][] améliorent les performances des requêtes qui recherchent des valeurs spécifiques dans l’index ColumnStore cluster. 
- [SÉLECTIONNER... DANS][] 
- [sp_spaceused ()][] affiche l’espace disque utilisé ou réservé dans une table ou une base de données.
- La prise en charge des [tables larges][] est identique à SQL Server 2016. La limite précédente de 32 K pour la taille de ligne n’existe plus. 

**Types de données**

- [Varchar (max)][], [nvarchar (max)][] et [varbinary (max)][]. Ces types de données LOB ont une taille maximale de 2 Go. Pour charger ces objets, utilisez l' [utilitaire bcp][]. Polybase et dwloader ne prennent pas en charge ces types de données pour le moment. 
- [SA][]
- [UNIQUEIDENTIFIER][]
- Types de données [Numeric][] et Decimal.

**Fonctions Windows**

- [Rows ou Range][] dans la clause on de l’instruction SELECT.
- [FIRST_VALUE][]
- [LAST_VALUE][]
- [CUME_DIST][]
- [PERCENT_RANK][]

**Fonctions de sécurité**

- [Checksum ()][] et [BINARY_CHECKSUM ()][]
- [HAS_PERMS_BY_NAME ()][]

**Fonctions supplémentaires**

- [NEWID ()][]
- [RAND ()][]

### <a name="polybasehadoop-enhancements"></a>Améliorations de Polybase/Hadoop

- Compatibilité avec Hortonworks HDP 2,4 et HDP 2,5
- Prise en charge Kerberos via les informations d’identification délimitées à la base de données
- Prise en charge des informations d’identification avec Azure Storage BLOB

### <a name="install-and-upgrade-enhancements"></a>Améliorations de l’installation et de la mise à niveau

**Mises à jour** de l’architecture d’entreprise La mise à niveau de votre appliance existante vers APS AU6 installe les dernières mises à jour du microprogramme et du pilote, y compris les correctifs de sécurité. 

Une nouvelle appliance de l’adaptateur HPE ou DELL comprend toutes les mises à jour les plus récentes, ainsi que les suivantes :

- Prise en charge du processeur de dernière génération (Broadwell)
- Mettre à jour les DIMMs DDR4
- Amélioration du débit DIMM

**Intégration**

- La prise en charge du nom de domaine complet (FQDN) permet de configurer une approbation de domaine pour l’appliance. 
- Pour utiliser le nom de domaine complet, vous devez effectuer une mise à niveau complète et vous abonner au cours de la mise à niveau. 

**Temps d’arrêt réduit** L’installation ou la mise à niveau vers APS AU6 est plus rapide et nécessite moins de temps d’arrêt que les versions précédentes. Pour réduire les temps d’arrêt, procédez à l’installation ou à la mise à niveau : 

 - Rationalise l’application des mises à jour WSUS à l’aide d’une image qui contient toutes les mises à jour jusqu’au 2016 juin
 - Applique les mises à jour de sécurité avec les mises à jour du pilote et du microprogramme
 - Place les derniers correctifs et l’utilitaire de vérification de l’appareil (PAV) sur votre appliance afin qu’ils soient prêts à être installés sans avoir besoin de les télécharger.

::: moniker-end

<!--
Link references to other articles in this same GitHub repo.

The link format that starts with '/sql/what-ever/my-artlcle' is not appropriate for common links within the same repo (as most of these link are).  The first couple links have been edited to show the proper syntax, but all other links in this article need to be similarly edited.
The proper formats have at least two big advantages.  One big advantage is that the proper formats enable the OPS Build system to detect broken links at Pull Request build time, instead of only later during run time.
-->
[database compatibility level 130]: ../t-sql/statements/alter-database-transact-sql-compatibility-level.md
[Classements SQL au niveau des colonnes]: ~/relational-databases/collations/collation-and-unicode-support.md

[Index non cluster sur les index ColumnStore en cluster]:/sql/t-sql/statements/create-index-transact-sql
[VARCHAR (MAX)]:/sql/t-sql/data-types/char-and-varchar-transact-sql
[NVARCHAR (MAX)]:/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql
[VARBINARY (MAX)]:/sql/t-sql/data-types/binary-and-varbinary-transact-sql
[SA]:/sql/relational-databases/system-catalog-views/sys-types-transact-sql
[SÉLECTIONNER... DANS]:/sql/t-sql/queries/select-into-clause-transact-sql
[sp_spaceused ()]:/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql
[Tableaux larges]:/sql/sql-server/maximum-capacity-specifications-for-sql-server
[BULK INSERT]:/sql/t-sql/statements/bulk-insert-transact-sql
[Utilitaire bcp]:/sql/tools/bcp-utility
[UNIQUEIDENTIFIER]:/sql/t-sql/data-types/uniqueidentifier-transact-sql
[NUMERIC]:/sql/t-sql/data-types/decimal-and-numeric-transact-sql
[ROWS ou RANGE]:/sql/t-sql/queries/select-over-clause-transact-sql
[FIRST_VALUE]:/sql/t-sql/functions/first-value-transact-sql
[LAST_VALUE]:/sql/t-sql/functions/last-value-transact-sql
[CUME_DIST]:/sql/t-sql/functions/cume-dist-transact-sql
[PERCENT_RANK]:/sql/t-sql/functions/percent-rank-transact-sql
[CHECKSUM ()]:/sql/t-sql/functions/checksum-transact-sql
[BINARY_CHECKSUM ()]:/sql/t-sql/functions/binary-checksum-transact-sql
[HAS_PERMS_BY_NAME ()]:/sql/t-sql/functions/has-perms-by-name-transact-sql
[NEWID ()]:/sql/t-sql/functions/newid-transact-sql
[RAND ()]:/sql/t-sql/functions/rand-transact-sql


  

