---
title: Transactions avec tables optimisées en mémoire | Microsoft Docs
description: Apprenez-en davantage sur les transactions pour les tables à mémoire optimisée et les procédures stockées compilées en mode natif, ainsi que leurs différences par rapport aux transactions pour les tables sur disque.
ms.custom: ''
ms.date: 01/16/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ba6f1a15-8b69-4ca6-9f44-f5e3f2962bc5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 04158e7fff01e16de25cfc48c552040e612bba06
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237572"
---
# <a name="transactions-with-memory-optimized-tables"></a>Transactions with Memory-Optimized Tables
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cet article décrit tous les aspects des transactions propres aux tables optimisées en mémoire et aux procédures stockées compilées en mode natif.  
  
Les niveaux d’isolation de la transaction dans SQL Server s’appliquent différemment aux tables optimisées en mémoire et aux tables sur disque. En outre, les mécanismes sous-jacents sont différents. La compréhension de ces différences permet au programmeur de concevoir un système à débit élevé. L’objectif de l’intégrité de la transaction est partagé dans tous les cas.  

Pour connaître les conditions d’erreur spécifiques aux transactions dans les tables optimisées en mémoire, passez à la section [Détection des conflits et logique des nouvelles tentatives](#conflict-detection-and-retry-logic).
  
Pour des informations générales, consultez [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
## <a name="pessimistic-versus-optimistic"></a>Approches pessimiste et optimiste  
  
Les différences fonctionnelles sont dues à la différence entre les approches pessimiste et optimiste pour l’intégrité de la transaction. Les tables optimisées en mémoire utilisent l’approche optimiste :  
  
- L’approche pessimiste utilise des verrous pour bloquer les conflits potentiels avant qu’ils surviennent. Les verrous sont appliqués lorsque l’instruction est exécutée et ouverts lorsque la transaction est validée.  
  
- L’approche optimiste détecte les conflits à mesure qu’ils se produisent et effectue les vérifications au moment de la validation.  
  - Erreur 1205, un interblocage ne peut pas se produire pour une table optimisée en mémoire.  
  
L’approche optimiste est moins lourde et souvent plus efficace, en partie parce que les conflits de transaction sont rares dans la plupart des applications. La principale différence d’ordre fonctionnel entre ces deux approches se situe au niveau des conflits. Avec l’approche pessimiste, vous attendez, alors qu’avec l’approche optimiste, l’une des transactions échoue et doit être retentée par le client. Les différences fonctionnelles sont plus grandes lorsque le niveau d’isolation REPEATABLE READ est appliqué et sont encore plus importantes pour le niveau SERIALIZABLE.  
  
## <a name="transaction-initiation-modes"></a>Modes d’initiation de la transaction  
  
SQL Server dispose des modes suivants pour l’initiation de la transaction :  
  
- **Validation automatique** : le début d’une requête simple ou d’une instruction DML ouvre implicitement une transaction, et la fin d’une instruction valide implicitement la transaction. **Validation automatique** est la valeur par défaut.  
  - En mode Validation automatique, vous n’êtes généralement pas obligé de coder un indicateur de table à propos du niveau d’isolation de la transaction sur la table optimisée en mémoire dans la clause FROM.  
  
- **Explicite** : votre instruction Transact-SQL contient le code BEGIN TRANSACTION, et éventuellement COMMIT TRANSACTION. Plusieurs instructions peuvent être rassemblées dans une même transaction.  
  - En mode Explicite, vous devez utiliser l’option de base de données MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT ou coder un indicateur de table à propos du niveau d’isolation de la transaction sur la table optimisée en mémoire dans la clause FROM.  
  
- **Implicite** : lorsque SET IMPLICIT_TRANSACTION ON est appliqué. Le nom IMPLICIT_BEGIN_TRANSACTION aurait sans doute été plus approprié, car cette option ne fait qu’appliquer implicitement l’équivalent d’une instruction BEGIN TRANSACTION explicite avant chaque instruction UPDATE si 0 = @@trancount. Par conséquent, c’est à votre code T-SQL d’émettre finalement une instruction COMMIT TRANSACTION explicite.   
  
- **Bloc ATOMIC** : toutes les instructions des blocs ATOMIC s’exécutent toujours dans le cadre d’une seule transaction. Les actions du bloc ATOMIC sont validées comme un tout en cas de réussite, ou les actions sont toutes annulées quand une défaillance se produit. Chaque procédure stockée compilée en mode natif nécessite un bloc ATOMIC.  
  
### <a name="code-example-with-explicit-mode"></a>Exemple de code avec le mode Explicite  
  
Le script Transact-SQL interprété suivant utilise :  
  
- une transaction explicite ;
- une table optimisée en mémoire, nommée dbo.Order_mo ;
- le contexte de niveau d’isolation de la transaction READ COMMITTED.  
  
Par conséquent, il est nécessaire d’avoir un indicateur de table sur la table optimisée en mémoire. L’indicateur doit être pour SNAPSHOT ou un niveau d’isolation supérieur. Dans le cas de l’exemple de code, l’indicateur est WITH (SNAPSHOT). Si cet indicateur est supprimé, le script rencontre une erreur 41368, pour laquelle une nouvelle tentative automatique n’est pas adaptée :  

#### <a name="error-41368"></a>Erreur 41368

L'accès aux tables optimisées en mémoire selon le niveau d'isolement READ COMMITTED est pris en charge uniquement pour les transactions validées automatiquement. Cela n'est pas pris en charge pour les transactions explicites ou implicites. Spécifiez un niveau d’isolation pris en charge pour la table optimisée en mémoire à l’aide d’un indicateur de table, comme WITH (SNAPSHOT).

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
GO  

BEGIN TRANSACTION;  -- Explicit transaction.  

-- Order_mo  is a memory-optimized table.  
SELECT * FROM  
           dbo.Order_mo  as o  WITH (SNAPSHOT)  -- Table hint.  
      JOIN dbo.Customer  as c  on c.CustomerId = o.CustomerId;  
COMMIT TRANSACTION;
```

L’utilisation de l’indicateur `WITH (SNAPSHOT)` peut être évitée grâce à l’utilisation de l’option de base de données `MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT`. Lorsque cette option a la valeur `ON`, si une table optimisée en mémoire dispose d’un niveau d’isolation bas, celui-ci est élevé automatiquement au niveau d’isolation SNAPSHOT.  

```sql
ALTER DATABASE CURRENT
    SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```

## <a name="row-versioning"></a>Contrôle de version de ligne  
  
Les tables optimisées en mémoire utilisent un système de contrôle de version de ligne très sophistiqué qui rend l’approche optimiste efficace, même au niveau d’isolation le plus strict de SERIALIZABLE. Pour en savoir plus, consultez [Introduction aux tables optimisées en mémoire](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md).  
  
Les tables sur disque disposent indirectement d’un système de contrôle de version de ligne lorsque le niveau d’isolation READ_COMMITTED_SNAPSHOT ou SNAPSHOT est appliqué. Ce système est basé sur tempdb, alors que les structures de données optimisées en mémoire ont un contrôle de version de ligne intégré pour une efficacité maximale.  
  
## <a name="isolation-levels"></a>Niveaux d’isolation 
  
La table suivante répertorie les niveaux d’isolation des transactions possibles, du plus faible au plus élevé. Pour plus d’informations sur les conflits qui peuvent se produire et la logique de nouvelles tentatives permettant de résoudre ces conflits, consultez [Détection des conflits et logique des nouvelles tentatives](#conflict-detection-and-retry-logic). 
  
| Niveau d’isolation | Description |   
| :-- | :-- |   
| READ UNCOMMITTED | Non disponible : les tables optimisées en mémoire ne sont pas accessibles si le niveau d’isolation est Read Uncommitted. Il est toujours possible d’accéder aux tables optimisées en mémoire ayant un niveau d’isolation SNAPSHOT si le niveau d’isolation de la transaction au niveau de la session a la valeur READ UNCOMMITTED. Pour cela, vous devez utiliser l’indicateur de table WITH (SNAPSHOT) ou définir le paramètre de base de données MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT sur ON. | 
| READ COMMITTED | Pris en charge pour les tables optimisées en mémoire uniquement lorsque le mode de validation automatique est appliqué. Il est toujours possible d’accéder aux tables optimisées en mémoire ayant un niveau d’isolation SNAPSHOT si le niveau d’isolation de la transaction au niveau de la session a la valeur READ COMMITTED. Pour cela, vous devez utiliser l’indicateur de table WITH (SNAPSHOT) ou définir le paramètre de base de données MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT sur ON.<br/><br/>Si l’option de base de données READ_COMMITTED_SNAPSHOT a la valeur ON, il n’est pas possible d’accéder à la fois à une table optimisée en mémoire et à une table sur disque situées dans une même instruction si le niveau d’isolation est READ COMMITTED. |  
| SNAPSHOT | Pris en charge pour les tables optimisées en mémoire. <br/><br/> En interne, le niveau d’isolation SNAPSHOT est le moins exigeant pour les tables optimisées en mémoire. <br/><br/> SNAPSHOT utilise moins de ressources système que REPEATABLE READ ou SERIALIZABLE. |  
| REPEATABLE READ | Pris en charge pour les tables optimisées en mémoire. Le niveau d’isolation REPEATABLE READ garantit qu’au moment de la validation, aucune transaction simultanée ne met à jour les lignes lues par cette transaction. <br/><br/> Le modèle optimiste n’a pas empêché les transactions simultanées de mettre à jour les lignes lues par cette transaction. En revanche, au moment de la validation, cette transaction a validé que le niveau d’isolation REPEATABLE READ a été respecté. Si le niveau d’isolation n’est pas respecté, cette transaction est annulée et doit être retentée. | 
| SERIALIZABLE | Pris en charge pour les tables optimisées en mémoire. <br/><br/> Ce niveau est appelé *Serializable* car l’isolation est tellement stricte que les transactions peuvent presque sembler exécutées en série plutôt que simultanément. | 

## <a name="transaction-phases-and-lifetime"></a>Durée de vie et phases de la transaction  
  
Quand une table optimisée en mémoire est impliquée, la durée de vie d’une transaction passe par les phases décrites dans l’image suivante :
  
![hekaton_transactions](../../relational-databases/in-memory-oltp/media/hekaton-transactions.gif)  
  
Voici une description des phases.  
  
#### <a name="regular-processing-phase-1-of-3"></a>Traitement normal : Phase 1 (sur 3)  
  
- Cette phase se compose de l’exécution de toutes les requêtes et des instructions DML de la requête.  
- Pendant cette phase, les instructions voient la version des tables optimisées en mémoire comme l’heure de début logique de la transaction.  
  
#### <a name="validation-phase-2-of-3"></a>Validation : Phase 2 (sur 3)  
  
- La phase de validation commence par l’assignation d’une heure de fin et donc le marquage d’une transaction comme logiquement terminée. Ceci rend toutes les modifications de la transaction visibles aux autres transactions qui dépendent de cette transaction. La validation des transactions dépendantes n’est pas autorisée tant que la validation de cette transaction n’a pas réussi. En outre, les transactions qui ont ces dépendances ne sont pas autorisées à retourner des jeux de résultats au client. De cette manière, le client voit seulement les données dont la validation a réussi dans la base de données.  
- Cette phase comprend les validations REPEATABLE READ et SERIALIZABLE. La validation REPEATABLE READ vérifie si des lignes lues par la transaction ont été mises à jour entre-temps. La validation SERIALIZABLE vérifie si une ligne a été insérée dans une plage de données analysée par cette transaction. Selon le tableau de la section [Niveaux d’isolation et conflits](#isolation-levels), les validations REPEATABLE READ et SERIALIZABLE peuvent avoir lieu quand vous utilisez l’isolation SNAPSHOT pour valider la cohérence des contraintes de clés étrangères et uniques.  
  
#### <a name="commit-processing-phase-3-of-3"></a>Traitement de la validation : Phase 3 (sur 3)  
  
- Durant la phase de validation, les modifications apportées aux tables durables sont écrites dans le journal, et le journal est écrit sur le disque. Le contrôle est ensuite rendu au client.  
- Une fois le traitement de validation terminé, toutes les transactions dépendantes sont informées qu’elles peuvent être validées.  
  
Comme toujours, nous vous recommandons d’utiliser des unités transactionnelles de travail aussi courtes et minimales que le permettent vos besoins en données.  
  
## <a name="conflict-detection-and-retry-logic"></a>Détection des conflits et logique des nouvelles tentatives 

Il existe deux types de conditions d’erreur qui peuvent causer l’échec et l’annulation d’une transaction. Dans la plupart des cas, lorsqu’un tel échec se produit, la transaction doit être retentée, comme dans le cas d’un blocage.
- Conflits entre plusieurs transactions simultanées. Il s’agit là de conflits de mise à jour et d’échecs de validation qui peuvent être dus à une violation du niveau d’isolation des transactions ou à une violation des contraintes.
- Échecs de dépendance. Ceux-ci sont causés par l’échec de la validation de transactions dont vous dépendez ou par un nombre excessif de dépendances.

Voici les conditions d’erreur qui peuvent entraîner l’échec des transactions quand elles accèdent à des tables optimisées en mémoire.

| Code d'erreur | Description | Cause |
| :-- | :-- | :-- |
| **41302** | Tentative de mise à jour d’une ligne qui a été mise à jour dans une autre transaction depuis le début de la transaction actuelle. | Cette erreur se produit si deux transactions simultanées tentent de mettre à jour ou de supprimer la même ligne au même moment. L’une des deux transactions reçoit ce message d’erreur et doit être retentée. <br/><br/>  | 
| **41305**| Échec de la validation de lecture renouvelable Une ligne lue dans une table optimisée en mémoire a été mise à jour par une autre transaction qui a été validée avant cette transaction. | Cette erreur peut se produire lors de l’utilisation du niveau d’isolation REPEATABLE READ ou SERIALIZABLE, et également si les actions d’une transaction simultanée provoquent la violation d’une contrainte FOREIGN KEY. <br/><br/>Une telle violation simultanée de contraintes de clé étrangère est rare, et indique généralement un problème avec la logique de l’application ou avec une entrée de données. Toutefois, l’erreur peut également se produire s’il n’existe aucun index sur les colonnes impliquées dans la contrainte FOREIGN KEY. Il est donc recommandé de toujours créer un index sur les colonnes de clé étrangère d’une table optimisée en mémoire. <br/><br/> Pour plus d’informations sur les échecs de validation causés par des violations de clé étrangère, consultez [ce billet de blog](/archive/blogs/sqlcat/considerations-around-validation-errors-41305-and-41325-on-memory-optimized-tables-with-foreign-keys) de l’équipe de consultants clients de SQL Server. |  
| **41325** | Échec de la validation sérialisable Une nouvelle ligne a été insérée dans une plage analysée précédemment par la transaction actuelle. Nous appelons cela une ligne fantôme. | Cette erreur peut se produire lors de l’utilisation du niveau d’isolation SERIALIZABLE, et également si les actions d’une transaction simultanée provoquent la violation d’une contrainte PRIMARY KEY, UNIQUE ou FOREIGN KEY. <br/><br/> Une telle violation simultanée des contraintes est rare, et indique généralement un problème avec la logique de l’application ou avec une entrée de données. Toutefois, comme pour les échecs de validation REPEATABLE READ, cette erreur peut également se produire s’il existe une contrainte FOREIGN KEY sans index sur les colonnes impliquées. |  
| **41301** | Échec de la dépendance : une dépendance a été prise sur une autre transaction dont la validation a échoué. | Cette transaction (Tx1) a pris une dépendance sur une autre transaction (Tx2) lorsque celle-ci (Tx2) était en phase de validation, en lisant des données écrites par Tx2. La validation de Tx2 a donc échoué. Le plus souvent, l’échec de la validation de Tx2 est causé par l’échec de la validation de REPEATABLE READ (41305) et de SERIALIZABLE (41325). La validation peut également échouer en cas d’échec d’E/S du journal, même si cela est moins fréquent. |
| **41823** et **41840** | Le quota pour les données utilisateur dans les tables optimisées en mémoire et les variables de table a été atteint. | L’erreur 41823 s’applique à SQL Server Express/Web/Standard Edition, ainsi qu’aux bases de données uniques dans [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. L’erreur 41840 s’applique aux pools élastiques dans [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/> Dans la plupart des cas, ces erreurs indiquent que la taille maximale des données utilisateur a été atteinte. Le moyen de résoudre l’erreur consiste à supprimer des données dans les tables optimisées en mémoire. Dans de rares cas cependant, cette erreur est temporaire. Nous recommandons donc de faire une nouvelle tentative la première fois que ces erreurs sont rencontrées.<br/><br/> Comme les autres erreurs de cette liste, les erreurs 41823 et 41840 entraînent l’abandon de la transaction active. |
| **41839** | Le nombre maximal de dépendances de validation d’une transaction a été dépassé. |**S’applique à :** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Aucune limite du nombre de dépendances de validation ne s’applique aux versions ultérieures de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br/><br/> Il existe une limite du nombre de transactions dont une transaction donnée (Tx1) peut dépendre. Ces transactions sont les dépendances sortantes. En outre, il existe une limite du nombre de transactions pouvant dépendre d’une transaction donnée (Tx1). Ces transactions sont les dépendances entrantes. La limite pour les deux est de 8 transactions. <br/><br/> Le plus souvent, cet échec est dû au fait qu’un nombre très important de transactions de lecture accède aux données écrites par une seule transaction d’écriture. La probabilité de cette erreur augmente si les opérations de lecture effectuent toutes une analyse importante des mêmes données et si la validation ou le traitement de validation de la transaction d’écriture sont longs, par exemple, si la transaction d’écriture effectue des analyses importantes avec un niveau d’isolation SERIALIZABLE (augmentation de la durée de la phase de validation) ou si le journal des transactions est placé sur un périphérique d’E/S de journal lent (augmentation de la durée de traitement de la validation). Si les transactions en lecture effectuent des explorations importantes et si elles sont censées accéder seulement à quelques lignes, cela peut indiquer qu’un index est manquant. De même, si la transaction d’écriture effectue des analyses importantes avec le niveau d’isolation SERIALIZABLE, mais qu’elle n’est censée accéder qu’à quelques lignes, cela peut également indiquer qu’un index est manquant. <br/><br/> Le nombre maximal de dépendances de validation peut être augmenté à l’aide de l’indicateur de trace **9926**. Utilisez cet indicateur de trace uniquement si vous obtenez cette erreur après avoir vérifié qu’aucun index n’est manquant, car cela pourrait masquer ces problèmes dans les cas mentionnés ci-dessus. Gardez également à l’esprit que la complexité des graphiques de dépendances, où chaque transaction comprend un grand nombre de dépendances entrantes et sortantes, ainsi que de nombreuses couches de dépendances, peut rendre le système inefficace.  |
  
### <a name="retry-logic"></a>Logique de nouvelle tentative 

Lorsqu’une transaction échoue en raison de l’une des conditions mentionnées ci-dessus, la transaction doit être retentée.
  
La logique des nouvelles tentatives peut être implémentée côté client ou côté serveur. La recommandation générale consiste à implémenter la logique des nouvelles tentatives côté client, car cela est plus efficace et vous permet d’accéder aux jeux de résultats retournés par la transaction avant que la défaillance ne se produise.  
  
#### <a name="retry-t-sql-code-example"></a>Exemple de code T-SQL de nouvelle tentative  
  
Une logique de nouvelle tentative côté serveur utilisant T-SQL doit être utilisée seulement pour des transactions qui ne retournent pas de jeux de résultats au client. Dans le cas contraire, les nouvelles tentatives peuvent engendrer des jeux de résultats supplémentaires s’ajoutant à ceux qu’il était prévu de retourner au client.  
  
Le script T-SQL interprété suivant illustre l’apparence de la logique de nouvelle tentative pour les erreurs associées à des conflits de transactions impliquant des tables optimisées en mémoire.

```sql
-- Retry logic, in Transact-SQL.
DROP PROCEDURE If Exists usp_update_salesorder_dates;
GO

CREATE PROCEDURE usp_update_salesorder_dates
AS
BEGIN
    DECLARE @retry INT = 10;

    WHILE (@retry > 0)
    BEGIN
        BEGIN TRY
            BEGIN TRANSACTION;

            UPDATE dbo.SalesOrder_mo WITH (SNAPSHOT)
                set OrderDate = GetUtcDate()
                where CustomerId = 42;

            UPDATE dbo.SalesOrder_mo WITH (SNAPSHOT)
                set OrderDate = GetUtcDate()
                where CustomerId = 43;

            COMMIT TRANSACTION;

            SET @retry = 0;  -- //Stops the loop.
        END TRY

        BEGIN CATCH
            SET @retry -= 1;

            IF (@retry > 0 AND
                ERROR_NUMBER() in (41302, 41305, 41325, 41301, 41823, 41840, 41839, 1205)
                )
            BEGIN
                IF XACT_STATE() = -1
                    ROLLBACK TRANSACTION;

                WAITFOR DELAY '00:00:00.001';
            END
            ELSE
            BEGIN
                PRINT 'Suffered an error for which Retry is inappropriate.';
                THROW;
            END
        END CATCH

    END -- //While loop
END;
GO

--  EXECUTE usp_update_salesorder_dates;
```

## <a name="cross-container-transaction"></a>Transaction entre conteneurs  
  
Une transaction est appelée transaction entre conteneurs si :  
  
- elle accède à une table optimisée en mémoire à partir du code Transact-SQL interprété ; ou  
- Exécute une procédure native alors qu’une transaction est déjà ouverte (XACT_STATE() = 1). 

Le qualificatif « entre conteneurs » signifie que la transaction s’exécute sur les deux conteneurs de gestion de transactions, celui pour les tables sur disque et celui pour les tables optimisées en mémoire.  
  
Dans une transaction entre conteneurs, différents niveaux d’isolation permettent d’accéder aux tables sur disque et optimisées en mémoire. Cette différence est exprimée par le biais des indicateurs de table explicites comme WITH (SERIALIZABLE) ou l’option de base de données MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT, qui élève implicitement le niveau d’isolation de la table optimisée en mémoire vers le niveau SNAPSHOT si TRANSACTION ISOLATION LEVEL est configuré en tant que READ COMMITTED ou READ UNCOMMITTED.  
  
Dans l’exemple de code Transact-SQL suivant :  
  
- L’accès à la table sur disque (Table_D1) se fait à l’aide du niveau d’isolation READ COMMITTED.  
- L’accès à la table optimisée en mémoire (Table_MO7) se fait à l’aide du niveau d’isolation SERIALIZABLE. La Table_MO6 n’a pas de niveau d’isolation spécifique, dans la mesure où les insertions sont toujours cohérentes et sont exécutées essentiellement avec un niveau d’isolation SERIALIZABLE.  


```sql
-- Different isolation levels for
-- disk-based tables versus memory-optimized tables,
-- within one explicit transaction.

SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
go

BEGIN TRANSACTION;

    -- Table_D1 is a traditional disk-based table, accessed using READ COMMITTED isolation.

    SELECT * FROM Table_D1;


    -- Table_MO6 and Table_MO7 are memory-optimized tables.
    -- Table_MO7 is accessed using SERIALIZABLE isolation,
    --   while Table_MO6 does not have a specific isolation level.

    INSERT Table_MO6
        SELECT * FROM Table_MO7 WITH (SERIALIZABLE);

COMMIT TRANSACTION;
go
```

## <a name="limitations"></a>Limites  
  
- Les transactions entre bases de données ne sont pas prises en charge avec les tables optimisées en mémoire. Si une transaction accède à une table optimisée en mémoire, elle ne peut pas accéder à une autre base de données, à l’exception de :  
  - base de données tempdb  
  - lecture seule à partir de la base de données MASTER.  
  
- Les transactions distribuées ne sont pas prises en charge : Lorsque BEGIN DISTRIBUTED TRANSACTION est utilisé, la transaction ne peut pas accéder à une table optimisée en mémoire.  
  
## <a name="natively-compiled-stored-procedures"></a>procédures stockées compilées en mode natif  
  
- Dans une procédure native, le bloc ATOMIQUE doit déclarer le niveau d’isolation de la transaction pour l’ensemble du bloc, par exemple :  
  - `... BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, ...) ...`  
  
- Les instructions de contrôle de transaction explicite ne sont pas autorisées dans le corps d’une procédure native. Les instructions BEGIN TRANSACTION, ROLLBACK TRANSACTION, etc. sont toutes interdites.  
  
- Pour plus d’informations sur le contrôle des transactions à l’aide de blocs ATOMIC, consultez [Blocs ATOMIC](atomic-blocks-in-native-procedures.md)  
  
## <a name="other-transaction-links"></a>Autres liens de transaction  
  
- [SET IMPLICIT_TRANSACTIONS](../../t-sql/statements/set-implicit-transactions-transact-sql.md)  
  
- [sp_getapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
  
- [Niveaux d'isolation basés sur le contrôle de version de ligne dans le moteur de base de données](/previous-versions/sql/sql-server-2008-r2/ms177404(v=sql.105))  
  
- [Contrôler la durabilité d'une transaction](../../relational-databases/logs/control-transaction-durability.md)