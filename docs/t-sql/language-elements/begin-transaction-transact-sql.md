---
description: BEGIN TRANSACTION (Transact-SQL)
title: BEGIN TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BEGIN_TRANSACTION_TSQL
- TRANSACTION_TSQL
- TRANSACTION
- BEGIN TRANSACTION
- BEGIN TRAN
- BEGIN_TRAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction logs [SQL Server], BEGIN TRANSACTION statement
- marked transactions [SQL Server], BEGIN TRANSACTION statement
- BEGIN TRANSACTION statement
- local transactions [SQL Server]
- marking transactions [SQL Server]
- transactions [SQL Server], starting
- transaction names [SQL Server]
- starting point marked for transactions
- starting transactions
ms.assetid: c6258df4-11f1-416a-816b-54f98c11145e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bad9134c5fbdfd3908491a510c6bedafbb93d977
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183342"
---
# <a name="begin-transaction-transact-sql"></a>BEGIN TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indique le début d'une transaction locale explicite. Les transactions explicites commencent avec l’instruction BEGIN TRANSACTION et se terminent avec l’instruction COMMIT ou ROLLBACK.  

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
--Applies to SQL Server and Azure SQL Database
 
BEGIN { TRAN | TRANSACTION }   
    [ { transaction_name | @tran_name_variable }  
      [ WITH MARK [ 'description' ] ]  
    ]  
[ ; ]  
```  
 
```syntaxsql
--Applies to Azure Synapse Analytics and Parallel Data Warehouse
 
BEGIN { TRAN | TRANSACTION }   
[ ; ]  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *transaction_name*  
 **S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database
 
 Nom attribué à la transaction. *transaction_name* doit respecter les règles applicables aux identificateurs, mais les identificateurs de plus de 32 caractères ne sont pas autorisés. Utilisez les noms de transaction seulement sur la paire la plus extérieure des instructions BEGIN...COMMIT ou BEGIN...ROLLBACK imbriquées. *transaction_name* respecte toujours la casse, même si l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’en tient pas compte.  
  
 @*tran_name_variable*  
 **S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database
 
 Nom d'une variable définie par l'utilisateur et contenant un nom de transaction valide. La variable doit être déclarée avec un type de données **char**, **varchar**, **nchar** ou **nvarchar**. Si plus de 32 caractères sont passés à la variable, seuls les 32 premiers caractères seront utilisés et les autres seront tronqués.  
  
 WITH MARK [ '*description*' ]  
**S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database

Indique que la transaction est marquée dans le journal. *description* est une chaîne qui décrit la marque. Une *description* de plus de 128 caractères est tronquée à 128 caractères avant d’être stockée dans la table msdb.dbo.logmarkhistory.  
  
 Si WITH MARK est utilisé, un nom de transaction doit être spécifié. WITH MARK permet de restaurer un journal de transactions par rapport à une marque nommée.  
  
## <a name="general-remarks"></a>Remarques d'ordre général
BEGIN TRANSACTION incrémente @@TRANCOUNT de 1.
  
BEGIN TRANSACTION représente le point auquel les données référencées par une connexion sont logiquement et physiquement cohérentes. Si des erreurs sont détectées, toutes les modifications apportées aux données après l'exécution de l'instruction BEGIN TRANSACTION peuvent être annulées afin que les données reviennent à cet état connu de cohérence. Une transaction se poursuit jusqu'à ce qu'elle se termine sans erreur et que l'instruction COMMIT TRANSACTION soit émise pour que les modifications soient intégrées de façon permanente à la base de données ou bien jusqu'à ce que des erreurs soient rencontrées, auquel cas les modifications sont effacées à l'aide de l'instruction ROLLBACK TRANSACTION.  
  
L'instruction BEGIN TRANSACTION démarre une transaction locale pour la connexion qui a émis cette instruction. Selon les paramètres de niveau d'isolement définis pour la transaction en cours, de nombreuses ressources acquises pour la prise en charge des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] émises par la connexion sont verrouillées par la transaction jusqu'à ce que celle-ci se termine soit par un COMMIT TRANSACTION, soit par un ROLLBACK TRANSACTION. Les transactions suspendues pendant de longues périodes de temps peuvent empêcher les autres utilisateurs d'accéder à ces ressources verrouillées, et elles peuvent également empêcher la troncature du fichier journal.  
  
 Bien que BEGIN TRANSACTION démarre une transaction locale, celle-ci n'est pas enregistrée dans le journal des transactions avant que l'application n'effectue une action qui doit y être consignée, telle que l'exécution d'une instruction INSERT, UPDATE ou DELETE. Une application peut effectuer des actions telles que l'acquisition de verrous afin de protéger le niveau d'isolement d'une transaction des instructions SELECT, mais rien n'est enregistré dans le journal avant que l'application n'effectue une action de modification.  
  
 L'attribution d'un nom à plusieurs transactions d'une série de transactions imbriquées a peu d'effet sur la transaction. En effet, seul le premier nom de transaction (le plus à l'extérieur) est inscrit avec le système. Une annulation vers tout autre nom (autre que celui d'un point de sécurité valide) génère une erreur. Dans ce cas, aucune des instructions exécutées avant l'annulation n'est en fait annulée au moment où l'erreur se produit. Les instructions ne sont annulées que lorsque l'instruction extérieure est elle-même annulée.  
  
 La transaction locale lancée par l'instruction BEGIN TRANSACTION est promue au rang de transaction distribuée si les actions suivantes sont effectuées avant que cette transaction soit validée ou annulée :  
  
-   Exécution d'une instruction INSERT, DELETE ou UPDATE faisant référence à une table distante ou à un serveur lié. L’instruction INSERT, UDPATE ou DELETE échoue si le fournisseur OLE DB utilisé pour l’accès au serveur lié ne prend pas en charge l’interface ITransactionJoin.  
  
-   Un appel à une procédure stockée distante est effectué quand l'option REMOTE_PROC_TRANSACTIONS a la valeur ON.  
  
 La copie locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devient le contrôleur de transaction et utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)] DTC pour gérer la transaction distribuée.  
  
 Vous pouvez exécuter explicitement une transaction en tant que transaction distribuée en utilisant BEGIN DISTRIBUTED TRANSACTION. Pour plus d’informations, consultez [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
 Si SET IMPLICIT_TRANSACTIONS a la valeur ON, une instruction BEGIN TRANSACTION crée deux transactions imbriquées. Pour plus d’informations, consultez [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md)  
  
## <a name="marked-transactions"></a>Transactions marquées  
 L'option WITH MARK permet de placer le nom de la transaction dans le journal des transactions. Lors de la restauration d'une base de données à un état antérieur, la transaction marquée peut être utilisée à la place d'une date et d'une heure. Pour plus d’informations, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md) et [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 En outre, les marques du journal des transactions sont nécessaires pour récupérer un ensemble de bases de données connexes dans un état cohérent d'un point de vue logique. Une transaction distribuée permet de placer des marques dans les journaux de transactions des bases de données connexes. La récupération de l'ensemble de bases de données connexes jusqu'à ces marques aboutit à un ensemble de bases de données cohérent d'un point de vue transactionnel. Le placement de marques dans des bases de données connexes implique des procédures particulières.  
  
 La marque n'est placée dans le journal des transactions que si la base de données est mise à jour par la transaction marquée. Les transactions qui ne modifient pas de données ne sont pas marquées.  
  
 BEGIN TRAN *new_name* WITH MARK peut être imbriquée dans une transaction existante non marquée. *new_name* devient alors le nom de marque de la transaction, malgré le nom éventuellement déjà affecté à cette transaction. Dans l'exemple suivant, `M2` est le nom de la marque.  
  
```sql  
BEGIN TRAN T1;  
UPDATE table1 ...;  
BEGIN TRAN M2 WITH MARK;  
UPDATE table2 ...;  
SELECT * from table1;  
COMMIT TRAN M2;  
UPDATE table3 ...;  
COMMIT TRAN T1;  
```  
  
 Lors de l'imbrication de transactions, si vous essayez de marquer une transaction déjà marquée, vous obtenez un message d'avertissement (et non pas d'erreur) :  
  
 « BEGIN TRAN T1 WITH MARK ...; »  
  
 « UPDATE table1 ...; »  
  
 « BEGIN TRAN M2 WITH MARK ...; »  
  
 « Serveur : Msg 3920, Niveau 16, État 1, Ligne 3 »  
  
 « L'option WITH MARK s'applique uniquement à la première instruction BEGIN TRAN WITH MARK. »  
  
 « L'option est ignorée. »  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-an-explicit-transaction"></a>R. Utilisation d’une transaction explicite
**S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], Parallel Data Warehouse

Cet exemple utilise AdventureWorks. 

```sql
BEGIN TRANSACTION;  
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT;  
```

### <a name="b-rolling-back-a-transaction"></a>B. Restauration d’une transaction
**S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], Parallel Data Warehouse

L’exemple suivant montre l’effet de la restauration d’une transaction. Dans cet exemple, l’instruction ROLLBACK restaure l’instruction INSERT, mais la table créée continue d’exister.

```sql
CREATE TABLE ValueTable (id INT);  
BEGIN TRANSACTION;  
       INSERT INTO ValueTable VALUES(1);  
       INSERT INTO ValueTable VALUES(2);  
ROLLBACK;  

```

### <a name="c-naming-a-transaction"></a>C. Attribution d'un nom à une transaction 
**S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database

L'exemple suivant montre comment nommer une transaction.  
  
```sql
DECLARE @TranName VARCHAR(20);  
SELECT @TranName = 'MyTransaction';  
  
BEGIN TRANSACTION @TranName;  
USE AdventureWorks2012;  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
  
COMMIT TRANSACTION @TranName;  
GO  
```  
  
### <a name="d-marking-a-transaction"></a>D. Marquage d'une transaction  
**S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database

L'exemple suivant montre comment marquer une transaction. La transaction `CandidateDelete` est marquée.  
  
```sql  
BEGIN TRANSACTION CandidateDelete  
    WITH MARK N'Deleting a Job Candidate';  
GO  
USE AdventureWorks2012;  
GO  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
GO  
COMMIT TRANSACTION CandidateDelete;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
