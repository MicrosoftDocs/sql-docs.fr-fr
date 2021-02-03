---
description: Guide d’architecture et gestion du journal des transactions SQL Server
title: Guide de gestion et d’architecture du journal des transactions
ms.custom: ''
ms.date: 10/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- transaction log architecture guide
- guide, transaction log architecture
- vlf
- transaction log guidance
- vlfs
- virtual log files
- virtual log size
- vlf size
- transaction log internals
ms.assetid: 88b22f65-ee01-459c-8800-bcf052df958a
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed532d22800cd10530ecc7db8595fcf75e3737da
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237940"
---
# <a name="sql-server-transaction-log-architecture-and-management-guide"></a>Guide d’architecture et gestion du journal des transactions SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Chaque base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possède un journal des transactions qui enregistre toutes les transactions et les modifications apportées par chacune d'entre elles. Le journal des transactions est un composant essentiel de la base de données et, en cas de défaillance du système, vous pouvez en avoir besoin pour rétablir la cohérence de la base de données. Ce guide contient des informations sur l'architecture physique et logique du journal des transactions. Ces informations pourront vous aider à gérer plus efficacement les journaux des transactions.  

  
##  <a name="transaction-log-logical-architecture"></a><a name="Logical_Arch"></a> Architecture logique du journal des transactions  
 Le journal des transactions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fonctionne de façon logique comme s'il s'agissait d'une chaîne d'enregistrements de journal. Chacun de ces enregistrements est identifié par un numéro séquentiel dans le journal (LSN). Chaque nouvel enregistrement est écrit à la fin logique du journal avec un LSN supérieur à celui de l'enregistrement qui le précède. Les enregistrements de journal sont stockés dans une séquence en série à mesure qu’ils sont créés de sorte que si LSN2 est supérieur à LSN1, la modification décrite par l'enregistrement de journal référencé par LSN2 se produit après la modification décrite par le numéro LSN1 d'enregistrement de journal. Chacun d'eux contient l'ID de la transaction à laquelle il appartient. Pour chaque transaction, tous les enregistrements de journal associés sont reliés de façon individuelle dans une chaîne grâce aux pointeurs arrière qui accélèrent la restauration de la transaction.  
  
 Les enregistrements de journal relatifs aux modifications de données consignent soit l'opération logique effectuée, soit les images avant/après des données modifiées. L'image avant est une copie des données avant que l'opération n'ait été effectuée, tandis que l'image après est une copie des données après que l'opération a été effectuée.  
  
Les étapes pour récupérer une opération dépendent du type de journal d'enregistrement :  
  
-   Opération logique enregistrée  
  
    -   Si vous repositionnez l'opération logique avant, elle est effectuée à nouveau.  
  
    -   Si vous annulez l'opération logique, l'opération inverse est effectuée.  
  
-   Image avant et après enregistrées  
  
    -   Si vous repositionnez l'opération avant, l'image après est appliquée.  
  
    -   Si vous annulez l'opération, l'image avant est appliquée.  
  
De nombreux types d'opérations sont enregistrés dans le journal des transactions. Ces opérations comprennent :  
  
-   Le début et la fin de chaque transaction.  
  
-   Chaque modification de données (insertion, mise à jour ou suppression). Il s'agit des modifications apportées aux tables, y compris les tables système, par les procédures stockées système ou les instructions DDL (Data Definition Language).  
  
-   Chaque allocation ou désallocation de page et d'étendue.  
  
-   Création ou suppression d'une table ou d'un index.  
  
 Les opérations de restauration sont également consignées dans le journal. Chaque transaction réserve de l'espace dans le journal des transactions afin qu'il existe suffisamment d'espace journal pour prendre en charge une restauration déclenchée par une instruction de restauration explicite ou par la détection d'une erreur. Le volume d'espace réservé dépend des opérations effectuées dans la transaction, mais il est généralement égal au volume d'espace utilisé pour la journalisation de chaque opération. Cet espace réservé est libéré lorsque la transaction est terminée.  
  
<a name="minlsn"></a> La section du fichier journal comprise entre le premier enregistrement de journal nécessaire à une restauration portant sur l’ensemble de la base de données et la fin du journal représente la partie active du journal, le *journal actif* ou la *fin du journal*. Cette section est indispensable pour procéder à une [récupération](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery) complète de la base de données. Aucune partie de ce journal actif ne peut être tronquée. Le LSN (numéro séquentiel dans le journal) de ce premier enregistrement est le **LSN de récupération minimum (*MinLSN*)** . Pour plus d’informations sur les opérations prises en charge par le journal des transactions, consultez [Journal des transactions (SQL Server)](../relational-databases/logs/the-transaction-log-sql-server.md).  

Les sauvegardes différentielles et de journaux font passer la base de données restaurée à une date ultérieure qui correspond à un numéro LSN supérieur. 
  
##  <a name="transaction-log-physical-architecture"></a><a name="physical_arch"></a> Architecture physique du journal des transactions  
Le journal des transactions d'une base de données s'étend sur un ou plusieurs fichiers physiques. D'un point de vue conceptuel, le fichier journal est une chaîne d'enregistrements. D'un point de vue physique, la séquence des enregistrements du journal est stockée de façon efficace dans l'ensemble de fichiers physiques qui implémente le journal des transactions. Chaque base de données doit posséder au moins un fichier journal.  
  
Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] divise chaque fichier journal physique en un certain nombre de fichiers journaux virtuels. La taille et le nombre de ces fichiers journaux virtuels sont variables. Le [!INCLUDE[ssDE](../includes/ssde-md.md)] choisit dynamiquement la taille des fichiers journaux virtuels en créant ou en étendant des fichiers journaux. Le [!INCLUDE[ssDE](../includes/ssde-md.md)] essaie de ne conserver qu’un petit nombre de fichiers virtuels. Après une extension du fichier journal, la taille des fichiers virtuels est la somme de la taille du journal existant et de la taille du nouvel incrément de fichier. La taille et le nombre des fichiers journaux virtuels ne peuvent être ni configurés, ni définis par les administrateurs.  

> [!NOTE]
> La création du fichier journal virtuel suit cette méthode :
> - Si la croissance suivante est inférieure à 1/8 de la taille physique actuelle du journal, 1 fichier journal virtuel qui couvre la taille de croissance est créé (à partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)])
> - Si la croissance suivante est supérieure à 1/8 de la taille actuelle du journal, utilisez la méthode antérieure à 2014 :
>    -  Si la croissance est inférieure à 64 Mo, 4 fichiers journaux virtuels qui couvrent la taille de croissance sont créés (par exemple, pour une croissance de 1 Mo, quatre fichiers journaux virtuels de 256 Ko sont créés)
>    -  Si la croissance se situe entre 64 Mo et 1 Go, 8 fichiers journaux virtuels qui couvrent la taille de croissance sont créés (par exemple, pour une croissance de 512 Mo, huit fichiers journaux virtuels de 64 Mo sont créés)
>    -  Si la croissance est supérieure à 1 Go, 16 fichiers journaux virtuels qui couvrent la taille de croissance sont créés (par exemple, pour une croissance de 8 Go, seize fichiers journaux virtuels de 512 Ko sont créés)

Si la taille des fichiers journaux s’accroît par de nombreux petits incréments, de nombreux fichiers journaux virtuels sont créés. **ce qui peut ralentir le démarrage de la base de données ainsi que les opérations de sauvegarde et de restauration.** À l’inverse, si les fichiers journaux sont définis avec une grande taille et avec un seul incrément ou peu d’incréments, un petit nombre de fichiers journaux virtuels très volumineux sont créés. Pour plus d’informations sur une estimation correcte de la **taille nécessaire** et de la **croissance automatique** d’un journal des transactions, reportez-vous à la section *Recommandations* de [Gérer la taille du fichier journal des transactions](../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md#Recommendations).

Il est conseillé d’affecter aux fichiers journaux une *taille* proche de la taille finale nécessaire, avec les incréments nécessaires pour atteindre la distribution optimale des fichiers journaux virtuels, et un *incrément_accroissement* relativement important. Consultez le conseil ci-dessous pour déterminer la distribution optimale des fichiers journaux virtuels pour la taille actuelle du journal des transactions. 
 - La valeur *size*, telle que définie par l’argument `SIZE` de `ALTER DATABASE` est la taille initiale du fichier journal.
 - La valeur de *incrément_accroissement* (également appelée valeur d’accroissement automatique), telle que définie par l’argument `FILEGROWTH` de `ALTER DATABASE`, correspond à la quantité d’espace ajoutée au fichier chaque fois de l’espace supplémentaire s’avère nécessaire. 
 
Pour plus d’informations sur les arguments `FILEGROWTH` et `SIZE` de `ALTER DATABASE`, consultez [Options de fichiers et de groupes de fichiers &#40;Transact-SQL&#41; ALTER DATABASE](../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).

> [!TIP]
> Pour déterminer la distribution optimale des fichiers journaux virtuels pour la taille actuelle du journal des transactions de toutes les bases de données dans une instance donnée, ainsi que les incréments de croissance pour atteindre la taille nécessaire, consultez ce [script](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs).
  
 Le journal des transactions est un fichier cumulatif. Considérons, par exemple, une base de données possédant un fichier journal physique divisé en quatre fichiers journaux virtuels. Lors de la création de la base de données, le fichier journal logique commence au début du fichier journal physique. Les nouveaux enregistrements du journal sont ajoutés à la fin du journal logique, qui s'étend vers la fin du journal physique. Le fait de tronquer le journal permet de libérer tous les journaux virtuels dont les enregistrements précèdent tous le MinLSN (numéro séquentiel dans le journal minimum). Le *MinLSN* est le numéro séquentiel dans le journal du plus ancien enregistrement du journal requis pour une opération de restauration réussie de l’ensemble de la base de données. Le journal des transactions de la base de données exemple ressemblerait à celui de l'illustration suivante :  
  
 ![Illustre la façon dont un fichier journal physique est divisé en journaux virtuels](../relational-databases/media/tranlog3.png)  
  
 Lorsque la fin du journal logique atteint la fin du fichier journal physique, le nouvel enregistrement du journal revient au début du fichier journal physique.  
  
![Illustre la façon dont un journal des transactions logique est enveloppé dans son fichier journal physique](../relational-databases/media/tranlog4.png)   
  
 Le cycle se répète indéfiniment tant que la fin du journal logique n'a pas atteint le début du journal logique. Si les anciens enregistrements du journal sont tronqués suffisamment souvent pour laisser de la place à tous les nouveaux enregistrements créées jusqu'au point de contrôle suivant, le journal ne se remplit jamais. Si la fin du journal logique atteint le début du journal logique, l'une ou l'autre des situations suivantes se produit :  
  
-   Si le paramètre `FILEGROWTH` est activé pour le journal et que l’espace disque est suffisant, le fichier s’étend en fonction de la taille spécifiée dans le paramètre *growth_increment*, les nouveaux enregistrements du journal étant ajoutés à l’extension. Pour plus d’informations sur le paramètre `FILEGROWTH`, consultez [Options de fichiers et de groupes de fichiers ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
-   Si le paramètre `FILEGROWTH` n’est pas activé ou si l’espace disque réservé au fichier journal est inférieur à la taille spécifiée dans le paramètre *growth_increment*, l’erreur 9002 est générée. Pour plus d’informations, consultez [Résoudre les problèmes liés à un journal des transactions saturé](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md).  
  
 Si le journal contient plusieurs fichiers journaux physiques, le journal logique va se déplacer dans tous les fichiers journaux physiques avant de revenir au début du premier fichier journal physique. 
 
> [!IMPORTANT]
> Pour plus d’informations sur la gestion de la taille du journal des transactions, consultez [Gérer la taille du fichier journal des transactions](../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).
  
### <a name="log-truncation"></a>Troncation de journal  
 La troncation du journal est essentielle pour empêcher que le journal se remplisse. La troncation du journal supprime les fichiers journaux virtuels inactifs du journal des transactions logique d'une base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , ce qui libère de l'espace dans le journal logique de façon à ce qu'il soit réutilisé par le journal des transactions physique. Si un journal des transactions n'était jamais tronqué, il finirait par occuper tout l'espace disque alloué à ses fichiers journaux physiques. Toutefois, une opération de point de contrôle est requise avant que le journal des transactions puisse être tronqué. Un point de contrôle écrit les pages modifiées en mémoire actuelles (appelées pages de modifications) et les informations du journal des transactions de la mémoire vers le disque. Lorsque le point de contrôle est créé, la partie inactive du journal des transactions est marquée comme réutilisable. Après cela, elle peut être libérée par troncation du journal. Pour plus d’informations sur les points de contrôle, consultez [Points de contrôle de base de données &#40;SQL Server&#41;](../relational-databases/logs/database-checkpoints-sql-server.md).  
  
 Les illustrations suivantes montrent un journal des transactions avant et après une troncation. La première illustration montre un journal des transactions qui n'a jamais été tronqué. Actuellement, quatre fichiers journaux virtuels sont utilisés par le journal logique. Le journal logique commence avant le premier fichier journal virtuel et se termine au journal virtuel 4. L'enregistrement NSEmin se trouve dans le journal virtuel 3. Les journaux virtuels 1 et 2 contiennent uniquement des enregistrements de journal inactifs. Ces enregistrements peuvent être tronqués. Le journal virtuel 5 est encore inutilisé et ne fait pas partie du journal logique actuel.  
  
![Illustre la façon dont un journal des transactions s’affiche avant qu’il ne soit tronqué](../relational-databases/media/tranlog2.png)  
  
 La deuxième illustration montre le journal après sa troncation. Les journaux virtuels 1 et 2 ont été libérés en vue de leur réutilisation. Le journal logique commence désormais au début du journal virtuel 3. Le journal virtuel 5 est encore inutilisé et ne fait pas partie du journal logique actuel.  
  
![Illustre la façon dont un journal des transactions s’affiche après avoir été tronqué](../relational-databases/media/tranlog3.png)  
  
 La troncation du journal se produit automatiquement après les événements suivants, à moins qu'elle ne soit retardée pour une raison quelconque :  
  
-   En mode de récupération simple, après un point de contrôle.  
-   En mode de restauration complète ou en mode de récupération utilisant les journaux de transactions, après une sauvegarde du journal, si un point de contrôle s'est produit depuis la dernière sauvegarde.  
  
 La troncation du journal peut être retardée pour différents motifs. En cas de retard prolongé de la troncation du journal, le journal des transactions peut se remplir complètement. Pour plus d’informations, consultez [Facteurs pouvant retarder la troncation du journal](../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation) et [Résoudre les problèmes liés à un journal des transactions saturé &#40;erreur SQL Server 9002&#41;](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md).  
  
##  <a name="write-ahead-transaction-log"></a><a name="WAL"></a> Journal des transactions à écriture anticipée  
 Cette section décrit le rôle que joue le journal des transactions à écriture anticipée (journal WAL) au niveau de l'enregistrement sur disque des modifications apportées aux données. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise un algorithme WAL (write-ahead logging) qui garantit qu’aucune modification de données n’est écrite sur le disque avant l’écriture du journal associé sur celui-ci. Ainsi, les propriétés ACID (Atomicité, Cohérence, Isolation et Durabilité) d'une transaction sont conservées.  
  
 Pour comprendre le fonctionnement du journal à écriture anticipée, il est important que vous sachiez comment les données modifiées sont écrites sur le disque. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] gère un cache des tampons dans lequel il lit les pages de données lorsque celles-ci doivent être extraites. Lorsqu’une page est modifiée dans le cache des tampons, elle n’est pas réécrite immédiatement sur le disque, mais elle est marquée comme *erronée*. Une page peut avoir plusieurs écritures logiques avant son écriture physique sur le disque. Pour chaque écriture logique, un enregistrement du journal des transactions est inséré dans le cache du journal qui enregistre la modification. L'enregistrement doit être écrit sur le disque avant que la page de modifications associée n'ait été supprimée du cache et écrite sur le disque. Le processus de point de contrôle analyse régulièrement le cache à la recherche de tampons contenant des pages issues d'une base de données spécifiée et écrit toutes les pages de modifications sur le disque. Les points de contrôle permettent une récupération ultérieure du système en créant un point où toutes les pages de modifications sont effectivement écrites sur le disque.  
  
 Le processus d'écriture d'une page de données modifiée, du cache des tampons vers le disque, porte le nom de vidage. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possède une logique qui empêche la suppression d’une page de modifications avant que l’enregistrement du journal associé n’ait été écrit. Les enregistrements de journal sont écrits sur le disque quand les tampons de journaux sont vidés.  Cela se produit chaque fois qu’une transaction est validée ou que les tampons de journaux sont saturés.  
  
##  <a name="transaction-log-backups"></a><a name="Backups"></a> Sauvegardes du journal de transactions  
 Cette section présente les concepts sur la sauvegarde et la restauration (application) des journaux de transactions. En mode de récupération complète et en mode de récupération utilisant les journaux de transactions, la sauvegarde régulière des journaux de transactions (*sauvegardes des journaux*) est indispensable pour pouvoir récupérer les données. Sauvegardez le journal pendant l'exécution d'une sauvegarde complète. Pour plus d’informations sur les modes de récupération, consultez [Sauvegarde et restauration des bases de données SQL Server](../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).  
  
 Avant de pouvoir créer la première sauvegarde du journal, vous devez créer une sauvegarde complète, telle qu'une sauvegarde de base de données ou la première d'une série de sauvegardes de fichiers. La restauration d'une base de données à l'aide seulement de sauvegardes de fichiers peut être complexe. Par conséquent, nous vous recommandons de commencer par une sauvegarde de base de données complète dès que possible. Puis, sauvegardez le journal des transactions régulièrement. Vous pouvez ainsi réduire les risques de perte de travail mais aussi permettre la troncation du journal des transactions. En général, le journal des transactions est tronqué après chaque sauvegarde de journal conventionnelle.  
  
> [!IMPORTANT]
> Nous vous recommandons d’effectuer des sauvegardes de journaux suffisamment fréquentes pour répondre à vos besoins, en particulier votre tolérance des pertes de données comme celles causées par un stockage de journal endommagé. La fréquence appropriée des sauvegardes de journaux dépend de votre gestion des risques liés aux pertes de données et du nombre de sauvegardes de journaux qu'il vous est possible de stocker, gérer et potentiellement restaurer. Pensez à [l’objectif de délai de récupération](https://wikipedia.org/wiki/Recovery_time_objective) et à [l’objectif de point de récupération](https://wikipedia.org/wiki/Recovery_point_objective) quand vous implémentez votre stratégie de récupération, en particulier la cadence des sauvegardes de fichier journal.
> Réaliser une sauvegarde de journal tous les 15 à 30 minutes peut être suffisant. Si vos besoins nécessitent de minimiser les risques de perte de travail, vous devez envisager des sauvegardes de journaux plus fréquentes. Une meilleure fréquence pour les sauvegardes de fichiers journaux offre l'avantage d'augmenter la fréquence de la troncation des journaux qui produit des fichiers journaux plus petits.  
  
> [!IMPORTANT]
> Pour limiter le nombre des sauvegardes de fichiers journaux à restaurer, il est essentiel de sauvegarder vos données régulièrement. Vous pouvez, par exemple, planifier une sauvegarde complète hebdomadaire et des sauvegardes différentielles quotidiennes de la base de données.  
> Là encore, pensez à [l’objectif de délai de récupération](https://wikipedia.org/wiki/Recovery_time_objective) et à [l’objectif de point de récupération](https://wikipedia.org/wiki/Recovery_point_objective) quand vous implémentez votre stratégie de récupération, en particulier la cadence des sauvegardes différentielles et complètes de base de données.

Pour plus d’informations sur les sauvegardes des journaux de transactions, consultez [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../relational-databases/backup-restore/transaction-log-backups-sql-server.md).
  
### <a name="the-log-chain"></a>Séquence de journaux de transactions consécutifs  
 Une séquence continue de sauvegardes de journaux s’appelle une *séquence de journaux de transactions consécutifs*. Une séquence de journaux de transactions consécutifs commence par une sauvegarde complète de la base de données. Généralement, une nouvelle séquence de journaux de transactions consécutifs ne démarre que lorsque la base de données est sauvegardée pour la première fois ou après que le mode de récupération simple est remplacé par le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions. Si vous ne choisissez pas de remplacer les jeux de sauvegarde existants lors de la création d'une sauvegarde complète de base de données, la séquence de journaux de transactions consécutifs existante reste intacte. Grâce à la séquence de journaux de transactions consécutifs intacte, vous pouvez restaurer votre base de données à partir d'une sauvegarde complète de base de données du support de sauvegarde, suivie de toutes les sauvegardes de fichiers journaux suivantes jusqu'à votre point de récupération. Le point de récupération peut être la fin de la dernière sauvegarde de fichier journal ou un point de récupération spécifique dans chacune des sauvegardes de fichiers journaux. Pour plus d’informations, consultez [Sauvegardes du journal des transactions &#40;SQL Server&#41;](../relational-databases/backup-restore/transaction-log-backups-sql-server.md).  
  
 Pour restaurer une base de données jusqu'au point d'échec, la séquence de journaux de transactions consécutifs doit être intacte. Autrement dit, la séquence ininterrompue des sauvegardes des journaux de transactions doit aller jusqu'au point de défaillance. Le point de commencement de cette séquence du journal dépend du type des sauvegardes de données que vous restaurez : base de données, partielle ou fichiers. Pour une sauvegarde partielle ou de base de données, la séquence des sauvegardes des journaux doit s'étendre à partir de la fin d'une sauvegarde partielle ou de base de données. Pour un jeu de sauvegardes de fichiers, la séquence des sauvegardes des journaux doit s'étendre à partir du début d'un jeu complet de sauvegardes de fichiers. Pour plus d’informations, consultez [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
### <a name="restore-log-backups"></a>Restaurer des sauvegardes du journal  
 La restauration d'une sauvegarde de journal restaure par progression les modifications enregistrées dans le journal des transactions, afin de recréer l'état exact de la base de données qui existait au début de la sauvegarde du journal. Lorsque vous restaurez une base de données, vous devez restaurer les sauvegardes des journaux créées à la suite de la sauvegarde complète de base de données que vous restaurez ou à partir de la première sauvegarde de fichiers que vous restaurez. En règle générale, vous devez restaurer une série de sauvegardes de journaux jusqu'au point de récupération, après avoir restauré les données les plus récentes ou une sauvegarde différentielle. Ensuite, vous récupérez la base de données. Cette opération restaure toutes les transactions qui n'étaient pas terminées au début de la récupération et place la base de données en ligne. Une fois la base de données récupérée, vous ne pouvez plus restaurer des sauvegardes. Pour plus d’informations, consultez [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  

## <a name="checkpoints-and-the-active-portion-of-the-log"></a>Points de contrôle et partie active du journal  

Les points de contrôle vident les pages de données incorrectes de la mémoire cache de la base de données active sur le disque, ce qui réduit la partie active du journal devant être traitée durant une récupération complète d'une base de données. Au cours d'une récupération complète, les types d'actions suivants sont effectués :

* Les enregistrements de journal concernant des modifications qui n'ont pas été vidées sur le disque avant l'arrêt du système sont restaurés par progression.
* Toutes les modifications associées à des transactions incomplètes (telles que les transactions pour lesquelles il n'existe pas d'enregistrement de journal COMMIT ou ROLLBACK) sont restaurées.

### <a name="checkpoint-operation"></a>Opération de point de contrôle

Un point de contrôle effectue les processus suivants dans la base de données :

* Écrit un enregistrement dans le fichier journal qui marque le début du point de contrôle.
* Stocke les informations enregistrées pour le point de contrôle dans une chaîne d'enregistrements de journal des points de contrôle.  
 
    L'une des informations consignées dans le point de contrôle est le numéro séquentiel dans le journal du premier enregistrement de journal qui doit être présent pour permettre une restauration à l'échelle de la base de données. Ce NSE porte le nom de NSE de récupération minimum (NSEmin). Le NSEmin est le minimum de : 

    * NSE du début du point de contrôle ;
    * NSE du début de la transaction active la plus ancienne ;
    * NSE du début de la transaction de réplication la plus ancienne qui n'a pas encore été transmise à la base de données de distribution. 

    Les enregistrements de point de contrôle contiennent également une liste de toutes les transactions actives qui ont modifié la base de données.

* Si la base de données utilise le mode de récupération simple, signalez pour une utilisation ultérieure l'espace qui précède le NSEmin. 
* Écrit sur le disque toutes les pages de journal et de données incorrectes.
* Écrit un enregistrement marquant la fin du point de contrôle dans le fichier journal.
* Écrit le numéro LSN du début de cette chaîne dans la page de démarrage de la base de données.

#### <a name="activities-that-cause-a-checkpoint"></a>Activités entraînant un point de contrôle

Des points de contrôle interviennent dans les situations suivantes :

* Une instruction CHECKPOINT est exécutée explicitement. Un point de contrôle intervient dans la base de données active pour la connexion.
* Une opération journalisée minimale est effectuée dans la base de données ; par exemple, une opération de copie en bloc est réalisée sur une base de données qui se sert du mode de récupération utilisant les journaux de transactions. 
* Des fichiers de base de données ont été ajoutés ou supprimés à l'aide de l'instruction ALTER DATABASE.
* Une instance de SQL Server est arrêtée par une instruction SHUTDOWN ou via l’arrêt du service SQL Server (MSSQLSERVER). Ces opérations provoquent la création d’un point de contrôle dans chaque base de données dans l’instance de SQL Server.
* Une instance de SQL Server génère régulièrement des points de contrôle automatiques dans chaque base de données, afin de réduire la durée nécessaire à l’instance pour récupérer la base de données.
* Une sauvegarde de la base de données est effectuée.
* Une activité nécessitant l'arrêt de la base de données est effectuée. Par exemple, la valeur ON est attribuée à AUTO_CLOSE et la dernière connexion utilisateur à la base de données est fermée, ou une modification d'une option de base de données nécessitant un redémarrage de la base de données est effectuée.

### <a name="automatic-checkpoints"></a>Points de contrôle automatiques

Le moteur de base de données SQL Server génère des points de contrôle automatiques. L'intervalle entre les points de contrôle automatiques est basé sur la quantité d'espace de journal utilisée et la durée écoulée depuis le dernier point de contrôle. Cet intervalle de temps entre les points de contrôle automatiques peut varier fortement et être long si les modifications apportées à la base de données sont peu nombreuses. Inversement, les points de contrôle automatiques peuvent être fréquents si les données modifiées sont nombreuses.

Utilisez l’option de configuration de serveur **intervalle de récupération** pour calculer l’intervalle pour toutes les bases de données sur une instance de serveur. Cette option spécifie la durée maximale que le moteur de base de données doit utiliser pour récupérer une base de données durant un redémarrage du système. Le moteur de base de données estime le nombre d’enregistrements de journal qu’il peut traiter au cours de l’ **intervalle de récupération** durant une opération de récupération. 

L'intervalle entre les points de contrôle automatiques dépend également du mode de récupération :

* Si la base de données utilise le mode de restauration complète ou le mode de récupération utilisant les journaux de transactions, un point de contrôle automatique est généré chaque fois que le nombre d’enregistrements du journal atteint une valeur que le moteur de base de données estime pouvoir traiter pendant la durée spécifiée dans l’option intervalle de récupération.
* Si la base de données utilise le mode de récupération simple, un point de contrôle automatique est généré chaque fois que le nombre des enregistrements de journal atteint la plus faible de ces deux valeurs : 

    * Le journal est saturé à 70 %.
    * Le nombre d’enregistrements de journal atteint le nombre que le moteur de base de données estime pouvoir traiter au cours de la durée spécifiée dans l’option intervalle de récupération.

Pour plus d’informations sur la configuration de l’intervalle de récupération, consultez [Configurer l’option de configuration du serveur recovery interval](../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).

> [!TIP]  
> L’option d’installation avancée -k de SQL Server permet à un administrateur de base de données de limiter le comportement d’E/S de point de contrôle en fonction du débit du sous-système d’E/S pour certains types de points de contrôle. L’option d’installation -k s’applique aux points de contrôle automatiques, ainsi qu’à tous les points de contrôle non accélérés. 
 
Les points de contrôle automatiques tronquent la section inutilisée du journal des transactions si la base de données utilise le mode de récupération simple. Cependant, ils ne tronquent pas le journal si la base de données utilise le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions. Pour plus d’informations, consultez [Journal des transactions](../relational-databases/logs/the-transaction-log-sql-server.md). 

L’instruction CHECKPOINT fournit désormais un argument checkpoint_duration facultatif qui spécifie la durée demandée, en secondes, permettant aux points de contrôle de terminer leurs tâches. Pour plus d’informations, consultez [CHECKPOINT](../t-sql/language-elements/checkpoint-transact-sql.md).

### <a name="active-log"></a>journal actif

La section du fichier journal comprise entre le MinLSN et le dernier enregistrement de journal écrit s’appelle la partie active du journal, ou journal actif. Cette section est indispensable pour procéder à une récupération complète de la base de données. Aucune partie de ce journal actif ne peut être tronquée. Tous les enregistrements de journal doivent être tronqués à partir des parties du journal situées avant le MinLSN.

L'illustration ci-dessous présente une version simplifiée de la fin d'un journal de transactions comportant deux transactions actives. Les enregistrements du point de contrôle ont été compactés en un enregistrement unique.

![Illustre un journal de fin de transaction avec deux transactions actives et un enregistrement de point de contrôle compacté](../relational-databases/media/active-log.png) 

LSN 148 est le dernier enregistrement du journal des transactions. Au moment où le point de contrôle enregistré au numéro LSN 147 était traité, Tran 1 avait été validée et Tran 2 était la seule transaction active. Ainsi, le premier enregistrement de Tran 2 est l'enregistrement de journal le plus ancien pour une transaction active au moment du dernier point de contrôle. Par ailleurs, le numéro LSN 142 est l'enregistrement du début de la transaction pour Tran 2, la valeur MinLSN.

### <a name="long-running-transactions"></a>Transactions de longue durée
Le journal actif doit contenir chaque partie de toutes les transactions non validées. Une application qui démarre une transaction et qui ne la valide pas ou ne la restaure pas empêche le [!INCLUDE[ssde_md](../includes/ssde_md.md)] de faire progresser le MinLSN. Ceci peut provoquer deux types de problèmes :

* Si le système est arrêté après que la transaction a effectué de nombreuses modifications non validées, la phase de récupération lors du démarrage ultérieur peut être beaucoup plus longue que la durée spécifiée dans l’option **intervalle de récupération** .
* Le journal peut devenir très volumineux parce qu'il ne peut pas être tronqué au-delà du MinLSN. Cela se produit même si la base de données utilise le modèle de récupération simple, dans lequel le journal des transactions est généralement tronqué sur chaque point de contrôle automatique.

À partir de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] et dans [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)], la récupération des transactions de longue durée et des problèmes décrits ci-dessus peut être évitée à l’aide de la [récupération de base de données accélérée](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr).  

### <a name="replication-transactions"></a>Transactions de réplication
L'Agent de lecture du journal surveille le journal des transactions de chaque base de données configurée pour la réplication transactionnelle et copie les transactions devant être répliquées à partir du journal des transactions dans la base de données de distribution. Le journal actif doit contenir toutes les transactions qui sont marquées pour la réplication mais qui n'ont pas encore été transmises à la base de données de distribution. Si ces transactions ne sont pas répliquées à temps, elles peuvent empêcher la troncature du journal. Pour plus d’informations, consultez [Réplication transactionnelle](../relational-databases/replication/transactional/transactional-replication.md).

## <a name="see-also"></a>Voir aussi 
Pour plus d’informations sur le journal des transactions et les bonnes pratiques relatives à la gestion des journaux, nous vous recommandons de lire les articles et les ouvrages suivants.  
  
[Journal des transactions &#40;SQL Server&#41;](../relational-databases/logs/the-transaction-log-sql-server.md)    
[Gérer la taille du fichier journal des transactions](../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)   
[Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
[Points de contrôle de base de données &#40;SQL Server&#41;](../relational-databases/logs/database-checkpoints-sql-server.md)   
[Configurer l’option de configuration de serveur d’intervalle de récupération](../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)    
[Récupération de base de données accélérée](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr)       
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)    
[Fonctionnement de la journalisation et de la récupération dans SQL Server, par Paul Randall](/previous-versions/technet-magazine/dd392031(v=msdn.10))    
[Gestion du journal des transactions SQL Server de Tony Davis et Gail Shaw](https://www.simple-talk.com/books/sql-books/sql-server-transaction-log-management-by-tony-davis-and-gail-shaw/)  
  
