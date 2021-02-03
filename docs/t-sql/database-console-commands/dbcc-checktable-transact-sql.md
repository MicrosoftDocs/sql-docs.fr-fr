---
description: DBCC CHECKTABLE (Transact-SQL)
title: DBCC CHECKTABLE (Transact-SQL) | Microsoft Docs
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKTABLE_TSQL
- DBCC_CHECKTABLE_TSQL
- DBCC CHECKTABLE
- CHECKTABLE
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], DBCC CHECKTABLE
- page integrity checks [SQL Server]
- consistency [SQL Server], tables
- consistency [SQL Server], indexed views
- DBCC CHECKTABLE statement
- integrity [SQL Server]
- low overhead checks
- table integrity checks [SQL Server]
ms.assetid: 0d6cb620-eb58-4745-8587-4133a1b16994
author: pmasl
ms.author: umajay
ms.openlocfilehash: adf908dbd20532903ba8bcc40251419d0bb956d6
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233096"
---
# <a name="dbcc-checktable-transact-sql"></a>DBCC CHECKTABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Vérifie l'intégrité de toute les pages et structures qui composent la table ou la vue indexée.

![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
    
## <a name="syntax"></a>Syntaxe    
    
```syntaxsql
DBCC CHECKTABLE     
(    
    table_name | view_name    
    [ , { NOINDEX | index_id }    
     |, { REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD }     
    ]     
)    
    [ WITH     
        { [ ALL_ERRORMSGS ]    
          [ , EXTENDED_LOGICAL_CHECKS ]     
          [ , NO_INFOMSGS ]    
          [ , TABLOCK ]     
          [ , ESTIMATEONLY ]     
          [ , { PHYSICAL_ONLY | DATA_PURITY } ]     
          [ , MAXDOP = number_of_processors ]    
        }    
    ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *table_name* | *view_name*  
 Table ou vue indexée pour laquelle exécuter des vérifications d'intégrité. Les noms de table ou de vue doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
    
NOINDEX  
 Indique qu'il ne faut pas effectuer de vérifications intensives des index non cluster pour les tables utilisateur. Cela diminue la durée d'exécution globale. NOINDEX n'affecte pas les tables système car les contrôles d'intégrité sont toujours effectués sur tous les index des tables système.  
    
 *index_id*  
 Identificateur d'index pour lequel la vérification de l'intégrité est effectuée. Si *index_id* est spécifié, DBCC CHECKTABLE exécute les vérifications d’intégrité uniquement sur cet index, en même temps que le segment de mémoire ou l’index cluster.  
    
REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD  
 Spécifie que DBCC CHECKTABLE répare les erreurs trouvées. Pour utiliser une option de réparation, la base de données doit être en mode Utilisateur unique.  
    
REPAIR_ALLOW_DATA_LOSS  
 Tente de réparer toutes les erreurs signalées. Ces réparations peuvent entraîner des pertes de données.  
    
REPAIR_FAST  
 La syntaxe n'est conservée que pour la compatibilité descendante. Aucune réparation n'est effectuée.  
    
REPAIR_REBUILD  
 Effectue des réparations qui ne présentent aucun risque de perte de données. Cela peut inclure des réparations rapides, telles que la réparation de lignes manquantes dans des index non-cluster, ainsi que des réparations nécessitant plus de temps, telles que la reconstruction d’un index.  
 Cet argument ne répare pas les erreurs impliquant des données FILESTREAM.  
    
 > [!NOTE]  
 > N'utilisez les options REPAIR qu'en dernier recours. Pour réparer les erreurs, nous vous recommandons d'effectuer une restauration à partir d'une sauvegarde. Les opérations de réparation ne prennent en compte aucune des contraintes qui peuvent exister sur les tables ou entre tables. Si la table spécifiée est impliquée dans une ou plusieurs contraintes, nous vous recommandons d’exécuter `DBCC CHECKCONSTRAINTS` après une réparation.
 > Si vous devez utiliser REPAIR, exécutez `DBCC CHECKTABLE` sans option de réparation afin de déterminer le niveau de réparation à utiliser. Si vous envisagez d’utiliser le niveau REPAIR_ALLOW_DATA_LOSS, nous vous recommandons de sauvegarder la base de données avant d’exécuter `DBCC CHECKTABLE` avec cette option.  
    
ALL_ERRORMSGS  
 Affiche un nombre illimité d'erreurs. Tous les messages d'erreur sont affichés par défaut. La spécification ou non de cette option n'a aucun effet.  
    
EXTENDED_LOGICAL_CHECKS  
 Si le niveau de compatibilité est 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) ou supérieur, effectue des vérifications de cohérence logique sur une vue indexée, des index XML et des index spatiaux, là où ils sont présents.  
 Pour plus d’informations, consultez *Exécution de vérifications de cohérence logique sur des index* dans la section [Notes](#remarks), plus loin dans cette rubrique.  
    
NO_INFOMSGS  
 Supprime tous les messages d'information.  
    
TABLOCK  
 Fait obtenir à DBCC CHECKTABLE un verrou de table partagé plutôt que d'utiliser un instantané de base de données interne. TABLOCK accélère l'exécution de DBCC CHECKTABLE sur une table dont la charge est importante, tout en diminuant la concurrence disponible dans cette dernière pendant l'exécution de DBCC CHECKTABLE.  
    
ESTIMATEONLY  
 Affiche une estimation de la quantité d’espace tempdb nécessaire pour exécuter DBCC CHECKTABLE avec toutes les autres options spécifiées.  
    
PHYSICAL_ONLY  
 Limite la vérification à l'intégrité de la structure physique de la page, des en-têtes d'enregistrement et de la structure physique des arbres B (B-trees). Conçue pour effectuer un léger contrôle de la cohérence physique de la table, cette vérification peut également détecter les pages endommagées et les erreurs matérielles courantes susceptibles de compromettre les données. Une exécution complète de DBCC CHECKTABLE peut prendre beaucoup plus de temps que dans les versions antérieures. Ce comportement se produit pour les raisons suivantes :  
 -   Les vérifications logiques sont plus complètes.  
 -   Certaines des structures sous-jacentes à vérifier sont plus complexes.  
 -   De nombreuses vérifications nouvelles ont été introduites pour inclure les nouvelles fonctionnalités.  
   
Par conséquent, l'utilisation de l'option PHYSICAL_ONLY étant susceptible de réduire considérablement l'exécution de DBCC CHECKTABLE sur des tables volumineuses, elle est recommandée pour une utilisation fréquente sur des systèmes de production. Nous vous recommandons toutefois d'effectuer régulièrement une exécution complète de DBCC CHECKTABLE. La fréquence de ces exécutions dépend de facteurs spécifiques à chaque entreprise et à chaque environnement de production. L'option PHYSICAL_ONLY implique toujours NO_INFOMSGS et n'est autorisée avec aucune des options de réparation.  
    
 > [!NOTE]  
 > La spécification de PHYSICAL_ONLY fait que DBCC CHECKTABLE ignorera toutes les vérifications des données FILESTREAM.  
    
DATA_PURITY  
 Génère la vérification de la table par DBCC CHECKTABLE pour les valeurs de colonnes qui ne sont pas valides ou hors limite. Par exemple, DBCC CHECKTABLE détecte des colonnes dont les dates et les heures sont supérieures ou inférieures à la plage acceptable pour le type de données **datetime**. Cette commande identifie aussi des colonnes de type **decimal** ou numeric approximatif avec des valeurs d’échelle ou de précision qui ne sont pas valides.  
 Les vérifications d'intégrité sur la base colonne-valeur sont activées par défaut et ne nécessitent pas l'option DATA_PURITY. Pour les bases de données mises à niveau à partir des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez faire appel à DBCC CHECKTABLE WITH DATA_PURITY pour détecter et corriger les erreurs sur une table spécifique ; cependant, les vérifications sur la base colonne-valeur sur la table ne sont pas activées par défaut tant la commande DBCC CHECKDB WITH DATA_PURITY n'est pas exécutée sans erreur sur cette base de données. Ensuite, les commandes DBCC CHECKDB et DBCC CHECKTABLE vérifient l'intégrité sur la base colonne-valeur par défaut.  
 Les erreurs de validation signalées par cette option ne peuvent pas être corrigées à l'aide des options de réparation DBCC. Pour des informations sur la correction manuelle de ces erreurs, consultez l’article 923247 (en anglais) de la Base de connaissances : [Dépannage d’erreur DBCC 2570 dans SQL Server 2005 et versions ultérieures](https://support.microsoft.com/kb/923247).  
 Si PHYSICAL_ONLY est spécifié, l'intégrité des colonnes n'est pas vérifiée.  
    
MAXDOP  
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 et versions ultérieures).  
 
 Remplace l’option de configuration **max degree of parallelism** de **sp_configure** pour l’instruction. MAXDOP peut dépasser la valeur configurée avec sp_configure. Si MAXDOP dépasse la valeur configurée avec Resource Governor, le moteur de base de données utilise la valeur MAXDOP de Resource Governor, décrite dans ALTER WORKLOAD GROUP (Transact-SQL). Toutes les règles sémantiques utilisées avec l'option de configuration max degree of parallelism sont applicables lorsque vous utilisez l'indicateur de requête MAXDOP. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
    
 > [!NOTE]  
 > Si MAXDOP est défini avec la valeur zéro, le serveur choisit le degré maximal de parallélisme.  
    
## <a name="remarks"></a>Notes    
    
> [!NOTE]    
> Pour exécuter DBCC CHECKTABLE sur chaque table de la base de données, vous devez utiliser [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).    
    
Pour la table spécifiée, DBCC CHECKTABLE vérifie les points suivants :
-   si les pages de données d'index, dans la ligne, LOB et de dépassement de capacité de ligne sont correctement liées ;    
-   l'ordre de tri des index est correct ;    
-   les pointeurs sont cohérents ;    
-   chaque page contient une quantité raisonnable de données, y compris les colonnes calculées ;    
-   les décalages de page sont acceptables ;    
-   chaque ligne de la table de base possède une ligne correspondante dans chaque index non-cluster, et vice-versa ;    
-   chaque ligne d'une table ou d'un index partitionné figure dans la partition correcte.    
-   la cohérence au niveau du lien entre le système de fichiers et la table lors du stockage de données **varbinary(max)** dans le système de fichiers à l’aide de FILESTREAM.    
    
## <a name="performing-logical-consistency-checks-on-indexes"></a>Exécution de vérifications de cohérence logique sur des index    
La vérification de la cohérence logique sur les index varie selon le niveau de compatibilité de la base de données, comme suit :
-   Si le niveau de compatibilité est égal à 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) ou supérieur :    
    -   À moins que l'option NOINDEX soit spécifiée, DBCC CHECKTABLE effectue des vérifications de cohérence physique et logique sur une table individuelle et sur tous ses index non-cluster. Toutefois, seules des vérifications de cohérence physique sont effectuées par défaut sur les index XML, les index spatiaux et les vues indexées.    
    -   Si WITH EXTENDED_LOGICAL_CHECKS est spécifié, des vérifications logiques sont effectués sur une vue indexée, des index XML et des index spatiaux, là où ils sont présents. Par défaut, les vérifications de cohérence physique sont effectuées avant les vérifications de cohérence logique. Si l'option NOINDEX est également spécifiée, seules les vérifications logiques sont effectuées.    
         Ces vérifications de cohérence logique effectuent une vérification croisée de la table d'index interne de l'objet d'index avec la table utilisateur à laquelle il fait référence. Pour rechercher les lignes excentrées, une requête interne est construite pour effectuer l'intersection complète de la table interne et de la table utilisateur. L'exécution de cette requête peut avoir un effet très important sur les performances et il n'est pas possible de suivre sa progression. Par conséquent, nous vous recommandons de spécifier WITH EXTENDED_LOGICAL_CHECKS seulement si vous soupçonnez des problèmes d'index qui ne sont pas liés à une altération physique ou si les sommes de contrôle au niveau de la page ont été désactivées et que vous soupçonnez un endommagement matériel au niveau des colonnes.    
    -   Si l'index est un index filtré, DBCC CHECKDB effectue des vérifications de cohérence pour vérifier que les entrées de l'index satisfont le prédicat du filtre.   
      
- À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], les vérifications supplémentaires sur les colonnes calculées persistantes, les colonnes UDT et les index filtrés ne seront pas exécutées par défaut afin d’éviter les évaluations d’expressions coûteuses. Cette modification réduit considérablement la durée de CHECKDB sur les bases de données contenant ces objets. Cependant, les vérifications de cohérence physique de ces objets sont toujours effectuées. Les évaluations d’expressions ne seront effectuées en plus des vérifications logiques déjà présentes (vue indexée, index XML et index spatiaux) dans le cadre de l’option EXTENDED_LOGICAL_CHECKS que quand l’option EXTENDED_LOGICAL_CHECKS est spécifiée.
-  Si le niveau de compatibilité est inférieur ou égal à 90 ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]), à moins que l’option NOINDEX ne soit spécifiée, DBCC CHECKTABLE effectue des vérifications de cohérence à la fois physique et logique sur une seule table ou vue indexée et sur tous ses index non cluster et XML. Les index spatiaux ne sont pas pris en charge.
    
 **Pour connaître le niveau de compatibilité d’une base de données**    
[Afficher ou modifier le niveau de compatibilité d’une base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)    
    
## <a name="internal-database-snapshot"></a>Instantané de base de données interne    
L'instruction DBCC CHECKTABLE utilise un instantané de base de données interne pour fournir la cohérence transactionnelle dont elle a besoin pour effectuer ces vérifications. Pour plus d’informations, consultez [Afficher la taille du fichier partiellement alloué d’un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) et la section « Utilisation de l’instantané de base de données interne DBCC » dans [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md).
S'il est impossible de créer un instantané, ou si TABLOCK est spécifié, la commande DBCC CHECKTABLE acquiert un verrou de table partagé pour obtenir la cohérence nécessaire.
    
> [!NOTE]    
> Si l’instruction DBCC CHECKTABLE est exécutée sur tempdb, elle doit acquérir un verrou de table partagé. En effet, pour des raisons de performances, les instantanés de base de données ne sont pas disponibles sur tempdb. Cela signifie que la cohérence transactionnelle requise ne peut pas être obtenue.    
    
## <a name="checking-and-repairing-filestream-data"></a>Vérification et réparation des données FILESTREAM    
Quand FILESTREAM est activé pour une base de données et une table, vous pouvez éventuellement stocker des objets BLOB(Binary Large Object) **varbinary(max)** dans le système de fichiers. Lorsque vous utilisez DBCC CHECKTABLE sur une table qui stocke des objets BLOB dans le système de fichiers, DBCC vérifie la cohérence au niveau du lien entre le système de fichiers et la base de données.
Par exemple, si une table contient une colonne **varbinary(max)** qui utilise l’attribut FILESTREAM, DBCC CHECKTABLE vérifiera qu’il existe un mappage un-à-un entre les répertoires et les fichiers du système de fichiers et les lignes, les colonnes et les valeurs de colonne de la table. DBCC CHECKTABLE peut réparer l'altération si vous spécifiez l'option REPAIR_ALLOW_DATA_LOSS. Pour réparer l'altération FILESTREAM, DBCC supprimera toutes les lignes de la table auxquelles il manque des données du système de fichiers et supprimera tous les répertoires et les fichiers qui ne sont pas mappés à une ligne, à une colonne ni à une valeur de colonne de la table.
    
## <a name="checking-objects-in-parallel"></a>Vérification des objets en parallèle    
DBCC CHECKTABLE effectue par défaut une vérification parallèle des objets. Le degré de parallélisme est automatiquement défini par le processeur de requêtes. Le degré maximal de parallélisme est configuré de la même manière que celui des requêtes parallèles. Pour limiter le nombre maximal de processeurs disponibles pour la vérification DBCC, utilisez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md). Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
La vérification parallèle peut être désactivée à l'aide de l'indicateur de trace 2528. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
    
> [!NOTE]    
> Pendant une opération DBCC CHECKTABLE, les octets stockés dans une colonne de type défini par l'utilisateur ordonné par octet doivent être identiques à la sérialisation calculée de la valeur du type défini par l'utilisateur. Dans le cas contraire, la routine DBCC CHECKTABLE signalera une erreur de cohérence.    
    
## <a name="understanding-dbcc-error-messages"></a>Présentation des messages d'erreur de DBCC    
Une fois la commande DBCC CHECKTABLE exécutée, un message est consigné dans le journal d'erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si la commande DBCC est correctement exécutée, le message indique que l'exécution a réussi, ainsi que la durée d'exécution de la commande. Si la commande DBCC est interrompue avant la fin de la vérification en raison d’une erreur, le message indique que la commande n’a pas abouti et précise une valeur d’état ainsi que la durée d’exécution de la commande. Le tableau suivant répertorie et décrit les valeurs d'état pouvant être incluses dans le message.
    
|State|Description|    
|-----------|-----------------|    
|0|Erreur numéro 8930 générée. Ceci indique une corruption des métadonnées qui a provoqué l'arrêt de la commande DBCC.|    
|1|Erreur numéro 8967 générée. Une erreur DBCC interne s'est produite.|    
|2|Une erreur s'est produite lors de la réparation de la base de données en mode urgence.|    
|3|Ceci indique une corruption des métadonnées qui a provoqué l'arrêt de la commande DBCC.|    
|4|Une assertion ou une violation d'accès a été détectée.|    
|5|Une erreur inconnue s'est produite et a arrêté la commande DBCC.|    
    
## <a name="error-reporting"></a>Rapport d'erreurs    
Un fichier minidump (`SQLDUMP*nnnn*.txt`) est créé dans le répertoire LOG de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chaque fois que DBCC CHECKTABLE détecte une erreur d’endommagement. Quand les fonctionnalités *Rapport d’erreurs* et de collecte des données *Utilisation de fonctionnalités* sont activées pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce fichier est automatiquement transféré à [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Les données collectées sont utilisées pour améliorer les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Le fichier de vidage contient les résultats de la commande DBCC CHECKTABLE ainsi que des informations de diagnostic supplémentaires. Ce fichier contient des listes de contrôle d'accès discrétionnaire (DACL, Discretionary Access Control Lists) avec accès restreint. L’accès est limité au compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux membres du rôle sysadmin. Par défaut, le rôle sysadmin contient tous les membres du groupe Windows BUILTIN\Administrators et du groupe Administrateurs local. La commande DBCC n'échoue pas si le processus de collecte des données échoue.
    
## <a name="resolving-errors"></a>Résolution des erreurs    
 Si DBCC CHECKTABLE signale des erreurs, il est recommandé de restaurer la base de données à partir de la sauvegarde plutôt que d'exécuter REPAIR avec l'une des options REPAIR. S'il n'existe aucune sauvegarde, l'exécution de REPAIR peut corriger les erreurs qui sont signalées. L'option REPAIR à utiliser est spécifiée à la fin de la liste des erreurs signalées. Cependant, la correction des erreurs à l'aide de l'option REPAIR_ALLOW_DATA_LOSS peut nécessiter la suppression de certaines pages et, par conséquent, de certaines données.    
La réparation peut être effectuée dans une transaction utilisateur pour permettre à celui-ci d'annuler les modifications effectuées. Si des réparations sont restaurées, la base de données contiendra encore des erreurs et il faudra donc la restaurer à partir d'une sauvegarde. Une fois toutes les réparations effectuées, sauvegardez la base de données.
    
## <a name="result-sets"></a>Jeux de résultats    
L'instruction DBCC CHECKTABLE retourne le jeu de résultats suivant. Le même jeu de résultats est retourné si vous spécifiez uniquement le nom de la table ou l'une des options.
    
```
DBCC results for 'HumanResources.Employee'.    
There are 288 rows in 13 pages for object 'Employee'.    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
DBCC CHECKTABLE retourne le jeu de résultats suivant si l'option ESTIMATEONLY est spécifiée :
    
```
Estimated TEMPDB space needed for CHECKTABLES (KB)     
--------------------------------------------------     
21    
(1 row(s) affected)    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
## <a name="permissions"></a>Autorisations    
L’utilisateur doit être propriétaire de la table ou être membre du rôle serveur fixe sysadmin, du rôle de base de données fixe db_owner ou du rôle de base de données fixe db_ddladmin.    
    
## <a name="examples"></a>Exemples    
    
### <a name="a-checking-a-specific-table"></a>R. Vérification d'une table spécifique    
L’exemple suivant vérifie l’intégrité des pages de données de la table `HumanResources.Employee` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee');    
GO    
```    
    
### <a name="b-performing-a-low-overhead-check-of-the-table"></a>B. Exécution d'une vérification à faible charge d'une table    
 L’exemple suivant effectue une vérification de faible charge de la table `Employee` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].    
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee') WITH PHYSICAL_ONLY;    
GO    
```    
    
### <a name="c-checking-a-specific-index"></a>C. Vérification d'un index spécifique    
 Cet exemple contrôle un index spécifique obtenu lors de l'accès à `sys.indexes`.    
    
```sql    
DECLARE @indid int;    
SET @indid = (SELECT index_id     
              FROM sys.indexes    
              WHERE object_id = OBJECT_ID('Production.Product')    
                    AND name = 'AK_Product_Name');    
DBCC CHECKTABLE ('Production.Product',@indid);    
```    
    
## <a name="see-also"></a>Voir aussi    
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)     
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)    
    
  
