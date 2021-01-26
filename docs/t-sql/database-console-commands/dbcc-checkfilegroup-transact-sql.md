---
description: DBCC CHECKFILEGROUP (Transact-SQL)
title: DBCC CHECKFILEGROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKFILEGROUP_TSQL
- DBCC_CHECKFILEGROUP_TSQL
- DBCC CHECKFILEGROUP
- CHECKFILEGROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKFILEGROUP statement
- database objects [SQL Server], checking
- allocation checks
- integrity [SQL Server], database objects
- filegroups [SQL Server], consistency checks
- table integrity checks [SQL Server]
- checking database objects
ms.assetid: 8c70bf34-7570-4eb6-877a-e35064a1380a
author: pmasl
ms.author: umajay
ms.openlocfilehash: 885a40a622f70649e004226fdfb9f317bad6bbdf
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597094"
---
# <a name="dbcc-checkfilegroup-transact-sql"></a>DBCC CHECKFILEGROUP (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Vérifie l'allocation et l'intégrité de la structure de toutes les tables et vues indexées dans le groupe de fichiers spécifié de la base de données active.
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DBCC CHECKFILEGROUP   
[  
    [ ( { filegroup_name | filegroup_id | 0 }   
        [ , NOINDEX ]   
  ) ]  
    [ WITH   
        {   
            [ ALL_ERRORMSGS | NO_INFOMSGS ]   
            [ , TABLOCK ]   
            [ , ESTIMATEONLY ]  
            [ , PHYSICAL_ONLY ]    
            [ , MAXDOP  = number_of_processors ]  
        }   
    ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *filegroup_name*  
 Nom du groupe de fichiers, dans la base de données active, pour lequel l'allocation de table et l'intégrité de la structure doivent être vérifiées. Si vous ne définissez pas cet argument ou si vous lui attribuez la valeur 0, le groupe de fichiers primaire est utilisé par défaut. Les noms de groupe de fichiers doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
 *filegroup_name* ne peut pas être un groupe de fichiers FILESTREAM.  
  
 *filegroup_id*  
 Numéro d'identification (ID) du groupe de fichiers, dans la base de données active, pour lequel l'allocation de table et l'intégrité de la structure sont vérifiées.  
  
 NOINDEX  
 Indique qu'il ne faut pas effectuer de vérifications intensives des index non cluster pour les tables utilisateur. Cela diminue la durée d'exécution globale. L'argument NOINDEX n'affecte pas les tables système, car DBCC CHECKFILEGROUP vérifie toujours tous les index des tables système.  
  
 ALL_ERRORMSGS  
 Affiche un nombre illimité d'erreurs par objet. Tous les messages d'erreur sont affichés par défaut. La spécification ou non de cette option n'a aucun effet.  
  
 NO_INFOMSGS  
 Supprime tous les messages d'information.  
  
 TABLOCK  
 Indique à DBCC CHECKFILEGROUP d'obtenir des verrous au lieu d'utiliser un instantané de base de données interne.  
  
 ESTIMATE ONLY  
 Affiche une estimation de la quantité d'espace tempdb requise pour exécuter DBCC CHECKFILEGROUP avec toutes les autres options spécifiées.  
  
 PHYSICAL_ONLY  
 Limite la vérification à l'intégrité de la structure physique de la page, des en-têtes d'enregistrement et de la structure physique des arbres B (B-trees). Conçue pour effectuer une vérification à faible charge de la cohérence physique du groupe de fichiers, cette vérification peut également détecter les pages endommagées et les erreurs matérielles courantes susceptibles de compromettre les données. Une exécution complète de DBCC CHECKFILEGROUP peut prendre beaucoup plus de temps que dans les versions antérieures. Ce comportement se produit pour les raisons suivantes :  
 -   Les vérifications logiques sont plus complètes.  
 -   Certaines des structures sous-jacentes à vérifier sont plus complexes.  
 -   De nombreuses vérifications nouvelles ont été introduites pour inclure les nouvelles fonctionnalités.  
 Par conséquent, l'utilisation de l'option PHYSICAL_ONLY étant susceptible de réduire considérablement la durée d'exécution de DBCC CHECKFILEGROUP sur des groupes de fichiers volumineux, elle est recommandée pour une utilisation fréquente sur des systèmes de production. Nous vous recommandons toutefois d'effectuer régulièrement une exécution complète de DBCC CHECKFILEGROUP. La fréquence de ces exécutions dépend de facteurs spécifiques à chaque entreprise et à chaque environnement de production. L'option PHYSICAL_ONLY implique toujours NO_INFOMSGS et n'est autorisée avec aucune des options de réparation.  
  
> [!NOTE]  
>  En raison de la spécification de PHYSICAL_ONLY, DBCC CHECKFILEGROUP ignore toutes les vérifications des données FILESTREAM.  
  
 MAXDOP  
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014 SP2 à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md).  
  
 Remplace l’option de configuration **max degree of parallelism** de **sp_configure** pour l’instruction. MAXDOP peut dépasser la valeur configurée avec sp_configure. Si MAXDOP dépasse la valeur configurée avec Resource Governor, le moteur de base de données utilise la valeur MAXDOP de Resource Governor, décrite dans ALTER WORKLOAD GROUP (Transact-SQL). Toutes les règles sémantiques utilisées avec l'option de configuration max degree of parallelism sont applicables lorsque vous utilisez l'indicateur de requête MAXDOP. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
> [!CAUTION]  
>  Si MAXDOP est défini avec la valeur zéro, le serveur choisit le degré maximal de parallélisme.  
  
## <a name="remarks"></a>Notes  
DBCC CHECKFILEGROUP et DBCC CHECKDB sont des commandes DBCC similaires. La principale différence réside dans le fait que la commande DBCC CHECKFILEGROUP est limitée au groupe de fichiers unique spécifié et aux tables requises.
DBCC CHECKFILEGROUP exécute les commandes suivantes :
-   [DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md) du groupe de fichiers ;  
-   [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) de chaque table et vue indexée dans le groupe de fichiers.  
  
L'exécution de DBCC CHECKALLOC ou DBCC CHECKTABLE indépendamment de DBCC CHECKFILEGROUP n'est pas requise.
  
## <a name="internal-database-snapshot"></a>Instantané de base de données interne  
DBCC CHECKFILEGROUP utilise un instantané de base de données interne pour fournir la cohérence transactionnelle nécessaire à la réalisation de ces vérifications. Pour plus d’informations, consultez [Afficher la taille du fichier partiellement alloué d’un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) et la section « Utilisation de l’instantané de base de données interne DBCC » dans [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md).
S'il est impossible de créer un instantané, ou si l'option TABLOCK est spécifiée, la commande DBCC CHECKFILEGROUP acquiert des verrous pour obtenir la cohérence nécessaire. Dans ce cas, un verrou de base de données exclusif est requis pour effectuer les vérifications d'allocation, tandis que des verrous de table partagés sont nécessaires pour effectuer les vérifications de table. TABLOCK accélère l'exécution de DBCC CHECKFILEGROUP sur une base de données dont la charge est importante, tout en diminuant la concurrence disponible dans cette dernière pendant l'exécution de DBCC CHECKFILEGROUP.
  
> [!NOTE]  
>  L'exécution de la commande DBCC CHECKFILEGROUP sur tempdb n'entraîne aucune vérification d'allocation ; par ailleurs, cette commande doit acquérir des verrous de table partagés pour vérifier les tables. En effet, pour des raisons de performances, les instantanés de base de données ne sont pas disponibles sur tempdb. Cela signifie que la cohérence transactionnelle requise ne peut pas être obtenue.  
  
## <a name="checking-objects-in-parallel"></a>Vérification des objets en parallèle  
DBCC CHECKFILEGROUP effectue par défaut une vérification parallèle des objets. Le degré de parallélisme est automatiquement défini par le processeur de requêtes. Le degré maximum de parallélisme est configuré de la même manière que les requêtes parallèles. Pour limiter le nombre maximal de processeurs disponibles pour la vérification DBCC, utilisez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md). Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
La vérification parallèle peut être désactivée à l'aide de l'indicateur de trace 2528. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
  
## <a name="nonclustered-indexes-on-separate-filegroups"></a>Index non cluster sur des groupes de fichiers distincts  
Si un index non cluster du groupe de fichiers spécifié est associé à une table d'un autre groupe de fichiers, l'index n'est pas vérifié, car la table de base n'est pas disponible pour être validée.
Si une table du groupe de fichiers spécifié possède un index non cluster dans un autre groupe de fichiers, l'index non cluster n'est pas vérifié pour les raisons suivantes :
-   La structure de la table de base n'est pas dépendante de la structure d'un index non cluster. Les index non cluster n'ont pas à être analysés pour permettre la validation de la table de base.  
-   La commande DBCC CHECKFILEGROUP valide les objets uniquement dans le groupe de fichiers spécifié.  
Un index cluster et une table ne peuvent pas se trouver dans des groupes de fichiers différents ; par conséquent, les considérations précédentes ne peuvent s'appliquer qu'aux index non cluster.
  
## <a name="partitioned-tables-on-separate-filegroups"></a>Tables partitionnées dans des groupes de fichiers distincts  
Lorsqu'une table partitionnée existe sur plusieurs groupes de fichiers, DBCC CHECKFILEGROUP vérifie les ensembles de lignes de partition présents sur le groupe de fichiers spécifiés et ignore les ensembles de lignes dans les autres groupes de fichiers. Le message d'information 2594 indique les partitions qui n'ont pas été vérifiées. Les index non cluster qui ne figurent pas dans le groupe de fichiers spécifié ne sont pas vérifiés.
  
## <a name="understanding-dbcc-error-messages"></a>Présentation des messages d'erreur de DBCC  
Une fois la commande DBCC CHECKFILEGROUP exécutée, un message est consigné dans le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si la commande DBCC est correctement exécutée, le message indique que l'exécution a réussi, ainsi que la durée d'exécution de la commande. Si la commande DBCC est interrompue avant la fin de la vérification en raison d’une erreur, le message indique que la commande n’a pas abouti et précise une valeur d’état ainsi que la durée d’exécution de la commande. Le tableau suivant répertorie et décrit les valeurs d'état pouvant être incluses dans le message.
  
|State|Description|  
|-----------|-----------------|  
|0|Erreur numéro 8930 générée. Ceci indique une corruption des métadonnées qui a provoqué l'arrêt de la commande DBCC.|  
|1|Erreur numéro 8967 générée. Une erreur DBCC interne s'est produite.|  
|2|Une erreur s'est produite lors de la réparation de la base de données en mode urgence.|  
|3|Ceci indique une corruption des métadonnées qui a provoqué l'arrêt de la commande DBCC.|  
|4|Une assertion ou une violation d'accès a été détectée.|  
|5|Une erreur inconnue s'est produite et a arrêté la commande DBCC.|  
  
## <a name="error-reporting"></a>Rapport d'erreurs  
Un mini-fichier de vidage (SQLDUMP *nnnn*.txt) est créé dans le répertoire LOG de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chaque fois que DBCC CHECKFILEGROUP détecte une erreur d’altération. Lorsque les fonctions de collecte des données d'utilisation des fonctionnalités et de rapport d'erreurs sont activées pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce fichier est automatiquement transféré à [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Les données collectées sont utilisées pour améliorer les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Le fichier de vidage contient les résultats de la commande DBCC CHECKFILEGROUP ainsi que des informations de diagnostic supplémentaires. Ce fichier contient des listes de contrôle d'accès discrétionnaire (DACL, Discretionary Access Control Lists) avec accès restreint. L’accès est limité au compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux membres du rôle **sysadmin**. Par défaut, le rôle **sysadmin** contient tous les membres du groupe Windows BUILTIN\Administrators et du groupe Administrateurs local. La commande DBCC n'échoue pas si le processus de collecte des données échoue.
  
## <a name="resolving-errors"></a>Résolution des erreurs  
Si des erreurs sont signalées par DBCC CHECKFILEGROUP, il est recommandé de restaurer la base de données à partir de la sauvegarde de cette dernière. Notez que les options de réparation ne peuvent pas être spécifiées pour DBCC CHECKFILEGROUP.
Si aucune sauvegarde n'existe, l'exécution de DBCC CHECKDB avec une option de réparation spécifiée corrige les erreurs signalées. L'option de réparation à utiliser est spécifiée à la fin de la liste des erreurs signalées. La correction des erreurs à l'aide de l'option REPAIR_ALLOW_DATA_LOSS peut nécessiter la suppression de certaines pages et, par conséquent, de certaines données.
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC CHECKFILEGROUP retourne le jeu de résultats suivant (les valeurs peuvent varier) :
-   sauf lorsque ESTIMATEONLY ou NO_INFOMSGS est spécifié ;  
-   pour la base de données active, si aucune base de données n'est spécifiée, que des options soient ou non définies (sauf NOINDEX).  
  
```
DBCC results for 'master'.  
DBCC results for 'sys.sysrowsetcolumns'.  
There are 630 rows in 7 pages for object 'sys.sysrowsetcolumns'.  
DBCC results for 'sys.sysrowsets'.  
There are 97 rows in 1 pages for object 'sys.sysrowsets'.  
DBCC results for 'sysallocunits'.  
There are 195 rows in 3 pages for object 'sysallocunits'.  
  
There are 2340 rows in 16 pages for object 'spt_values'.  
DBCC results for 'MSreplication_options'.  
There are 2 rows in 1 pages for object 'MSreplication_options'.  
CHECKFILEGROUP found 0 allocation errors and 0 consistency errors in database 'master'.  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
Si NO_INFOMSGS est spécifié, DBCC CHECKFILEGROUP retourne le résultat suivant :
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
 Si ESTIMATEONLY est spécifié, DBCC CHECKFILEGROUP retourne le résultat suivant (les valeurs peuvent varier) :  

```
Estimated TEMPDB space needed for CHECKALLOC (KB)
-------------------------------------------------   
15  
  
(1 row(s) affected)  
  
Estimated TEMPDB space needed for CHECKTABLES (KB)   
--------------------------------------------------   
207  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .
  
## <a name="examples"></a>Exemples  
### <a name="a-checking-the-primary-filegroup-in-the-a-database"></a>R. Vérification du groupe de fichiers PRIMARY d'une base de données  
L'exemple suivant vérifie le groupe de fichiers primaire de la base de données active.
  
```sql
DBCC CHECKFILEGROUP;  
GO  
```  
  
### <a name="b-checking-the-adventureworks-primary-filegroup-without-nonclustered-indexes"></a>B. Vérification du groupe de fichiers PRIMARY de la base de données AdventureWorks sans index non cluster  
L’exemple suivant vérifie le groupe de fichiers principal de la base de données `AdventureWorks2012` (à l’exception des index non-cluster) en spécifiant le numéro d’identification du groupe de fichiers principal et `NOINDEX`.
  
```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKFILEGROUP (1, NOINDEX);  
GO  
```  
  
### <a name="c-checking-the-primary-filegroup-with-options"></a>C. Vérification du groupe de fichiers PRIMARY avec des options  
L'exemple suivant vérifie le groupe de fichiers primaire de la base de données `master` et spécifie l'option `ESTIMATEONLY`.
  
```sql
USE master;  
GO  
DBCC CHECKFILEGROUP (1)  
WITH ESTIMATEONLY;  
```  
  
## <a name="see-also"></a>Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[FILEGROUP_ID &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-id-transact-sql.md)  
[sp_helpfile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)  
[sp_helpfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)  
[sys.sysfilegroups &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysfilegroups-transact-sql.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[DBCC CHECKALLOC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)  
[DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)
  
