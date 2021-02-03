---
title: Vue d’ensemble de la restauration et de la récupération (SQL Server) | Microsoft Docs
description: Cet article est une vue d’ensemble des opérations qu’implique la récupération d’une base de données SQL Server après une défaillance en procédant à la restauration d’un ensemble de sauvegardes SQL Server à la suite.
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring tables [SQL Server]
- backups [SQL Server], restore scenarios
- database backups [SQL Server], restore scenarios
- database restores [SQL Server]
- restoring [SQL Server]
- restores [SQL Server]
- table restores [SQL Server]
- restoring databases [SQL Server], about restoring databases
- database restores [SQL Server], scenarios
- accelerated database recovery
ms.assetid: e985c9a6-4230-4087-9fdb-de8571ba5a5f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 72bf9910a86d66ac36cbebbb0be66f8a8df832e5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235765"
---
# <a name="restore-and-recovery-overview-sql-server"></a>Vue d'ensemble de la restauration et de la récupération (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Pour récupérer une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suite à une erreur, un administrateur de base de données doit restaurer un jeu de sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans une séquence de restauration correcte du point de vue logique et explicite. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la restauration de données à partir de sauvegardes d'une base de données entière, d'un fichier de données ou d'une page de données, comme suit :  
  
-   La base de données ( *restauration de base de données complète*)  
  
     La base de données complète est restaurée et récupérée, et la base de données est hors connexion pendant la durée des opérations de restauration et de récupération.  
  
-   Le fichier de données ( *restauration de fichiers*)  
  
     Un fichier de données ou un ensemble de fichiers est restauré et récupéré. Au cours d'une restauration de fichiers, les groupes de fichiers contenant les fichiers sont mis automatiquement hors connexion pendant la durée de la restauration. Toute tentative d'accès à un groupe de fichiers hors connexion produit une erreur.  
  
-   La page de données ( *restauration de pages*)  
  
     En mode de restauration complète ou de récupération utilisant les journaux de transactions, vous pouvez restaurer des bases de données individuelles. La restauration des pages peut être effectuée pour n'importe quelle base de données, quel que soit le nombre de groupes de fichiers.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La sauvegarde et la restauration fonctionnent sur tous les systèmes d’exploitation pris en charge. Pour plus d’informations sur les systèmes d’exploitation pris en charge, consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). Pour plus d’informations sur la prise en charge de sauvegardes provenant de versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez la section « Prise en charge de la compatibilité » de [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
##  <a name="overview-of-restore-scenarios"></a><a name="RestoreScenariosOv"></a> Présentation des scénarios de restauration  
 Un *scénario de restauration* dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est le processus de restauration des données à partir d'une ou de plusieurs sauvegardes, puis de récupération de la base de données. Les scénarios de restauration pris en charge dépendent du mode de récupération de la base de données et de l'édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le tableau suivant présente les scénarios de restauration possibles pris en charge pour différents modes de récupération.  
  
|scénario de restauration|En mode de récupération simple|En modes de restauration complète et de récupération utilisant les journaux de transactions|  
|----------------------|---------------------------------|----------------------------------------------|  
|restauration de base de données complète|Il s'agit de la stratégie de restauration de base. Une restauration complète de base de données peut impliquer simplement la restauration et la récupération d'une sauvegarde complète de base de données. Une restauration complète de base de données peut également impliquer la restauration d'une sauvegarde complète de base de données suivie de la restauration et de la récupération d'une sauvegarde différentielle.<br /><br /> Pour plus d’informations, consultez [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md).|Il s'agit de la stratégie de restauration de base. Une restauration complète de base de données inclut la restauration d'une sauvegarde complète, et éventuellement d'une sauvegarde différentielle (le cas échéant), suivie par la restauration de toutes les sauvegardes de journal consécutives (en séquence). La restauration de base de données complète s'achève par la récupération de la dernière sauvegarde de journal ainsi que sa restauration (RESTORE WITH RECOVERY).<br /><br /> Pour plus d’informations, consultez [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).|  
|Restauration de fichiers * *\** _|Restaure un ou plusieurs fichiers endommagés en lecture seule, sans restaurer toute la base de données. La restauration de fichiers est uniquement disponible si la base de données comporte au moins un groupe de fichiers en lecture seule.|Restaure un ou plusieurs fichiers, sans restaurer la totalité de la base de données. La restauration de fichiers peut être effectuée lorsque la base de données est hors connexion ou, pour certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], alors que la base de données reste en ligne. Pendant une restauration de fichiers, les groupes de fichiers contenant les fichiers à restaurer restent toujours hors connexion.|  
|Restauration de pages|Non applicable|Restaure une ou plusieurs pages endommagées. La restauration de pages peut être effectuée lorsque la base de données est hors connexion ou, pour certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], alors que la base de données reste en ligne. Pendant une restauration de pages, les pages en cours de restauration restent toujours hors connexion.<br /><br /> Une chaîne ininterrompue de sauvegardes de journaux doit être disponible, jusqu'au fichier journal actuel, et elles doivent toutes être appliquées pour mettre la page à jour par rapport au fichier journal actuel.<br /><br /> Pour plus d’informations, consultez [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).|  
|Restauration fragmentaire _*\**_|Restaure et récupère la base de données par phases au niveau du groupe de fichiers, en commençant par les groupes de fichiers primaires et tous les groupes de fichiers secondaires en lecture-écriture.|Restaure et récupère la base de données par étapes au niveau du groupe de fichiers, en commençant par le groupe de fichiers primaire.<br /><br /> Pour plus d’informations, consultez [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)|  
  
 _*\**_ La restauration en ligne est prise en charge uniquement dans l’édition Enterprise.  

### <a name="steps-to-restore-a-database"></a>Étapes de restauration d’une base de données
Pour effectuer une restauration de fichiers, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] exécute deux étapes : 

-   Création du ou des fichiers de base de données manquants.

-   Copie des données des périphériques de sauvegarde dans le ou les fichiers de base de données.

Pour effectuer une restauration de base de données, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] exécute trois étapes : 

-   Création des fichiers de base de données et du journal des transactions s'ils n'existent pas déjà.

-   Copie de toutes les données, du journal et des pages d'index des supports de sauvegarde d'une base de données vers les fichiers de base de données. 

-   Application du journal des transactions dans ce qui est connu sous le nom de [processus de récupération](#TlogAndRecovery).

 Indépendamment du mode de restauration des données, avant de pouvoir récupérer une base de données, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] garantit la cohérence logique de l'intégralité de la base de données. Par exemple, si vous restaurez un fichier, vous ne pouvez pas le récupérer et le mettre en ligne tant qu'il n'a pas été restauré par progression suffisamment pour être cohérent avec la base de données.  
  
### <a name="advantages-of-a-file-or-page-restore"></a>Avantages de la restauration d'un fichier ou d'une page  
 Restaurer et récupérer des fichiers ou des pages plutôt que la base de données entière offre plusieurs avantages :  
  
-   Le fait de restaurer moins de données réduit le temps nécessaire pour les copier et les récupérer.  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la restauration des pages ou des fichiers peut permettre à d'autres données de la base de données de rester en ligne pendant l'opération de restauration.  

## <a name="recovery-and-the-transaction-log"></a><a name="TlogAndRecovery"></a> Récupération et journal des transactions
Pour la plupart des scénarios de restauration, il est nécessaire d’appliquer une sauvegarde du journal des transactions et de permettre à [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] d’exécuter le _ *processus de récupération** pour que la base de données soit mise en ligne. La récupération est le processus que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise pour chaque base de données pour démarrer dans un état cohérent (ou propre) en termes de transaction.

En cas de défaillance ou autre arrêt non sain, il peut arriver que certaines modifications effectuées dans les bases de données n’aient jamais pu être écrites de la mémoire tampon vers les fichiers de données ou proviennent de transactions incomplètes dans les fichiers de données. Lorsqu’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est démarrée, elle exécute une récupération de chaque base de données, ce qui comporte trois phases, en fonction du dernier [point de contrôle de base de données](../../relational-databases/logs/database-checkpoints-sql-server.md) :

-   La **phase d’analyse** analyse le journal des transactions pour déterminer quel est le dernier point de contrôle, et crée les tables DPT (Dirty Page Table) et ATT (Active Transaction Table). La table DPT contient les enregistrements des pages qui étaient incorrectes au moment de l’arrêt de la base de données. La table ATT contient les enregistrements des transactions qui étaient actives au moment de l’arrêt non sain de la base de données.

-   La **phase de restauration** restaure par progression toutes les modifications enregistrées dans le journal qui n’ont peut-être pas été écrites dans les fichiers de données au moment où la base de données a été arrêtée. Le [numéro séquentiel dans le journal minimal](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#minlsn) (minLSN) requis pour une récupération réussie à l’échelle de la base de données se trouve dans la table DPT et marque le début des opérations de restauration nécessaires sur toutes les pages incorrectes. À ce stade, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] écrit sur le disque toutes les pages incorrectes appartenant aux transactions validées.

-   La **phase d’annulation** restaure les transactions incomplètes trouvée dans la table ATT afin de préserver l'intégrité de la base de données. Après la restauration, la base de données passe en ligne, et aucune autre sauvegarde du journal des transactions ne peut être appliquée à la base de données.

Les informations sur la progression de chaque phase de la récupération de la base de données sont consignées dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [journal des erreurs](../../tools/configuration-manager/viewing-the-sql-server-error-log.md). La progression de la récupération de la base de données peut également être suivie via Événements étendus. Pour plus d’informations, consultez le billet de blog [New extended events for database recovery progress](/archive/blogs/sql_server_team/new-extended-events-for-database-recovery-progress) (Nouveaux événements étendus pour la progression de la récupération de la base de données).

> [!NOTE]
> Dans le cas d’un scénario de restauration fragmentaire, si un groupe de fichiers est en lecture seule avant la création de la sauvegarde de fichiers, l'application des sauvegardes de journaux au groupe de fichiers n'est pas nécessaire et n'est pas effectuée par la restauration de fichiers. 

<a name="FastRecovery"></a>
> [!NOTE]
> Pour optimiser la disponibilité des bases de données dans un environnement d’entreprise, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Édition Entreprise peut mettre une base de données en ligne après la phase de restauration, tandis que la phase d’annulation est toujours en cours d’exécution. Cela s’appelle la récupération rapide.

##  <a name="recovery-models-and-supported-restore-operations"></a><a name="RMsAndSupportedRestoreOps"></a> Modes de récupération et opérations de restauration prises en charge  
 Les opérations de restauration disponibles pour une base de données dépendent de son mode de récupération. Le tableau suivant présente le niveau de prise en charge des modes de récupération dans un scénario de restauration donné.  
  
|Opération de restauration|Mode de restauration complète|Mode de récupération utilisant les journaux de transactions|Mode de récupération simple|  
|-----------------------|-------------------------|---------------------------------|---------------------------|  
|Récupération de données|Récupération complète (si le journal est disponible).|Risque de perte de données.|Les données postérieures à la dernière sauvegarde différentielle ou complète sont perdues.|  
|Restauration dans le temps|Toute heure couverte par les sauvegardes de fichiers journaux.|Non autorisé si la sauvegarde de fichier journal contient des modifications journalisées en bloc.|Non pris en charge.|  
|Restauration de fichiers * *\** _|Prise en charge complète.|Parfois._ *\*\** *|Disponible uniquement pour les fichiers secondaires en lecture seule.|  
|Restauration de pages * *\** _|Prise en charge complète.|Parfois._ *\*\** *|Aucun.|  
|Restauration fragmentaire (niveau groupe de fichiers) * *\** _|Prise en charge complète.|Parfois._ *\*\** *|Disponible uniquement pour les fichiers secondaires en lecture seule.|  
  
 * *\** _ Disponible uniquement dans l’édition Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 _ *\*\** * Pour les conditions requises, consultez [Restrictions de restauration en mode de récupération simple](#RMsimpleScenarios), plus loin dans cette rubrique.  
  
> [!IMPORTANT]  
> Quel que soit le mode de récupération d'une base de données, une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être restaurée vers une version de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] antérieure à la version qui a créé la sauvegarde.  
  
## <a name="restore-scenarios-under-the-simple-recovery-model"></a><a name="RMsimpleScenarios"></a> Restrictions de restauration en mode de récupération simple  
 Le mode de récupération simple impose les restrictions suivantes aux opérations de restauration :  
  
-   La restauration de fichiers et la restauration fragmentaire ne s'adressent qu'aux groupes de fichiers secondaires en lecture seule. Pour plus d’informations sur ces scénarios de restauration, consultez [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md) et [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
-   La restauration de pages n'est pas autorisée.  
  
-   La restauration dans le temps n'est pas autorisée.  
  
 Si ces restrictions ne correspondent pas à vos besoins de récupération, nous vous recommandons d'envisager le mode de restauration complète. Pour plus d’informations, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
> [!IMPORTANT]  
> Quel que soit le mode de récupération d'une base de données, une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être restaurée par une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à la version qui a créé la sauvegarde.  
  
##  <a name="restore-under-the-bulk-logged-recovery-model"></a><a name="RMblogRestore"></a> Restauration en mode de récupération utilisant les journaux de transactions  
 Cette section traite de considérations relatives à la restauration qui sont propres au mode de récupération utilisant les journaux de transactions et qui vient en complément d'une utilisation exclusive du mode de restauration complète.  
  
> [!NOTE]  
> Pour obtenir une présentation du mode de récupération utilisant les journaux de transactions, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 Généralement, le mode de récupération utilisant les journaux de transactions est comparable au mode de restauration complète. En outre, les informations fournies pour ce dernier valent aussi pour le premier. Cependant, la récupération jusqu'à une date et heure et la restauration en ligne sont concernées par le mode de récupération utilisant les journaux de transactions.  
  
### <a name="restrictions-for-point-in-time-recovery"></a>Restrictions relatives à la récupération jusqu'à une date et heure  
 Si une sauvegarde du journal effectuée dans le mode de récupération utilisant les journaux de transactions contient des modifications journalisées en bloc, la récupération jusqu'à une date et heure n'est pas autorisée. Si vous tentez d'effectuer une récupération jusqu'à une date et heure sur une sauvegarde du journal contenant des modifications en bloc, l'opération de restauration peut échouer.  
  
### <a name="restrictions-for-online-restore"></a>Restrictions relatives à la restauration en ligne  
 Une séquence de restauration en ligne fonctionne uniquement si les conditions suivantes sont satisfaites :  
  
-   Toutes les sauvegardes de journaux nécessaires doivent avoir été effectuées avant le démarrage de la séquence de restauration.  
  
-   Les modifications en bloc doivent avoir été sauvegardées avant de démarrer la séquence de restauration en ligne.  
  
-   Si des modifications en bloc existent dans la base de données, tous les fichiers doivent être en ligne ou [obsolètes](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md). (Cela signifie qu'ils n'appartiennent plus à la base de données.)  
  
 Si ces conditions ne sont pas satisfaites, la séquence de restauration en ligne échoue.  
  
> [!NOTE]  
>  Il est recommandé de basculer en mode de restauration complète avant de démarrer la restauration en ligne. Pour plus d’informations, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 Pour plus d’informations sur l’exécution d’une restauration en ligne, consultez [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
##  <a name="database-recovery-advisor-sql-server-management-studio"></a><a name="DRA"></a> Assistant de récupération de base de données (SQL Server Management Studio)  
L'Assistant Récupération de base de données permet de créer des plans de restauration qui implémentent des séquences de restauration correctes et optimales. De nombreux problèmes connus, liés à la restauration de la base de données, et améliorations demandées par les clients ont été pris en considération. Les améliorations importantes introduites par l'Assistant Récupération de base de données sont les suivantes :  
  
-   **Algorithme de plan de restauration :**  l’algorithme utilisé pour créer des plans de restauration a été amélioré considérablement, en particulier pour les scénarios de restauration complexes. Nombre de cas limites, notamment la réplication de scénarios dans les restaurations ponctuelles, sont gérés plus efficacement que dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **Restaurations dans le temps :**  l’Assistant Récupération de base de données simplifie considérablement la restauration d’une base de données à un moment donné. Une chronologie visuelle de sauvegarde améliore considérablement la prise en charge des restaurations dans le temps. La chronologie visuelle vous permet d'identifier un point possible comme point de récupération cible pour restaurer une base de données. La chronologie permet de parcourir un chemin de récupération ramifié (un chemin d'accès qui couvre les branchements de récupération). Un plan spécifique de restauration dans le temps inclut automatiquement les sauvegardes pertinentes pour la restauration à un point cible (date et heure). Pour plus d’informations, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
Pour plus d'informations sur l'Assistant Récupération de base de données, consultez les blogs de gestion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivants :  
  
-   [Assistant Récupération : introduction](/archive/blogs/managingsql/recovery-advisor-an-introduction)  
  
-   [Assistant Récupération : utilisation de SSMS pour créer/restaurer des sauvegardes fractionnées](/archive/blogs/managingsql/recovery-advisor-using-ssms-to-createrestore-split-backups)  

## <a name="accelerated-database-recovery"></a><a name="adr"></a> Récupération de base de données accélérée
[La récupération de base de données accélérée](/azure/sql-database/sql-database-accelerated-database-recovery/) est disponible dans [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. La récupération de base de données accélérée améliore considérablement la disponibilité des bases de données, notamment en présence de transactions durables, en redéfinissant le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [processus de récupération](#TlogAndRecovery). Une base de données pour laquelle la récupération de base de données accélérée a été activée termine le processus de récupération beaucoup plus rapidement après un basculement ou tout autre arrêt qui n’est pas propre. Lorsqu’elle est activée, la récupération de base de données accélérée effectue également la restauration des transactions longues annulées beaucoup plus rapidement.

La récupération de base de données accélérée peut être activée par base de données sur [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] à l’aide de la syntaxe suivante :

```sql
ALTER DATABASE <db_name> SET ACCELERATED_DATABASE_RECOVERY = ON;
```

> [!NOTE]
> La récupération de base de données accélérée est activée par défaut sur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

## <a name="see-also"></a><a name="RelatedContent"></a> Voir aussi  
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)      
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)     
 [Guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)     
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)     
 [Appliquer les sauvegardes du journal de transactions (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)