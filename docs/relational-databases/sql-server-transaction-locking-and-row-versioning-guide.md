---
description: Guide du verrouillage des transactions et du contrôle de version de ligne
title: Guide du verrouillage des transactions et du contrôle de version de ligne
ms.custom: seo-dt-2019
ms.date: 03/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- guide, transaction locking and row versioning
- transaction locking and row versioning guide
- lock compatibility matrix, [SQL Server]
- lock granularity and hierarchies, [SQL Server]
- deadlocks, [SQL Server]
- lock escalation, [SQL Server]
- lock partitioning, [SQL Server]
ms.assetid: 44fadbee-b5fe-40c0-af8a-11a1eecf6cb7
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47545adc6e3e620cd74585d477bdd028f1e8625f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352491"
---
# <a name="transaction-locking-and-row-versioning-guide"></a>Guide du verrouillage des transactions et du contrôle de version de ligne
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Dans une base de données, une mauvaise gestion des transactions conduit souvent à des problèmes de contention et de détérioration des performances dans les systèmes comprenant de nombreux utilisateurs. Plus le nombre d'utilisateurs qui ont accès aux données est grand, plus il est important que les applications utilisent les transactions de manière efficace. Ce guide présente les mécanismes de verrouillage et de contrôle de version de ligne utilisés par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pour garantir l'intégrité physique de chaque transaction et contient des informations sur la façon dont les applications peuvent contrôler efficacement les transactions.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ([!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] jusqu’à [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], sauf indication contraire) et [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]. 
  
##  <a name="transaction-basics"></a><a name="Basics"></a> Principes de base sur les transactions  
 Une transaction est une suite d'opérations effectuées comme une seule unité logique de travail. Une unité logique de travail doit posséder quatre propriétés appelées propriétés ACID (Atomicité, Cohérence, Isolation et Durabilité), pour être considérée comme une transaction :  
  
 **Atomicité**  
 Une transaction doit être une unité de travail indivisible ; soit toutes les modifications de données sont effectuées, soit aucune ne l'est.  
  
 **Cohérence**  
 Lorsqu'elle est terminée, une transaction doit laisser les données dans un état cohérent. Dans une base de données relationnelle, toutes les règles doivent être appliquées aux modifications apportées par la transaction, afin de conserver l'intégrité de toutes les données. Toutes les structures de données internes, comme les index B-tree ou les listes à chaînage double, doivent être cohérentes à la fin de la transaction.  
  
 **Isolement**  
 Les modifications effectuées par des transactions concurrentes doivent être isolées transaction par transaction. Une transaction reconnaît les données dans l'état où elles se trouvaient avant d'être modifiées par une transaction simultanée, ou les reconnaît une fois que la deuxième transaction est terminée, mais ne reconnaît jamais un état intermédiaire. Cette propriété est nommée mise en série, car elle permet de recharger les données de départ et de répéter une suite de transactions dont le résultat sur les données sera identique à celui des transactions d'origine.  
  
 **Durabilité**  
 Lorsqu'une transaction durable est terminée, ses effets sur le système sont permanents. Les modifications sont conservées même en cas de défaillance du système. [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] et versions ultérieures permettent les transactions durables retardées. Les transactions durables retardées sont validées avant de consigner l'enregistrement du journal des transactions sur le disque. Pour plus d’informations sur la durabilité des transactions retardées, consultez la rubrique [Durabilité des transactions](../relational-databases/logs/control-transaction-durability.md).  
  
 Les programmeurs SQL doivent concevoir des transactions dont les points de début et de fin permettent de maintenir la cohérence logique des données. La séquence de modifications des données qu'ils définissent doivent laisser les données dans un état cohérent par rapport aux règles d'entreprise définies par leur société. Ces instructions de modification des données doivent par conséquent être contenues dans une seule transaction pour que le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] puisse garantir l'intégrité physique de la transaction.  
  
 Un système de base de données d'entreprise, par exemple une instance de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], se doit de fournir des mécanismes permettant de garantir l'intégrité physique de chaque transaction. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] fournit les éléments suivants :  
  
-   Des fonctionnalités de verrouillage permettant d'assurer l'isolement des transactions.  
  
-   Des fonctionnalités de consignation assurent la durabilité des transactions. Pour les transactions durables, l'enregistrement du journal est renforcé sur le disque avant les validations des transactions. Ainsi, en cas de défaillance du matériel serveur, du système d’exploitation ou de l’instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] lui-même, l’instance utilise au redémarrage les journaux des transactions pour restaurer automatiquement toutes les transactions incomplètes jusqu’au moment de la défaillance du système. Les transactions durables retardées sont validées avant de renforcer l'enregistrement du journal des transactions sur le disque. Ces transactions peuvent être perdues en cas de défaillance du système avant que l'enregistrement du journal ne soit renforcé sur le disque. Pour plus d’informations sur la durabilité des transactions retardées, consultez la rubrique [Durabilité des transactions](../relational-databases/logs/control-transaction-durability.md).  
  
-   Des fonctionnalités de gestion des transactions qui assurent l'atomicité et la cohérence des transactions. Lorsqu'une transaction a débuté, elle doit se dérouler correctement jusqu'à la fin (validée), sans quoi l'instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] annule toutes les modifications effectuées sur les données depuis le début de la transaction. Cette opération est appelée restauration d'une transaction, car elle retourne les données telles qu'elles étaient avant ces modifications.  
  
### <a name="controlling-transactions"></a>Contrôle des transactions  
 Le contrôle des transactions par les applications consiste principalement à spécifier des points de début et de fin de chaque transaction. La spécification est effectuée à l'aide des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] ou des fonctions d'API de base de données. Le système doit aussi être capable de gérer les erreurs interrompant une transaction avant sa fin normale. Pour plus d’informations, consultez [Transactions](../t-sql/language-elements/transactions-transact-sql.md), [Transactions dans ODBC](../relational-databases/native-client/odbc/performing-transactions-in-odbc.md) et [Transactions dans SQL Server Native Client (OLEDB)](../relational-databases/native-client-ole-db-transactions/transactions.md).  
  
 Par défaut, les transactions sont gérées au niveau de la connexion. Lorsqu'une transaction est démarrée lors d'une connexion, toutes les instructions [!INCLUDE[tsql](../includes/tsql-md.md)] exécutées lors de cette connexion font partie de la transaction jusqu'à la fin de celle-ci. Toutefois, dans une session MARS (Multiple Active Result Set), une transaction [!INCLUDE[tsql](../includes/tsql-md.md)] explicite ou implicite devient une transaction dont l'étendue est définie par traitement gérée au niveau du lot. À la fin du traitement, si une transaction dont l'étendue est définie par traitement n'est pas validée ou restaurée, elle est automatiquement restaurée par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Utilisation de MARS (Multiple Active Result Sets)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
#### <a name="starting-transactions"></a>Démarrage des transactions  
 À l'aide des fonctions API et des instructions [!INCLUDE[tsql](../includes/tsql-md.md)], vous pouvez démarrer des transactions en mode explicite, implicite ou validation automatique dans les instances du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
 **Transactions explicites**  
 Une transaction est explicite si vous définissez le début et la fin de la transaction de manière explicite à l’aide d’une fonction API ou en exécutant les instructions [!INCLUDE[tsql](../includes/tsql-md.md)] BEGIN TRANSACTION, COMMIT TRANSACTION, COMMIT WORK, ROLLBACK TRANSACTION ou ROLLBACK WORK[!INCLUDE[tsql](../includes/tsql-md.md)]. À la fin de la transaction, la connexion revient au mode de transaction sélectionné avant le début de la transaction, c'est-à-dire implicite ou autocommit.  
  
 Vous pouvez utiliser toutes les instructions [!INCLUDE[tsql](../includes/tsql-md.md)] dans une transaction explicite, à l'exception des instructions suivantes :  
  
:::row:::
    :::column:::
        ALTER DATABASE
    :::column-end:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT INDEX
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        CREATE FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        RECONFIGURE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT INDEX
    :::column-end:::
    :::column:::
        CREATE FULLTEXT INDEX
    :::column-end:::
    :::column:::
        RESTORE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        BACKUP
    :::column-end:::
    :::column:::
        DROP DATABASE
    :::column-end:::
    :::column:::
        Procédures stockées système de recherche en texte intégral
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        Option sp_dboption pour définir les options de base de données, ou une des procédures système qui modifient la base de données master à l'intérieur de transactions explicites ou implicites.
    :::column-end:::
:::row-end:::

> [!NOTE]  
> UPDATE STATISTICS peut être utilisée à l'intérieur d'une transaction explicite. Toutefois, UPDATE STATISTICS est validée indépendamment de la transaction qui la contient et ne peut pas être restaurée.  
  
 **Transactions en mode de validation automatique**  
 Le mode de validation automatique est le mode de gestion par défaut des transactions du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Chaque instruction [!INCLUDE[tsql](../includes/tsql-md.md)] est validée ou restaurée dès qu'elle se termine. Lorsqu'une instruction est exécutée avec succès, elle est validée ; si une erreur se produit, elle est restaurée. Une connexion à une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] fonctionne par défaut en mode autocommit si les modes explicite ou implicite n'ont pas été spécifiés pour une transaction. Le mode autocommit est également le mode par défaut pour ADO, OLE DB, ODBC et DB-Library.  
  
 **Transactions implicites**  
 Lorsqu'une connexion fonctionne en mode de transaction implicite, l'instance de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] démarre automatiquement une nouvelle transaction après la validation ou la restauration de la transaction en cours. Vous n'avez pas à définir le début d'une transaction, il vous suffit de valider ou de restaurer chaque transaction. Le mode de transaction implicite génère une succession continue de transactions. Activez le mode de transaction implicite en utilisant une fonction d'API ou l'instruction [!INCLUDE[tsql](../includes/tsql-md.md)] SET IMPLICIT_TRANSACTIONS ON.  Ce mode est également appelé « Autocommit OFF ». Consultez [Méthode setAutoCommit dans JDBC](../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md) 
  
 Lorsque le mode de transaction implicite est activé pour une connexion, l'instance de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] démarre automatiquement une transaction lorsqu'il exécute pour la première fois l'une des instructions suivantes :  
  
:::row:::
    :::column:::
        ALTER TABLE
    :::column-end:::
    :::column:::
        FETCH
    :::column-end:::
    :::column:::
        REVOKE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE
    :::column-end:::
    :::column:::
        GRANT
    :::column-end:::
    :::column:::
        SELECT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        Suppression
    :::column-end:::
    :::column:::
        INSERT
    :::column-end:::
    :::column:::
        TRUNCATE TABLE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DROP
    :::column-end:::
    :::column:::
        OPEN
    :::column-end:::
    :::column:::
        UPDATE
    :::column-end:::
:::row-end:::

-  **Transactions délimitées au traitement**  
   Uniquement applicable aux ensembles de résultats MARS (Multiple Active Result Sets), une transaction [!INCLUDE[tsql](../includes/tsql-md.md)] explicite ou implicite qui démarre sous une session MARS devient une transaction dont l'étendue est définie par traitement. Une transaction dont l'étendue est définie par traitement qui n'est pas validée ou restaurée à la fin du traitement est automatiquement restaurée par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-  **Transactions distribuées**  
   Les transactions distribuées sont réparties sur plusieurs serveurs nommés gestionnaires de ressources. La gestion de la transaction doit être coordonnée entre les gestionnaires de ressources par un composant du serveur nommé gestionnaire de transactions. Chaque instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] peut être utilisée comme gestionnaire de ressources dans les transactions distribuées coordonnées par un gestionnaire de transactions tel que MS DTC ([!INCLUDE[msCoName](../includes/msconame-md.md)] Distributed Transaction Coordinator), ou tout autre gestionnaire de transactions prenant en charge les spécifications Open Group XA pour le traitement des transactions distribuées. Pour plus d'informations, consultez la documentation MS DTC.  
  
   Une transaction exécutée sur une seule instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], mais utilisant plusieurs bases de données, est en réalité une transaction distribuée. Cette instance gère la transaction distribuée de manière interne ; elle apparaît comme une transaction locale pour l'utilisateur.  
  
   Dans une application, une transaction distribuée est gérée de manière comparable à une transaction locale. À la fin de la transaction, l'application requiert que la transaction soit validée ou restaurée. La validation d'une transaction distribuée doit être gérée de façon particulière par le gestionnaire de transaction pour minimiser les risques qu'une défaillance du réseau entraîne la validation de la transaction par certains gestionnaires de ressources, alors qu'elle sera restaurée par d'autres. Pour cela, le processus de validation est géré en deux phases, une phase de préparation et une phase de validation, d'où son nom de validation à deux phases (2PC).  
  
   -  **Phase de préparation**  
      Lorsque le gestionnaire de transactions reçoit une requête de validation, il envoie une commande de préparation à tous les gestionnaires de ressources concernés par la transaction. Chaque gestionnaire de ressources effectue alors toutes les opérations nécessaires à l'enregistrement de la transaction, et tous les tampons contenant les images du journal de la transaction sont vidés sur le disque. Lorsque chaque gestionnaire de ressources a terminé la phase de préparation, il retourne un message de succès ou d'échec au gestionnaire de transactions. [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] a introduit la durabilité des transactions retardées. Les transactions durables retardées sont validées avant de vider les images de journal pour la transaction sur le disque. Pour plus d’informations sur la durabilité des transactions retardées, consultez la rubrique [Durabilité des transactions](../relational-databases/logs/control-transaction-durability.md).  
  
   -  **Phase de validation**  
      Si le gestionnaire de transactions reçoit des messages de préparation réussie de tous les gestionnaires de ressources, il envoie une commande de validation à chacun d'entre eux. Les gestionnaires de ressources peuvent alors effectuer la validation. Si tous les gestionnaires de ressources signalent le succès de la validation, le gestionnaire de transactions envoie alors une notification de succès à l'application. Si l'un des gestionnaires de ressources indique un échec de la préparation, le gestionnaire de transactions envoie une commande de restauration à chaque gestionnaire de ressources et notifie l'échec de la validation à l'application.  
  
      Les applications du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] peuvent gérer les transactions distribuées à l'aide de [!INCLUDE[tsql](../includes/tsql-md.md)] ou de l'API de base de données. Pour plus d’informations, consultez [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
#### <a name="ending-transactions"></a>Fin des transactions  
 Terminez les transactions avec une instruction COMMIT ou ROLLBACK, ou au moyen d'une fonction API correspondante.  
  
-  **COMMIT**  
   Si une transaction est réussie, validez-la. L'instruction COMMIT garantit que toutes les modifications effectuées sur la base de données au cours de la transaction sont permanentes. Cette instruction libère également les ressources, telles que les verrous, qui ont été utilisées par la transaction.  
  
-  **ROLLBACK**  
   Si une erreur se produit pendant une transaction ou si l'utilisateur décide de l'abandonner, restaurez-la. Une instruction ROLLBACK annule toutes les modifications effectuées par la transaction en rétablissant les données dans l'état où elles étaient avant le début de celle-ci. Cette instruction libère également les ressources bloquées par la transaction.  
  
> [!NOTE]  
> Dans le cas des  connexions prenant en charge les ensembles de résultats MARS (Multiple Active Result Sets), une transaction explicite démarrée par le biais d'une fonction API ne peut pas être validée alors que des demandes sont en attente d'exécution. Toute tentative de validation d'une transaction de ce type entraîne une erreur si des opérations sont toujours en attente.  
  
#### <a name="errors-during-transaction-processing"></a>Erreurs de traitement au cours d'une transaction  
 Si une erreur entrave le bon déroulement d'une transaction, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] la restaure automatiquement et libère toutes les ressources bloquées par la transaction. Si la connexion réseau du client à une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] est interrompue, toutes les transactions en cours associées à cette connexion sont restaurées au moment de la notification de l'instance de l'interruption. En cas de défaillance de l'application cliente et de panne ou de redémarrage de l'ordinateur client, la connexion est interrompue. L'instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] restaure toutes les transactions en cours au moment de la notification de la panne par le réseau. Si le client se déconnecte de l'application, toutes les transactions en cours sont restaurées.  
  
 Si une instruction génère une erreur d'exécution (comme une violation de contrainte) dans un traitement, la réaction par défaut du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] est de restaurer seulement l'instruction ayant généré l'erreur. Vous pouvez modifier ce comportement à l’aide de l’instruction `SET XACT_ABORT`. Après l’exécution de `SET XACT_ABORT` ON, toute erreur d’exécution causée par une instruction déclenche automatiquement la restauration de la transaction en cours. Les erreurs de compilation, comme les erreurs de syntaxe, ne sont pas affectées par `SET XACT_ABORT`. Pour plus d’informations, consultez [SET XACT_ABORT &#40;Transact-SQL&#41;](../t-sql/statements/set-xact-abort-transact-sql.md).  
  
 Quand une erreur se produit, l’action corrective (`COMMIT` ou `ROLLBACK`) doit être incluse dans le code de l’application. Pour gérer efficacement les erreurs, notamment celles qui surviennent au cours de transactions, utilisez la construction `TRY...CATCH` [!INCLUDE[tsql](../includes/tsql-md.md)]. Pour plus d’informations et d’exemples portant sur les transactions, consultez [TRY...CATCH &#40;Transact-SQL&#41;](../t-sql/language-elements/try-catch-transact-sql.md). À partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], l’instruction `THROW` peut être utilisée pour lever une exception et transférer l’exécution à un bloc `CATCH` d’une construction `TRY...CATCH`. Pour plus d’informations, consultez [THROW &#40;Transact-SQL&#41;](../t-sql/language-elements/throw-transact-sql.md).  
  
##### <a name="compile-and-run-time-errors-in-autocommit-mode"></a>Erreurs de compilation et d'exécution en mode de validation automatique  
 En mode de validation automatique, il peut arriver qu'une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] semble restaurer un lot entier au lieu d'une instruction SQL unique. Ceci se produit en cas d'erreur de compilation et non en cas d'erreur d'exécution. Une erreur de compilation empêche le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de créer un plan d'exécution, de telle sorte qu'aucune instruction du traitement n'est exécutée. Bien qu'il semble que toutes les instructions précédant celle qui a produit l'erreur soient restaurées, en réalité l'erreur rend impossible l'exécution de toutes les instructions du lot. Dans l'exemple qui suit, une erreur de compilation empêche l'exécution de toutes les instructions `INSERT` du troisième lot. Les deux premières instructions `INSERT` semblent avoir été restaurées alors qu'elles n'ont en fait jamais été exécutées.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUSE (3, 'ccc');  -- Syntax error.  
GO  
SELECT * FROM TestBatch;  -- Returns no rows.  
GO  
```  
  
 Dans l'exemple ci-dessous, la troisième instruction `INSERT` génère une erreur d'exécution causée par une clé primaire en double. Les deux premières instructions `INSERT` étant correctes et validées, elles ne sont pas restaurées après l'erreur d'exécution.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUES (1, 'ccc');  -- Duplicate key error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```  
  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] introduit la résolution de nom différée, dans laquelle la résolution des noms d’objet est effectuée au moment de l’exécution seulement. Dans l'exemple suivant, les deux premières instructions `INSERT` sont exécutées et validées, et ces deux lignes restent dans la table `TestBatch`, même une fois que la référence à une table inexistante dans la troisième instruction `INSERT` a généré une erreur d'exécution.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBch VALUES (3, 'ccc');  -- Table name error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```   
  
##  <a name="locking-and-row-versioning-basics"></a><a name="Lock_Basics"></a> Principes de base sur le verrouillage et le contrôle de version de ligne  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise les mécanismes suivants pour garantir l'intégrité des transactions et gérer la cohérence des bases de données lorsque plusieurs utilisateurs accèdent simultanément aux données :  
  
-  **Verrouillage**    

   Chaque transaction demande des verrous de différents types sur les ressources dont elle dépend, telles que les lignes, les pages ou les tables. Les verrous demandés empêchent les autres transactions d'apporter aux ressources des modifications susceptibles de nuire à la transaction. Chaque transaction libère ses verrous lorsqu'elle ne dépend plus des ressources verrouillées.  
  
-  **Gestion de versions de ligne**    

   Lorsqu'un niveau d'isolement basé sur le contrôle de version de ligne est activé, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] gère les versions de chaque ligne modifiée. Les applications peuvent, au lieu de protéger toutes les lectures avec des verrous, spécifier qu'une transaction utilise les versions de ligne pour consulter les données telles qu'elles existaient à son démarrage ou à l'exécution de la requête. L'utilisation du contrôle de version de ligne réduit sensiblement le risque qu'une opération de lecture bloque les autres transactions.  
  
 Le verrouillage et le contrôle de version de ligne empêchent les utilisateurs de lire les données non validées et plusieurs utilisateurs de modifier simultanément les mêmes données. Sans le verrouillage ou le contrôle de version de ligne, les requêtes exécutées sur ces données pourraient produire des résultats inattendus en retournant des données qui ne sont pas encore validées dans la base de données.  
  
 Les applications peuvent choisir les niveaux d'isolement de transaction, qui définissent le niveau de protection d'une transaction contre les modifications apportées par les autres transactions. Vous pouvez spécifier des indicateurs de table pour des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] spécifiques en fonction des besoins de l'application.  
  
### <a name="managing-concurrent-data-access"></a>Gestion de l'accès concurrentiel aux données  
 Lorsque plusieurs utilisateurs accèdent à une ressource en même temps, on parle d'accès concurrentiel. L'accès concurrentiel aux données requiert certains mécanismes permettant de contrer les effets négatifs de la modification d'une ressource déjà en cours d'utilisation.  
  
#### <a name="concurrency-effects"></a>Effet des accès concurrentiels  
 Les utilisateurs qui modifient des données peuvent interférer avec d'autres utilisateurs en train de lire ou de modifier les mêmes données en même temps. On dit que ces utilisateurs accèdent aux données de manière concurrentielle. Si un système de stockage de données est dépourvu de contrôle des accès concurrentiels, les utilisateurs peuvent constater les effets secondaires suivants :  
  
-   **Mises à jour perdues** 
  
     Les mises à jour perdues se produisent lorsque deux transactions ou plus sélectionnent la même ligne, puis la mettent à jour en fonction de la valeur qui s'y trouvait à l'origine. Aucune transaction n'a connaissance des autres transactions. La dernière mise à jour écrase les mises à jour effectuées par les autres transactions, ce qui entraîne une perte de données.  
  
     Exemple : deux éditeurs font une copie électronique du même document. Chaque éditeur modifie son document et l'enregistre ensuite, en écrasant le document original. Le dernier éditeur à avoir enregistré le document écrase les modifications effectuées par l'autre éditeur. Le problème pourrait être évité en empêchant un éditeur d'accéder au fichier tant que l'autre éditeur n'a pas terminé et validé la transaction.  
  
-   **Dépendance non validée (lecture erronée)**  
  
     Une dépendance non validée se produit lorsqu'une deuxième transaction sélectionne une ligne qui est actuellement mise à jour par une autre transaction. La deuxième transaction lit les données qui n'ont pas encore été validées et qui peuvent être modifiées par la transaction de mise à jour de la ligne.  
  
     Supposons par exemple qu'un éditeur effectue des modifications dans un document électronique. Pendant les modifications, un second éditeur fait une copie du document comprenant toutes les modifications effectuées jusqu'alors et distribue ce dernier aux destinataires concernés. Le premier éditeur décide alors que les modifications effectuées sont incorrectes, les supprime et enregistre le document. Le document qui a été distribué comprend donc des modifications qui n'existent plus et devraient être traitées comme si elles n'avaient jamais existé. Le problème pourrait être évité en interdisant la lecture du document modifié tant que le premier éditeur n'a pas effectué l'enregistrement final des modifications et validé la transaction.  
  
-   **Analyse incohérente (lecture non reproductible)**  
  
     Une analyse incohérente se produit lorsqu'une deuxième transaction accède à la même ligne plusieurs fois et lit différentes données à chaque fois. Une analyse incohérente est similaire à une dépendance non validée en ce sens qu'une autre transaction change les données qu'une deuxième transaction est en train de lire. Cependant, dans une analyse incohérente, les données lues par la deuxième transaction sont validées par la transaction qui a effectué la modification. En outre, une analyse incohérente implique plusieurs lectures (deux ou plus) de la même ligne dont les informations sont systématiquement modifiées par une autre transaction, d'où l'expression de lecture non renouvelable.  
  
     Par exemple, un éditeur relit le même document deux fois, mais l'auteur réécrit le document entre les relectures. Lorsque l'éditeur relit le document pour la seconde fois, le document a changé. La relecture initiale n'est donc pas renouvelable. Ce problème pourrait être évité si l'auteur ne pouvait pas modifier le document tant que l'éditeur n'a pas terminé la dernière lecture.  
  
-   **Lectures fantômes**  
  
     Une lecture fantôme est une situation qui se produit lorsque deux requêtes identiques sont exécutées et que la collection de lignes retournée par la deuxième requête est différente. L'exemple suivant montre comment cela peut arriver. Supposons que les deux transactions ci-dessous s'exécutent en même temps. Les deux instructions `SELECT` dans la première transaction peuvent retourner des résultats différents parce que l'instruction `INSERT` dans la deuxième transaction modifie les données utilisées par les deux transactions.  
  
    ```sql  
    --Transaction 1  
    BEGIN TRAN;  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    --The INSERT statement from the second transaction occurs here.  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    COMMIT;  
    ```  
  
    ```sql  
    --Transaction 2  
    BEGIN TRAN;  
    INSERT INTO dbo.employee  
      (Id, Name) VALUES(6 ,'New');  
    COMMIT;   
    ```  
  
-   **Lectures manquantes et en double provoquées par des mises à jour de ligne**  
  
    -   Manquer une ligne mise à jour ou consulter une ligne mise à jour plusieurs fois  
  
         Les transactions qui s’exécutent au niveau `READ UNCOMMITTED` ne génèrent pas de verrous partagés pour empêcher d’autres transactions de modifier des données lues par la transaction en cours. Les transactions qui s'exécutent au niveau READ COMMITTED génèrent des verrous partagés, mais les verrous de ligne ou de page sont libérés une fois la ligne lue. Dans les deux cas, lorsque vous analysez un index, si un autre utilisateur modifie la colonne de clé d'index de la ligne pendant votre lecture, la ligne peut apparaître de nouveau si la modification apportée à la clé a déplacé la ligne à une position située en aval de votre analyse. De même, la ligne peut ne pas apparaître si la modification apportée à la clé a déplacé la ligne à une position dans l'index que vous aviez déjà lue. Pour éviter cela, utilisez l’indicateur `SERIALIZABLE` ou `HOLDLOCK`, ou le contrôle de version de ligne. Pour plus d’informations, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-table.md).  
  
    -   Manquer une ou plusieurs lignes qui n'étaient pas la cible de la mise à jour  
  
         Quand vous utilisez `READ UNCOMMITTED`, si votre requête lit des lignes à l’aide d’une analyse d’ordre d’allocation (à l’aide de pages IAM), vous risquez de manquer des lignes si une autre transaction provoque un fractionnement de page. Cela ne peut pas se produire lorsque vous utilisez la lecture validée car un verrou de table est maintenu pendant un fractionnement de page et ne se produit pas si la table n'a pas d'index cluster, car les mises à jour ne provoquent pas de fractionnements de page.  
  
#### <a name="types-of-concurrency"></a>Types de concurrence  
 Lorsque plusieurs personnes tentent de modifier des données dans une base de données en même temps, il convient d'implémenter un système de contrôle de manière à ce que les modifications apportées par une personne n'en pénalisent pas une autre. Ce système s'appelle le contrôle de concurrence.  
  
 La théorie du contrôle de concurrence repose sur deux méthodes de classification :  
  
-   Contrôle de concurrence **pessimiste**  
  
     Un système de verrouillage empêche les utilisateurs de modifier des données d'une manière qui affecterait les autres utilisateurs. Lorsqu'un utilisateur a effectué une action qui applique un verrouillage, les autres utilisateurs ne peuvent plus effectuer d'actions qui entreraient en conflit avec ce verrouillage tant que celui-ci n'est pas libéré par le propriétaire. Cette méthode s'appelle le contrôle pessimiste car elle est principalement utilisée dans les environnements où les données sont très sollicitées et où le coût de protection des données par verrouillage est inférieur au coût de restauration des transactions en cas de contentions de concurrence.  
  
-   Contrôle de concurrence **optimiste**  
  
     Dans le cas du contrôle de concurrence optimiste, les utilisateurs ne verrouillent pas les données quand ils les lisent. Lors d'une mise à jour, le système vérifie si un autre utilisateur a modifié les données après leur lecture. Si tel est le cas, une erreur est générée. Généralement, l'utilisateur recevant l'erreur restaure la transaction et recommence. Cette méthode s'appelle le contrôle optimiste car elle est principalement utilisée dans les environnements où les données sont peu demandées et où le coût de la restauration occasionnelle d'une transaction est inférieur au coût du verrouillage des données lors de la lecture.  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge plusieurs niveaux de contrôle de concurrence. Les utilisateurs spécifient le type de contrôle de concurrence lorsqu'ils choisissent les niveaux d'isolement des transactions pour les connexions et les options de concurrence sur les curseurs. Ces attributs peuvent être définis à l'aide des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] ou des propriétés et attributs des API de base de données telles que ADO, ADO.NET, OLE DB et ODBC.  
  
#### <a name="isolation-levels-in-the-ssdenoversion"></a>Niveaux d’isolation du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]  
 Les transactions spécifient un niveau d'isolement. Ce niveau définit le degré d'isolement d'une transaction par rapport aux modifications de ressource ou de données apportées par d'autres transactions. Les niveaux d'isolation déterminent les effets secondaires de la concurrence (lectures incorrectes, lectures fantômes) qui sont autorisés.  
  
 Le niveau d'isolation d'une transaction régit les éléments suivants :  
  
-   l'acquisition de verrous lors de la lecture de données, et le type de verrous nécessaires ;  
-   la durée de vie des verrous de lecture ;  
-   la réaction d'une opération de lecture qui fait référence à des lignes modifiées par une autre transaction :  
    -   blocage jusqu'à ce que le verrou exclusif sur la ligne soit levé,  
    -   récupération de la version validée de la ligne telle qu'elle était au début de l'instruction ou de la transaction,  
    -   lecture de la modification des données non validées.  
  
> [!IMPORTANT]  
> Le choix d'un niveau d'isolation n'a aucune influence sur les verrous acquis pour protéger les modifications de données. Une transaction acquiert toujours un verrou exclusif sur les données qu'elle modifie et garde celui-ci jusqu'à ce qu'elle ait terminé son travail, indépendamment du niveau d'isolation défini pour elle. Dans le cas des opérations de lecture, le niveau d'isolation d'une transaction définit principalement son niveau de protection contre les effets des modifications apportées par les autres transactions.  
  
 Plus le niveau d'isolation est faible, plus le nombre de personnes susceptibles d'accéder aux données en même temps est élevé, et plus les effets secondaires de la concurrence (lectures incorrectes, mises à jour perdues) sont nombreux. Inversement, plus le niveau d'isolation est élevé, plus le nombre de types d'effets secondaires de la concurrence qu'un utilisateur est susceptible de rencontrer est réduit. Cependant, la quantité de ressources système nécessaires et la probabilité d'un blocage mutuel de transactions sont plus élevées. Le choix du niveau d'isolation adéquat dépend d'une mise en équilibre de l'espace réservé et des exigences en matière d'intégrité des données de l'application. Le niveau le plus élevé, sérialisable, garantit qu'une transaction récupère exactement les mêmes données à chaque fois qu'elle répète une opération de lecture, mais en utilisant un niveau de verrouillage susceptible de gêner les autres utilisateurs dans les systèmes multi-utilisateurs. Le niveau le plus bas, lecture non validée, permet la récupération de données qui ont été modifiées mais non validées par d'autres transactions. Ce niveau permet l'apparition de tous les effets secondaires de la concurrence, mais la charge du système est réduite puisqu'il n'y a ni verrouillage de lecture, ni contrôle de version de ligne.  
  
##### <a name="database-engine-isolation-levels"></a>Niveaux d'isolation du moteur de base de données  
 La norme ISO définit les niveaux d'isolation suivants, tous pris en charge par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] :  
  
|Niveau d’isolation|Définition|  
|---------------------|----------------|  
|**Lecture non validée**|Le niveau d'isolement le plus bas et suffisant pour s'assurer que les données physiquement corrompues ne sont pas lues. À ce niveau, les lectures de modifications sont autorisées. Ainsi, une transaction peut afficher les modifications qui ne sont pas encore validées apportées par d'autres transactions.|  
|**Lecture validée**|Permet à une transaction de lire des données lues auparavant (non modifiées) par une autre transaction, sans attendre la fin de la première transaction. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] conserve les verrous d'écriture (acquis sur les données sélectionnées) jusqu'à la fin de la transaction, mais les verrous le lecture sont libérés dès que l'opération SELECT est terminée. C'est le niveau par défaut du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].|  
|**Lecture renouvelée**|Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] conserve les verrous de lecture et d'écriture acquis sur les données sélectionnées jusqu'à la fin de la transaction. Cependant, étant donné que les verrous d'étendus ne sont pas gérés, des lectures fantômes peuvent se produire.|  
|**Sérialisable**|Niveau le plus élevé, dans lequel les transactions sont totalement isolées les unes des autres. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] conserve les verrous de lecture et d'écriture acquis sur les données sélectionnées de façon à les libérer à la fin de la transaction. Les verrous d'étendues sont acquis lorsqu'une opération SELECT utilise une clause WHERE, plus particulièrement pour éviter les lectures fantômes.<br /><br /> **Remarque :** Les opérations DDL et les transactions sur les tables répliquées peuvent échouer lorsque le niveau d’isolation sérialisable est demandé. Cela est dû au fait que les requêtes de réplication utilisent des indicateurs qui peuvent être incompatibles avec le niveau d'isolation sérialisable.|  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend également en charge deux niveaux d'isolement de la transaction supplémentaires qui utilisent le contrôle de version de ligne. Le premier est une implémentation de l'isolement read committed, et le deuxième est un niveau d'isolement, l'instantané.  
  
|Niveau d'isolement basé sur le contrôle de version de ligne|Définition|  
|------------------------------------|----------------|  
|**Instantané read committed (RCSI)**|Lorsque l'option de base de données READ_COMMITTED_SNAPSHOT a la valeur ON, le niveau d'isolation read committed utilise le contrôle de version de ligne pour procurer une lecture cohérente au niveau des instructions. Les opérations de lecture ont besoin uniquement de verrous de table SCH-S, et pas de verrous de page ni de ligne. À savoir, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise le contrôle de version de ligne pour présenter à chaque instruction un instantané cohérent des données (du point de vue transactionnel) telles qu’elles étaient au début de l’instruction. Les verrous ne sont pas utilisés pour protéger les données des mises à jour par d'autres transactions. Une fonction définie par l'utilisateur peut retourner des données qui ont été validées après l'heure de début de l'instruction contenant cette fonction.<br /><br /> Si l’option de base de données `READ_COMMITTED_SNAPSHOT` a la valeur OFF (valeur par défaut), l’isolation read committed utilise des verrous partagés pour empêcher d’autres transactions de modifier des lignes pendant que la transaction active exécute une opération de lecture. Les verrous partagés empêchent également l'instruction de lire des lignes modifiées par d'autres transactions, tant que celles-ci ne sont pas terminées. Les deux variantes sont conformes à la définition de l'isolation read committed de l'ISO.|  
|**Instantané**|Le niveau d'isolation d'instantané utilise le contrôle de version de ligne pour assurer la cohérence des lectures au niveau de la transaction. Les opérations de lecture ont besoin uniquement de verrous de table SCH-S, et pas de verrous de page ni de ligne. Lorsque l'opération de lecture lit des lignes modifiées par une autre transaction, elle récupère la version de la ligne du début de la transaction. Vous pouvez utiliser l’isolation d’instantané pour une base de données uniquement quand la valeur ON est attribuée à l’option de base de données `ALLOW_SNAPSHOT_ISOLATION`. Par défaut, cette option est désactivée (OFF) pour les bases de données utilisateur.<br /><br /> **Remarque :** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne prend pas en charge le contrôle de version des métadonnées. Pour cette raison, il existe des restrictions sur les opérations DDL pouvant être effectuées dans une transaction explicite exécutée avec le niveau d'isolement d'instantané. Les instructions DDL suivantes ne sont pas autorisées en isolement d’instantanée après une instruction BEGIN TRANSACTION : ALTER TABLE, CREATE INDEX, CREATE XML INDEX, ALTER INDEX, DROP INDEX, DBCC REINDEX, ALTER PARTITION FUNCTION, ALTER PARTITION SCHEME ou toute instruction DDL CLR (Common Language Runtime). Ces instructions sont autorisées quand vous utilisez l’isolement d’instantané au sein de transactions implicites. Par définition, une transaction implicite est une instruction unique qui permet d'appliquer la sémantique de l'isolation d'instantané, même avec des instructions DDL. Tout manquement à ce principe peut provoquer l’erreur 3961 : `Snapshot isolation transaction failed in database '%.*ls' because the object accessed by the statement has been modified by a DDL statement in another concurrent transaction since the start of this transaction. It is not allowed because the metadata is not versioned. A concurrent update to metadata could lead to inconsistency if mixed with snapshot isolation.`|  
  
 Le tableau suivant répertorie les effets secondaires de la concurrence provoqués par les différents niveaux d'isolation.  
  
|Niveau d'isolation|Lecture incorrecte|Lecture non renouvelable|Fantôme|  
|---------------------|----------------|------------------------|-------------|  
|**Lecture non validée**|Oui|Oui|Oui|  
|**Lecture validée**|Non|Oui|Oui|  
|**Lecture renouvelée**|Non|Non|Oui|  
|**Instantané**|Non|Non|Non|  
|**Sérialisable**|Non|Non|Non|  
  
 Pour plus d’informations sur les types spécifiques de verrouillage ou de contrôle de version de ligne contrôlés par chaque niveau d’isolation de la transaction, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
 Les niveaux d'isolement des transactions peuvent être définis à l'aide de [!INCLUDE[tsql](../includes/tsql-md.md)] ou d'une API de base de données.  
  
 **[!INCLUDE[tsql](../includes/tsql-md.md)]**                                      
 Les scripts [!INCLUDE[tsql](../includes/tsql-md.md)] utilisent l’instruction `SET TRANSACTION ISOLATION LEVEL`.  
  
 **ADO**  
 Les applications ADO attribuent à la propriété `IsolationLevel` de l’objet **Connection** la valeur adXactReadUncommitted, adXactReadCommitted, adXactRepeatableRead ou adXactReadSerializable.  
  
 **ADO.NET**  
 Les applications ADO.NET qui utilisent l’espace de noms managé `System.Data.SqlClient` peuvent appeler la méthode `SqlConnection.BeginTransaction` et attribuer à l’option *IsolationLevel* la valeur Unspecified, Chaos, ReadUncommitted, ReadCommitted, RepeatableRead, Serializable et Snapshot.  
  
 **OLE DB**  
 Au début d’une transaction, les applications qui utilisent OLE DB appellent `ITransactionLocal::StartTransaction` avec la valeur ISOLATIONLEVEL_READUNCOMMITTED, ISOLATIONLEVEL_READCOMMITTED, ISOLATIONLEVEL_REPEATABLEREAD, ISOLATIONLEVEL_SNAPSHOT ou ISOLATIONLEVEL_SERIALIZABLE attribuée au paramètre *isoLevel*.  
  
 Lorsque vous spécifiez le niveau d'isolement d'une transaction en mode « autocommit », les applications OLE DB peuvent attribuer aux niveaux DBPROP_SESS_AUTOCOMMITISOLEVELS de la propriété DBPROPSET_SESSION la valeur DBPROPVAL_TI_CHAOS, DBPROPVAL_TI_READUNCOMMITTED, DBPROPVAL_TI_BROWSE, DBPROPVAL_TI_CURSORSTABILITY, DBPROPVAL_TI_READCOMMITTED, DBPROPVAL_TI_REPEATABLEREAD, DBPROPVAL_TI_SERIALIZABLE, DBPROPVAL_TI_ISOLATED ou DBPROPVAL_TI_SNAPSHOT.  
  
 **ODBC**  
 Les applications ODBC appellent `SQLSetConnectAttr` avec la valeur SQL_ATTR_TXN_ISOLATION pour le paramètre *Attribute* et la valeur SQL_TXN_READ_UNCOMMITTED, SQL_TXN_READ_COMMITTED, SQL_TXN_REPEATABLE_READ ou SQL_TXN_SERIALIZABLE pour le paramètre *ValuePtr*.  
  
 Pour les transactions d’instantanés, les applications appellent `SQLSetConnectAttr` avec la valeur SQL_COPT_SS_TXN_ISOLATION pour le paramètre Attribute et la valeur SQL_TXN_SS_SNAPSHOT pour le paramètre ValuePtr. Une transaction d'instantané peut être extraite à l'aide de SQL_COPT_SS_TXN_ISOLATION ou de SQL_ATTR_TXN_ISOLATION.  
  
##  <a name="locking-in-the-database-engine"></a><a name="Lock_Engine"></a> Verrouillage du moteur de base de données  
 Le verrouillage est un mécanisme utilisé par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pour synchroniser l'accès simultané de plusieurs utilisateurs à la même donnée.  
  
 Avant qu'une transaction acquière une dépendance sur l'état actuel d'un élément de données, par exemple par sa lecture ou la modification d'une donnée, elle doit se protéger des effets d'une autre transaction qui modifie la même donnée. Pour ce faire, la transaction demande un verrou sur l'élément de données. Le verrou possède plusieurs modes, par exemple partagé ou exclusif. Le mode de verrouillage définit le niveau de dépendance de la transaction sur les données. Aucune transaction ne peut obtenir un verrou qui entrerait en conflit avec le mode d'un verrou déjà accordé sur ces données à une autre transaction. Si une transaction demande un mode de verrouillage qui est en conflit avec un verrou déjà accordé aux mêmes données, l'instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] suspend la transaction concernée jusqu'à ce que le premier verrou soit libéré.  
  
 Lorsqu'une transaction modifie une donnée, elle conserve le verrou qui protège la modification jusqu'à la fin de la transaction. La durée pendant laquelle une transaction conserve le verrou acquis pour protéger les opérations de lecture dépend de la configuration du niveau d'isolement de la transaction. Tous les verrous conservés par une transaction sont libérés lorsque cette dernière est terminée (validée ou restaurée).  
  
 En général, les applications ne demandent pas de verrous directement. Les verrous sont gérés en interne par une partie du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], nommée gestionnaire de verrous. Lorsqu'une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] traite une instruction [!INCLUDE[tsql](../includes/tsql-md.md)], le processeur de requêtes du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] détermine les ressources qui doivent être accédées. Le processeur de requêtes détermine les types de verrou nécessaires pour protéger chaque ressource, en fonction du type d'accès et de la configuration du niveau d'isolement de la transaction. Le processeur de requêtes demande ensuite les verrous appropriés auprès du gestionnaire de verrous. Le gestionnaire de verrous accorde les verrous s'il n'existe aucun verrou en conflit détenu par d'autres transactions.  
  
## <a name="lock-granularity-and-hierarchies"></a>Granularité et hiérarchie des verrous  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] possède un verrouillage multigranulaire qui permet à différents types de ressources d'être verrouillés par une transaction. Pour minimiser le coût du verrouillage, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verrouille automatiquement les ressources au niveau approprié pour la tâche. Le verrouillage à un faible niveau de granularité (tel que les lignes) augmente la simultanéité d'accès, mais à un coût plus élevé, puisqu'un grand nombre de verrous doit être maintenu si de nombreuses lignes sont verrouillées. Le verrouillage à un niveau de granularité élevé (tel que les tables) est coûteux en termes de simultanéité d'accès, car le verrouillage d'une table entière empêche les autres transactions d'accéder à d'autres parties de la table. Cependant, son coût est moindre puisque les verrous sont peu nombreux.  
  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] doit souvent acquérir des verrous à plusieurs niveaux de granularité pour protéger intégralement une ressource. Ce groupe de verrous à plusieurs niveaux de granularité est appelé « hiérarchie des verrous ». Par exemple, pour protéger complètement la lecture d'un index, une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] devra peut-être acquérir des verrous partagés sur les lignes et des verrous partagés Intent sur les pages et la table.  
  
 Le tableau suivant répertorie les ressources que le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] peut verrouiller.  
  
|Ressource|Description|  
|--------------|-----------------|  
|RID|Identificateur de ligne utilisé pour verrouiller une seule ligne dans un segment de mémoire.|  
|KEY|Verrou de ligne dans un index utilisé pour protéger des groupes de clés dans les transactions susceptibles d'être sérialisées.|  
|PAGE|Page de 8 kilo-octets (Ko) dans une base de données, par exemple des pages de données ou d'index.|  
|EXTENT|Groupe contigu de huit pages, par exemple des pages de données ou d'index.|  
|HoBT|Segment de mémoire ou arbre B (B-tree). Verrou protégeant un arbre B (B-tree) (index) ou les pages de données de segment de mémoire d'une table ne possédant pas d'index cluster.|  
|TABLE|Table complète comprenant tous les index et toutes les données.|  
|FILE|Fichier de base de données.|  
|APPLICATION|Ressource spécifiée par une application.|  
|METADATA|Verrous des métadonnées.|  
|ALLOCATION_UNIT|Unité d'allocation.|  
|DATABASE|Base de données complète.|  
  
> [!NOTE]  
> Les verrous HoBT et TABLE peuvent être affectés par l’option LOCK_ESCALATION de l’instruction [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="lock-modes"></a><a name="lock_modes"></a> Modes de verrouillage  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verrouille les ressources en utilisant différents modes de verrouillage qui déterminent le mode d'accès aux ressources par des transactions simultanées.  
  
 Le tableau suivant illustre les modes de verrouillage des ressources utilisés par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
|Mode de verrouillage|Description|  
|---------------|-----------------|  
|**Partagé (S)**|Utilisé pour les opérations de lecture qui n'effectuent aucune modification ou mise à jour des données, par exemple une instruction SELECT.|  
|**Mise à jour (U)**|Utilisé pour les ressources pouvant être mises à jour. Empêche une forme de blocage courante qui se produit lorsque plusieurs sessions lisent, verrouillent et mettent à jour des ressources ultérieurement.|  
|**Exclusif (X)**|Utilisé par les opérations de modification de données, telles que INSERT, UPDATE ou DELETE. Empêche des mises à jour multiples sur la même ressource au même moment.|  
|**Intention**|Permet d'établir une hiérarchie de verrouillage. Les types de verrouillage intentionnels sont les suivants : les verrous de partage intentionnel (IS), d'exclusion intentionnelle (IX) et de partage intentionnel exclusif (SIX).|  
|**Schéma**|Utilisé lors de l'exécution d'une opération associée au schéma d'une table. Les types de verrouillage de schéma sont les suivant : la stabilité du schéma (Sch-S) et la modification du schéma (Sch-M).|  
|**Mise à jour en bloc (BU)**|Utilisé lors de la copie en bloc de données dans une table avec l’indicateur **TABLOCK** spécifié.|  
|**Groupe de clés**|Protège la plage de lignes lue par une requête lorsque le niveau d'isolation des transactions SERIALIZABLE est utilisé. Garantit qu'aucune autre transaction ne peut insérer des lignes susceptibles de répondre aux requêtes de la transaction sérialisable si ces dernières étaient réexécutées.|  
  
### <a name="shared-locks"></a><a name="shared"></a> Verrous partagés  
 Les verrous partagés (S) permettent à des transactions simultanées de lire (SELECT) une ressource dans des conditions de contrôle d'accès concurrentiel pessimiste. Aucune autre transaction ne peut modifier les données de la ressource tant que des verrous partagés (S) existent sur la ressource. Les verrous partagés (S) sur une ressource sont enlevés dès que l'opération de lecture est terminée, à moins que le niveau d'isolation de la transaction soit de type lecture renouvelable ou plus élevé, ou qu'un indicateur de verrouillage conserve les verrous partagés (S) pendant toute la durée de la transaction.  
  
### <a name="update-locks"></a><a name="update"></a> Verrous de mise à jour  
 Les verrous de mise à jour (U) empêchent une forme fréquente de blocage. Une transaction isolée avec le niveau sérialisable ou de lecture renouvelable lit les données en obtenant un verrou partagé (S) sur la ressource (page ou ligne), puis modifie ces données, ce qui nécessite une conversion du verrou en mode exclusif (X). Si deux transactions acquièrent des verrous partagés sur une ressource et tentent ensuite de mettre à jour des données de manière simultanée, une transaction tente de convertir le verrou en verrou exclusif (X). La conversion du verrou partagé au mode de verrou exclusif reste en attente, car le verrou exclusif de la première transaction n'est pas compatible avec le verrou partagé de l'autre transaction. Une attente de verrouillage se produit alors. La deuxième transaction impliquée essaie d'acquérir un verrou exclusif (X) pour sa mise à jour. Puisque les deux transactions effectuant la conversion en verrous exclusifs (X) attendent que l'autre transaction libère son verrou partagé, un blocage se produit.  
  
 Les verrous de mise à jour (U) permettent de résoudre les problèmes de blocage. Une seule transaction à la fois peut obtenir un verrou de mise à jour (U) pour une ressource. Si une transaction modifie une ressource, le verrou de mise à jour (U) est converti en verrou exclusif (X).  
  
### <a name="exclusive-locks"></a><a name="exclusive"></a> Verrous exclusifs  
 Les verrous exclusifs (X) empêchent l'accès à une ressource par des transactions simultanées. Un verrou exclusif (X) empêche toute autre transaction de modifier les données ; les opérations de lecture ne peuvent avoir lieu qu'avec l'indicateur NOLOCK ou le niveau d'isolation « lecture non validée ».  
  
 Les instructions qui modifient les données telles que INSERT, UPDATE et DELETE combinent des opérations de modification et de lecture. Elles commencent par les opérations de lecture pour obtenir les données, puis elles effectuent les opérations de modification. Par conséquent, les instructions qui modifient les données demandent généralement à la fois des verrous partagés et des verrous exclusifs. Ainsi, une instruction UPDATE peut modifier les lignes d'une table en fonction d'une jointure avec une autre table. Dans ce cas, l'instruction UPDATE demande des verrous partagés sur les lignes lues dans la table jointe en plus des verrous exclusifs sur les lignes mises à jour.  
  
### <a name="intent-locks"></a><a name="intent"></a> Verrous intentionnels  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise des verrous intentionnels pour protéger le placement de verrous partagés (S) ou exclusifs (X) sur une ressource hiérarchiquement inférieure. Les verrous intentionnels sont appelés ainsi parce qu'ils sont obtenus avant un verrou de niveau inférieur et signalent par conséquent l'intention de placer des verrous à un niveau inférieur.  
  
 Les verrous intentionnels ont deux fonctions :  
  
-   Empêcher les autres transactions de modifier la ressource de niveau supérieur de façon à invalider le verrou au niveau inférieur. 
-   Améliorer l’efficacité du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] en matière de détection de conflits de verrous au plus haut niveau de granularité.  
  
 Par exemple, un verrou de partage intentionnel est demandé au niveau table avant une demande de verrous partagés (S) sur les pages ou les lignes de cette table. Un verrou intentionnel placé au niveau de la table empêche une autre transaction d'acquérir un verrou exclusif (X) sur la table contenant cette page. Les verrous intentionnels améliorent les performances, car le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] n’examine les verrous intentionnels qu’au niveau des tables afin de déterminer si une transaction peut acquérir un verrou en toute sécurité sur chaque table. Cela supprime la nécessité d'examiner chaque verrou de ligne ou page pour déterminer si une transaction peut verrouiller la table entière.  
  
<a name="lock_intent_table"></a> Les verrous intentionnels comprennent les verrous de partage intentionnel (IS), d'exclusion intentionnelle (IX) et de partage intentionnel exclusif (SIX).  
  
|Mode de verrouillage|Description|  
|---------------|-----------------|  
|**Intent partagé (IS)**|Protège les verrous partagés demandés ou acquis sur certaines ressources (mais pas toutes) de niveau inférieur dans la hiérarchie.|  
|**Intent exclusif (IX)**|Protège les verrous exclusifs demandés ou acquis sur certaines ressources (mais pas toutes) de niveau inférieur dans la hiérarchie. IX est un sur-ensemble du mode IS qui protège également les demandes de verrou partagé sur des ressources de niveau inférieur.|  
|**Partagé avec intent exclusif (SIX)**|Protège les verrous partagés (S) demandés ou acquis sur toutes les ressources de niveau inférieur dans la hiérarchie et les verrous d'exclusion intentionnelle sur certaines ressources de niveau inférieur (mais pas toutes). Les lectures simultanées sur les ressources de niveau supérieur sont autorisées. Par exemple, l'obtention d'un verrou SIX sur une table permet également d'obtenir des verrous d'exclusion intentionnelle sur les pages modifiées et des verrous exclusifs sur les lignes modifiées. Il ne peut y avoir qu'un seul verrou SIX par ressource à la fois pour empêcher la mise à jour des ressources par une autre transaction, bien que cette dernière peut lire les ressources inférieures dans la hiérarchie en obtenant des verrous IS au niveau des tables.|  
|**Mise à jour intentionnelle (IU)**|Protège les verrous de mise à jour demandés ou acquis sur toutes les ressources de niveau inférieur dans la hiérarchie. Les verrous IU sont utilisés uniquement sur les ressources de page. Les verrous IU sont convertis en verrous IX si une opération de mise à jour a lieu.|  
|**Mise à jour intentionnelle partagée (SIU)**|Combinaison de verrous S et IU résultant de l'acquisition séparée de ces verrous et de leur gestion simultanée. Par exemple, une transaction peut exécuter une requête avec l'indicateur PAGLOCK, puis une opération de mise à jour. La requête contenant l'indicateur PAGLOCK obtient le verrou S et l'opération de mise à jour obtient le verrou IU.|  
|**Mise à jour intentionnelle exclusive (UIX)**|Combinaison de verrous U et IX résultant de l'acquisition séparée de ces verrous et de leur gestion simultanée.|  
  
### <a name="schema-locks"></a><a name="schema"></a> Verrous de schéma  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise les verrous de modification de schémas (Sch-M) quand une opération de langage de définition de données (DDL, Data Definition Language) est effectuée sur une table (ajout d’une colonne ou suppression d’une table, par exemple). Pendant le temps de sa détention, le verrou Sch-M empêche les accès simultanés à la table. Cela signifie que le verrou Sch-M bloque toutes les opérations externes jusqu'à ce que le verrou soit libéré.  
  
 Certaines opérations DML(langage de manipulation de données), comme la troncation de table, utilisent les verrous SCH-M pour empêcher l'accès aux tables affectées par des opérations simultanées.  
  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise les verrous de stabilité de schéma (Sch-S) lors de la compilation et l’exécution des requêtes. Les verrous Sch-S ne bloquent aucun verrou transactionnel, verrous exclusifs (X) y compris. Par conséquent, les autres transactions, y compris celles avec des verrous exclusifs (X) sur une table, continuent à s'exécuter pendant la compilation d'une requête. Toutefois, les opérations DDL simultanées, ainsi que les opérations DML simultanées qui définissent des verrous Sch-M, ne peuvent pas être exécutées sur la table.  
  
### <a name="bulk-update-locks"></a><a name="bulk_update"></a> Verrous de mise à jour en bloc (BU)  
 Les verrous BU permettent à plusieurs threads de charger simultanément en masse des données dans la même table tout en empêchant les processus qui n'effectuent pas de chargement de données en masse d'accéder à cette table. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise des verrous BU lorsque les deux conditions suivantes sont vraies.  
  
-   Vous utilisez l’instruction [!INCLUDE[tsql](../includes/tsql-md.md)] BULK INSERT ou la fonction OPENROWSET(BULK), ou l’une des commandes d’API Bulk Insert, telles que .NET SqlBulkCopy, les API OLEDB Fast Load ou les API ODBC Bulk Copy pour copier en bloc des données dans une table.  
-   L’indicateur **TABLOCK** est spécifié ou l’option de table **table lock on bulk load** est définie à l’aide de **sp_tableoption**.  
  
> [!TIP]  
> Contrairement à l’instruction BULK INSERT, qui maintient un verrou de mise à jour en bloc moins restrictif, INSERT INTO...SELECT avec l’indicateur TABLOCK maintient un verrou exclusif (X) sur la table. Cela signifie que vous ne pouvez pas insérer de lignes à l'aide d'opérations d'insertion parallèles.  
  
### <a name="key-range-locks"></a><a name="key_range"></a> Verrous d’étendues de clés  
 Les verrous d'étendues de clés protègent une plage de lignes implicitement incluses dans un jeu d'enregistrements lu par une instruction [!INCLUDE[tsql](../includes/tsql-md.md)] lors de l'utilisation du niveau d'isolement des transactions sérialisable. Le verrouillage d'étendues de clés empêche les lectures fantômes. Les verrous d'étendues de clés couvrent des enregistrements individuels et les étendues entre les enregistrements, empêchant les insertions ou les suppressions fantômes dans un ensemble d'enregistrements auquel accède une transaction.  
  
## <a name="lock-compatibility"></a><a name="lock_compatibility"></a> Compatibilité de verrouillage  
 La compatibilité de verrouillage détermine si plusieurs transactions peuvent simultanément acquérir des verrous sur la même ressource. Si une ressource est déjà verrouillée par une autre transaction, une demande de nouveau verrou ne peut être accordée que si le mode du verrou demandé est compatible avec celui du verrou existant. Si le mode du verrou demandé n'est pas compatible avec le verrou existant, la transaction qui demande le nouveau verrou attend que le verrou existant soit libéré ou que l'intervalle de délai de verrouillage ait expiré. Par exemple, aucun mode de verrou n'est compatible avec les verrous exclusifs. Lorsqu'un verrou exclusif (X) est posé, aucune autre transaction ne peut acquérir un verrou de quelque sorte que ce soit (partagé, mise à jour, exclusif) sur cette ressource tant que le verrou exclusif (X) n'a pas été libéré. Inversement, si un verrou partagé (S) a été appliqué à une ressource, les autres transactions peuvent aussi acquérir un verrou partagé ou de mise à jour (U) sur cet élément, même si la première transaction n'est pas terminée. Toutefois, les autres transactions ne peuvent pas acquérir un verrou exclusif tant que le verrou partagé n'a pas été libéré.  
  
<a name="lock_compat_table"></a> Le tableau suivant décrit la compatibilité des modes de verrouillage les plus courants.  
  
||Mode accordé existant||||||  
|------|---------------------------|------|------|------|------|------|  
|**Mode requis**|**IS**|**S**|**U**|**IX**|**SIX**|**X**|  
|**Intent partagé (IS)**|Oui|Oui|Oui|Oui|Oui|Non|  
|**Partagé (S)**|Oui|Oui|Oui|Non|Non|Non|  
|**Mise à jour (U)**|Oui|Oui|Non|Non|Non|Non|  
|**Intent exclusif (IX)**|Oui|Non|Non|Oui|Non|Non|  
|**Partagé avec intent exclusif (SIX)**|Oui|Non|Non|Non|Non|Non|  
|**Exclusif (X)**|Non|Non|Non|Non|Non|Non|  
  
> [!NOTE]  
> Un verrou intent exclusif (IX) est compatible avec un mode de verrouillage IX car IX signifie l'intention de mettre à jour uniquement certaines lignes et non pas toutes les lignes. Les autres transactions qui essaient de lire ou de mettre à jour certaines lignes sont aussi autorisées si elles ne mettent pas à jour les lignes en cours de mise à jour par les autres transactions. De plus, si deux transactions essaient de mettre à jour la même ligne, un verrou IX sera accordé aux deux transactions au niveau de la table et de la page. Toutefois, un verrou X sera accordé à une transaction au niveau de la ligne. L'autre transaction doit attendre jusqu'à ce que le verrouillage au niveau de la ligne soit supprimé.  
  
<a name="lock_matrix"></a> Utilisez le tableau suivant pour déterminer la compatibilité de tous les modes de verrouillage disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 ![lock_conflicts](../relational-databases/media/LockConflictTable.png)  
  
## <a name="key-range-locking"></a>Verrouillage d'étendues de clés  
 Les verrous d'étendues de clés protègent une plage de lignes implicitement incluses dans un jeu d'enregistrements lu par une instruction [!INCLUDE[tsql](../includes/tsql-md.md)] lors de l'utilisation du niveau d'isolement des transactions sérialisable. Le niveau d'isolement sérialisable exige que toute requête exécutée pendant une transaction obtienne le même jeu de lignes à chaque exécution lors de la transaction. Un verrou d'étendues de clés protège cette exigence en empêchant d'autres transactions d'insérer de nouvelles lignes dont les clés sont comprises dans la plage des clés lues par la transaction sérialisable.  
  
 Le verrouillage d'étendues de clés empêche les lectures fantômes. La protection des étendues de clés entre les lignes permet également d'empêcher les insertions fantômes dans un jeu d'enregistrements auquel une transaction accède.  
  
 Un verrou d'étendues de clés est placé sur un index, spécifiant une valeur de clé de début et de fin. Ce verrou bloque toute tentative d'insertion, de mise à jour ou de suppression de ligne possédant une valeur de clé comprise dans cette étendue, car ces opérations doivent d'abord acquérir un verrou sur l'index. Par exemple, une transaction sérialisable peut émettre une instruction `SELECT` qui lit toutes les lignes dont les valeurs de clés remplissent la condition `BETWEEN 'AAA' AND 'CZZ'`. Un verrou de groupes de clés sur les valeurs de clés comprises entre **'** AAA **'** et **'** CZZ **'** empêche les autres transactions d’insérer des lignes possédant des valeurs de clés comprises dans ce groupe, telles que **'** ADG **'** , **'** BBD **'** ou **'** CAL **'** .  
  
### <a name="key-range-lock-modes"></a><a name="key_range_modes"></a> Modes de verrouillage d'étendues de clés  
 Les verrous d'étendues de clés comprennent un composant étendue et un composant ligne, au format étendue-ligne :  
  
-   L'étendue représente le mode de verrouillage protégeant l'étendue entre deux entrées d'index successives.  
-   La ligne représente le mode de verrouillage protégeant l'entrée de l'index.  
-   Le mode représente la combinaison de modes de verrouillage utilisée. Les modes de verrouillage d'étendues de clés comportent deux parties. La première représente le type de verrou utilisé pour verrouiller l’étendue d’index (Range *T*) et la deuxième représente le type de verrou utilisé pour verrouiller une clé spécifique (*K*). Les deux parties sont reliées par un tiret (-), par exemple Range *T*-*K*.  
  
    |Plage|Ligne|Mode|Description|  
    |-----------|---------|----------|-----------------|  
    |RangeS|S|RangeS-S|Verrou de ressource partagé, étendue partagée ; analyse de plage sérialisable.|  
    |RangeS|U|RangeS-U|Verrou de mise à jour de ressource, étendue partagée ; analyse d'étendue sérialisable.|  
    |RangeI|Null|RangeI-N|Verrou de ressource NULL, étendue d'insertion ; utilisé pour tester les étendues avant l'insertion d'une nouvelle clé dans un index.|  
    |RangeX|X|RangeX-X|Verrou de ressource exclusif, étendue exclusive ; utilisé lors de la mise à jour d'une clé dans une étendue.|  
  
> [!NOTE]  
> Le mode interne de verrouillage Null est compatible avec tous les autres modes de verrouillage.  
  
 Les modes de verrouillage d'étendues de clés possèdent un tableau de compatibilité qui indique quels verrous sont compatibles avec les autres verrous obtenus sur les clés et étendues se chevauchant.  
  
||Mode accordé existant|||||||  
|------|---------------------------|------|------|------|------|------|------|  
|**Mode requis**|**S**|**U**|**X**|**RangeS-S**|**RangeS-U**|**RangeI-N**|**RangeX-X**|  
|**Partagé (S)**|Oui|Oui|Non|Oui|Oui|Oui|Non|  
|**Mise à jour (U)**|Oui|Non|Non|Oui|Non|Oui|Non|  
|**Exclusif (X)**|Non|Non|Non|Non|Non|Oui|Non|  
|**RangeS-S**|Oui|Oui|Non|Oui|Oui|Non|Non|  
|**RangeS-U**|Oui|Non|Non|Oui|Non|Non|Non|  
|**RangeI-N**|Oui|Oui|Oui|Non|Non|Oui|Non|  
|**RangeX-X**|Non|Non|Non|Non|Non|Non|Non|  
  
### <a name="conversion-locks"></a><a name="lock_conversion"></a> Verrous de conversion  
 Les verrous de conversion sont créés lorsqu'un verrou d'étendue de clés chevauche un autre verrou.  
  
|Verrou 1|Verrou 2|Verrou de conversion|  
|------------|------------|---------------------|  
|S|RangeI-N|RangeI-S|  
|U|RangeI-N|RangeI-U|  
|X|RangeI-N|RangeI-X|  
|RangeI-N|RangeS-S|RangeX-S|  
|RangeI-N|RangeS-U|RangeX-U|  
  
 Les verrous de conversion peuvent être observés pendant une courte période dans différentes circonstances complexes, parfois lors de l'exécution de processus concurrents.  
  
### <a name="serializable-range-scan-singleton-fetch-delete-and-insert"></a>Analyse d'étendue sérialisable, extraction singleton, suppression et insertion  
 Le verrouillage d'étendues de clés permet la sérialisation des opérations suivantes :  
  
-   Requête d'analyse d'étendue  
-   Extraction singleton de ligne inexistante  
-   Opération de suppression  
-   Opération d'insertion  
  
 Les conditions suivantes doivent être satisfaites pour qu'un verrouillage d'étendues de clés puisse se produire :  
  
-   Le niveau d'isolement de la transaction doit être défini sur SERIALIZABLE.  
-   Le processeur de requêtes doit utiliser un index pour implémenter le prédicat de filtre de l'étendue. Par exemple, la clause WHERE dans une instruction SELECT peut établir une condition d’étendue avec le prédicat suivant : ColonneX BETWEEN N **’** AAA **’** AND N **’** CZZ **’** . Un verrou de groupes de clés ne peut être acquis que si **ColumnX** est couvert par une clé d’index.  
  
### <a name="examples"></a>Exemples  
 La table et l'index suivants sont utilisés comme base pour les exemples de verrouillage d'étendues de clés ci-dessous.  
  
 ![btree](../relational-databases/media/btree4.png)  
  
#### <a name="range-scan-query"></a>Requête d'analyse d'étendue  
 Pour qu'une requête d'analyse d'étendue soit sérialisable, cette requête doit retourner les mêmes résultats chaque fois qu'elle est exécutée dans la même transaction. De nouvelles lignes ne doivent pas être insérées dans la requête d'analyse d'étendue par d'autres transactions, sinon celles-ci deviennent des insertions fantômes. Par exemple, la requête suivante utilise la table et l'index de l'illustration précédente :  
  
```sql  
SELECT name  
FROM mytable  
WHERE name BETWEEN 'A' AND 'C';  
```  
  
 Des verrous de groupes de clés sont placés sur les entrées d’index correspondant à l’étendue de lignes de données dans laquelle le nom se trouve entre `Adam` et `Dale`, empêchant l’insertion ou la suppression de nouvelles lignes correspondant à la requête précédente. Bien que le premier nom de l’étendue soit `Adam`, le verrou de groupes de clés du mode RangeS-S sur cette entrée d’index veille à ce qu’aucun nouveau nom commençant par la lettre A ne soit ajouté avant `Adam`, comme `Abigail`. De manière similaire, le verrou de groupes de clés RangeS-S sur l’entrée d’index pour `Dale` fait en sorte qu’aucun nom commençant par C ne puisse être ajouté après `Carlos`, comme `Clive`.  
  
> [!NOTE]  
> Le nombre de verrous RangeS-S maintenus est *n*+1, où *n* est le nombre de lignes répondant aux critères de la requête.  
  
#### <a name="singleton-fetch-of-nonexistent-data"></a>Extraction d'un singleton de données non existantes  
 Si une requête à l'intérieur d'une transaction tente de sélectionner une ligne qui n'existe pas, l'exécution de la requête plus loin dans la même transaction doit retourner le même résultat. Aucune autre transaction ne peut être autorisée à insérer cette ligne inexistante. Supposons par exemple la requête suivante :  
  
```sql  
SELECT name  
FROM mytable  
WHERE name = 'Bill';  
```  
  
 Un verrou d'étendues de clés est placé sur l'entrée d'index correspondant à l'étendue de noms se trouvant entre `Ben` et `Bing`, car le nom `Bill` serait inséré entre ces deux entrées d'index adjacentes. Le verrou d'étendues de clés du mode RangeS-S est placé sur l'entrée d'index `Bing`. Ceci empêche toute autre transaction d'insérer des valeurs, telles que `Bill`, entre les entrées d'index `Ben` et `Bing`.  
  
#### <a name="delete-operation"></a>Opération de suppression  
 Lors de la suppression d'une valeur dans une transaction, l'étendue dans laquelle la valeur se trouve ne doit pas nécessairement être verrouillée pendant toute la durée de la transaction effectuant l'opération de suppression. Le verrouillage de la valeur de clé supprimée jusqu'à la fin de la transaction est suffisant pour assurer la sérialisation. Par exemple, pour l'instruction DELETE suivante :  
  
```sql  
DELETE mytable  
WHERE name = 'Bob';  
```  
  
 Un verrou exclusif (X) est placé sur l'entrée d'index correspondant au nom `Bob`. Les autres transactions peuvent insérer ou supprimer des valeurs avant ou après la valeur effacée `Bob`. Toutefois, toute transaction tentant de lire, insérer ou supprimer la valeur `Bob` sera bloquée jusqu'à ce que la transaction effectuant la suppression soit validée ou restaurée.  
  
 La suppression d'étendues peut être exécutée à l'aide de trois modes de verrouillage de base : verrouillage de ligne, de page ou de table. La stratégie de verrouillage de ligne, de page ou de table est décidée par l’optimiseur de requête, ou peut être spécifiée par l’utilisateur via des options d’optimiseur de requête comme ROWLOCK, PAGLOCK ou TABLOCK. Lorsque l'option PAGLOCK ou TABLOCK est utilisée, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] désalloue immédiatement une page d'index page si toutes les lignes qu'elle contient sont supprimées. En revanche, lorsque l'option ROWLOCK est utilisée, toutes les lignes supprimées sont uniquement marquées en tant que telles ; elles sont effectivement retirées de la page d'index ultérieurement, à l'aide d'une tâche d'arrière-plan.  
  
#### <a name="insert-operation"></a>Opération d'insertion  
 Lors de l'insertion d'une valeur à l'intérieur d'une transaction, l'étendue dans laquelle la valeur se trouve ne doit pas nécessairement être verrouillée pendant la durée de l'opération effectuant l'opération d'insertion. Le verrouillage de la valeur de clé jusqu'à la fin de la transaction suffit pour assurer la sérialisation. Par exemple, étant donné l'instruction INSERT suivante :  
  
```sql  
INSERT mytable VALUES ('Dan');  
```  
  
 Le verrou d'étendues de clés du mode RangeI-N est placé sur l'entrée d'index correspondant au nom David pour le test de l'étendue. Si le verrou est accordé, la valeur `Dan` est insérée et un verrou exclusif (X) est placé sur la valeur `Dan`. Le verrou d'étendues de clés du mode RangeI-N est uniquement nécessaire pour le test de l'étendue et n'est pas maintenu pendant la durée de la transaction effectuant l'opération d'insertion. D'autres transactions peuvent insérer ou supprimer des valeurs avant ou après la valeur `Dan` insérée. Toutefois, toute transaction essayant de lire, écrire ou supprimer la valeur `Dan` est verrouillée jusqu'à ce que la transaction d'insertion soit validée ou restaurée.  
  
## <a name="lock-escalation"></a>Escalade de verrous
L'escalade de verrous est le processus de conversion d'un grand nombre de verrous détaillés en verrous moins détaillés, tout en réduisant la charge du système et en augmentant la probabilité de contention de simultanéité.

Quand le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] acquiert des verrous de bas niveau, il place également des verrous intentionnels sur les objets qui contiennent les objets de niveau inférieur :

-   Lors du verrouillage de lignes ou de plages de clés d’index, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] place un verrou intentionnel sur les pages qui contiennent les lignes ou les clés.
-   Lors du verrouillage de pages, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] place un verrou intentionnel sur les objets de niveau supérieur qui contiennent ces pages. Outre le verrou intentionnel sur l'objet, des verrous de page intentionnels sont demandés sur les objets suivants :
    -  Pages de niveau feuille d'index non-cluster
    -  Pages de données d'index cluster
    -  Pages de données de segment de mémoire

Le [!INCLUDE[ssde_md](../includes/ssde_md.md)] peut effectuer un verrouillage de ligne et un verrouillage de page pour la même instruction afin de réduire au minimum le nombre de verrous et la probabilité qu’il faille en escalader. Par exemple, le moteur de base de données peut placer des verrous de page sur un index non cluster (si suffisamment de clés contiguës dans le nœud d’index sont sélectionnées pour satisfaire à la requête) et des verrous de ligne sur les données.

Pour escalader des verrous, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] essaie de remplacer le verrou intentionnel sur la table par le verrou complet correspondant, par exemple, en substituant un verrou exclusif (X) à un verrou intentionnel exclusif (IX) ou un verrou partagé (S) à un verrou intentionnel partagé (IS). Si la tentative d'escalade de verrous réussit et que le verrou de table complet est acquis, tous les verrous de page (PAGE), de niveau ligne (RID), de segment ou d'arbre B (B-tree) détenus par la transaction sur le segment ou sur l'index sont libérés. Si le verrou complet ne peut pas être acquis, aucune escalade de verrous ne se produit à ce stade et le moteur de base de données continue d’acquérir des verrous de ligne, de clé ou de page.

Le [!INCLUDE[ssde_md](../includes/ssde_md.md)] n’escalade pas les verrous de ligne ou de plage de clés en verrous de page, mais il les escalade directement en verrous de table. De même, les verrous de page sont systématiquement escaladés en verrous de table. Le verrouillage des tables partitionnées peut être escaladé au niveau HoBT de la partition associée plutôt qu’au verrou de la table. Un verrou au niveau du HoBT ne verrouille pas nécessairement les HoBT alignés pour la partition.

> [!NOTE]
> Les verrous au niveau du HoBT augmentent habituellement la concurrence, mais introduisent le potentiel des blocages lorsque les transactions qui verrouillent différentes partitions souhaitent étendre leurs verrous exclusifs aux autres partitions. Dans de rares instances, la granularité de verrouillage de TABLE peut s'avérer plus efficace.

Si une tentative d’escalade de verrous échoue en raison de verrous en conflit détenus par des transactions simultanées, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] renouvelle la tentative d’escalade de verrous chaque fois que la quantité de verrous supplémentaires acquis par la transaction atteint 1 250.

Chaque événement d’escalade fonctionne essentiellement au niveau d’une instruction [!INCLUDE[tsql](../includes/tsql-md.md)] unique. Lorsque l’événement démarre, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] essaie d’escalader tous les verrous détenus par la transaction en cours dans toutes les tables ayant été référencées par l’instruction active, sous réserve que celle-ci satisfasse aux contraintes de seuil d’escalade. Si l'événement d'escalade démarre avant que l'instruction n'ait accédé à une table, aucune tentative n'est réalisée pour escalader les verrous sur cette table. Si l'escalade de verrous réussit, tous les verrous acquis par la transaction dans une instruction antérieure et toujours détenus au moment du démarrage de l'événement sont escaladés si la table est référencée par l'instruction en cours et qu'elle figure dans l'événement d'escalade.

Par exemple, supposons qu'une session effectue les opérations suivantes :

-  Commence une transaction.
-  Met à jour `TableA`. Cette opération génère dans TableA des verrous de ligne exclusifs détenus jusqu'à la fin de la transaction.
-  Met à jour `TableB`. Cette opération génère dans TableB des verrous de ligne exclusifs détenus jusqu'à la fin de la transaction.
-  Exécute une instruction SELECT qui joint `TableA` à `TableC`. Le plan d’exécution de requête demande à ce que les lignes soient extraites de `TableA` avant d’être extraites de `TableC`.
-  L’instruction SELECT déclenche une escalade de verrous pendant qu’elle extrait les lignes de `TableA` et avant d’accéder à `TableC`.

Si l’escalade de verrous réussit, seuls les verrous détenus par la session sur `TableA` sont escaladés. Cela inclut les verrous partagés acquis par l'instruction SELECT et les verrous exclusifs acquis par l'instruction UPDATE antérieure Alors que seuls les verrous acquis par la session dans `TableA` pour l’instruction SELECT sont décomptés pour déterminer si l’escalade de verrous doit être réalisée, une fois que celle-ci a réussi, tous les verrous détenus par la session dans `TableA` sont escaladés en verrou exclusif sur la table et tous les autres verrous de granularité inférieure de `TableA`, y compris les verrous intentionnels, sont libérés.

Aucune tentative n’est réalisée pour escalader les verrous de `TableB` car l’instruction SELECT ne comportait aucune référence active à `TableB`. De même, aucune tentative n’est réalisée pour escalader les verrous de `TableC` car l’instruction n’avait toujours pas accédé à cette table au moment de l’escalade.

### <a name="lock-escalation-thresholds"></a>Seuils d'escalade de verrous

L’escalade de verrous est déclenchée lorsqu’elle n’est pas désactivée sur la table à l’aide de l’option `ALTER TABLE SET LOCK_ESCALATION` et lorsque l’une ou l’autre des conditions suivantes existe :

-  Une instruction [!INCLUDE[tsql](../includes/tsql-md.md)] unique acquiert au moins 5 000 verrous sur une table ou un index unique et non partitionné.
-  Une instruction [!INCLUDE[tsql](../includes/tsql-md.md)] unique acquiert au moins 5 000 verrous sur une partition unique d’une table partitionnée et l’option `ALTER TABLE SET LOCK_ESCALATION` a la valeur AUTO.
-  Le nombre de verrous dans une instance du [!INCLUDE[ssde_md](../includes/ssde_md.md)] dépasse les seuils de mémoire ou de configuration.

Si des verrous ne peuvent pas être escaladés en raison de verrous en conflit, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] déclenche régulièrement une escalade de verrous chaque fois que le nombre de nouveaux verrous acquis atteint 1 250.

### <a name="escalation-threshold-for-a-transact-sql-statement"></a>Seuil d'escalade pour une instruction Transact-SQL
Quand le [!INCLUDE[ssde_md](../includes/ssde_md.md)] vérifie les escalades possibles chaque fois que 1 250 verrous ont été acquis, une escalade de verrous se produit si et seulement si une instruction [!INCLUDE[tsql](../includes/tsql-md.md)] a acquis au moins 5 000 verrous sur une référence unique d’une table. L’escalade de verrous est déclenchée quand une instruction [!INCLUDE[tsql](../includes/tsql-md.md)] acquiert au moins 5 000 verrous sur une seule référence d’une table. Par exemple, l'escalade de verrous n'est pas déclenchée si une instruction acquiert 3 000 verrous dans un index et 3 000 verrous dans un autre index de la même table. De même, l'escalade de verrous n'est pas déclenchée si une instruction possède une jointure réflexive sur une table et que chaque référence à la table n'acquiert que 3 000 verrous dans celle-ci.

L'escalade de verrous ne se produit que pour les tables auxquelles l'instruction a accédé au moment du déclenchement de l'escalade. Supposons une instruction SELECT unique représentant une jointure qui accède successivement à trois tables : `TableA`, `TableB` et `TableC`. L’instruction acquiert 3 000 verrous de ligne dans l’index cluster de `TableA` et au moins 5 000 verrous de ligne dans l’index cluster de `TableB`, mais n’a toujours pas accédé à `TableC`. Lorsque le [!INCLUDE[ssde_md](../includes/ssde_md.md)] détecte que l’instruction a acquis au moins 5 000 verrous de ligne dans `TableB`, il essaie d’escalader tous les verrous détenus par la transaction en cours dans `TableB`. Il essaie également d’escalader tous les verrous détenus par la transaction en cours dans `TableA` mais, dans la mesure où le nombre de verrous dans `TableA` est inférieur à 5 000, l’escalade échoue. Aucune tentative d’escalade de verrous n’est réalisée pour `TableC` car l’instruction n’y avait toujours pas accédé lorsque l’escalade s’est produite.

### <a name="escalation-threshold-for-an-instance-of-the-database-engine"></a>Seuil d'escalade pour une instance du moteur de base de données
Chaque fois que le nombre de verrous est supérieur au seuil de mémoire pour l’escalade de verrous, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] déclenche l’escalade de verrous. Le seuil de mémoire dépend du paramétrage de l’[option de configuration locks](../database-engine/configure-windows/configure-the-locks-server-configuration-option.md) :

-   Si l’option **locks** est paramétrée sur sa valeur par défaut 0, le seuil d’escalade de verrous est atteint lorsque la mémoire utilisée par les objets de verrou représente 24 % de la mémoire utilisée par le moteur de base de données, à l’exclusion de la mémoire AWE. La structure de données utilisée pour représenter un verrou occupe approximativement 100 octets. Ce seuil est dynamique car le moteur de base de données acquiert et libère dynamiquement la mémoire en fonction de l’importance des charges de travail.

-   Si l’option **locks** n’a pas pour valeur 0, le seuil d’escalade de verrous représente 40 % de la valeur de l’option (ou moins en cas d’insuffisance de mémoire).

Le [!INCLUDE[ssde_md](../includes/ssde_md.md)] peut choisir pour l’escalade n’importe quelle instruction active depuis n’importe quelle session et, chaque fois que 1 250 nouveaux verrous sont acquis, il choisit des instructions pour l’escalade, sous réserve que la mémoire des verrous utilisée dans l’instance demeure au-dessus du seuil.

### <a name="escalating-mixed-lock-types"></a>Escalade de types de verrous mixtes
Lorsque l'escalade de verrous se produit, le verrou sélectionné pour le segment ou pour l'index est en mesure de satisfaire aux conditions du verrou de niveau inférieur le plus contraignant.

Par exemple, supposons la session suivante :

-  Commence une transaction.
-  Met à jour une table contenant un index cluster.
-  Émet une instruction SELECT qui référence la même table.

L'instruction UPDATE acquiert les verrous suivants :

-  Des verrous exclusifs (X) sur les lignes de données mises à jour
-  Des verrous intentionnels exclusifs (IX) sur les pages d'index cluster contenant ces lignes
-  Un verrou IX sur l'index cluster et un autre sur la table

L'instruction SELECT acquiert les verrous suivants :

-  Des verrous partagés (S) sur toutes les lignes de données qu'elle lit, sauf si la ligne est déjà protégée par un verrou X acquis par l'instruction UPDATE
-  Des verrous intentionnels partagés sur toutes les pages d'index cluster contenant ces lignes, sauf si la page est déjà protégée par un verrou IX
-  Aucun verrou sur la table ou sur l'index cluster car ils sont déjà protégés par des verrous IX

Si l'instruction SELECT acquiert suffisamment de verrous pour déclencher l'escalade de verrous et que celle-ci réussit, le verrou IX de la table est converti en verrou X et tous les verrous de ligne, de page et d'index sont libérés. Les mises à jour et les lectures sont protégées par le verrou X de la table.

### <a name="reducing-locking-and-escalation"></a>Réduction du verrouillage et de l'escalade
Dans la plupart des cas, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] offre les meilleures performances lorsqu’il utilise ses paramètres par défaut de verrouillage et d’escalade de verrous. Si une instance du [!INCLUDE[ssde_md](../includes/ssde_md.md)] génère beaucoup de verrous et donne lieu à de fréquentes escalades de verrous, pensez à réduire la quantité de verrouillage à l’aide :

-   d’un niveau d’isolement qui ne génère pas de verrous partagés pour les opérations de lecture ;
    -  d'un niveau d'isolement READ COMMITTED lorsque l'option de base de données READ_COMMITTED_SNAPSHOT a pour valeur ON ;
    -  d'un niveau d'isolement SNAPSHOT ;
    -  d'un niveau d'isolement READ UNCOMMITTED. Celui-ci ne peut être utilisé que pour les systèmes qui prennent en charge les lectures incorrectes ;    
  
    > [!NOTE]
    > Le changement du niveau d’isolement affecte toutes les tables sur l’instance du [!INCLUDE[ssde_md](../includes/ssde_md.md)].

-   des indicateurs de table PAGLOCK ou TABLOCK afin que le moteur de base de données utilise des verrous de page, d’index ou de segment au lieu de verrous de ligne. Toutefois, cette option augmente le risque pour un utilisateur de bloquer un autre utilisateur essayant d'accéder aux mêmes données et ne peut être utilisée que dans les systèmes prenant en charge une faible quantité d'utilisateurs simultanés.

-   Pour les tables partitionnées, utilisez l’option LOCK_ESCALATION de [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md) pour escalader des verrous au niveau du HoBT au lieu de la table ou désactiver l’escalade de verrous.

-   Divisez les opérations de traitement par lots volumineuses en plusieurs opérations plus petites. Par exemple, supposons que vous avez exécuté la requête suivante pour supprimer plusieurs centaines de milliers d’enregistrements d’une table d’audit, et que vous avez découvert que cela a entraîné une escalade de verrous à l’origine du blocage d’autres utilisateurs :
   
    ```sql
    DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    ```

    En supprimant ces enregistrements par plusieurs centaines à la fois, vous pouvez réduire considérablement le nombre de verrous qui s’accumulent par transaction et empêcher l’escalade de verrous. Par exemple :

    ```sql
    SET ROWCOUNT 500
    delete_more:
      DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    IF @@ROWCOUNT > 0 GOTO delete_more
    SET ROWCOUNT 0
    ```

-   Réduisez l’empreinte des verrous d’une requête en rendant la requête aussi efficace que possible. Les analyses de grande envergure ou les nombres élevés de recherches par signet peuvent augmenter le risque d’escalade de verrous ; de plus, cela augmente le risque d’interblocages et dégrade généralement la concurrence et les performances. Une fois que vous avez trouvé la requête à l’origine de l’escalade de verrous, recherchez les opportunités de créer des index ou d’ajouter des colonnes à un index existant pour supprimer les analyses d’index ou de table et maximiser l’efficacité des recherches d’index. Envisagez d’utiliser l’[Assistant Paramétrage du moteur de base de données](../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md) pour effectuer une analyse d’index automatique sur la requête. Pour plus d’informations, consultez [Didacticiel : Assistant Paramétrage du moteur de base de données](../tools/dta/tutorial-database-engine-tuning-advisor.md).
    L’un des objectifs de cette optimisation est de faire en sorte que les recherches d’index renvoient le moins de lignes possible pour réduire le coût des recherches par signet (maximiser la sélectivité de l’index pour une requête particulière). Si le [!INCLUDE[ssde_md](../includes/ssde_md.md)] estime qu’un opérateur logique de recherche par signet peut retourner de nombreuses lignes, il peut utiliser une prérécupération pour effectuer la recherche par signet. Si le [!INCLUDE[ssde_md](../includes/ssde_md.md)] utilise une prérécupération pour une recherche par signet, il doit augmenter le niveau d’isolation de la transaction d’une partie de la requête à la lecture renouvelable pour une partie de la requête. Cela signifie que ce qui peut ressembler à une instruction SELECT à un niveau d’isolation read-committed peut acquérir plusieurs milliers de verrous de clé (à la fois sur l’index cluster et sur un index non cluster), ce qui peut entraîner un dépassement des seuils d’escalade de verrous pour une telle requête. Cela s’avère particulièrement important si vous constatez que le verrou escaladé est un verrou de table partagé, qui, en revanche, n’est pas communément visible au niveau d’isolation read-committed par défaut. Si une clause WITH PREFETCH de recherche par signet est à l’origine de l’escalade, envisagez d’ajouter des colonnes supplémentaires à l’index non cluster qui apparaît dans l’opérateur logique Recherche d’index ou Analyse d’index sous l’opérateur logique Recherche par signet dans le plan de requête. Il peut être possible de créer un index de couverture (un index qui inclut toutes les colonnes d’une table qui ont été utilisées dans la requête), ou au moins un index qui couvre les colonnes utilisées pour les critères de jointure ou dans la clause WHERE si l’inclusion de tous les éléments dans la liste de sélection de colonnes n’est pas pratique.
    Une jointure de boucle imbriquée peut également utiliser une prérécupération, ce qui entraîne le même comportement de verrouillage.
   
-   L’escalade de verrous ne peut pas se produire si un autre SPID contient actuellement un verrou de table incompatible. L’escalade de verrous passe toujours à un verrou de table et jamais à des verrous de page. De plus, si une tentative d’escalade de verrous échoue parce qu’un autre SPID contient un verrou TAB incompatible, la requête qui a tenté l’escalade n’est pas bloquée pendant l’attente d’un verrou TAB. Elle continue plutôt d’acquérir des verrous à son niveau d’origine, plus précis (ligne, clé ou page), en effectuant régulièrement des tentatives d’escalade supplémentaires. Ainsi, une méthode pour empêcher l’escalade de verrous sur une table particulière consiste à acquérir et à conserver un verrou sur une autre connexion qui n’est pas compatible avec le type de verrou escaladé. Un verrou IX (exclusion intentionnelle) au niveau de la table ne verrouille aucune ligne ni aucune page, mais il n’est quand même pas compatible avec un verrou TAB S (partagé) ou X (exclusif) remonté. Par exemple, supposons que vous devez exécuter un programme de traitement par lots qui modifie un grand nombre de lignes dans la table mytable et qui a entraîné un blocage en raison d’une escalade de verrous. Si ce programme s’exécute toujours en moins d’une heure, vous pouvez créer un travail [!INCLUDE[tsql](../includes/tsql-md.md)] qui contient le code suivant et planifier le démarrage du nouveau travail plusieurs minutes avant l’heure de début d’exécution du programme de traitement par lots :
  
    ```sql
    BEGIN TRAN
    SELECT * FROM mytable (UPDLOCK, HOLDLOCK) WHERE 1=0
    WAITFOR DELAY '1:00:00'
    COMMIT TRAN
    ```
   
    Cette requête acquiert et conserve un verrou IX sur mytable pendant une heure, ce qui empêche l’escalade de verrous sur la table pendant cette période. Ce programme ne modifie pas les données et ne bloque pas les autres requêtes (sauf si l’autre requête force un verrou de table avec l’indicateur TABLOCK ou si un administrateur a désactivé des verrous de page ou de ligne à l’aide d’une procédure stockée sp_indexoption).

En outre, vous pouvez utiliser les indicateurs de trace 1211 et 1224 pour désactiver la totalité ou une partie des escalades de verrous. Toutefois, ces [indicateurs de trace](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) désactivent toutes les escalades de verrous globalement pour tout le [!INCLUDE[ssde_md](../includes/ssde_md.md)]. Une escalade de verrous joue un rôle très utile dans le [!INCLUDE[ssde_md](../includes/ssde_md.md)] en optimisant l’efficacité des requêtes qui, sinon, sont ralenties par la surcharge due à l’acquisition et à la libération de plusieurs milliers de verrous. Une escalade de verrous permet également de réduire la mémoire nécessaire pour effectuer le suivi des verrous. La mémoire que le [!INCLUDE[ssde_md](../includes/ssde_md.md)] peut allouer dynamiquement pour les structures de verrous est limitée. Par conséquent, si vous désactivez l’escalade de verrous et que la mémoire de verrous augmente de façon suffisante, toute tentative d’allocation de verrous supplémentaires pour une requête risque d’échouer et l’erreur suivante se produit :

```Error: 1204, Severity: 19, State: 1
The SQL Server cannot obtain a LOCK resource at this time. Rerun your statement when there are fewer active users or ask the system administrator to check the SQL Server lock and memory configuration.
```

> [!NOTE]
> Quand l’[erreur 1204](../relational-databases/errors-events/mssqlserver-1204-database-engine-error.md) se produit, elle arrête le traitement de l’instruction en cours et entraîne une restauration de la transaction active. La restauration elle-même risque de bloquer les utilisateurs ou d’entraîner une longue durée de récupération de base de données si vous redémarrez le service de base de données.

> [!NOTE]
> L’utilisation d’un indicateur de verrou comme ROWLOCK modifie uniquement le plan de verrouillage initial. Les indicateurs de verrou n’empêchent pas l’escalade de verrous. 

De plus, supervisez l’escalade de verrous en utilisant l’événement étendu (xEvent) `lock_escalation`, comme dans l’exemple suivant :

```sql
-- Session creates a histogram of the number of lock escalations per database 
CREATE EVENT SESSION [Track_lock_escalation] ON SERVER 
ADD EVENT sqlserver.lock_escalation(SET collect_database_name=(1),collect_statement=(1)
    ACTION(sqlserver.database_id,sqlserver.database_name,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,sqlserver.sql_text,sqlserver.username))
ADD TARGET package0.histogram(SET source=N'sqlserver.database_id')
GO
```

> [!IMPORTANT]
> L’événement étendu (xEvent) `lock_escalation` doit être utilisé à la place de la classe d’événements Lock:Escalation dans SQL Trace ou SQL Profiler.

## <a name="dynamic-locking"></a><a name="dynamic_locks"></a> Verrouillage dynamique
 L'utilisation de verrous de bas niveau, comme les verrous de ligne, augmente la concurrence car elle diminue la probabilité d'avoir deux transactions qui demandent des verrous sur les mêmes données en même temps. L'utilisation de verrous de bas niveau augmente également le nombre de verrous et les ressources nécessaires à leur gestion. Les verrous de table ou de page de haut niveau réduisent la charge mais au détriment de la concurrence.  
  
 ![Coût de verrouillage et coût de concurrence](../relational-databases/media/lockcht.png) 
  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise une stratégie de verrouillage dynamique pour déterminer les verrous les plus rentables. Lorsqu'une requête est exécutée, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] détermine automatiquement les verrous les plus appropriés sur la base des caractéristiques du schéma et de la requête. Par exemple, pour réduire l'utilisation des verrous, l'optimiseur peut choisir d'utiliser des verrous de niveau page dans un index lors de l'analyse de l'index.  
  
 Le verrouillage dynamique offre les avantages suivants :  
  
-   Administration de bases de données simplifiée. Les administrateurs de la base de données ne doivent pas ajuster les seuils d'escalade des verrous.  
-   Performances améliorées. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] réduit la charge sur le système en utilisant les verrous appropriés pour la tâche.  
-   Les développeurs d'applications peuvent se concentrer sur le développement. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] adapte le verrouillage automatiquement.  
  
 À compter de [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)], le comportement d’escalade de verrous a changé avec l’introduction de l’option `LOCK_ESCALATION`. Pour plus d’informations, consultez l’option `LOCK_ESCALATION` de l’instruction [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md). 
   
## <a name="deadlocks"></a><a name="deadlocks"></a> Interblocages  
 Un interblocage se produit lorsque deux tâches ou plus se bloquent mutuellement de façon permanente. Dans ce cas, chaque tâche place un verrou sur une ressource que la ou les autres tâches essaient de verrouiller. Par exemple :  
  
-   La transaction A obtient un verrou partagé sur la ligne 1.  
-   La transaction B obtient un verrou partagé sur la ligne 2.  
-   La transaction A demande un verrou exclusif sur la ligne 2, mais elle est bloquée jusqu’à la fin de la transaction B qui libérera le verrou partagé sur la ligne 2.  
-   La transaction B demande un verrou exclusif sur la ligne 1, mais elle est bloquée jusqu’à la fin de la transaction A qui libérera le verrou partagé sur la ligne 1.  
  
 La transaction A ne peut pas se terminer tant que la transaction B n'est pas terminée, mais la transaction B est bloquée par la transaction A. Il s’agit d’une dépendance cyclique : La transaction A est dépendante de la transaction B, mais celle-ci ne peut pas s’exécuter, car elle est dépendante de la transaction A.  
  
 Les deux transactions en interblocage attendent indéfiniment que la situation soit débloquée par un processus externe. Le moniteur d’interblocage du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche périodiquement les tâches d’interblocage. S'il détecte une situation de dépendance cyclique, il désigne une des tâches comme victime et met fin à sa transaction avec un message d'erreur. Cela permet à l'autre tâche de terminer sa transaction. L'application qui exécutait la transaction abandonnée peut effectuer une nouvelle tentative qui réussit en général une fois que l'autre transaction est terminée.  
  
 L'interblocage est souvent confondu avec le blocage ordinaire. Lorsqu'une transaction demande un verrou sur une ressource verrouillée par une autre transaction, elle attend que le verrou soit libéré. Par défaut, les transactions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n'ont pas de délai d'expiration, à moins que LOCK_TIMEOUT ne soit activé. La transaction qui demande le verrou est bloquée, mais pas indéfiniment puisqu'elle n'a rien fait pour bloquer la transaction détenant le verrou. La transaction qui détient le verrou va finir par se terminer et libérer le verrou ; l'autre transaction pourra alors obtenir son verrou et s'exécuter normalement.  
  
> [!NOTE]
> Les interblocages sont parfois appelés « étreintes fatales ».  
  
 Une situation d'interblocage peut se produire sur tout système multithread, pas uniquement sur les systèmes de gestion de bases de données relationnelles. Elle peut également concerner des ressources autres que les verrous sur les objets de base de données. Par exemple, un thread dans un système multithread peut acquérir une ou plusieurs ressources (par exemple, des blocs de mémoire). Si la ressource acquise appartient actuellement à une autre thread, la première thread devra éventuellement attendre que le thread propriétaire libère la ressource cible. le thread en attente a une dépendance sur le thread propriétaire de cette ressource particulière. Dans une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], les sessions peuvent se bloquer lors de l’acquisition de ressources autres que des objets de base de données, notamment des blocs de mémoire ou des threads.  
  
 ![Diagramme montrant le blocage de la transaction](../relational-databases/media/deadlock.png)  
  
 Dans l’illustration, la transaction T1 est dépendante de la transaction T2 pour la ressource de verrou de table `Part`. De même, la transaction T2 est dépendante de T1 pour la ressource de verrou de table `Supplier`. Comme ces dépendances forment un cycle, il y a interblocage entre les transactions T1 et T2.  
  
 Des interblocages peuvent également se produire quand une table est partitionnée et que le paramètre `LOCK_ESCALATION` de `ALTER TABLE` a la valeur AUTO. Quand `LOCK_ESCALATION` a la valeur AUTO, la concurrence augmente en permettant au [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de verrouiller des partitions de table au niveau HoBT plutôt qu’au niveau des tables. Toutefois, lorsque des transactions distinctes maintiennent des verrous de partition dans une table et souhaitent un verrou sur l'autre partition de transactions, cela provoque un interblocage. Ce type d’interblocage peut être évité en affectant à `LOCK_ESCALATION` la valeur `TABLE`, bien que ce paramètre réduise la concurrence en forçant les mises à jour volumineuses d’une partition à attendre un verrou de table.  
  
### <a name="detecting-and-ending-deadlocks"></a>Détection et fin des blocages  
 Un interblocage se produit lorsque deux tâches ou plus se bloquent mutuellement de façon permanente. Dans ce cas, chaque tâche place un verrou sur une ressource que la ou les autres tâches essaient de verrouiller. Le graphique suivant présente un aperçu d'un état de blocage où :  
  
-   La tâche T1 a placé un verrou sur la ressource R1 (indiquée par la flèche reliant R1 à T1) et a demandé un verrou sur la ressource R2 (indiquée par la flèche reliant T1 à R2).  
-   La tâche T2 a placé un verrou sur la ressource R2 (indiquée par la flèche reliant R2 à T2) et a demandé un verrou sur la ressource R1 (indiquée par la flèche reliant T2 à R1).  
-   Dans la mesure où aucune des deux tâches ne peut continuer tant qu'il n'y a pas de ressource disponible et que ni l'une ni l'autre des ressources ne peut être libérée avant la poursuite d'une tâche, un état de blocage se produit.  
  
 ![Diagramme montrant des tâches dans un état de blocage](../relational-databases/media/Task_Deadlock_State.png)  
  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] détecte automatiquement les cycles de blocage dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] choisit l'une des sessions comme victime et la transaction en cours se termine par une erreur, ce qui met fin à la situation de blocage.  
  
#### <a name="resources-that-can-deadlock"></a><a name="deadlock_resources"></a> Ressources susceptibles de se bloquer  
 Chaque session utilisateur peut avoir une ou plusieurs tâches en cours d'exécution, chacune de ces tâches pouvant obtenir ou être en attente d'obtention de diverses ressources. Les types de ressources susceptibles de provoquer un blocage sont les suivants :  
  
-   **Verrous**. L'attente d'obtention de verrous sur des ressources, telles qu'objets, pages, lignes, métadonnées et applications peut provoquer un blocage. Par exemple, la transaction T1 a un verrou partagé (S) sur la ligne r1 et elle attend d'obtenir un verrou exclusif (X) sur r2. La transaction T2 a un verrou partagé (S) sur r2 et elle attend d'obtenir un verrou exclusif (X) sur la ligne r1. Il en résulte un cycle de verrouillage où T1 et T2 attendent l'une de l'autre la libération des ressources que chacune a verrouillées.  
  
-   **Threads de travail**. Une tâche en attente d'un thread de travail disponible peut provoquer un blocage. Si la tâche en file d'attente est propriétaire des ressources qui bloquent tous les threads de travail, un blocage en résulte. Par exemple, la session S1 démarre une transaction et obtient un verrou partagé (S) sur la ligne r1 pour ensuite se mettre en veille. Les sessions actives en cours d'exécution sur tous les threads de travail disponibles essaient d'obtenir des verrous exclusifs (X) sur la ligne r1. Étant donné que la session S1 ne peut pas obtenir de thread de travail, elle ne peut pas valider la transaction et libère le verrou au niveau sur la ligne r1. Cela produit un blocage.  
  
-   **Mémoire**. Lorsque des demandes concurrentes sont en attente d'allocation de mémoire qui ne peut être satisfaite faute de mémoire suffisante, un blocage peut se produire. Par exemple, deux demandes concurrentes, Q1 et Q2, qui s'exécutant en tant que fonctions définies par l'utilisateur, obtiennent respectivement 10 Mo et 20 Mo de mémoire. Si chaque requête nécessite 30 Mo et que la quantité de mémoire disponible est de 20 Mo, Q1 et Q2 doivent attendre que chacune libère la mémoire, ce qui entraîne un blocage.  
  
-   **Ressources liées à l’exécution de requêtes parallèles**. Les threads de coordination, production ou consommation associées à un port d’échange peuvent se bloquer mutuellement et provoquer un interblocage, qui se produit généralement lors de l’introduction d’au moins un autre processus étranger à la requête parallèle. De même, quand commence l'exécution d'une requête parallèle, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] détermine le degré de parallélisme, ou le nombre de threads de travail, en fonction de la charge de travail en cours. Si la charge de travail change de façon inattendue, par exemple si de nouvelles requêtes commencent à s'exécuter sur le serveur ou que le système se trouve à court de threads de travail, il peut s'ensuivre un blocage.  
  
-   **Ressources MARS (Multiple Active Result Sets)** . Ces ressources servent à contrôler l'entrelacement de plusieurs demandes actives sous MARS. Pour plus d’informations, consultez [Utilisation de MARS (Multiple Active Result Sets)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
    -   **Ressource utilisateur**. Lorsqu'un thread est en attente d'une ressource potentiellement contrôlée par une application d'utilisateur, la ressource est considérée comme étant une ressource externe ou utilisateur et est traitée comme un verrou.  
  
    -   **Exclusion mutuelle de session**. Les tâches exécutées au cours d'une session sont entrelacées, ce qui signifie que seule une tâche peut s'exécuter à un moment donné dans le cadre de la session. Avant de pouvoir s'exécuter, la tâche doit disposer d'un accès exclusif à l'exclusion mutuelle de la session.  
  
    -   **Exclusion mutuelle de transaction**. Toutes les tâches qui s'exécutent lors d'une transaction sont entrelacées, ce qui signifie que seule une tâche peut s'exécuter à un moment donné dans le cadre de la transaction. Avant de pouvoir s'exécuter, la tâche doit disposer d'un accès exclusif à l'exclusion mutuelle de la transaction.  
  
     Pour pouvoir s'exécuter sous MARS, une tâche doit obtenir l'exclusion mutuelle de session. Si la tâche s'exécute dans le cadre d'une transaction, elle doit obtenir l'exclusion mutuelle de transaction. Vous serez ainsi assuré qu'il n'y a qu'une seule tâche active à la fois pour une session et une transaction données. Dès lors que les exclusions mutuelles requises ont été acquises, la tâche peut s'exécuter. Quand la tâche est terminée ou qu'elle aboutit au milieu de la demande, elle libère l'exclusion mutuelle de transaction avant l'exclusion mutuelle de session, c'est-à-dire dans l'ordre inverse de leur acquisition. Cependant, des blocages peuvent se produire avec ces ressources. Dans l'exemple de code suivant, deux tâches, la demande d'utilisateur U1 et la demande d'utilisateur U2, s'exécutent lors d'une même session.  
  
    ```  
    U1:    Rs1=Command1.Execute("insert sometable EXEC usp_someproc");  
    U2:    Rs2=Command2.Execute("select colA from sometable");  
    ```  
  
     La procédure stockée qui s'exécute à partir de la demande d'utilisateur U1 a obtenu l'exclusion mutuelle de session. Si son exécution se prolonge, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] considère que la procédure stockée attend une entrée de données de la part de l'utilisateur. La demande d'utilisateur U2 attend l'exclusion mutuelle de session alors que l'utilisateur attend le jeu de résultats d'U2, et U1 attend une ressource utilisateur. Il s'agit d'un état de blocage logiquement illustré ainsi :  
  
 ![LogicFlowExamplec](../relational-databases/media/udb9_LogicFlowExamplec.png)  
  
### <a name="deadlock-detection"></a><a name="deadlock_detection"></a> Détection de blocage  
 Toutes les ressources énumérées dans la section précédente sont visées par le dispositif de détection de blocage du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. La détection de blocage est mise en œuvre par un thread de contrôle des verrous qui lance périodiquement une recherche sur toutes les tâches d'une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Le processus de recherche présente les caractéristiques suivantes :  
  
-   L'intervalle par défaut est de 5 secondes.  
-   Si le thread de contrôle des verrous détecte des blocages, de 5 secondes, l'intervalle de détection de blocage pourra descendre jusqu'à 100 millisecondes, en fonction de la fréquence des blocages.  
-   Si le thread de contrôle des verrous ne détecte plus de blocages, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] refera passer l'intervalle de recherche à 5 secondes.  
-   Si un blocage vient d'être détecté, les prochains threads qui doivent attendre un verrou sont supposés entrer dans le cycle de blocage. Les deux premières attentes de verrous postérieures à une détection de blocage déclencheront immédiatement une recherche de blocage sans attendre le prochain intervalle de détection de blocage. Par exemple, si l'intervalle courant est de 5 secondes et qu'un blocage vient d'être détecté, la prochaine attente de verrou lancera immédiatement le détecteur de blocage. Si cette attente de verrou est impliquée dans un blocage, celui-ci sera détecté sur le champ et non lors de la prochaine recherche de blocage.  
  
 En règle générale, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] n'opère qu'une détection de blocage périodique. Puisque le nombre de blocages rencontrés dans le système est généralement faible, la détection de blocages périodique permet de réduire l'intendance des détections de blocage dans le système.  
  
 Lorsque le contrôleur de verrous initialise une recherche de blocage pour une thread particulière, il identifie la ressource sur laquelle le thread est en attente. Il recherche ensuite le ou les propriétaires de la ressource concernée et continue la recherche de façon récursive, jusqu'à ce qu'il trouve un cycle. Un cycle identifié de cette manière forme un blocage.  
  
 Dès lors qu'un blocage est détecté, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] met fin à un blocage en choisissant l'un des threads comme victime. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] met fin au traitement en cours d'exécution pour le thread, annule la transaction de la victime, puis retourne une erreur 1205 à l'application. L'annulation de la transaction de la victime du blocage a pour effet de libérer tous les verrous détenus par la transaction. Cela permet aux transactions des autres threads de se débloquer et de continuer. L'erreur de victime de blocage 1205 enregistre des informations sur les threads et les ressources impliqués dans un blocage dans le journal des erreurs.  
  
 Par défaut, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] choisit comme victime du blocage la session qui exécute la transaction dont l'annulation est la moins coûteuse. Un utilisateur peut également spécifier la priorité des sessions dans une situation de blocage au moyen de l’instruction `SET DEADLOCK_PRIORITY`. DEADLOCK_PRIORITY accepte les valeurs LOW, NORMAL ou HIGH, voire toute valeur entière comprise entre -10 et 10. La valeur par défaut de la priorité de blocage est NORMAL. Si deux sessions ont des priorités de blocage différentes, c'est la session qui a la priorité la plus basse qui est choisie comme victime. Si les deux sessions ont la même priorité de blocage, c'est celle dont la transaction est la moins coûteuse à annuler qui est choisie. Si les sessions impliquées dans le cycle de blocage présentent une priorité de blocage et un coût identiques, la victime est choisie de façon aléatoire.  
  
 Lorsque les fonctionnalités CLR sont utilisées, le moniteur de blocage détecte automatiquement le blocage des ressources de synchronisation (moniteurs, verrou de lecture/écriture et jointure de thread) qui font l'objet d'accès à l'intérieur des procédures gérées. Toutefois, le blocage est résolu par la levée d'une exception dans la procédure qui a été sélectionnée comme victime du blocage. Il est important de comprendre que l'exception ne libère pas automatiquement les ressources actuellement détenues par la victime ; les ressources doivent être libérées explicitement. Conformément au comportement des exceptions, l'exception utilisée pour identifier une victime de blocage peut être interceptée et annulée.  
  
### <a name="deadlock-information-tools"></a><a name="deadlock_tools"></a> Outils d'information sur les blocages  
 Pour afficher des informations sur l’interblocage, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] fournit des outils de surveillance sous la forme de la session XEvent system\_health, de deux indicateurs de trace ainsi que de l’événement Deadlock Graph dans SQL Profiler.  

#### <a name="deadlock-extended-event"></a><a name="deadlock_xevent"></a> Événement étendu de blocage
À compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], l’événement étendu `xml_deadlock_report` (xEvent) doit être utilisé à la place de la classe d’événements Deadlock Graph dans SQL Trace ou SQL Profiler.

De plus, à compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], en cas d’interblocage, la session ***system\_health** _ capture déjà tous les événements étendus `xml_deadlock_report` qui contiennent le graphique d’interblocage. Étant donné que la session _system\_health* est activée par défaut, il n’est pas nécessaire qu’une session xEvent distincte soit configurée pour capturer les informations d’interblocage. 

L’événement Deadlock Graph capturé a généralement trois nœuds distincts :
-   **victim-list**. Identificateur du processus victime de l’interblocage.
-   **process-list**. Informations sur tous les processus impliqués dans l’interblocage.
-   **resource-list**. Informations sur les ressources impliquées dans l’interblocage.

Lors de l’ouverture du fichier ou de la mémoire tampon en anneau de session system\_health, si l’événement xEvent `xml_deadlock_report` est enregistré, [!INCLUDE[ssManStudio](../includes/ssManStudio-md.md)] affiche une représentation graphique des tâches et ressources impliquées dans un interblocage, comme indiqué dans l’exemple suivant : 

![Graphique de blocage xEvent](../relational-databases/media/udb9_xEventDeadlockGraphc.png)

La requête suivante peut afficher tous les événements d’interblocage capturés par la mémoire tampon en anneau de session *system\_health* :

```sql
SELECT xdr.value('@timestamp', 'datetime') AS [Date],
    xdr.query('.') AS [Event_Data]
FROM (SELECT CAST([target_data] AS XML) AS Target_Data
            FROM sys.dm_xe_session_targets AS xt
            INNER JOIN sys.dm_xe_sessions AS xs ON xs.address = xt.event_session_address
            WHERE xs.name = N'system_health'
              AND xt.target_name = N'ring_buffer'
    ) AS XML_Data
CROSS APPLY Target_Data.nodes('RingBufferTarget/event[@name="xml_deadlock_report"]') AS XEventData(xdr)
ORDER BY [Date] DESC
```

[!INCLUDE[ssResult](../includes/ssresult-md.md)]

![system_health_xevent_query_result](../relational-databases/media/system_health_qry.png)

L’exemple suivant illustre la sortie après avoir cliqué sur le premier lien du résultat ci-dessus :

```xml
<event name="xml_deadlock_report" package="sqlserver" timestamp="2018-02-18T08:26:24.698Z">
  <data name="xml_report">
    <type name="xml" package="package0" />
    <value>
      <deadlock>
        <victim-list>
          <victimProcess id="process27b9b0b9848" />
        </victim-list>
        <process-list>
          <process id="process27b9b0b9848" taskpriority="0" logused="0" waitresource="KEY: 5:72057594214350848 (1a39e6095155)" waittime="1631" ownerId="11088595" transactionname="SELECT" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27b9f79fac0" lockMode="S" schedulerid="9" kpid="15336" status="suspended" spid="62" sbid="0" ecid="0" priority="0" trancount="0" lastbatchstarted="2018-02-18T00:26:22.893" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="7908" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088595" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p1" line="3" stmtstart="78" stmtend="180" sqlhandle="0x0300050020766505ca3e07008ba8000001000000000000000000000000000000000000000000000000000000">
SELECT c2, c3 FROM t1 WHERE c2 BETWEEN @p1 AND @p1+    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x020000006263ec01ebb919c335024a072a2699958d3fcce60000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p1 4
END
   </inputbuf>
          </process>
          <process id="process27b9ee33c28" taskpriority="0" logused="252" waitresource="KEY: 5:72057594214416384 (e5b3d7e750dd)" waittime="1631" ownerId="11088593" transactionname="UPDATE" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27ba15a4490" lockMode="X" schedulerid="6" kpid="5584" status="suspended" spid="58" sbid="0" ecid="0" priority="0" trancount="2" lastbatchstarted="2018-02-18T00:26:22.890" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="15316" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088593" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p2" line="3" stmtstart="76" stmtend="150" sqlhandle="0x03000500599a5906ce3e07008ba8000001000000000000000000000000000000000000000000000000000000">
UPDATE t1 SET c2 = c2+1 WHERE c1 = @p    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x02000000008fe521e5fb1099410048c5743ff7da04b2047b0000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p2 4
END
   </inputbuf>
          </process>
        </process-list>
        <resource-list>
          <keylock hobtid="72057594214350848" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="cidx" id="lock27b9dd26a00" mode="X" associatedObjectId="72057594214350848">
            <owner-list>
              <owner id="process27b9ee33c28" mode="X" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9b0b9848" mode="S" requestType="wait" />
            </waiter-list>
          </keylock>
          <keylock hobtid="72057594214416384" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="idx1" id="lock27afa392600" mode="S" associatedObjectId="72057594214416384">
            <owner-list>
              <owner id="process27b9b0b9848" mode="S" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9ee33c28" mode="X" requestType="wait" />
            </waiter-list>
          </keylock>
        </resource-list>
      </deadlock>
    </value>
  </data>
</event>
```

Pour plus d’informations, consultez [Utiliser la session system_health](../relational-databases/extended-events/use-the-system-health-session.md)

#### <a name="trace-flag-1204-and-trace-flag-1222"></a><a name="deadlock_traceflags"></a> Indicateur de trace 1204 et indicateur de trace 1222  
 En cas de situation de blocage, l'indicateur de trace 1204 et l'indicateur de trace 1222 retournent des informations qui sont recueillies dans le journal des erreurs [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. L'indicateur de trace 1024 signale les informations de blocage mises en forme par chaque nœud impliqué dans le blocage. L'indicateur de trace 1222 met en forme les informations de blocage, en commençant par les processus et en poursuivant avec les ressources. Il est possible d'activer deux indicateurs de trace pour obtenir deux représentations du même événement de blocage.  

> [!IMPORTANT]
> Évitez d’utiliser les indicateurs de trace 1204 et 1222 sur les systèmes gourmands en charge de travail qui provoquent des blocages. L’utilisation de ces indicateurs de trace peut entraîner des problèmes de performances. Utilisez plutôt l’événement étendu d’interblocage (#deadlock_xevent).
  
 En dehors de la définition des propriétés des indicateurs de trace 1204 et 1222, le tableau suivant contient également les ressemblances et les différences.  
  
|Propriété|Indicateur de trace 1204 et indicateur de trace 1222|Indicateur de trace 1204 uniquement|Indicateur de trace 1222 uniquement|  
|--------------|-----------------------------------------|--------------------------|--------------------------|  
|Format de sortie|La sortie est capturée dans le journal des erreurs de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|Les nœuds impliqués dans le blocage sont privilégiés. Chaque nœud dispose d'une section dédiée, tandis que la section finale décrit la victime du blocage.|Retourne des informations dans un format de type XML, mais non conforme au schéma XSD (XML Schema Definition). Le format possède trois sections principales. La première déclare la victime du blocage. La deuxième décrit chaque processus impliqué dans le blocage. La troisième décrit les ressources synonymes des nœuds de l'indicateur de trace 1204.|  
|Identification d'attributs|**SPID:<x\> ECID:<x\>.** Identifie le thread de l'ID du processus système en cas de traitements parallèles. L’entrée `SPID:<x> ECID:0`, où la valeur SPID remplace <x\>, représente le thread principal. L’entrée `SPID:<x> ECID:<y>`, où la valeur SPID remplace <x\> et où <y\> est supérieur à 0, représente les sous-threads du même SPID.<br /><br /> **BatchID** (**sbid** pour l’indicateur de trace 1222). Identifie le traitement à partir duquel l'exécution du code demande ou détient un verrou. Lorsque MARS (Multiple Active Result Sets) est désactivé, la valeur BatchID est 0. Quand MARS est activé, la valeur des lots actifs est 1 pour *n*. Si la session ne comporte pas de traitements actifs, BatchID a pour valeur 0.<br /><br /> **Mode**. Spécifie, pour une ressource particulière, le type de verrou demandé, accordé ou attendu par un thread. Les différents modes sont IS (intent partagé), S (partagé), U (mise à jour), IX (intent exclusif), SIX (partagé avec intent exclusif) et X (exclusif).<br /><br /> **Line #** (**line** pour l’indicateur de trace 1222). Indique le numéro de ligne du traitement qui était en cours d'exécution lorsque le blocage s'est produit.<br /><br /> **Input Buf** (**inputbuf** pour l’indicateur de trace 1222). Dresse la liste de toutes les instructions du traitement en cours.|**Node**. Il s'agit du numéro d'entrée dans la chaîne de blocage.<br /><br /> **Lists**. Le propriétaire du verrou peut faire partie des listes suivantes :<br /><br /> **Grant List**. Énumère les propriétaires actuels de la ressource.<br /><br /> **Convert List**. Énumère les propriétaires en cours qui essaient de convertir leurs verrous vers un niveau supérieur.<br /><br /> **Wait List**. Énumère les nouvelles demandes de verrou en cours pour la ressource.<br /><br /> **Statement Type**. Décrit le type d'instructions DML (SELECT, INSERT, UPDATE ou DELETE) sur lesquelles les threads disposent d'autorisations.<br /><br /> **Victim Resource Owner**. Spécifie le thread choisi comme victime par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour rompre le cycle de blocage. Il est alors mis fin au thread choisi et à tous les sous-threads existants.<br /><br /> **Next Branch**. Représente les deux sous-threads (ou plus) du même SPID qui participent au cycle de blocage.|**deadlock victim**. Représente l’adresse de mémoire physique de la tâche (consultez [sys.dm_os_tasks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)) qui a été sélectionnée comme victime de l’interblocage. Elle est égale à 0 (zéro) en cas de non résolution du blocage. Une tâche en cours d'annulation ne peut pas être choisie comme victime de blocage.<br /><br /> **executionstack**. Représente le code [!INCLUDE[tsql](../includes/tsql-md.md)] en cours d'exécution lorsque le blocage se produit.<br /><br /> **priority**. Représente la priorité de blocage. Dans certains cas, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] peut choisir de modifier la priorité de blocage pendant un bref laps de temps afin de favoriser la concurrence.<br /><br /> **logused**. Espace journal utilisé par la tâche.<br /><br /> **owner id**. ID de la transaction qui contrôle la demande.<br /><br /> **status**. État de la tâche. Il prend l'une des valeurs suivantes :<br /><br /> >> **pending**. En attente d'un thread de travail.<br /><br /> >> **runnable**. Prêt à s'exécuter, mais en attente d'un quantum.<br /><br /> >> **running**. En cours d'exécution sur le planificateur.<br /><br /> >> **suspended**. L'exécution est suspendue.<br /><br /> >> **done**. La tâche est achevée.<br /><br /> >> **spinloop**. En attente de libération d'un spinlock.<br /><br /> **waitresource**. Ressource convoitée par la tâche.<br /><br /> **waittime**. Délai d'attente de la ressource en millisecondes.<br /><br /> **schedulerid**. Planificateur associé à cette tâche. Consultez [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).<br /><br /> **hostname**. Nom de la station de travail.<br /><br /> **isolationlevel**. Niveau d'isolement des transactions en cours.<br /><br /> **Xactid**. ID de la transaction qui contrôle la demande.<br /><br /> **currentdb**. ID de la base de données.<br /><br /> **lastbatchstarted**. Dernière fois qu'un processus client a démarré une exécution de traitement.<br /><br /> **lastbatchcompleted**. Dernière fois qu'un processus client a terminé une exécution de traitement.<br /><br /> **clientoption1 et clientoption2**. Options définies pour cette connexion cliente. Il s'agit d'un masque de bits qui contient des informations sur les options habituellement contrôlées par les instructions SET, telles que SET NOCOUNT et SET XACTABORT.<br /><br /> **associatedObjectId**. Représente l'ID HoBT (Heap or B-tree, segment de mémoire ou arborescence binaire).|  
|Attributs des ressources|**RID**. Identifie la ligne d'une table pour laquelle un verrou est détenu ou demandé. RID est représenté comme RID : *db_id:file_id:page_no:row_no*. Par exemple : `RID: 6:1:20789:0`.<br /><br /> **OBJECT**. Identifie la table pour laquelle un verrou est détenu ou demandé. OBJECT est représenté comme OBJECT : *db_id:object_id*. Par exemple : `TAB: 6:2009058193`.<br /><br /> **KEY**. Identifie la plage de clés d'un index pour laquelle un verrou est détenu ou demandé. KEY est représenté comme KEY : *db_id:hobt_id* (*valeur de hachage de la clé d’index*). Par exemple : `KEY: 6:72057594057457664 (350007a4d329)`.<br /><br /> **PAG**. Identifie la ressource de page pour laquelle un verrou est détenu ou demandé. PAG est représenté comme PAG : *db_id:file_id:page_no*. Par exemple : `PAG: 6:1:20789`.<br /><br /> **EXT**. Identifie la structure d'extension. EXT est représenté comme EXT : *db_id:file_id:extent_no*. Par exemple : `EXT: 6:1:9`.<br /><br /> **DB**. Identifie le verrou de base de données. **DB est représenté de l’une des manières suivantes :**<br /><br /> DB : *db_id*<br /><br /> DB : *db_id*[BULK-OP-DB], qui identifie le verrou de base de données pris par la base de données de sauvegarde.<br /><br /> DB : *db_id*[BULK-OP-LOG], qui identifie le verrou pris par le journal de sauvegarde pour cette base de données spécifique.<br /><br /> **APP**. Identifie le verrou pris par une ressource d'application. APP est représenté comme APP : *lock_resource*. Par exemple : `APP: Formf370f478`.<br /><br /> **METADATA**. Représente les ressources de métadonnées impliquées dans un blocage. Comme METADATA possède de nombreuses sous-ressources, la valeur retournée dépend de la sous-ressource bloquée. Par exemple, METADATA.USER_TYPE retourne `user_type_id =` <*integer_value*>. Pour plus d’informations sur les ressources et sous-ressources METADATA, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).<br /><br /> **HOBT**. Représente un segment de mémoire ou d'arbre B (B-Tree) impliqué dans un blocage.|Non exclusif à cet indicateur de trace.|Non exclusif à cet indicateur de trace.|  
  
##### <a name="trace-flag-1204-example"></a>Exemple d'indicateur de trace 1204  
 L'exemple suivant illustre la sortie obtenue quand l'indicateur de trace 1204 est activé. Dans ce cas, la table du nœud 1 est un segment de mémoire sans index et la table du nœud 2 est un segment de mémoire avec un index non-cluster. La clé d'index du nœud 2 est en cours de mise à jour lorsque le blocage se produit.  
  
```  
Deadlock encountered .... Printing deadlock information  
Wait-for graph  
  
Node:1  
  
RID: 6:1:20789:0               CleanCnt:3 Mode:X Flags: 0x2  
 Grant List 0:  
   Owner:0x0315D6A0 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:55 ECID:0 XactLockInfo: 0x04D9E27C  
   SPID: 55 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
BEGIN TRANSACTION  
   EXEC usp_p2  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x03A3DAD0   
     Mode: U SPID:54 BatchID:0 ECID:0 TaskProxy:(0x04976374) Value:0x315d200 Cost:(0/868)  
  
Node:2  
  
KEY: 6:72057594057457664 (350007a4d329) CleanCnt:2 Mode:X Flags: 0x0  
 Grant List 0:  
   Owner:0x0315D140 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:54 ECID:0 XactLockInfo: 0x03A3DAF4  
   SPID: 54 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
     BEGIN TRANSACTION  
       EXEC usp_p1  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
  
Victim Resource Owner:  
 ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
```  
  
##### <a name="trace-flag-1222-example"></a>Exemple d'indicateur de trace 1222  
 L'exemple suivant illustre la sortie obtenue quand l'indicateur de trace 1222 est activé. Dans ce cas, une table est un segment de mémoire sans index et l'autre table un segment de mémoire avec un index non-cluster. Dans la seconde table, la clé d'index est en cours de mise à jour lorsque le blocage se produit.  
  
```  
deadlock-list  
 deadlock victim=process689978  
  process-list  
   process id=process6891f8 taskpriority=0 logused=868   
   waitresource=RID: 6:1:20789:0 waittime=1359 ownerId=310444   
   transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:42.733 XDES=0x3a3dad0   
   lockMode=U schedulerid=1 kpid=1952 status=suspended spid=54   
   sbid=0 ecid=0 priority=0 transcount=2   
   lastbatchstarted=2005-09-05T11:22:42.733   
   lastbatchcompleted=2005-09-05T11:22:42.733   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310444 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p1 line=6 stmtstart=202   
     sqlhandle=0x0300060013e6446b027cbb00c69600000100000000000000  
     UPDATE T2 SET COL1 = 3 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600856aa70f503b8104000000000000000000000000  
     EXEC usp_p1       
    inputbuf  
      BEGIN TRANSACTION  
       EXEC usp_p1  
   process id=process689978 taskpriority=0 logused=380   
   waitresource=KEY: 6:72057594057457664 (350007a4d329)     
   waittime=5015 ownerId=310462 transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:44.077 XDES=0x4d9e258 lockMode=U   
   schedulerid=1 kpid=3024 status=suspended spid=55 sbid=0 ecid=0   
   priority=0 transcount=2 lastbatchstarted=2005-09-05T11:22:44.077   
   lastbatchcompleted=2005-09-05T11:22:44.077   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310462 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p2 line=6 stmtstart=200   
     sqlhandle=0x030006004c0a396c027cbb00c69600000100000000000000  
     UPDATE T1 SET COL1 = 4 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600d688e709b85f8904000000000000000000000000  
     EXEC usp_p2       
    inputbuf  
      BEGIN TRANSACTION  
        EXEC usp_p2      
  resource-list  
   ridlock fileid=1 pageid=20789 dbid=6 objectname=AdventureWorks2016.dbo.T2   
   id=lock3136940 mode=X associatedObjectId=72057594057392128  
    owner-list  
     owner id=process689978 mode=X  
    waiter-list  
     waiter id=process6891f8 mode=U requestType=wait  
   keylock hobtid=72057594057457664 dbid=6 objectname=AdventureWorks2016.dbo.T1   
   indexname=nci_T1_COL1 id=lock3136fc0 mode=X   
   associatedObjectId=72057594057457664  
    owner-list  
     owner id=process6891f8 mode=X  
    waiter-list  
     waiter id=process689978 mode=U requestType=wait  
```  
  
#### <a name="profiler-deadlock-graph-event"></a>Evénement Deadlock Graph de SQL Profiler  
Il s’agit d’un événement propre à SQL Profiler qui présente une description graphique des tâches et des ressources impliquées dans un interblocage. L’exemple suivant illustre la sortie obtenue à partir de SQL Profiler quand l’événement Deadlock Graph est activé.  
  
 ![ProfilerDeadlockGraphc](../relational-databases/media/udb9_ProfilerDeadlockGraphc.png)  
  
Pour plus d’informations sur l’événement de blocage, consultez [Classe d’événements Lock:Deadlock](../relational-databases/event-classes/lock-deadlock-event-class.md).

Pour plus d’informations sur l’exécution de l’événement Deadlock Graph SQL Profiler, consultez [Enregistrer les événements Deadlock Graph &#40;SQL Server Profiler&#41;](../relational-databases/performance/save-deadlock-graphs-sql-server-profiler.md).  
  
### <a name="handling-deadlocks"></a>Gestion des blocages  
 Quand une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] choisit une transaction comme victime d’un interblocage, elle met fin au lot en cours, annule la transaction, puis retourne le message d’erreur 1205 à l’application.  
  
 `Your transaction (process ID #52) was deadlocked on {lock | communication buffer | thread} resources with another process and has been chosen as the deadlock victim. Rerun your transaction.`  
  
 Dans la mesure où toute application soumettant des requêtes [!INCLUDE[tsql](../includes/tsql-md.md)] peut être choisie comme victime de blocage, les applications doivent intégrer un gestionnaire d'erreurs capable d'intercepter le message d'erreur 1205. Si une application n'intercepte pas cette erreur, elle peut continuer en ignorant que sa transaction a été annulée, et des erreurs peuvent se produire.  
  
 L'implémentation d'un gestionnaire d'erreurs capable d'intercepter le message d'erreur 1205 permet à une application de gérer les situations de blocage et de réagir en conséquence, par exemple en re-soumettant automatiquement la requête impliquée dans le blocage. Cette nouvelle soumission automatique rend la gestion du blocage entièrement transparente pour l'utilisateur.  
  
 L'application doit marquer un bref temps d'arrêt avant de soumettre à nouveau la requête. Cela permet à l'autre transaction impliquée dans le blocage d'aboutir et de libérer ses verrous qui faisaient partie du cycle de blocage. Les risques qu'un blocage se reproduise au moment où la requête de nouveau soumise demande ses verrous sont ainsi réduits.  
  
### <a name="minimizing-deadlocks"></a><a name="deadlock_minimizing"></a> Réduction des blocages  
 Même si les interblocages ne peuvent pas être totalement évités, le respect de certaines conventions de codage peut minimiser le risque d'en générer. La réduction des blocages peut augmenter le débit des transactions et réduire la charge du système, car il y a moins de transactions :  
  
-   restaurées, en annulant ce qui a été accompli par la transaction ;  
-   resoumises par les applications, car ces transactions ont été restaurées lors du blocage.  
  
 Pour réduire le nombre de blocages :  
  
-   Accédez aux objets dans le même ordre.  
-   Évitez les interactions utilisateur dans les transactions.  
-   Créez des transactions courtes dans le même traitement.  
-   Utilisez un niveau d'isolement plus faible.  
-   Utilisez un niveau d'isolement basé sur le contrôle de version de ligne.  
    -   Affectez à l’option de base de données `READ_COMMITTED_SNAPSHOT` la valeur ON pour activer les transactions read-committed afin d’utiliser la gestion de versions de ligne.  
    -   Utilisez un isolement d'instantané.  
-   Utilisez des connexions liées.  
  
#### <a name="access-objects-in-the-same-order"></a>Accéder aux objets dans le même ordre  
 Si toutes les transactions concurrentes accèdent aux objets dans le même ordre, le risque de blocage diminue. Par exemple, si deux transactions concurrentes obtiennent un verrou sur la table **Supplier**, puis sur la table **Part**, l’une des transactions est bloquée sur la table **Supplier** jusqu’à ce que l’autre transaction se termine. Après la validation ou la restauration de la première transaction, la seconde continue et aucun blocage ne se produit. L'utilisation de procédures stockées pour toutes les modifications de données peut standardiser l'ordre d'accès aux objets.  
  
 ![deadlock2](../relational-databases/media/dedlck2.png)  
  
#### <a name="avoid-user-interaction-in-transactions"></a>Éviter les interactions utilisateur dans les transactions  
 Évitez d'écrire des transactions comprenant une interaction utilisateur, car la vitesse d'exécution des traitements sans intervention de l'utilisateur est beaucoup plus rapide que la vitesse à laquelle un utilisateur doit répondre manuellement aux requêtes telles que la demande d'un paramètre requis par une application. Par exemple, si une transaction attend une entrée de la part de l'utilisateur, et si ce dernier va déjeuner ou rentre chez lui pour le week-end, l'utilisateur empêche la transaction de se terminer. Ceci dégrade les performances du système, car tous les verrous détenus par la transaction ne sont libérés qu'une fois la transaction validée ou restaurée. Même si une situation de blocage ne se produit pas, toutes les autres transactions en attente de la même ressource sont bloquées, en attente de la fin de la transaction.  
  
#### <a name="keep-transactions-short-and-in-one-batch"></a>Créer des transactions courtes dans le même traitement  
 Un blocage se produit souvent lorsque plusieurs transactions longues sont exécutées de manière concurrente dans la même base de données. Plus la transaction est longue, plus la durée de détention du verrou exclusif ou de mise à jour est importante, ce qui bloque les autres activités et peut entraîner une situation de blocage.  
  
 La création de transactions courtes dans un seul traitement limite les allers-retours sur le réseau en réduisant les délais potentiels d'achèvement de la transaction et de suppression des verrous.  
  
#### <a name="use-a-lower-isolation-level"></a>Utiliser un niveau d'isolement plus faible  
 Déterminez si une transaction peut être exécutée à un niveau d'isolement faible. L'implémentation de la lecture validée (read committed) permet à une transaction de lire des données lues auparavant (non modifiées) par une autre transaction, sans attendre la fin de la première transaction. L'utilisation d'un niveau d'isolement faible (comme la lecture validée, par exemple) permet de conserver les verrous partagés pendant une durée inférieure à celle d'un niveau d'isolement supérieur (comme le niveau sérialisable) et réduit ainsi la contention de verrouillage.  
  
#### <a name="use-a-row-versioning-based-isolation-level"></a>Niveau d'isolement basé sur le contrôle de version de ligne  
 Quand l’option de base de données `READ_COMMITTED_SNAPSHOT` a la valeur ON, une transaction qui s’exécute sous un niveau d’isolation read committed utilise le contrôle de version de ligne plutôt que les verrous partagés lors des opérations de lecture.  
  
> [!NOTE]  
> Certaines applications se basent sur le comportement de verrouillage et de blocage de l'isolement de lecture validée. Avec ces applications, certaines modifications sont nécessaires pour pouvoir activer cette option.  
  
 L'isolement d'instantané utilise également le contrôle de version de ligne, qui n'emploie pas de verrous partagés pendant les opérations de lecture. Pour qu’une transaction puisse s’exécuter sous une isolation d’instantané, l’option de base de données `ALLOW_SNAPSHOT_ISOLATION` doit avoir la valeur ON.  
  
 Implémentez ces niveaux d'isolement pour minimiser les blocages pouvant survenir entre les opérations de lecture et d'écriture.  
  
#### <a name="use-bound-connections"></a>Utiliser des connexions liées  
 En utilisant des connexions liées, deux connexions ou plus ouvertes par la même application peuvent coopérer entre elles. Tout verrou acquis par la connexion secondaire apparaît comme s'il avait été posé par la connexion primaire et vice-versa. Ils ne se bloquent donc pas réciproquement.  
  
## <a name="lock-partitioning"></a><a name="lock_partitioning"></a> Partitionnement de verrous  
 Pour les gros systèmes informatiques, des verrous sur des objets souvent référencés peuvent affaiblir les performances, car l'acquisition et la libération des verrous provoque une contention sur les ressources des verrous internes. Le partitionnement de verrous améliore les performances du verrouillage en fractionnant une ressource de verrou en plusieurs. Cette fonctionnalité n'est disponible que pour les systèmes dotés d'au moins 16 UC ; elle est activée automatiquement et ne peut pas être désactivée. Seuls les verrous d'objets peuvent être partitionnés. Les verrous d'objets dotés d'un sous-type ne sont pas partitionnés. Pour plus d’informations, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
### <a name="understanding-lock-partitioning"></a>Présentation du partitionnement de verrous  
 Les opérations de verrouillage accèdent à plusieurs ressources partagées, dont deux sont optimisées par le partitionnement de verrous :  
  
-   **Verrouillage tournant**. Contrôle l'accès à une ressource de verrou, par exemple une ligne ou une table.  
  
     Sans partitionnement de verrous, un verrouillage spinlock gère toutes les demandes de verrouillage pour une seule ressource de verrou. Sur des systèmes qui connaissent une activité intense, une contention peut se produire pendant que les demandes de verrouillage attendent que le verrouillage spinlock devienne disponible. Dans ce cas, l'acquisition de verrous peut causer un goulet d'étranglement et détériorer les performances.  
  
     Pour réduire la contention sur une ressource de verrou, le partitionnement de verrous fractionne une ressource de verrou en plusieurs ressources, afin de répartir la charge sur plusieurs verrouillages spinlock.  
  
-   **Mémoire**. Permet de stocker les structures des ressources de verrous.  
  
     Une fois le verrouillage spinlock acquis, les structures des verrous sont stockées dans la mémoire, puis utilisées et éventuellement modifiées. La répartition de l'accès aux verrous entre plusieurs ressources permet d'éliminer la nécessité de transférer des blocs de mémoire entre les UC, ce qui améliore les performances.  
  
### <a name="implementing-and-monitoring-lock-partitioning"></a>Mise en œuvre et surveillance du partitionnement de verrous  
 Le partitionnement de verrous est activé par défaut pour les systèmes comportant 16 UC ou plus. Quand il est activé, un message d'informations est inscrit dans le journal des erreurs de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Lors de l'acquisition de verrous sur une ressource partitionnée :  
  
-   Seuls les modes de verrouillage NL, SCH-S, IS, IU et IX sont acquis sur une seule partition.  
  
-   Les verrous partagés (S), exclusifs (X) et autres dans les modes autres que NL, SCH-S, IS, IU et IX doivent être acquis sur toutes les partitions à partir de la partition ID 0, et ensuite dans l'ordre selon l'ID. Ces verrous situés sur une ressource partitionnée utiliseront plus de mémoire que des verrous dans le même mode sur une ressource non partitionnée, puisque chaque partition constitue en fait un verrou distinct. La quantité de mémoire en plus est déterminée par le nombre de partitions. Les compteurs de verrous [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de l'Analyseur de performances Windows affiche des informations sur la mémoire utilisée par les verrous partitionnés et non partitionnés.  
  
 Une transaction est affectée à une partition au début de la transaction. Pour la transaction, toutes les demandes de verrou qui peuvent être partitionnés utilisent la partition attribuée à cette transaction. Avec cette méthode, l'accès aux ressources de verrous du même objet par différentes transactions est réparti sur plusieurs partitions.  
  
 La colonne `resource_lock_partition` de la vue de gestion dynamique `sys.dm_tran_locks` fournit l’ID de partition de verrou pour une ressource partitionnée de verrou. Pour plus d’informations, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
### <a name="working-with-lock-partitioning"></a>Utilisation du partitionnement de verrous  
 Les exemples de code suivants illustrent le partitionnement de verrous : dans ces exemples, deux transactions sont exécutées dans deux sessions différentes pour montrer le comportement du partitionnement de verrous sur un système informatique doté de 16 UC.  
  
 Ces instructions [!INCLUDE[tsql](../includes/tsql-md.md)] créent des objets de test logiques qui sont utilisés dans les exemples qui suivent.  
  
```sql  
-- Create a test table.  
CREATE TABLE TestTable  (col1 int);  
GO  
  
-- Create a clustered index on the table.  
CREATE CLUSTERED INDEX ci_TestTable   
    ON TestTable (col1);  
GO  
  
-- Populate the table.  
INSERT INTO TestTable VALUES (1);  
GO  
```  
  
#### <a name="example-a"></a>Exemple A  
 Session 1 :  
  
 Une instruction `SELECT` est exécutée sous une transaction. À cause de l'indicateur de verrou `HOLDLOCK`, cette instruction va acquérir et conserver un verrou IS (Intent Shared) sur la table (pour cette illustration, les verrous de ligne et de page sont ignorés). Le verrou IS sera acquis uniquement sur la partition attribuée à la transaction. Pour cet exemple, il est supposé que le verrou IS est acquis sur l'ID de partition 7.  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 Session 2 :  
  
 Une transaction a démarré et l'instruction `SELECT` qui s'exécute sous cette transaction va acquérir et conserver un verrou partagé (S) sur la table. Le verrou S sera acquis sur toutes les partitions, ce qui aboutit à plusieurs verrous de table, un pour chaque partition. Par exemple, sur un système à 16 UC, 16 verrous S seront créés sur les ID de partition de 0 à 15. Comme le verrou S est compatible avec le verrou IS qui se trouve sur la partition ID 7 du fait de la transaction de la session 1, il n'y a pas de blocages entre les transactions.  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCK, HOLDLOCK);  
```  
  
 Session 1 :  
  
 L'instruction `SELECT` suivante est exécutée sous la transaction qui est encore active sous la session 1. En raison de l'indicateur de verrou de table exclusif (X), la transaction va essayer d'acquérir un verrou exclusif X sur la table. Toutefois, le verrou S qui est maintenu par la transaction de la session 2 bloquera le verrou X au niveau de la partition ID 0.  
  
```sql  
SELECT col1  
FROM TestTable  
WITH (TABLOCKX);  
```  
  
#### <a name="example-b"></a>Exemple B  
 Session 1 :  
  
 Une instruction `SELECT` est exécutée sous une transaction. À cause de l'indicateur de verrou `HOLDLOCK`, cette instruction va acquérir et conserver un verrou IS (Intent Shared) sur la table (pour cette illustration, les verrous de ligne et de page sont ignorés). Le verrou IS sera acquis uniquement sur la partition attribuée à la transaction. Pour cet exemple, on suppose que le verrou IS est acquis sur la partition ID 6.  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 Session 2 :  
  
 Une instruction `SELECT` est exécutée sous une transaction. En raison de l'indicateur de verrou `TABLOCKX`, la transaction essaie d'acquérir un verrou exclusif (X) sur la table. Souvenez-vous que le verrou X doit être acquis sur toutes les partitions à partir de la partition ID 0. Le verrou X sera acquis sur toutes les partitions, de l'ID 0 à 5, mais il sera bloqué par le verrou IS acquis sur la partition ID 6.  
  
 Sur les ID de partition de 7 à 15 que le verrou X n'a pas encore atteint, d'autres transactions peuvent continuer d'acquérir des verrous.  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCKX, HOLDLOCK);  
```   
  
##  <a name="row-versioning-based-isolation-levels-in-the-ssdenoversion"></a><a name="Row_versioning"></a> Niveaux d’isolation basés sur le contrôle de version de ligne dans le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]  
 À partir de [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] introduit une implémentation d’un niveau d’isolation de la transaction existant, read committed, qui fournit un instantané au niveau des instructions basé sur le contrôle de version de ligne. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] offre également un niveau d’isolation de la transaction, instantané, qui fournit un instantané au niveau des transactions basé aussi sur le contrôle de version de ligne.  
  
 Le contrôle de version de ligne est une infrastructure générale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui appelle un mécanisme de copie sur écriture lorsqu'une ligne est modifiée ou supprimée. Il exige que l'ancienne version de la ligne soit disponible pendant l'exécution de la transaction pour les transactions qui nécessitent un état antérieur cohérent d'un point de vue transactionnel. Le contrôle de version de ligne est utilisé pour la prise en charge des fonctionnalités suivantes :  
  
-   Génération des tables **insérées** et **supprimées** dans les déclencheurs. Toutes les lignes modifiées par le déclencheur reçoivent une version, y compris celles modifiées par l'instruction qui a lancé le déclencheur, de même que toute modification de données effectuée par le déclencheur.  
-   Prise en charge des ensembles de résultats actifs multiples (MARS, Multiple Active Result Sets). Si une session MARS émet une instruction de modification de données (par exemple, `INSERT`, `UPDATE` ou `DELETE`) à un moment où il existe un jeu de résultats actif, les lignes concernées par l’instruction de modification sont avec contrôle de version.  
-   Prise en charge des opérations d'index qui spécifient l'option ONLINE.  
-   Prise en charge des niveaux d'isolement des transactions basés sur le contrôle de version de ligne :  
    -   Une nouvelle implémentation du niveau d'isolement read committed qui utilise le contrôle de version de ligne pour assurer la cohérence de la lecture au niveau de l'instruction.  
    -   Un nouveau niveau d'isolement, l'instantané, pour assurer la cohérence de la lecture au niveau de la transaction.  
  
 La base de données `tempdb` doit avoir suffisamment d’espace pour contenir la banque des versions. Quand la base de données `tempdb` est pleine, les opérations de mise à jour ne génèrent plus de versions et continuent d’aboutir, mais les opérations de lecture risquent d’échouer en raison de l’absence d’une version de ligne particulière requise. Ceci a des conséquences sur les opérations comme les déclencheurs, MARS et l'indexation en ligne.  
  
 L'utilisation du contrôle de version de ligne pour les transactions read committed et les transactions d'instantanés se fait en deux étapes :  
  
1.  Activez (valeur ON) l’option de base de données `READ_COMMITTED_SNAPSHOT` et/ou l’option `ALLOW_SNAPSHOT_ISOLATION`.  
2.  Définissez le niveau d'isolement des transactions approprié dans une application :  

    -   Quand l’option `READ_COMMITTED_SNAPSHOT` est activée (ON), les transactions qui définissent le niveau d’isolation read committed utilisent le contrôle de version de ligne.  
    -   Quand l’option de base de données `ALLOW_SNAPSHOT_ISOLATION` est activée (ON), les transactions peuvent définir le niveau d’isolation d’instantané.  
  
 Quand l’option de base de données `READ_COMMITTED_SNAPSHOT` ou `ALLOW_SNAPSHOT_ISOLATION` est activée, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] affecte un numéro de séquence de transaction (XSN) à chaque transaction qui manipule des données à l’aide du contrôle de version de ligne. Les transactions démarrent au moment où une instruction `BEGIN TRANSACTION` est exécutée. En revanche, le numéro de séquence de la transaction commence à la première opération de lecture ou d'écriture suivant l'instruction BEGIN TRANSACTION. Ce numéro augmente de 1 à chaque fois qu'il est attribué.  
  
 Quand une seule des deux options de base de données (`READ_COMMITTED_SNAPSHOT` ou `ALLOW_SNAPSHOT_ISOLATION`) est activée, des copies logiques (versions) sont maintenues pour toutes les modifications de données effectuées dans la base de données. Chaque fois qu’une ligne est modifiée par une transaction, l’instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] enregistre une version de l’image précédemment validée de la ligne dans `tempdb`. Chaque version porte le numéro de séquence de la transaction responsable de la modification. Les versions des lignes modifiées sont enchaînées au moyen d'une liste de liens. La valeur de ligne la plus récente est toujours stockée dans la base de données active et enchaînée aux lignes avec contrôle de version stockées dans `tempdb`.  
  
> [!NOTE]  
> Dans le cas de la modification d’objets LOB, seul le fragment modifié est copié dans la banque des versions dans `tempdb`.  
  
 Les versions de lignes sont conservées suffisamment longtemps pour satisfaire aux besoins des transactions qui s'exécutent sous le régime d'isolement « contrôle de version de ligne ». Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] recherche le plus ancien numéro de séquence de transaction et supprime de façon périodique toutes les versions de lignes dont le numéro de séquence de transaction est inférieur à celui-ci.  
  
 Lorsque les deux options de base de données sont désactivées, seules les lignes modifiées par des déclencheurs ou des sessions MARS, ou lues par des opérations d'index ONLINE, sont avec version. Ces versions de lignes sont libérées lorsqu'elles ne sont plus nécessaires. Un thread d'arrière-plan supprime périodiquement les versions de lignes dépassées.  
  
> [!NOTE]  
> Pour les transactions de courte durée, il arrive qu’une version d’une ligne modifiée soit mise en cache dans le pool de mémoires tampons sans être écrite dans les fichiers de la base de données `tempdb` sur le disque. Si cette ligne avec version n'est plus nécessaire, elle est simplement supprimée du pool de mémoires tampons, ce qui lui évite de générer du trafic E/S.  
  
### <a name="behavior-when-reading-data"></a>Comportement lors de la lecture de données  
 Lorsque des transactions s'exécutant sous le régime d'isolement « contrôle de version de ligne », les opérations de lecture n'acquièrent pas de verrous partagés sur les données lues, et par conséquent ne bloquent pas les transactions qui modifient des données. De plus, la charge liée au verrouillage des ressources est minimisée en raison de la réduction du nombre de verrous acquis. L'isolement read committed avec contrôle de version de ligne et l'isolement d'instantané sont conçus pour garantir la cohérence des données avec version au niveau de l'instruction ou de la transaction.  
  
 Toutes les requêtes, y compris les transactions qui s'exécutent sous les niveaux d'isolement basés sur le contrôle de version de ligne, acquièrent des verrous de stabilité du schéma (Sch-S) au cours de la compilation et de l'exécution. Par conséquent, les requêtes sont bloquées lorsqu'une transaction simultanée détient un verrou de modification du schéma (Sch-M) sur la table. Par exemple, une opération DDL (Data Definition Language) acquiert un verrou Sch-M avant de modifier les informations de schéma de la table. Les transactions de type requête, y compris celles qui s'exécutent sous un niveau d'isolement basé sur le contrôle de version de ligne, sont bloquées lors d'une tentative visant à acquérir un verrou Sch-S. Inversement, une requête qui détient un verrou Sch-S bloque une transaction simultanée qui tente d'acquérir un verrou Sch-M.  
  
 Lorsqu'une transaction avec niveau d'isolement d'instantané est lancée, l'instance de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] enregistre toutes les transactions en cours. Lorsque la transaction lit une ligne qui a une chaîne de versions, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] remonte la chaîne et récupère la ligne dont le numéro de séquence de transaction :  
  
-   se rapproche le plus, sans le dépasser, du numéro de séquence de la transaction qui lit la ligne ;  
  
-   ne figure pas dans la liste de transactions actives au moment de la création de la transaction.  
  
 Les opérations de lecture effectuées par une transaction d'instantané récupèrent la dernière version de chaque ligne validée au début de la transaction. Ceci permet de disposer d'un instantané cohérent de manière transactionnelle des données présentes au début de la transaction.  
  
 Les transactions « read committed » avec contrôle de version de ligne fonctionnement pratiquement de la même manière. La différence est que ces transactions n'utilisent pas leurs propres numéros de séquence lors du choix des versions de lignes. Chaque fois qu'une instruction est lancée, la transaction lit le dernier numéro de séquence émis pour cette instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. C'est ce numéro qui servira à sélectionner les bonnes versions de lignes pour cette instruction. Ceci permet aux transactions read committed de voir un instantané des données telles qu'elles existaient au début de chaque instruction.  
  
> [!NOTE]  
> Même si les transactions read commited utilisant le contrôle de version de ligne fournissent une vue cohérente d'un point de vue transactionnel des données au niveau d'une instruction, les versions de ligne générées ou accédées par ce type de transaction sont conservées jusqu'à la fin de la transaction.  
  
### <a name="behavior-when-modifying-data"></a>Comportement lors de la modification de données  
 Dans une transaction read committed avec contrôle de version de ligne, le choix des lignes à mettre à jour se fait au moyen d'une analyse bloquante. Au cours de celle-ci, un verrou de mise à jour (U) est acquis sur la ligne de données au fur et à mesure que les valeurs de données sont lues. La même chose se produit avec une transaction read committed qui n'utilise pas le contrôle de version de ligne. Si la ligne de données ne répond pas aux critères de mise à jour, le verrou de mise à jour est déplacé sur la ligne suivante, qui est analysée.  
  
 Les transactions s'exécutant avec isolement d'instantané adoptent une approche optimiste en matière de modification de données car elles ne verrouillent les lignes que lorsque les données qui s'y trouvent doivent être modifiées. Sinon, les verrous ne sont pas placés sur les données tant que celles-ci doivent être modifiées. Lorsqu'une ligne de données répond aux critères de mise à jour, la transaction vérifie que la ligne n'a pas été modifiée par une transaction concomitante validée après elle. Si la ligne de données a été modifiée en dehors de la transaction, un conflit de mise à jour se produit et la transaction est arrêtée. Le conflit de mise à jour est géré par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Il n'y a aucun moyen de désactiver la détection des conflits de mise à jour.  
  
> [!NOTE]  
> Les opérations de mise à jour s'exécutant avec isolement d'instantané s'exécutent en interne sous le régime d'isolement read committed lorsque la transaction accède à un des éléments suivants :  
>  
> une table avec contrainte FOREIGN KEY ;  
>  
> une table à laquelle la contrainte FOREIGN KEY d'une autre table fait référence ;  
>  
> une vue indexée faisant référence à plusieurs tables.  
>  
> Cependant, même sous ces conditions, l'opération de mise à jour continue à vérifier que les données n'ont pas été modifiées par une autre transaction. Si c'est le cas, il y a conflit de mise à jour et la transaction est arrêtée.  
  
### <a name="behavior-in-summary"></a>Synthèse des comportements  
 Le tableau suivant synthétise les différences entre l'isolement d'instantané et l'isolement read committed avec contrôle de version de ligne :  
  
|Propriété|Niveau d'isolement READ COMMITED utilisant le contrôle de version de ligne|Niveau d'isolement d'instantané|  
|--------------|----------------------------------------------------------|------------------------------|  
|L'option de base de données doit être activée (ON) pour assurer la prise en charge nécessaire.|READ_COMMITTED_SNAPSHOT|ALLOW_SNAPSHOT_ISOLATION|  
|Manière dont une session demande le type spécifique de contrôle de version de ligne.|Utilisez le niveau d'isolement par défaut (read-committed) ou exécutez l'instruction SET TRANSACTION ISOLATION LEVEL pour spécifier le niveau d'isolement READ COMMITTED. Ceci peut se faire après le début de la transaction.|Requiert l'exécution de l'instruction SET TRANSACTION ISOLATION LEVEL pour spécifier le niveau d'isolement SNAPSHOT avant le début de la transaction.|  
|La version des données lue par les instructions.|Toutes les données qui ont été validées avant le début de chaque instruction.|Toutes les données qui ont été validées avant le début de chaque transaction.|  
|Manière dont les mises à jour sont gérées.|Passe des versions de lignes aux données réelles pour sélectionner les lignes à mettre à jour et utilise des verrous de mise à jour sur les lignes sélectionnées. Acquiert des verrous exclusifs sur les lignes à modifier réellement. Pas de détection de conflit de mise à jour.|Utilise les versions de lignes pour sélectionner les lignes à mettre à jour. Essaie d'acquérir un verrou exclusif sur les lignes à modifier réellement et, si les données ont été modifiées par une autre transaction, génère un conflit de mise à jour qui entraîne l'arrêt de la transaction.|  
|Détection d'un conflit de mise à jour.|Aucun.|Prise en charge intégrée. Ne peut être désactivée.|  
  
### <a name="row-versioning-resource-usage"></a>Utilisation de la ressource de contrôle de version de ligne  
 L'infrastructure de contrôle de version de ligne prend en charge les fonctionnalités suivantes dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :  
  
-   Déclencheurs  
-   MARS (Multiple Active Results Sets)  
-   Indexation en ligne  
  
 La structure de contrôle de version de ligne prend également en charge les niveaux d'isolement des transactions basé sur le contrôle de version de ligne, qui sont désactivés par défaut :  
  
-   Quand l’option de base de données `READ_COMMITTED_SNAPSHOT` est activée (ON), les transactions `READ_COMMITTED` permettent une lecture cohérente au niveau des instructions grâce au contrôle de version de ligne.  
-   Quand l’option de base de données `ALLOW_SNAPSHOT_ISOLATION` est activée (ON), les transactions `SNAPSHOT` permettent une lecture cohérente au niveau des transactions grâce au contrôle de version de ligne.  
  
 Les niveaux d'isolement basé sur le contrôle de version de ligne réduisent le nombre de verrous obtenus par la transaction en supprimant l'utilisation des verrous partagés dans les opérations de lecture. Les performances système sont ainsi accrues et les ressources nécessaires à la gestion des verrous diminuées. La réduction des blocages d'une transaction par des verrous obtenus par d'autres transactions permet également d'augmenter les performances.  
  
 Les niveaux d'isolement basé sur le contrôle de version de ligne augmentent les ressources nécessaires pour la modification de données. L'activation de ces options induit automatiquement le contrôle de version de toutes les modifications apportées aux données de la base de données. Une copie des données avant modification est stockée dans tempdb, même s'il n'existe aucune transaction active utilisant l'isolement basé sur le contrôle de version de ligne. Les données modifiées contiennent un pointeur vers les données de version stockées dans tempdb. En ce qui concerne les objets volumineux, seule la partie de l'objet ayant été modifiée est copiée dans tempdb.  
  
#### <a name="space-used-in-tempdb"></a>Espace occupé dans tempdb  
 Pour toute instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], tempdb doit disposer d’un espace suffisant pour conserver les versions de ligne générées pour chaque base de données dans l’instance. L'administrateur de base de données doit s'assurer que tempdb dispose de suffisamment d'espace pour la prise en charge de la banque des versions. tempdb intègre deux banques de versions :  
  
-   la banque des versions de construction d'index en ligne, utilisée pour les constructions d'index en ligne dans l'ensemble des bases de données ;  
-   la banque des versions commune, utilisée pour toutes les autres opérations de modification des données dans l'ensemble des bases de données.  
  
 Les versions de ligne doivent être stockées pour toute la durée au cours de laquelle une transaction active doit être accessible. Chaque minute, un thread d'arrière-plan supprime les versions de ligne qui ne sont plus nécessaires et libère de l'espace dans tempdb. Une transaction longue empêche la libération d'espace dans une banque des versions si l'une des conditions suivantes est remplie :  
  
-   elle utilise l'isolement basé sur le contrôle de version de ligne ;  
-   elle utilise des déclencheurs, des jeux MARS ou des opérations de construction d'index en ligne ;  
-   elle génère des versions de ligne.  
  
> [!NOTE]  
> Quand un déclencheur est appelé au sein d'une transaction, les versions de ligne créées par le déclencheur sont conservées jusqu'à la fin de la transaction, même si les versions de ligne ne sont plus nécessaires après l'exécution du déclencheur. Ce point s'applique aussi aux transactions à lecture validée qui utilisent le contrôle de version de ligne. Dans ce type de transaction, une vue cohérente sur le plan transactionnel de la base de données n'est nécessaire que pour chaque instruction de la transaction. Cela signifie que les versions de ligne créées pour une instruction de la transaction ne sont plus nécessaires une fois l'instruction exécutée. Cependant, les versions de ligne créées par chaque instruction de la transaction sont conservées jusqu'à la fin de la transaction.  
  
 Quand tempdb n’a plus d’espace, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] force la réduction des banques des versions. Lors de ce processus de réduction, les transactions les plus longues n'ayant pas encore généré de versions de ligne sont marquées comme victimes. Le message 3967 est inscrit dans le journal d'erreurs pour chaque transaction victime. Toute transaction marquée comme victime ne peut plus lire les versions de ligne de la banque des versions. En cas de tentative de lecture des versions de ligne, le message 3966 est généré et la transaction est restaurée. En cas de réussite du processus de réduction, l'espace est disponible dans tempdb. Dans le cas contraire, l'espace de tempdb devient insuffisant, avec les conséquences suivantes :  
  
-   L'exécution des opérations d'écriture se poursuit, mais sans génération de versions. Un message d'information (3959) apparaît dans le journal d'erreurs. La transaction d'écriture des données n'en est pas affectée.  
  
-   Les transactions qui tentent d'accéder aux versions de ligne n'ayant pas été générées à cause d'une restauration complète dans tempdb se terminent sur l'erreur 3958.  
  
#### <a name="space-used-in-data-rows"></a>Espace occupé dans les lignes de données  
 Chaque ligne de base de données peut, à des fins d'informations sur le contrôle de version de ligne, utiliser un maximum de 14 octets en fin de ligne. Les informations sur le contrôle de version de ligne contiennent le numéro de séquence de la transaction ayant validé la version et le pointeur vers la ligne avec version. Ces 14 octets sont ajoutés lors de la première modification de la ligne ou lors de l'insertion d'une nouvelle ligne, pour autant que l'une des conditions suivantes soit remplie :  
  
-   l’option `READ_COMMITTED_SNAPSHOT` ou `ALLOW_SNAPSHOT_ISOLATION` est activée (ON) ;  
-   la table comporte un déclencheur ;  
-   des jeux MARS (Multiple Active Results Sets) sont en cours d'utilisation ;  
-   des opérations de construction d'index en ligne sont en cours d'exécution dans la table.  
  
 Ces 14 octets sont supprimés de la ligne de base de données lors de la première modification de la ligne, pour autant que toutes les conditions suivantes soient remplies :  
  
-   les options `READ_COMMITTED_SNAPSHOT` et `ALLOW_SNAPSHOT_ISOLATION` sont désactivées (OFF) ;  
-   le déclencheur n'existe plus dans la table ;  
-   les jeux MARS ne sont pas en cours d'utilisation ;  
-   aucune opération de construction d'index en ligne n'est en cours d'exécution.  
  
 En cas d'utilisation de l'une des fonctionnalités de contrôle de version de ligne, vous devrez peut-être allouer un espace disque suffisant pour permettre les 14 octets nécessaires par ligne de base de données. L'ajout d'informations sur le contrôle de version de ligne peut entraîner le fractionnement des pages d'index ou l'allocation d'une nouvelle page de données en cas d'insuffisance d'espace disponible sur la page actuelle. Par exemple, si la longueur de ligne moyenne est de 100 octets, les 14 octets supplémentaires peuvent provoquer une augmentation de 14 pour cent de la table existante.  
  
 La réduction du [facteur de remplissage](../relational-databases/indexes/specify-fill-factor-for-an-index.md) peut permettre d’empêcher ou de réduire la fragmentation des pages d’index. Pour afficher les informations de fragmentation des données et des index de la table ou vue spécifiée, vous pouvez utiliser [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).  
  
#### <a name="space-used-in-large-objects"></a>Espace occupé dans les objets volumineux  
 Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] prend en charge six types de données pouvant contenir des chaînes volumineuses d’une longueur de 2 gigaoctets (Go) au maximum : `nvarchar(max)`, `varchar(max)`, `varbinary(max)`, `ntext`, `text` et `image`. Les chaînes volumineuses stockées à l'aide de ces types de données sont stockées dans une série de fragments de données associés à la ligne de données. Les informations sur le contrôle de version de ligne sont stockées dans chaque fragment utilisé pour le stockage des chaînes volumineuses. Les fragments de données sont un ensemble de pages dédiées aux objets volumineux d'une table.  
  
 Lorsque des valeurs importantes sont ajoutées dans une base de données, elles sont allouées avec un maximum de 8 040 octets de données par fragment. Les versions antérieures du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pouvaient stocker jusqu’à 8 080 octets de données `ntext`, `text` ou `image` par fragment.  
  
 Les données des objets volumineux (LOB) `ntext`, `text` et `image` existants ne sont pas mises à jour pour libérer de l'espace pour les informations sur le contrôle de version de ligne lorsqu'une base de données est mise à niveau vers [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à partir d'une version antérieure de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Cependant, lors de leur première modification, les données LOB sont mises à niveau de manière dynamique pour permettre le stockage des informations sur le contrôle de version, même si des versions de lignes sont générées. Une fois la mise à niveau des données LOB terminée, le nombre maximum d'octets stockés par fragment passe de 8 080 à 8 040. Le processus de mise à niveau équivaut à supprimer la valeur LOB et à réinsérer la même valeur. Les données LOB sont mises à niveau même en cas de modification d'un seul octet. Cette opération est unique pour chaque colonne `ntext`, `text` ou `image` Chaque opération peut néanmoins générer une quantité importante d'allocations de pages et d'activité E/S selon la taille des données LOB, ainsi qu'une activité importante d'écriture dans le journal si la modification doit être écrite en entier dans le journal. Les opérations WRITETEXT et UPDATETEXT sont écrites dans le journal de façon minimale si le mode de récupération de la base de données n'est pas défini sur FULL.  
  
 Les types de données `nvarchar(max)`, `varchar(max)` et `varbinary(max)` ne sont pas disponibles dans les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Vous ne rencontrerez pas conséquent aucun problème de mise à niveau.  
  
 Un espace disque suffisant doit être alloué pour satisfaire à cette exigence.  
  
#### <a name="monitoring-row-versioning-and-the-version-store"></a>Contrôle du contrôle de version de ligne et du magasin de versions  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fournit des outils pour le contrôle du contrôle de version de ligne, du magasin de versions et des processus d'isolement d'instantané : les vues DMV (Dynamic Management Views) et les compteurs de performances dans le Moniteur système Windows.  
  
##### <a name="dmvs"></a>Vues DMV  
 Les vues DMV suivantes fournissent des informations sur l'état système actuel de tempdb et du magasin de versions, ainsi que sur les transactions utilisant le contrôle de version de ligne.  
  
 sys.dm_db_file_space_usage. Retourne des informations sur l'utilisation de l'espace pour chaque fichier de la base de données. Pour plus d’informations, consultez [sys.dm_db_file_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md).  
  
 sys.dm_db_session_space_usage. Renvoie les activités d'allocation ou de désallocation des pages par session de la base de données. Pour plus d’informations, consultez [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md).  
  
 sys.dm_db_task_space_usage. Renvoie l'activité d'allocation/désallocation des pages par tâche pour la base de données. Pour plus d’informations, consultez [sys.dm_db_task_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md).  
  
 sys.dm_tran_top_version_generators. Renvoie une table virtuelle pour les objets générant la majorité des versions d'un magasin de versions. Agrégation des 256 premières longueurs d'enregistrement selon database_id et rowset_id. Utilisez cette fonction pour rechercher les clients les plus volumineux de la banque de versions. Pour plus d’informations, consultez [sys.dm_tran_top_version_generators &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-top-version-generators-transact-sql.md).  
  
 sys.dm_tran_version_store. Renvoie une table virtuelle qui affiche tous les enregistrements de version du magasin de versions commun. Pour plus d’informations, consultez [sys.dm_tran_version_store &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md).  

 sys.dm_tran_version_store_space_usage. Retourne une table virtuelle qui affiche l’espace total dans tempdb utilisé par les enregistrements de banque des versions pour chaque base de données. Pour plus d’informations, consultez [sys.dm_tran_version_store_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-space-usage.md).  

> [!NOTE]  
> sys.dm_tran_top_version_generators et sys.dm_tran_version_store sont des fonctions potentiellement très compliquées à exécuter dans la mesure où elles interrogent toutes deux le magasin de versions entier, qui peut s'avérer très volumineux.  
> La vue sys.dm_tran_version_store_space_usage est efficace et peu coûteuse à exécuter, car elle ne parcourt pas les enregistrements de banque des versions individuels et retourne un espace de banque des versions agrégé consommé dans tempdb par base de données
  
 sys.dm_tran_active_snapshot_database_transactions. Retourne une table virtuelle pour toutes les transactions actives dans l'ensemble des bases de données d'une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilisant le contrôle de version de ligne. Les transactions système n'apparaissent pas dans cette vue DMV. Pour plus d’informations, consultez [sys.dm_tran_active_snapshot_database_transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md).  
  
 sys.dm_tran_transactions_snapshot. Renvoie une table virtuelle qui affiche les instantanés pris par chaque transaction. L'instantané contient le numéro de séquence des transactions actives utilisant le contrôle de version de ligne. Pour plus d’informations, consultez [sys.dm_tran_transactions_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-transactions-snapshot-transact-sql.md).  
  
 sys.dm_tran_current_transaction. Renvoie une ligne unique affichant des informations sur l'état du contrôle de version de ligne pour la transaction de la session en cours. Pour plus d’informations, consultez [sys.dm_tran_current_transaction &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-transaction-transact-sql.md).  
  
 sys.dm_tran_current_snapshot. Retourne une table virtuelle affichant toutes les transactions actives au début de la transaction d'isolement d'instantané. Si la transaction actuelle utilise l'isolement d'instantané, cette fonction ne retourne aucune ligne. sys.dm_tran_current_snapshot est similaire à sys.dm_tran_transactions_snapshot, mis à part qu’elle retourne uniquement les transactions actives pour l’instantané actuel. Pour plus d’informations, consultez [sys.dm_tran_current_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-snapshot-transact-sql.md).  
  
##### <a name="performance-counters"></a>Compteurs de performance  
 Les compteurs de performance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fournissent des informations sur les performances système affectées par les processus de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Les compteurs de performances suivants contrôlent tempdb et le magasin de versions, ainsi que les transactions utilisant le contrôle de version de ligne. Les compteurs de performances se trouvent dans l'objet de performances SQLServer:Transactions.  
  
 **Espace disponible dans tempdb (Ko)** . Contrôle la quantité, en kilooctets (Ko), d'espace libre dans la base de données tempdb. tempdb doit disposer d'un espace libre suffisant pour gérer le magasin de versions prenant en charge l'isolement d'instantané.  
  
 La formule ci-dessous vous donne une estimation grossière de la taille du magasin de versions. Pour estimer la taille du magasin de versions en ce qui concerne les transactions longues, il peut s'avérer utile de contrôler les taux de génération et de nettoyage.  
  
 [taille de la banque des versions commune] = 2 * [données de la banque des versions générées par minute] \* [délai d’exécution le plus long (en minutes) de la transaction]  
  
 Le délai le plus long d'exécution de transaction ne doit pas inclure les constructions d'un index en ligne. Étant donné que ces dernières opérations peuvent prendre un certain temps pour les tables volumineuses, elles utilisent un autre magasin de versions. La taille approximative du magasin de versions utilisé pour les constructions d'un index en ligne équivaut à la quantité de données modifiées dans la table, y compris tous les index, pendant toute la durée d'activité de la construction de l'index en ligne.  
  
 **Taille de la banque des versions (Ko)** . Contrôle la taille en Ko de tous les magasins de versions. Cette information permet de déterminer la quantité d'espace nécessaire dans la base de données tempdb pour le magasin de versions. Le contrôle de ce compteur sur une période de temps fournit une estimation utile de l'espace supplémentaire requis pour tempdb.  
  
 `Version Generation rate (KB/s)`. Contrôle le taux de génération de version en Ko par seconde pour tous les magasins de versions.  
  
 `Version Cleanup rate (KB/s)`. Contrôle le taux de nettoyage de version en Ko par seconde pour tous les magasins de versions.  
  
> [!NOTE]  
> Les informations obtenues à l'aide des compteurs Taux de génération de version (Ko/s) et Taux de nettoyage de version (Ko/s) permettent de prévoir l'espace nécessaire pour tempdb.  
  
 **Nombre d’unités dans la banque des versions**. Contrôle le nombre d'unités dans le magasin de versions.  
  
 **Création d’unité dans la banque des versions**. Contrôle le nombre total d'unités créées dans le magasin de versions pour le stockage des versions de lignes depuis le démarrage de l'instance.  
  
 **Troncation d’unité dans la banque des versions**. Contrôle le nombre total d'unités tronquées dans le magasin de versions depuis le démarrage de l'instance. Une unité de magasin de versions est tronquée lorsque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] spécifie qu'aucune des lignes de versions stockées dans l'unité du magasin de versions n'est requise pour l'exécution des transactions actives.  
  
 **Proportion de conflits de mise à jour**. Contrôle la proportion de transactions d'instantanés de mise à jour présentant des conflits de mise à jour par rapport au nombre total de transactions d'instantanés de mise à jour.  
  
 **Délai le plus long d’exécution de transaction**. Contrôle le délai le plus long (en secondes) d'exécution de toute transaction utilisant le contrôle de version de ligne. Ce compteur permet de déterminer si l'exécution de l'une des transactions est trop longue.  
  
 **Transactions**. Contrôle le nombre total de transactions actives. Les transactions système ne sont pas prises en compte.  
  
 `Snapshot Transactions`. Contrôle le nombre total de transactions d'instantanés actives.  
  
 `Update Snapshot Transactions`. Contrôle le nombre total de transactions d'instantanés effectuant des opérations de mise à jour.  
  
 `NonSnapshot Version Transactions`. Contrôle le nombre total de transactions actives non liées à des instantanés générant des enregistrements de versions.  
  
> [!NOTE]  
> La somme des compteurs Transactions d'instantanés de mise à jour et Transactions de versions non liées à des instantanés représente le nombre total de transactions participant à la génération d'une version. La différence entre les compteurs Transactions d'instantanés et Transactions d'instantanés de mise à jour indique le nombre de transactions d'instantanés en lecture seule.  
  
### <a name="row-versioning-based-isolation-level-example"></a>Exemple de niveau d'isolement basé sur le contrôle de version de ligne  
 Les exemples ci-dessous illustrent les différences de comportement entre les transactions d'isolement d'instantané et les transactions validées en écriture qui utilisent le contrôle de version de ligne.  
  
#### <a name="a-working-with-snapshot-isolation"></a>R. Utilisation du niveau d'isolement d'instantané  
 Dans cet exemple, une transaction exécutée sous isolement d'instantané lit des données qui sont ensuite modifiées par une autre transaction. La transaction d'instantané ne bloque pas l'opération de mise à jour exécutée par l'autre transaction et continue de lire les données à partir de la ligne avec version, en ignorant la modification apportée aux données. Toutefois, lorsque la transaction d'instantané tente de modifier des données qui ont déjà été modifiées par l'autre transaction, la transaction d'instantané génère une erreur et est terminée.  
  
 Sur la session 1 :  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Enable snapshot isolation on the database.  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Start a snapshot transaction  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Sur la session 2 :  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under snapshot isolation shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Sur la session 1 :  
  
```sql  
    -- Reissue the SELECT statement - this shows  
    -- the employee having 48 vacation hours.  The  
    -- snapshot transaction is still reading data from  
    -- the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Sur la session 2 :  
  
```sql  
-- Commit the transaction; this commits the data  
-- modification.  
COMMIT TRANSACTION;  
GO  
```  
  
 Sur la session 1 :  
  
```sql  
    -- Reissue the SELECT statement - this still   
    -- shows the employee having 48 vacation hours  
    -- even after the other transaction has committed  
    -- the data modification.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- Because the data has been modified outside of the  
    -- snapshot transaction, any further data changes to   
    -- that data by the snapshot transaction will cause   
    -- the snapshot transaction to fail. This statement   
    -- will generate a 3960 error and the transaction will   
    -- terminate.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION  
GO  
```  
  
#### <a name="b-working-with-read-committed-using-row-versioning"></a>B. Utilisation d'une transaction validée en lecture à l'aide du contrôle de version de ligne  
 Dans cet exemple, une transaction validée en lecture à l'aide du contrôle de version de ligne est exécutée en même temps qu'une autre transaction. La transaction validée en lecture se comporte différemment de la transaction d'instantané. À l'instar d'une transaction d'instantané, la transaction validée en lecture lit les lignes avec version même après la modification des données effectuée par l'autre transaction. Toutefois, contrairement à une transaction d'instantané, la transaction validée en lecture :  
  
-   lit les données modifiées une fois que l'autre transaction a validé les modifications de données ;  
-   peut mettre à jour les données modifiées par l'autre transaction, alors que cette opération n'est pas possible avec la transaction d'instantané.  
  
 Sur la session 1 :  
  
```sql  
USE AdventureWorks2016;  -- Or any earlier version of the AdventureWorks database.  
GO  
  
-- Enable READ_COMMITTED_SNAPSHOT on the database.  
-- For this statement to succeed, this session  
-- must be the only connection to the AdventureWorks2016  
-- database.  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
GO  
  
-- Start a read-committed transaction  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Sur la session 2 :  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under read-committed using row versioning shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Sur la session 1 :  
  
```sql  
    -- Reissue the SELECT statement - this still shows  
    -- the employee having 48 vacation hours.  The  
    -- read-committed transaction is still reading data   
    -- from the versioned row and the other transaction   
    -- has not committed the data changes yet.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Sur la session 2 :  
  
```sql  
-- Commit the transaction.  
COMMIT TRANSACTION;  
GO  
```  
  
 Sur la session 1 :  
  
```sql  
    -- Reissue the SELECT statement which now shows the   
    -- employee having 40 vacation hours.  Being   
    -- read-committed, this transaction is reading the   
    -- committed data. This is different from snapshot  
    -- isolation which reads from the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- This statement, which caused the snapshot transaction   
    -- to fail, will succeed with read-committed using row versioning.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION;  
GO  
```  
  
### <a name="enabling-row-versioning-based-isolation-levels"></a>Activation des niveaux d'isolement selon le contrôle de version de ligne  
 Les administrateurs de bases de données déterminent les paramètres de contrôle de version de ligne définis au niveau de la base de données à l’aide des options de base de données `READ_COMMITTED_SNAPSHOT` et `ALLOW_SNAPSHOT_ISOLATION` de l’instruction ALTER DATABASE.  
  
 Quand l’option de base de données `READ_COMMITTED_SNAPSHOT` est activée (ON), les mécanismes de prise en charge de l’option sont immédiatement activés. Lors du paramétrage de l’option READ_COMMITTED_SNAPSHOT, seule la connexion exécutant la commande `ALTER DATABASE` est autorisée dans la base de données. La base de données ne peut contenir aucune autre connexion ouverte avant la fin de l'exécution de la commande ALTER DATABASE. Il n'est pas nécessaire que la base de données soit en mode mono-utilisateur.  
  
 L’instruction [!INCLUDE[tsql](../includes/tsql-md.md)] suivante active `READ_COMMITTED_SNAPSHOT` :  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
```  
  
 Quand l’option de base de données `ALLOW_SNAPSHOT_ISOLATION` est activée (ON), l’instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ne génère le contrôle de version de ligne pour les données modifiées que quand l’exécution de toutes les transactions actives modifiant les données de la base de données est terminée. En cas de transactions de modification actives, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] affecte à l’option l’état `PENDING_ON`. Une fois l'exécution des transactions de modification terminées, l'état de l'option passe sur ON. Les utilisateurs ne peuvent lancer une transaction d'instantané que quand l'option a la valeur ON. La base de données passe par l’état PENDING_OFF quand son administrateur affecte à l’option `ALLOW_SNAPSHOT_ISOLATION` la valeur OFF.  
  
 L'instruction [!INCLUDE[tsql](../includes/tsql-md.md)] suivante permet la prise en charge de l'option ALLOW_SNAPSHOT_ISOLATION :  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
```  
  
 Le tableau suivant répertorie et décrit les différents états de l'option ALLOW_SNAPSHOT_ISOLATION. L'utilisation de la commande ALTER DATABASE avec l'option ALLOW_SNAPSHOT_ISOLATION ne bloque pas les utilisateurs qui sont en cours d'accès aux données de la base de données.  
  
|État de l'infrastructure d'isolement d'instantané pour la base de données actuelle|Description|  
|----------------------------------------------------------------|-----------------|  
|OFF|La prise en charge des transactions d'isolement d'instantané n'est pas activée. Aucune transaction d'isolement d'instantané n'est autorisée.|  
|PENDING_ON|La prise en charge des transactions d'isolement d'instantané est en état de transition (de OFF à ON). L'exécution de toutes les transactions ouvertes doit être terminée.<br /><br /> Aucune transaction d'isolement d'instantané n'est autorisée.|  
|ACTIVÉ|La prise en charge des transactions d'isolement d'instantané est activée.<br /><br /> Les transactions d'isolement d'instantané sont autorisées.|  
|PENDING_OFF|La prise en charge des transactions d'isolement d'instantané est en état de transition (de ON à OFF).<br /><br /> Les transactions d'instantané lancées dès ce moment ne peuvent accéder à la base de données. Les transactions de mise à jour assument la responsabilité du versioning dans la base de données. Les transactions d'instantané existantes peuvent toujours accéder à la base de données sans aucun problème. L'état PENDING_OFF ne passe sur OFF qu'à la fin de l'exécution de toutes les transactions d'instantané activées lorsque l'état d'isolement d'instantané de la base de données correspondait à ON.|  
  
 Utilisez l'affichage catalogue `sys.databases` pour déterminer l'état des deux options de contrôle de version de ligne de la base de données.  
  
 Toutes les mises à jour des tables utilisateur et de certaines tables système stocku dans les tables de données master et msdb génère le contrôle de version de ligne.  
  
 L’option `ALLOW_SNAPSHOT_ISOLATION` est automatiquement activée (ON) dans les bases de données master et msdb. Elle ne peut être désactivée.  
  
 Les bases de données master, tempdb et msdb ne permettent pas aux utilisateurs d’affecter à l’option `READ_COMMITTED_SNAPSHOT` la valeur ON.  
  
### <a name="using-row-versioning-based-isolation-levels"></a>Utilisation de niveaux d'isolement basés sur le contrôle de version de ligne  
 L'infrastructure de contrôle de version de ligne est toujours activée dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et est utilisée par plusieurs fonctionnalités. En plus de fournir des niveaux d'isolement basés sur le contrôle de version de ligne, elle permet la prise en charge des modifications apportées aux déclencheurs et aux sessions MARS (Multiple Active Result Sets), ainsi que la prise en charge des lectures de données pour les opérations d'index ONLINE.  
  
 Les niveaux d'isolement basés sur le contrôle de version de ligne sont activés au niveau de la base de données. Toute application accédant à des objets de bases de données activées peut exécuter des requêtes en utilisant les niveaux d'isolement suivants :  
  
-   READCOMMITTED (lu-validé) avec utilisation du contrôle de version de ligne par l'activation de l'option de base de données `READ_COMMITTED_SNAPSHOT` (valeur `ON`) comme illustré dans l'exemple de code suivant :  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET READ_COMMITTED_SNAPSHOT ON;  
    ```  
  
     Quand l’option `READ_COMMITTED_SNAPSHOT` est activée pour la base de données, toutes les requêtes s’exécutant sous le niveau d’isolation read committed utilisent le contrôle de version de ligne, ce qui signifie que les opérations de lecture ne bloquent pas les opérations de mise à jour.  
  
-   Niveau d'isolement d'instantané en définissant l'option de base de données `ALLOW_SNAPSHOT_ISOLATION` avec la valeur `ON` comme illustré dans l'exemple de code suivant :  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET ALLOW_SNAPSHOT_ISOLATION ON;  
    ```  
  
     Une transaction s'exécutant sous le niveau d'isolement d'instantané (SNAPSHOT) peut accéder aux tables de la base de données qui ont été activées pour les instantanés. Pour accéder aux tables qui n'ont pas été activées pour les instantanés, le niveau d'isolement doit être modifié. Ainsi, dans l'exemple de code suivant, une instruction `SELECT` exécutée dans le cadre d'une transaction d'instantané joint deux tables. Une table appartient à une base de données dans laquelle le niveau d'isolement d'instantané (SNAPSHOT) n'est pas activé. Lorsque l'instruction `SELECT` s'exécute sous le niveau d'isolement d'instantané, elle échoue.  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
     Dans l'exemple de code suivant, la même instruction `SELECT` a été modifiée pour faire passer le niveau d'isolation de la transaction à READCOMMITTED (lu-validé). Grâce à cette modification, l'exécution de l'instruction `SELECT` aboutit.  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            WITH (READCOMMITTED)  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
#### <a name="limitations-of-transactions-using-row-versioning-based-isolation-levels"></a>Limites liées aux transactions utilisant les niveaux d'isolement basés sur le contrôle de version de ligne  
 Tenez compte des limites suivantes lors de l'utilisation des niveaux d'isolement basés sur le contrôle de version de ligne :  
  
-   L’option `READ_COMMITTED_SNAPSHOT` ne peut pas être activée dans les bases de données tempdb, msdb et master.  
-   Les tables temporaires globales sont stockées dans tempdb. Si une transaction d'instantané implique l'accès à des tables temporaires globales, vous devez effectuer l'une des opérations suivantes :  
    -   Activez l’option de base de données `ALLOW_SNAPSHOT_ISOLATION` (ON) dans tempdb.  
    -   Utilisez un indicateur d'isolement afin de modifier le niveau d'isolement pour l'instruction.  
-   Les transactions d'instantané échouent dans les cas suivants :  
    -   Une base de données est passée en lecture seule après que la transaction d'instantané ait démarré, mais avant que celle-ci ait accédé à la base de données.  
    -   S'il y a eu accès à des objets de plusieurs bases de données, l'état d'une base de données a été modifié au point qu'une récupération de base de données a eu lieu après que la transaction d'instantané ait démarré, mais avant que celle-ci ait accédé à la base de données. Par exemple, la base de données a été définie sur OFFLINE puis sur ONLINE, la base de données s'est fermée automatiquement puis rouverte, ou elle s'est détachée puis rattachée.  
-   Les transactions distribuées, notamment les requêtes dans les bases de données partitionnées distribuées, ne sont pas prises en charge sous le niveau d'isolement d'instantané.  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne conserve pas plusieurs versions des métadonnées système. Les instructions DDL (Data Definition Language) portant sur des tables et autres objets de base de données (index, vues, types de données, procédures stockées et fonctions CLR (Common Language Runtime)) modifient les métadonnées. Si une instruction DDL modifie un objet, toute référence simultanée à l'objet sous le niveau d'isolement d'instantané entraînera l'échec de la transaction d'instantané. Les transactions READCOMMITTED (lu-validé) ne présentent pas cette limitation lorsque l'option de base de données READ_COMMITTED_SNAPSHOT est activée (valeur ON).  
  
     Supposons, par exemple, qu'un administrateur de base de données exécute l'instruction `ALTER INDEX` suivante.  
  
    ```sql  
    USE AdventureWorks2016;  
    GO  
    ALTER INDEX AK_Employee_LoginID  
        ON HumanResources.Employee REBUILD;  
    GO  
    ```  
  
     Toute transaction d'instantané qui est active au moment de l'exécution de l'instruction `ALTER INDEX` recevra une erreur si elle tente de faire référence à la table `HumanResources.Employee` après l'exécution de l'instruction `ALTER INDEX`. Les transactions READCOMMITTED (lu-validé) utilisant le contrôle de version de ligne ne sont pas affectées.  
  
    > [!NOTE]  
    > Les opérations BULK INSERT peuvent entraîner des modifications au niveau des métadonnées de la table cible (par exemple, lors de la désactivation des vérifications de contraintes). Dans ce cas, les transactions simultanées d'isolement d'instantané qui accèdent à des tables faisant l'objet d'insertion en bloc échouent.  
  
   
## <a name="customizing-locking-and-row-versioning"></a>Personnalisation du verrouillage et du contrôle de version de ligne  
  
### <a name="customizing-the-lock-time-out"></a>Personnalisation du délai d'attente de verrouillage  
 Quand une instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../includes/msconame-md.md)] ne peut pas accorder un verrou à une transaction, car une autre transaction a déjà un verrou en conflit dans la ressource, la première transaction se bloque, dans l’attente de la libération du verrou existant. Par défaut, il n'existe pas de délai d'expiration obligatoire et aucun moyen de tester si une ressource est déjà verrouillée avant de la verrouiller, excepté par une tentative d'accès aux données (avec un risque de blocage infini).  
  
> [!NOTE]  
> Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], utilisez la vue de gestion dynamique **sys.dm_os_waiting_tasks** pour déterminer si un processus est bloqué et identifier l’auteur du blocage. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], utilisez la procédure stockée système **sp_who**.  
  
 Le paramètre `LOCK_TIMEOUT` permet à une application de définir la durée maximale pendant laquelle une instruction reste en attente sur une ressource bloquée. Si l'attente d'une instruction dépasse la valeur du paramètre LOCK_TIMEOUT, l'instruction bloquée est automatiquement annulée, et le message d'erreur 1222 (`Lock request time-out period exceeded`) retourné à l'application. Toute transaction contenant la commande n'est toutefois pas restaurée ou annulée par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. L'application doit donc posséder un gestionnaire d'erreurs capable d'intercepter le message d'erreur 1222. Si une application ne gère pas cette erreur, elle peut continuer en ignorant que l'une des instructions de la transaction a été annulée. Des erreurs peuvent se produire lorsque les instructions ultérieures dans la transaction dépendent de l'instruction qui n'a jamais été exécutée.  
  
 L'implémentation d'un gestionnaire d'erreurs qui intercepte le message d'erreur 1222 permet à une application de prendre les mesures conséquentes au délai d'expiration, par exemple soumettre à nouveau l'instruction qui été bloquée ou restaurer toute la transaction.  
  
 Pour déterminer le paramètre `LOCK_TIMEOUT` actuel, exécutez la fonction `@@LOCK_TIMEOUT` :  
  
```sql  
SELECT @@lock_timeout;  
GO  
```  
  
### <a name="customizing-transaction-isolation-level"></a>Personnalisation du niveau d'isolation des transactions  
 READ COMMITTED est le niveau d’isolation par défaut pour le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../includes/msconame-md.md)]. Si une application doit fonctionner à un niveau d'isolation différent, elle peut le définir selon plusieurs méthodes :  
  
-   Exécuter l’instruction [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
-   Les applications ADO.NET utilisant l’espace de noms managé System.Data.SqlClient peuvent indiquer une option *IsolationLevel* via la méthode SqlConnection.BeginTransaction.  
-   Les applications qui utilisent ADO peuvent définir la propriété `Autocommit Isolation Levels`.  
-   Lors du lancement d’une transaction, les applications utilisant OLE DB peuvent appeler ITransactionLocal::StartTransaction avec le paramètre *isoLevel* défini sur le niveau d’isolation de la transaction souhaité. Lorsque vous spécifiez le niveau d'isolation en mode de validation automatique, les applications utilisant OLE DB peuvent affecter à la propriété DBPROP_SESS_AUTOCOMMITISOLEVELS de DBPROPSET_SESSION la valeur de niveau d'isolation de la transaction souhaitée.  
-   Les applications qui utilisent ODBC peuvent définir l’attribut SQL_COPT_SS_TXN_ISOLATION à l’aide de SQLSetConnectAttr.  
  
Lorsque le niveau d'isolation est spécifié, le verrouillage s'applique à ce niveau d'isolation, à toutes les requêtes et instructions DML de la session [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le niveau d'isolation reste en vigueur jusqu'à la fin de la session ou jusqu'à ce qu'il soit modifié.  
  
L'exemple suivant montre comment définir le niveau d'isolation `SERIALIZABLE` :  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
SELECT BusinessEntityID  
    FROM HumanResources.Employee;  
GO  
```  
  
Le niveau d'isolation peut être remplacé si nécessaire pour des requêtes ou des instructions DML individuelles, en spécifiant un indicateur de niveau table. Un indicateur de niveau table n'affecte pas les autres instructions de la session. Il est recommandé de n'utiliser les indicateurs de niveau table pour modifier le comportement par défaut qu'en cas d'absolue nécessité.  
  
Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] peut être obligé d’acquérir des verrous lors de la lecture de métadonnées même si le niveau d’isolation est tel que les verrous partagés ne sont pas nécessaires pendant la lecture des données. Par exemple, une transaction qui s'exécute au niveau d'isolation READ UNCOMMITTED n'acquiert pas de verrous partagés pendant la lecture de données, mais elle peut en demander quelquefois lors de la lecture d'un affichage catalogue système. Autrement dit, une transaction de lecture non validée peut provoquer un blocage si elle interroge une table pendant qu'une transaction modifie simultanément les données de cette table.  
  
Pour déterminer le niveau d'isolation des transactions en cours, utilisez l'instruction `DBCC USEROPTIONS` comme dans l'exemple qui suit. Le jeu de résultats peut être différent sur votre système.  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
DBCC USEROPTIONS;  
GO  
```  
  
[!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```
Set Option                   Value  
---------------------------- -------------------------------------------  
textsize                     2147483647  
language                     us_english  
dateformat                   mdy  
datefirst                    7  
...                          ...  
Isolation level              repeatable read  
 
(14 row(s) affected)   
 
DBCC execution completed. If DBCC printed error messages, contact your system administrator.
```  
  
### <a name="locking-hints"></a>Indicateurs de verrouillage  
 Il est possible de spécifier des indicateurs de verrouillage pour des références de table individuelles dans les instructions SELECT, INSERT, UPDATE et DELETE. Ces indicateurs déterminent le type de verrouillage ou de contrôle de version de ligne qu’utilise l’instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pour les données de la table. Les indicateurs de verrouillage au niveau des tables peuvent être utilisés pour un contrôle plus fin des types de verrous acquis sur un objet. Ces options de verrouillage remplacent le niveau d'isolement courant de la transaction pour la session.  
  
 Pour plus d’informations sur les indicateurs de verrouillage spécifiques et leurs comportements, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-table.md).  
  
> [!NOTE]  
> Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] choisit presque toujours le niveau de verrouillage correct. Nous vous recommandons d'utiliser les indicateurs de verrouillage au niveau des tables à la place du verrouillage par défaut seulement lorsque cela est nécessaire. La désactivation d'un niveau de verrouillage peut affecter défavorablement la concurrence d'accès.  
  
 Il se peut que le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] doive obtenir des verrous lors de la lecture de métadonnées, même lors du traitement d'une sélection avec un indicateur de verrouillage qui empêche les demandes de verrous de partage lors de la lecture de données. Par exemple, une instruction `SELECT` qui utilise l’indicateur `NOLOCK` n’obtient pas de verrous partagés lors de la lecture de données, mais elle peut occasionnellement demander des verrous quand elle lit un affichage catalogue système. Cela signifie qu’il est possible qu’une instruction `SELECT` utilisant `NOLOCK` soit bloquée.  
  
 Comme l'illustre l'exemple suivant, si le niveau d'isolement d'une transaction est `SERIALIZABLE` et que l'indicateur de verrouillage `NOLOCK` au niveau des tables est utilisé avec l'instruction `SELECT`, les verrous de clé habituellement utilisés pour préserver des transactions sérialisables ne sont pas appliqués.  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT JobTitle  
    FROM HumanResources.Employee WITH (NOLOCK);  
GO  
  
-- Get information about the locks held by   
-- the transaction.  
SELECT    
        resource_type,   
        resource_subtype,   
        request_mode  
    FROM sys.dm_tran_locks  
    WHERE request_session_id = @@spid;  
  
-- End the transaction.  
ROLLBACK;  
GO  
```  
  
 Le seul verrou appliqué faisant référence à `HumanResources.Employee` est le verrou de stabilité de schéma (Sch-S). Dans ce cas, la possibilité de sérialisation n'est plus garantie.  
  
 Dans [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], l’option `LOCK_ESCALATION` de l’instruction `ALTER TABLE` peut défavoriser des verrous de table et activer des verrous HoBT sur des tables partitionnées. Cette option n'est pas un indicateur de verrouillage, mais elle peut servir à réduire l'escalade de verrous. Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="customizing-locking-for-an-index"></a><a name="Customize"></a> Personnalisation du verrouillage pour un index  
 Dans la plupart des cas, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise une stratégie de verrouillage dynamique qui choisit automatiquement la granularité de verrouillage la plus appropriée pour les requêtes. Nous vous recommandons de ne pas remplacer les niveaux de verrouillage par défaut, pour lesquels le verrouillage de page et de ligne est activé, sauf si les modèles d'accès à la table ou à l'index sont bien assimilés et cohérents et s'il existe une contention de ressources à résoudre. Le remplacement d’un niveau de verrouillage peut affecter considérablement les accès simultanés à une table ou un index. Par exemple, la spécification de verrous de niveau table uniquement sur une table de grande taille à laquelle les utilisateurs accèdent fréquemment peut provoquer des goulets d’étranglement, car les utilisateurs doivent attendre que le verrou de niveau table soit libéré avant de pouvoir accéder à la table.  
  
 Il existe quelques cas où l'interdiction du verrouillage de page ou de ligne peut être avantageuse, à condition que les modèles d'accès soient bien assimilés et cohérents. Par exemple, une application de base de données utilise une table de recherche mise à jour chaque semaine via un processus par lot. Les lecteurs simultanés accèdent à la table avec un verrou partagé (S) et la mise à jour par lot hebdomadaire accède à la table avec un verrou exclusif (X). La désactivation du verrouillage de page et de ligne sur la table réduit la charge de traitement liée au verrouillage tout au long de la semaine en permettant aux lecteurs d'accéder simultanément à la table via des verrous de table partagés. Lorsque le programme de traitement par lot s'exécute, il peut effectuer la mise à jour efficacement, car il obtient un verrou de table exclusif.  
  
 La désactivation du verrouillage de page et de ligne peut parfois être acceptable, car la mise à jour par lot hebdomadaire empêche les lecteurs simultanés d'accéder à la table pendant l'exécution de la mise à jour. Si le programme de traitement par lot modifie seulement quelques lignes ou pages, vous pouvez changer le niveau de verrouillage afin d'autoriser le verrouillage de ligne ou de page ; cela permet à la table d'être lue sans blocage dans d'autres sessions. Si le programme de traitement par lot doit effectuer un grand nombre de mises à jour, l'obtention d'un verrou exclusif sur la table peut représenter la meilleure solution pour garantir l'efficacité et la totalité de son exécution.  
  
 Parfois, un blocage se produit dans les circonstances suivantes : deux opérations simultanées acquièrent des verrous de ligne sur la même table, puis se bloquent, car elles doivent toutes les deux verrouiller la page. L'interdiction des verrous de ligne force l'une des opérations à attendre, ce qui évite le blocage.  
  
 La granularité de verrouillage utilisée sur un index peut être définie via les instructions `CREATE INDEX` et `ALTER INDEX`. Les paramètres de verrouillage s'appliquent à la fois aux pages d'index et aux pages de table. De plus, les instructions `CREATE TABLE` et `ALTER TABLE` peuvent être utilisées pour définir la granularité de verrouillage sur les contraintes `PRIMARY KEY` et `UNIQUE`. À des fins de compatibilité descendante, la procédure stockée système `sp_indexoption` peut également définir la granularité. Pour afficher l’option de verrouillage en cours pour un index donné, utilisez la fonction `INDEXPROPERTY`. Les verrous au niveau des pages, des lignes, ou une combinaison de ces derniers peuvent être refusés pour un index donné.  
  
|Verrous refusés|Accès à l'index par|  
|----------------------|-----------------------|  
|Niveau page|Verrous au niveau des lignes et des tables|  
|Niveau ligne|Verrous au niveau des pages et des tables|  
|Niveau page et niveau ligne|Verrous au niveau des tables|  
  
##  <a name="advanced-transaction-information"></a><a name="Advanced"></a> Informations sur les transactions avancées  
  
### <a name="nesting-transactions"></a>Transactions imbriquées  
 Les transactions explicites peuvent être imbriquées. Cette fonctionnalité est avant tout destinée à la prise en charge des transactions dans les procédures stockées appelées par un processus faisant partie d'une transaction, ou par des processus ne disposant pas de transaction active.  
  
 L'exemple suivant illustre cette utilisation des transactions imbriquées. La procédure *TransProc* applique sa transaction quel que soit le mode de transaction du processus qui l’exécute. Si *TransProc* est appelée alors qu’une transaction est active, la transaction imbriquée dans *TransProc* est en grande partie ignorée, et les instructions `INSERT` qu’elle contient sont validées ou restaurées en fonction de la dernière action effectuée dans la transaction la plus externe. Si `TransProc` est exécutée par un processus pour lequel aucune transaction n’est en cours, l’instruction `COMMIT TRANSACTION` qui se trouve à la fin de la procédure déclenche la validation effective des instructions `INSERT`.  
  
```sql  
SET QUOTED_IDENTIFIER OFF;  
GO  
SET NOCOUNT OFF;  
GO  
CREATE TABLE TestTrans(Cola INT PRIMARY KEY,  
               Colb CHAR(3) NOT NULL);  
GO  
CREATE PROCEDURE TransProc @PriKey INT, @CharCol CHAR(3) AS  
BEGIN TRANSACTION InProc  
INSERT INTO TestTrans VALUES (@PriKey, @CharCol)  
INSERT INTO TestTrans VALUES (@PriKey + 1, @CharCol)  
COMMIT TRANSACTION InProc;  
GO  
/* Start a transaction and execute TransProc. */  
BEGIN TRANSACTION OutOfProc;  
GO  
EXEC TransProc 1, 'aaa';  
GO  
/* Roll back the outer transaction, this will  
   roll back TransProc's nested transaction. */  
ROLLBACK TRANSACTION OutOfProc;  
GO  
EXECUTE TransProc 3,'bbb';  
GO  
/* The following SELECT statement shows only rows 3 and 4 are   
   still in the table. This indicates that the commit  
   of the inner transaction from the first EXECUTE statement of  
   TransProc was overridden by the subsequent rollback. */  
SELECT * FROM TestTrans;  
GO  
```  
  
 La validation des transactions internes est ignorée par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. La transaction est validée ou restaurée en fonction de l'action prise à la fin de la transaction la plus externe. Si la transaction externe est validée, les transactions imbriquées dans celle-ci le sont aussi. Si la transaction externe est restaurée, toutes les transactions internes le sont aussi, qu'elles aient été validées individuellement ou non.  
  
 Chaque appel à `COMMIT TRANSACTION` ou `COMMIT WORK` s’applique à la dernière instruction `BEGIN TRANSACTION` exécutée. Si les instructions `BEGIN TRANSACTION` sont imbriquées, une instruction `COMMIT` s’applique uniquement à la dernière transaction imbriquée, qui est la transaction la plus intérieure. Même si une instruction `COMMIT TRANSACTION` *transaction_name* d’une transaction imbriquée fait référence à la transaction externe, seule la transaction la plus intérieure est validée.  
  
 Le paramètre *transaction_name* d’une instruction `ROLLBACK TRANSACTION` ne doit pas faire référence aux transactions internes d’un groupe de transactions imbriquées nommées. *transaction_name* ne peut faire référence qu’au nom de la transaction la plus extérieure. Si une instruction ROLLBACK TRANSACTION *transaction_name* utilisant le nom de la transaction externe est exécutée à n’importe quel niveau d’un ensemble de transactions imbriquées, toutes les transactions imbriquées sont restaurées. Si une instruction `ROLLBACK WORK` ou `ROLLBACK TRANSACTION` sans un paramètre *transaction_name* est exécutée à n’importe quel niveau d’un ensemble de transactions imbriquées, toutes les transactions imbriquées, la plus extérieure comprise, sont restaurées.  
  
 La fonction `@@TRANCOUNT` enregistre le niveau d’imbrication de la transaction en cours. Chaque instruction `BEGIN TRANSACTION` incrémente `@@TRANCOUNT` d’une unité. Chaque instruction `COMMIT TRANSACTION` ou `COMMIT WORK` décrémente `@@TRANCOUNT` d’une unité. Une instruction `ROLLBACK WORK` ou `ROLLBACK TRANSACTION` pour laquelle aucun nom de transaction n’est spécifié restaure toutes les transactions imbriquées et remet `@@TRANCOUNT` à 0. Une instruction `ROLLBACK TRANSACTION` dans laquelle le nom de la transaction la plus extérieure d’un groupe de transactions imbriquées est spécifié restaure toutes les transactions imbriquées et remet `@@TRANCOUNT` à 0. Si vous ne savez pas si une transaction est déjà active, utilisez `SELECT @@TRANCOUNT` pour déterminer si sa valeur est supérieure ou égale à 1. Si la valeur de `@@TRANCOUNT` est égale à 0, vous n’êtes pas dans une transaction.  
  
### <a name="using-bound-sessions"></a>Utilisation de sessions associées  
 Les sessions associées facilitent la coordination des actions entre plusieurs sessions exécutées sur le même serveur. Les sessions associées permettent à plusieurs sessions de partager la même transaction et les mêmes verrous. Elles peuvent opérer sur les mêmes données sans conflits de verrous. Les sessions associées peuvent être créées à partir de plusieurs sessions de la même application ou à partir de sessions distinctes de plusieurs applications.  
  
 Pour participer à une session liée, une session doit appeler `sp_getbindtoken` ou `srv_getbindtoken` (via Open Data Services) pour obtenir un jeton de liaison. Un jeton d'association est une chaîne de caractères qui identifie de manière unique chaque transaction associée. Le jeton d'association est ensuite transmis aux autres sessions à associer à la session en cours. Les autres sessions se lient à la transaction en appelant **sp_bindsession** avec le jeton de liaison reçu de la première session.  
  
> [!NOTE]  
> Une session doit avoir une transaction utilisateur active pour que l’exécution de `sp_getbindtoken` ou `srv_getbindtoken` aboutisse.  
  
 Les jetons d'association doivent être transmis par le code de l'application qui crée la première session au code des applications qui lient leurs sessions à la première. Il n'existe aucune instruction [!INCLUDE[tsql](../includes/tsql-md.md)] ni fonction d'API qui permette à une application d'obtenir le jeton d'association pour une transaction démarrée par un autre processus. Les méthodes suivantes peuvent être utilisées pour transmettre un jeton d'association :  
  
-   Si toutes les sessions sont ouvertes à partir du même processus d'application, les jetons d'association peuvent être stockés en mémoire globale ou passés comme paramètres à des fonctions.  
  
-   Si les sessions sont créées à partir de processus d'application différents, les jetons d'association peuvent être transmis à l'aide d'un système de communication entre processus (IPC), comme l'appel de procédure distante (RPC) ou l'échange dynamique de données (DDE).  
  
-   Les jetons d'association peuvent être stockés dans une instance de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] dans une table qui peut être lue par les processus voulant s'associer à la première session.  
  
 Dans un ensemble de sessions associées, une seule session peut être active à la fois. Si une session exécute une instruction sur l'instance ou si elle attend des résultats de l'instance, aucune des autres sessions associées ne peut accéder à l'instance tant que la session active n'a pas terminé ou annulé l'exécution de l'instruction en cours. Si l'instance est occupée à traiter une instruction provenant d'une autre des sessions associée, un message d'erreur s'affiche pour indiquer que l'espace de transaction est utilisé et que la session doit renouveler la tentative ultérieurement.  
  
 Chacune des sessions associées conserve son niveau d'isolation propre. Si vous utilisez SET TRANSACTION ISOLATION LEVEL pour changer le niveau d'isolation d'une session, cela n'affecte pas celui des autres sessions associées.  
  
#### <a name="types-of-bound-sessions"></a>Types de sessions associées  
 Les sessions associées peuvent être locales ou distribuées.  
  
-   **Session liée locale**  
    Les sessions associées locales peuvent partager l'espace de transaction d'une transaction unique dans une seule instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
-   **Session liée distribuée**  
    Les sessions associées distribuées peuvent partager la même transaction entre plusieurs instances jusqu'à ce que la transaction complète soit validée ou annulée à l'aide de [!INCLUDE[msCoName](../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC).  
  
 Les sessions associées distribuées ne sont pas identifiées par la chaîne de caractères d'un jeton de liaison, mais par des numéros d'identification de transaction distribuée. Si une session liée est impliquée dans une transaction locale et exécute un appel de procédure distante (RPC) sur un serveur distant avec `SET REMOTE_PROC_TRANSACTIONS ON`, la transaction liée locale est automatiquement promue au rang de transaction liée distribuée par MS DTC et une session MS DTC est lancée.  
  
#### <a name="when-to-use-bound-sessions"></a>Utilisation des sessions associées  
 Dans les versions précédentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], les sessions associées étaient principalement utilisées pour développer des procédures stockées étendues devant exécuter des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] pour le processus qui les appelle. Le passage par le processus appelant d'un jeton d'association comme paramètre de la procédure stockée permet à celle-ci d'accéder à l'espace de transaction du processus appelant, et d'intégrer ainsi la procédure stockée étendue à ce dernier.  
  
 Dans le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], les procédures stockées écrites à l'aide de CLR sont supérieures aux procédures stockées étendues en termes de sécurité, d'évolutivité et de stabilité. Les procédures stockées CLR utilisent l’objet **SqlContext** et non `sp_bindsession` pour joindre le contexte de la session appelante.  
  
 Les sessions associées peuvent être utilisées pour développer des applications à trois niveaux où la logique de gestion est incluse dans des programmes distincts qui peuvent travailler en coopération sur une seule transaction commerciale. Ces programmes doivent être codés de manière à coordonner soigneusement leur accès à une base de données. Comme les deux sessions partagent les mêmes verrous, les deux programmes ne doivent pas essayer de modifier simultanément les mêmes données. A tout moment, une seule session peut participer à la transaction ; aucune exécution en parallèle n'est possible. La transaction ne peut passer d'une session à l'autre que lors de points d'interruption bien définis, notamment lorsque l'exécution de toutes les instructions DML est terminée et que les résultats correspondants ont été extraits.  
  
### <a name="coding-efficient-transactions"></a>Écriture de transactions performantes  
 Il est important de réduire la durée des transactions au minimum. Au démarrage d'une transaction, le SGBD, autrement dit le système de gestion de base de données, doit utiliser de nombreuses ressources pour toute la durée de la transaction afin de préserver les propriétés ACID (atomicité, cohérence, isolement et durabilité) de la transaction. En cas de modification des données, les lignes modifiées doivent être protégées par des verrous exclusifs qui empêchent les autres transactions de lire ces lignes, et ces verrous doivent être maintenus jusqu'à ce que la transaction soit validée ou restaurée. En fonction des paramètres de niveau d’isolation de la transaction, les instructions `SELECT` peuvent acquérir des verrous qui doivent être maintenus jusqu’à la restauration ou la validation de la transaction. Dans le cas de systèmes comprenant de nombreux utilisateurs, les transactions doivent être aussi courtes que possible afin de limiter la contention des ressources par les verrous pour des connexions concurrentes. Des transactions longues et peu performantes peuvent ne pas poser de problème pour un nombre réduit d'utilisateurs, mais elles sont inacceptables dans le cas d'un système comprenant plusieurs milliers d'utilisateurs. À partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge les transactions durables retardées. Les transactions durables retardées ne garantissent pas la durabilité. Consultez la rubrique [Durabilité des transactions](../relational-databases/logs/control-transaction-durability.md) pour plus d’informations.  
  
#### <a name="coding-guidelines"></a><a name="guidelines"></a> Directives de codage  
 Vous trouverez ci-dessous des directives vous permettant de coder des transactions performantes :  
  
-   Évitez l'entrée de données par l'utilisateur au cours d'une transaction.  
    Effectuez toutes les entrées de données par l'utilisateur avant le début d'une transaction. Si d'autres données doivent être entrées par l'utilisateur au cours d'une transaction, restaurez la transaction en cours et redémarrez-la après l'entrée des données. Même si les utilisateurs répondent immédiatement, le temps de réaction d'un être humain est incomparablement plus lent que la vitesse d'un ordinateur. Toutes les ressources utilisées par la transaction sont verrouillées pendant un temps extrêmement long, ce qui peut entraîner des problèmes de blocage du système. Si les utilisateurs ne répondent pas, la transaction reste active et verrouille les ressources critiques jusqu'à ce qu'ils répondent, ce qui peut prendre plusieurs minutes, voire des heures.  
  
-   Si possible, n'ouvrez pas une transaction alors que vous êtes en train de consulter des données.  
    Ne démarrez pas de transaction avant que l'analyse préliminaire des données soit terminée.  
  
-   Limitez la durée de la transaction autant que possible.  
    Lorsque vous connaissez les modifications effectuées, démarrez une transaction, exécutez les instructions de modification, puis validez-les ou restaurez-les immédiatement. N'ouvrez pas la transaction tant que ce n'est pas nécessaire.  
  
-   Pour réduire les blocages, envisagez d'utiliser un niveau d'isolement basé sur le contrôle de version de ligne pour les requêtes en lecture seule.  
  
-   Utilisez avec discernement les niveaux d'isolement de transaction inférieurs.  
    De nombreuses applications peuvent être facilement codées pour utiliser le niveau d'isolement de lecture validée. Toutes les transactions ne nécessitent pas l'utilisation d'un niveau d'isolement de transaction sérialisable.  
  
-   Utilisez les options de concurrence d'accès des curseurs les plus faibles, telles que les options de concurrence d'accès optimistes.  
    Dans un système pour lequel la probabilité de modifications concurrentes est faible, le temps supplémentaire nécessaire pour traiter une erreur causée par la modification de vos données par un autre utilisateur après que vous les ayez lues peut être inférieur à celui qu'entraîne le verrouillage systématique des lignes au moment de leur lecture.  
  
-   Limitez autant que possible le volume de données auxquelles accède votre transaction.  
    Le nombre de lignes verrouillées est ainsi limité, ce qui limite également la contention des transactions.  
    
-   Évitez autant que possible les indicateurs de verrouillage pessimiste tels que HOLDLOCK. 
    Des indicateurs tels que HOLDLOCK ou le niveau d’isolation SERIALIZABLE peuvent entraîner l’attente des processus même sur des verrous partagés et réduire la concurrence.

-   Évitez autant que possible d’utiliser des transactions implicites. Ces dernières peuvent introduire un comportement imprévisible en raison de leur nature. Consultez [Transactions implicites et problèmes de concurrence](#implicit-transactions-and-avoiding-concurrency-and-resource-problems).

-   Concevez des index avec un [facteur de remplissage](indexes/specify-fill-factor-for-an-index.md) réduit. La réduction du facteur de remplissage peut vous aider à empêcher ou à réduire la fragmentation des pages d’index et donc à réduire les temps de recherche d’index, en particulier lors de leur récupération sur le disque. Pour afficher les informations de fragmentation des données et des index de la table ou vue spécifiée, vous pouvez utiliser sys.dm_db_index_physical_stats. 
  
#### <a name="implicit-transactions-and-avoiding-concurrency-and-resource-problems"></a>Transactions implicites et prévention des problèmes de concurrence et de ressources  
 Pour prévenir les problèmes de concurrence et de ressources, soyez minutieux dans la gestion des transactions implicites. Dans les transactions implicites, l’instruction [!INCLUDE[tsql](../includes/tsql-md.md)] qui suit une instruction `COMMIT` ou `ROLLBACK` démarre automatiquement une nouvelle transaction. Une nouvelle transaction risque ainsi d'être ouverte alors que l'application consulte des données, ou qu'elle attend une entrée de données par l'utilisateur. Après avoir terminé la dernière transaction nécessaire à la protection des modifications, désactivez les transactions implicites jusqu'à ce qu'une transaction doive à nouveau protéger les modifications de données. Cette procédure permet à [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] d'utiliser le mode autocommit lorsque l'application consulte des données ou attend une entrée de données par l'utilisateur.  
  
 De plus, quand le niveau d’isolation d’instantané est activé, même si une nouvelle transaction ne contient pas de verrous, une exécution longue empêche la suppression des anciennes versions dans `tempdb`.  
  
### <a name="managing-long-running-transactions"></a>Gestion des transactions de longue durée  
 Une *transaction longue* est une transaction active qui n’a pas été validée ou restaurée à temps. Par exemple, si le début et la fin d'une transaction sont contrôlés par l'utilisateur, une raison classique de l'existence d'une transaction de longue durée est qu'un utilisateur a commencé une transaction puis est parti alors que la transaction attend une réponse de l'utilisateur.  
  
 Une transaction longue peut entraîner de graves problèmes pour une base de données, comme suit :  
  
-   Si une instance de serveur est arrêtée après qu’une transaction active a effectué de nombreuses modifications non validées, la phase de récupération du redémarrage suivant peut prendre plus de temps que le temps indiqué par l’option de configuration du serveur **recovery interval** ou par l’option `ALTER DATABASE ... SET TARGET_RECOVERY_TIME`. Ces options contrôlent la fréquence des points de contrôle actifs et indirects, respectivement. Pour plus d’informations sur les types de points de contrôle, consultez [Points de contrôle de base de données &#40;SQL Server&#41;](../relational-databases/logs/database-checkpoints-sql-server.md).  
  
-   Plus important, bien qu'une transaction en attente génère très peu d'entrées de journal, elle empêche la troncation du journal et entraîne ainsi sa croissance et son remplissage. Si le journal des transactions est rempli, la base de données ne peut plus effectuer de mises à jour. Pour plus d’informations, consultez [Guide d’architecture et gestion du journal des transactions SQL Server](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md), [Résoudre les problèmes liés à un journal des transactions saturé &#40;erreur SQL Server 9002&#41;](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md) et [Journal des transactions &#40;SQL Server&#41;](../relational-databases/logs/the-transaction-log-sql-server.md).  
  
#### <a name="discovering-long-running-transactions"></a>Découverte des transactions de longue durée  
 Pour rechercher des transactions de longue durée, appliquez une des procédures suivantes :  
  
-   **sys.dm_tran_database_transactions**  
  
    Cet affichage de gestion dynamique retourne des informations sur les transactions au niveau de la base de données. Pour une transaction longue, les colonnes particulièrement intéressantes incluent l’heure du premier enregistrement de journal (**database_transaction_begin_time**), l’état actuel de la transaction (**database_transaction_state**) et le numéro séquentiel dans le journal de l’enregistrement initial dans le journal des transactions (**database_transaction_begin_lsn**).  
  
    Pour plus d’informations, consultez [sys.dm_tran_database_transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md).  
  
-   `DBCC OPENTRAN`  
  
    Cette instruction vous permet d'identifier l'ID du propriétaire de la transaction et éventuellement de retrouver la source de la transaction pour y mettre fin dans les règles de l'art (la valider au lieu de la restaurer). Pour plus d’informations, consultez [DBCC OPENTRAN &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-opentran-transact-sql.md).  
  
#### <a name="stopping-a-transaction"></a>Arrêt d'une transaction  
 Vous devrez peut-être utiliser l'instruction KILL. Utilisez cette instruction avec précaution, particulièrement lorsque des processus critiques sont en cours d'exécution. Pour plus d’informations, consultez [KILL &#40;Transact-SQL&#41;](../t-sql/language-elements/kill-transact-sql.md).  
  
##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Lecture supplémentaire   
[Charge du contrôle de version de ligne](/archive/blogs/sqlserverstorageengine/overhead-of-row-versioning)   
[Événements étendus](../relational-databases/extended-events/extended-events.md)   
[sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)     
[Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)      
[Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)