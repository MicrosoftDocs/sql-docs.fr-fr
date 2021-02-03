---
description: SET TRANSACTION ISOLATION LEVEL (Transact-SQL)
title: SET TRANSACTION ISOLATION LEVEL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEVEL
- LEVEL_TSQL
- SET TRANSACTION ISOLATION LEVEL
- ISOLATION
- ISOLATION_TSQL
- SET_TRANSACTION_ISOLATION_LEVEL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET TRANSACTION ISOLATION LEVEL statement
- row versioning [SQL Server], isolation levels
- TRANSACTION ISOLATION LEVEL option
- isolation levels [SQL Server], setting
- locking [SQL Server], isolation levels
- transactions [SQL Server], isolation levels
ms.assetid: 016fb05e-a702-484b-bd2a-a6eabd0d76fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7fd0098e9b4e213619e65973c2367a03f0197852
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161099"
---
# <a name="set-transaction-isolation-level-transact-sql"></a>SET TRANSACTION ISOLATION LEVEL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Contrôle le verrouillage et le comportement de contrôle de version de ligne des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] émises par une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
SET TRANSACTION ISOLATION LEVEL
    { READ UNCOMMITTED
    | READ COMMITTED
    | REPEATABLE READ
    | SNAPSHOT
    | SERIALIZABLE
    }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse
  
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
```

>[!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] implémente les transactions ACID. Par défaut, le niveau d'isolation de la prise en charge transactionnelle est READ UNCOMMITTED.  Vous pouvez le remplacer par READ COMMITTED SNAPSHOT ISOLATION en activant l'option de base de données READ_COMMITTED_SNAPSHOT pour une base de données utilisateur lorsqu'elle est connectée à la base de données MASTER.  Une fois activée, toutes les transactions de cette base de données sont exécutées sous READ COMMITTED SNAPSHOT ISOLATION et la définition de READ UNCOMMITTED au niveau de la session n’est pas honorée. Pour plus d'informations, consultez [Options ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 READ UNCOMMITTED  
 Spécifie que les instructions peuvent lire des lignes qui ont été modifiées par d'autres transactions, mais pas encore validées.  
  
 Les transactions qui s’exécutent au niveau READ UNCOMMITTED ne génèrent pas de verrous partagés pour empêcher d’autres transactions de modifier des données lues par la transaction en cours. Par ailleurs, les transactions READ UNCOMMITTED ne sont pas bloquées par des verrous exclusifs qui empêcheraient la transaction active de lire des lignes modifiées, mais non validées, par d'autres transactions. Lorsque cette option est définie, vous avez la possibilité de lire des modifications non validées, appelées lectures incorrectes. Les valeurs peuvent changer dans les données et des lignes peuvent apparaître ou disparaître dans le dataset avant la fin de la transaction. Cette option a le même effet que l'activation de l'option NOLOCK dans toutes les tables de toutes les instructions SELECT d'une transaction. Il s'agit du niveau d'isolement le moins restrictif.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez également limiter les contentions de verrouillage tout en protégeant les transactions de lectures erronées de modifications de données non validées en utilisant :  
  
-   Le niveau d’isolation READ COMMITTED avec l’option de base de données READ_COMMITTED_SNAPSHOT activée (ON)  
  
-   le niveau d'isolement SNAPSHOT. Pour plus d’informations sur l’isolation des instantanés, consultez [Isolation d’instantanés dans SQL Server](/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server). 
  
 READ COMMITTED  
 Spécifie que les instructions ne peuvent pas lire les données modifiées mais non validées par d'autres transactions. Cela permet d'éviter les lectures incorrectes. Les données peuvent être modifiées par d'autres transactions entre deux instructions au sein de la transaction active, ce qui aboutit à des lectures non renouvelables ou à des données fantômes. Il s'agit de l'option par défaut dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le comportement de READ COMMITTED dépend de la valeur affectée à l'option de base de données READ_COMMITTED_SNAPSHOT :  
  
-   Si l’option READ_COMMITTED_SNAPSHOT a la valeur OFF (valeur par défaut sur SQL Server), le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise des verrous partagés pour empêcher d’autres transactions de modifier des lignes pendant que la transaction active exécute une opération de lecture. Les verrous partagés empêchent également l'instruction de lire des lignes modifiées par d'autres transactions, tant que celles-ci ne sont pas terminées. Le type du verrou partagé détermine quand il sera levé. Les verrous de ligne sont levés avant que la ligne suivante ne soit traitée. Les verrous de page sont levés quand la page suivante est lue et les verrous de table sont levés quand l’exécution de l’instruction se termine.  
  
-   Si READ_COMMITTED_SNAPSHOT a la valeur ON (valeur par défaut sur Azure SQL Database), le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise la gestion de versions de ligne pour présenter à chaque instruction un instantané cohérent des données (du point de vue transactionnel) telles qu’elles étaient au début de l’instruction. Les verrous ne sont pas utilisés pour protéger les données des mises à jour par d'autres transactions.

> [!IMPORTANT]  
> Le choix d'un niveau d'isolation n'a aucune influence sur les verrous acquis pour protéger les modifications de données. Une transaction acquiert toujours un verrou exclusif sur les données qu'elle modifie et garde celui-ci jusqu'à ce qu'elle ait terminé son travail, indépendamment du niveau d'isolation défini pour elle. De plus, une mise à jour effectuée au niveau d’isolation READ_COMMITTED utilise des verrous de mise à jour sur les lignes de données sélectionnées, tandis qu’une mise à jour apportée au niveau d’isolation SNAPSHOT utilise des versions de ligne pour sélectionner les lignes à mettre à jour. Dans le cas des opérations de lecture, le niveau d'isolation d'une transaction définit principalement son niveau de protection contre les effets des modifications apportées par les autres transactions. Pour plus d’informations, consultez [Guide du verrouillage des transactions et de la gestion de versions de ligne](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md).

> [!NOTE]  
>  L'isolement d'instantané prend en charge les données FILESTREAM. En mode d'isolement d'instantané, les données FILESTREAM lues par n'importe quelle instruction d'une transaction représenteront la version cohérente d'un point de vue transactionnel des données qui existaient au début de la transaction.  
  
 Lorsque l'option de base de données READ_COMMITTED_SNAPSHOT a la valeur ON, vous pouvez utiliser l'indicateur de table READCOMMITTEDLOCK pour demander le verrouillage partagé au lieu du contrôle de version de ligne pour chacune des instructions individuelles des transactions qui s'exécutent au niveau d'isolation READ COMMITTED.  
  
> [!NOTE]  
>  Lorsque vous définissez l'option READ_COMMITTED_SNAPSHOT, seule la connexion exécutant la commande ALTER DATABASE est autorisée dans la base de données. La base de données ne peut contenir aucune autre connexion ouverte avant la fin de l'exécution de la commande ALTER DATABASE. Il n'est pas nécessaire que la base de données soit en mode mono-utilisateur.  
  
 REPEATABLE READ  
 Spécifie que les instructions ne peuvent pas lire des données qui ont été modifiées mais pas encore validées par d'autres transactions, et qu'aucune autre transaction ne peut modifier les données lues par la transaction active tant que celle-ci n'est pas terminée.  
  
 Des verrous partagés sont placés sur toutes les données lues par chaque instruction de la transaction et maintenus jusqu'à la fin de la transaction. Cela évite que d'autres transactions modifient des lignes qui ont été lues par la transaction active. D'autres transactions peuvent insérer de nouvelles lignes lorsque celles-ci correspondent aux conditions de recherche des instructions émises par la transaction active. Si par la suite la transaction active réexécute l'instruction, elle récupère les nouvelles lignes, ce qui aboutit à des lectures fantômes. Comme les verrous partagés sont maintenus jusqu'à la fin d'une transaction au lieu d'être débloqués à la fin de chaque instruction, l'accès concurrentiel est moindre qu'avec le niveau d'isolation READ COMMITTED par défaut. Utilisez cette option uniquement si c'est nécessaire.  
  
 SNAPSHOT  
 Spécifie que les données lues par n’importe quelle instruction d’une transaction représentent la version cohérente d’un point de vue transactionnel des données qui existaient au début de la transaction. La transaction peut seulement reconnaître les modifications de données qui ont été validées avant qu'elle ne commence. Autrement dit, les modifications de données effectuées par d'autres transactions après le début de la transaction active ne sont pas visibles pour les instructions qui s'exécutent dans le cadre de ladite transaction. C’est comme si les instructions d’une transaction obtenaient un instantané des données validées telles qu’elles existaient au début de la transaction.  
  
 Sauf lors de la récupération d'une base de données, les transactions SNAPSHOT ne demandent pas de verrouillage lors de la lecture des données. Les transactions SNAPSHOT qui lisent des données n'empêchent pas d'autres transactions d'écrire des données. De même, les transactions qui écrivent des données n'empêchent pas des transactions SNAPSHOT de lire des données.  
  
 Au cours de la phase de restauration de la récupération d'une base de données, les transactions SNAPSHOT demandent un verrou en cas de tentative de lecture de données verrouillées par une autre transaction en cours de restauration. La transaction SNAPSHOT est bloquée jusqu'à la restauration de l'autre transaction. Le verrouillage est levé dès qu'il a été accordé.  
  
 L'option de base de données ALLOW_SNAPSHOT_ISOLATION doit être activée (ON) pour que vous puissiez démarrer une transaction utilisant le niveau d'isolation SNAPSHOT. Si une transaction qui utilise le niveau d'isolation SNAPSHOT accède à des données figurant dans plusieurs bases de données, l'option ALLOW_SNAPSHOT_ISOLATION doit avoir la valeur ON dans chacune de ces bases de données.  
  
 Une transaction ne peut pas être définie avec le niveau d'isolation SNAPSHOT si elle a commencé avec un autre niveau d'isolation ; cela la ferait échouer. Si une transaction commence au niveau d'isolation SNAPSHOT, vous pouvez la faire passer à un autre niveau d'isolation puis revenir au niveau SNAPSHOT. Une transaction démarre la première fois qu'elle accède aux données.  
  
 Une transaction exécutée avec le niveau d'isolation SNAPSHOT peut voir les modifications qui ont été effectuées par cette transaction. Par exemple, si la transaction effectue une opération UPDATE sur une table puis exécute une instruction SELECT sur cette même table, les données modifiées figureront dans le jeu de résultats.  
  
> [!NOTE]  
>  En mode d'isolement d'instantané, les données FILESTREAM lues par n'importe quelle instruction d'une transaction représenteront la version cohérente d'un point de vue transactionnel des données qui existaient au début de la transaction, et non au début de l'instruction.  
  
 SERIALIZABLE  
 Spécifie les indications suivantes :  
  
-   Les instructions ne peuvent pas lire des données qui ont été modifiées mais pas encore validées par d'autres transactions.  
  
-   Aucune autre transaction ne peut modifier des données qui ont été lues par la transaction active tant que celle-ci n'est pas terminée.  
  
-   Les autres transactions ne peuvent pas insérer de nouvelles lignes avec des valeurs de clés comprises dans le groupe de clés lues par des instructions de la transaction active, tant que celle-ci n'est pas terminée.  
  
 Des verrous de groupes sont placés dans les groupes de valeurs de clés qui correspondent aux conditions de recherche de chaque instruction exécutée dans une transaction. Cela empêche les autres transactions de mettre à jour ou d'insérer des lignes qui pourraient intervenir dans les instructions exécutées par la transaction active. Autrement dit, si l'une ou l'autre des instructions d'une transaction est exécutée une seconde fois, elle lira le même groupe de lignes. Les verrous de groupes sont conservés jusqu'au terme de la transaction. C'est le plus restrictif des niveaux d'isolation, parce qu'il verrouille des groupes de clés entiers et laisse les verrous en place jusqu'à la fin de la transaction. Comme l'accès concurrentiel est plus limité, utilisez cette option uniquement lorsque cela s'avère nécessaire. Cette option a le même effet que l'utilisation de l'option HOLDLOCK dans toutes les tables de toutes les instructions SELECT d'une transaction.  
  
## <a name="remarks"></a>Notes  
 Une seule des options de niveau d'isolation peut être définie à la fois, et elle reste en vigueur durant cette connexion tant qu'elle n'est pas explicitement modifiée. Toutes les opérations de lecture effectuées au sein de la transaction obéissent aux règles du niveau d'isolation spécifié, sauf si un indicateur de table de la clause FROM d'une instruction spécifie un autre verrouillage ou un autre comportement de contrôle de version pour une table.  
  
 Les niveaux d'isolation des transactions définissent le type de verrous placés sur les opérations de lecture. Les verrous partagés placés pour READ COMMITTED ou REPEATABLE READ sont généralement des verrous de ligne, bien que ceux-ci puissent être promus au rang de verrous de page ou de table si la lecture fait référence à un nombre important de lignes d'une page ou d'une table. Si une ligne est modifiée par la transaction après avoir été lue, la transaction acquiert un verrou exclusif pour protéger cette ligne et ce verrou est maintenu jusqu'à la fin de la transaction. Par exemple, si une transaction REPEATABLE READ utilise un verrou partagé sur une ligne et qu'elle modifie ensuite cette ligne, le verrou partagé est converti en verrou exclusif de ligne.  
  
 À une exception près, vous pouvez changer de niveau d'isolation à tout moment au cours d'une transaction. L'exception intervient lorsque vous passez d'un niveau d'isolation (quel qu'il soit) au niveau d'isolation SNAPSHOT. Ce changement de niveau provoque l'échec et l'annulation de la transaction. Cependant, vous pouvez faire passer une transaction démarrée au niveau d'isolation SNAPSHOT à un autre niveau d'isolation.  
  
 Dans ce cas, les ressources lues après cette modification sont protégées conformément aux règles du nouveau niveau. Les ressources lues avant la modification continuent d'être protégées selon les règles du niveau précédent. Par exemple, si une transaction est passée du niveau READ COMMITTED au niveau SERIALIZABLE, les verrous partagés acquis après le changement sont alors maintenus jusqu'à la fin de la transaction.  
  
 Si vous émettez SET TRANSACTION ISOLATION LEVEL dans une procédure stockée ou un déclencheur, lorsque l'objet rend le contrôle, le niveau d'isolation est rétabli au niveau qui était en vigueur lors de l'appel de cet objet. Par exemple, si vous définissez REPEATABLE READ dans un traitement et que ce traitement appelle ensuite une procédure stockée qui affecte la valeur SERIALIZABLE au niveau d'isolation, vous revenez au niveau d'isolation REPEATABLE READ lorsque la procédure stockée rend le contrôle au traitement.  
  
> [!NOTE]  
>  Les fonctions définies par l'utilisateur et les types CLR (Common Language Runtime) définis par l'utilisateur ne peuvent pas exécuter SET TRANSACTION ISOLATION LEVEL. Cependant, vous pouvez remplacer le niveau d'isolation en utilisant un indicateur de table. Pour plus d’informations, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Lorsque vous utilisez sp_bindsession pour lier deux sessions, chacune d'elles conserve son niveau d'isolation. L'utilisation de SET TRANSACTION ISOLATION LEVEL pour redéfinir le niveau d'isolation d'une session n'affecte pas celui des autres sessions liées.  
  
 SET TRANSACTION ISOLATION LEVEL prend effet au moment de l'exécution, et non durant l'analyse.  
  
 Les opérations de chargement en masse optimisées portant sur des segments verrouillent les requêtes qui s'exécutent sous les niveaux d'isolation suivants :  
  
-   SNAPSHOT  
  
-   READ UNCOMMITTED  
  
-   READ COMMITTED avec le contrôle de version de ligne  
  
 Inversement, les requêtes exécutées sous ces niveaux d'isolation bloquent les opérations de chargement en masse optimisées portant sur des segments. Pour plus d’informations sur les opérations de chargement en bloc, consultez [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
 Les bases de données compatibles FILESTREAM prennent en charge les niveaux d'isolement des transactions suivants.  
  
|Niveau d'isolation|Accès Transact SQL|Accès au système de fichiers|  
|---------------------|-------------------------|------------------------|  
|Lecture non validée|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|Non pris en charge|  
|Lecture validée|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|Lecture renouvelable|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|Non pris en charge|  
|Sérialisable|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|Non pris en charge|  
|Capture instantanée Read Committed|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|Instantané|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit le paramètre `TRANSACTION ISOLATION LEVEL` pour la session. Pour chaque instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] maintient tous les verrous partagés jusqu'à la fin de la transaction.  
  
```sql
USE AdventureWorks2012;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT *   
    FROM HumanResources.EmployeePayHistory;  
GO  
SELECT *   
    FROM HumanResources.Department;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [DBCC USEROPTIONS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-useroptions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)  
  
