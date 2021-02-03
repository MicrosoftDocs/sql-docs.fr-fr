---
description: CREATE TABLE (Transact-SQL) IDENTITY (propriété)
title: IDENTITY (propriété) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY property
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY property
- autonumbers, identity numbers
ms.assetid: 8429134f-c821-4033-a07c-f782a48d501c
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e239ff0edf653c5c172e0dfacb6b9ee835901a91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177939"
---
# <a name="create-table-transact-sql-identity-property"></a>CREATE TABLE (Transact-SQL) IDENTITY (propriété)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Crée une colonne d'identité dans une table. Cette propriété est utilisée avec les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE TABLE et ALTER TABLE.  
  
> [!NOTE]  
>  La propriété IDENTITY diffère de la propriété SQL-DMO **Identity** qui expose la propriété d’identité de lignes d’une colonne.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
IDENTITY [ (seed , increment) ]
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *seed*  
 Valeur utilisée pour la toute première ligne chargée dans la table.  
  
 *increment*  
 Valeur d'incrément ajoutée à la valeur d'identité de la ligne précédemment chargée.

 > [!NOTE]
 > Dans Azure Synapse Analytics, les valeurs d’identité ne sont pas incrémentielles en raison de l’architecture distribuée de l’entrepôt de données. Pour plus d’informations, consultez [Création de clés de substitution dans un pool Synapse SQL avec IDENTITY](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-identity#allocation-of-values).
  
 Vous devez spécifier à la fois la valeur initiale et l'incrément, ou aucun des deux. Si vous n'en spécifiez aucun, la valeur par défaut est (1,1).  
  
## <a name="remarks"></a>Notes  
 Les colonnes d'identité peuvent être utilisées pour générer des valeurs de clé. La propriété d'identité sur une colonne garantit ce qui suit :  
  
-   Chaque nouvelle valeur est générée en fonction de la valeur initiale et de l’incrément actuels.  
  
-   Chaque nouvelle valeur pour une transaction spécifique est différente des autres transactions simultanées sur la table.  
  
 La propriété d'identité sur une colonne ne garantit pas ce qui suit :  
  
-   **Unicité de la valeur** : l’unicité doit être appliquée avec une contrainte **PRIMARY KEY** ou **UNIQUE**, ou avec un index **UNIQUE**. - 
 
> [!NOTE]
> Azure Synapse Analytics ne prend pas en charge la contrainte **PRIMARY KEY** ou **UNIQUE** ou l’index **UNIQUE**. Pour plus d’informations, consultez [Création de clés de substitution dans un pool Synapse SQL avec IDENTITY](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-identity#what-is-a-surrogate-key).

-   **Valeurs consécutives dans une transaction** : il n’est pas garanti qu’une transaction insérant plusieurs lignes obtienne des valeurs consécutives pour les lignes, car d’autres insertions simultanées peuvent se produire sur la table. Si les valeurs doivent être consécutives, alors la transaction doit utiliser un verrou sur la table ou le niveau d’isolation **SERIALIZABLE**.  
  
-   **Valeurs consécutives après le redémarrage du serveur ou d’autres échecs** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut mettre en cache les valeurs d’identité pour des raisons de performance, et certaines valeurs affectées peuvent être perdues lors d’un échec de base de données ou du redémarrage du serveur. Cela peut entraîner des intervalles de valeur d'identité à l'insertion. Si les intervalles ne sont pas acceptables, alors l'application doit utiliser son propre mécanisme pour générer des valeurs clés. L’utilisation d’un générateur de séquence avec l’option **NOCACHE** peut limiter les intervalles pour les transactions qui ne sont jamais validées.  
  
-   **Réutilisation des valeurs** : pour une propriété d’identité donnée, avec une valeur/un incrément spécifique, les valeurs d’identité ne sont pas réutilisées par le moteur. Si une instruction d'insertion donnée échoue, ou si l'instruction d'insertion est restaurée, alors les valeurs d'identité consommées sont perdues et ne peuvent plus être générées. Cela peut entraîner des intervalles lorsque les valeurs d'identité ultérieures sont générées.  
  
 Ces restrictions sont dues à la conception et visent à améliorer les performances, car elles sont acceptables dans la plupart des situations. Si vous ne pouvez pas utiliser les valeurs d'identité en raison de ces restrictions, créez une table distincte contenant une valeur actuelle et gérez l'accès à la table et l'affectation de numéro dans votre application.  
  
 Si une table possédant une colonne d'identité est publiée pour la réplication, la colonne doit être gérée de manière à ce qu'elle soit compatible avec le type de réplication utilisée. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 Une seule colonne d'identité peut être créée par table.  
  
 Pour les tables optimisées en mémoire, la valeur initiale et l'incrément doivent être définis à « 1,1 ». L’affectation d’une valeur de départ ou d’incrément autre que 1 provoque l’erreur suivante : L’utilisation de valeurs de départ ou d’incrément autres que 1 n’est pas prise en charge avec les tables à mémoire optimisée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-identity-property-with-create-table"></a>R. Utilisation de la propriété IDENTITY avec CREATE TABLE  
 L'exemple crée une table avec la propriété `IDENTITY` pour incrémenter automatiquement un numéro d'identification.  
  
```sql  
USE AdventureWorks2012;  
  
IF OBJECT_ID ('dbo.new_employees', 'U') IS NOT NULL  
   DROP TABLE new_employees;  
GO  
CREATE TABLE new_employees  
(  
 id_num int IDENTITY(1,1),  
 fname varchar (20),  
 minit char(1),  
 lname varchar(30)  
);  
  
INSERT new_employees  
   (fname, minit, lname)  
VALUES  
   ('Karin', 'F', 'Josephs');  
  
INSERT new_employees  
   (fname, minit, lname)  
VALUES  
   ('Pirkko', 'O', 'Koskitalo');  
```  
  
### <a name="b-using-generic-syntax-for-finding-gaps-in-identity-values"></a>B. Utilisation d'une syntaxe générique afin de trouver des intervalles entre les valeurs d'identité  
 L'exemple suivant illustre une syntaxe générique qui permet de trouver des intervalles entre les valeurs d'identité lorsque des données sont supprimées.  
  
> [!NOTE]  
>  La première partie du script [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant sert uniquement d'illustration. Vous pouvez exécuter le script [!INCLUDE[tsql](../../includes/tsql-md.md)] qui commence par le commentaire : `-- Create the img table`.  
  
```sql 
-- Here is the generic syntax for finding identity value gaps in data.  
-- The illustrative example starts here.  
SET IDENTITY_INSERT tablename ON;  
DECLARE @minidentval column_type;  
DECLARE @maxidentval column_type;  
DECLARE @nextidentval column_type;  
SELECT @minidentval = MIN($IDENTITY), @maxidentval = MAX($IDENTITY)  
    FROM tablename  
IF @minidentval = IDENT_SEED('tablename')  
   SELECT @nextidentval = MIN($IDENTITY) + IDENT_INCR('tablename')  
   FROM tablename t1  
   WHERE $IDENTITY BETWEEN IDENT_SEED('tablename') AND   
      @maxidentval AND  
      NOT EXISTS (SELECT * FROM tablename t2  
         WHERE t2.$IDENTITY = t1.$IDENTITY +   
            IDENT_INCR('tablename'))  
ELSE  
   SELECT @nextidentval = IDENT_SEED('tablename');  
SET IDENTITY_INSERT tablename OFF;  
-- Here is an example to find gaps in the actual data.  
-- The table is called img and has two columns: the first column   
-- called id_num, which is an increasing identification number, and the   
-- second column called company_name.  
-- This is the end of the illustration example.  
  
-- Create the img table.  
-- If the img table already exists, drop it.  
-- Create the img table.  
IF OBJECT_ID ('dbo.img', 'U') IS NOT NULL  
   DROP TABLE img;  
GO  
CREATE TABLE img (id_num INT IDENTITY(1,1), company_name sysname);  
INSERT img(company_name) VALUES ('New Moon Books');  
INSERT img(company_name) VALUES ('Lucerne Publishing');  
-- SET IDENTITY_INSERT ON and use in img table.  
SET IDENTITY_INSERT img ON;  
  
DECLARE @minidentval SMALLINT;  
DECLARE @nextidentval SMALLINT;  
SELECT @minidentval = MIN($IDENTITY) FROM img  
 IF @minidentval = IDENT_SEED('img')  
    SELECT @nextidentval = MIN($IDENTITY) + IDENT_INCR('img')  
    FROM img t1  
    WHERE $IDENTITY BETWEEN IDENT_SEED('img') AND 32766 AND  
      NOT    EXISTS (SELECT * FROM img t2  
          WHERE t2.$IDENTITY = t1.$IDENTITY + IDENT_INCR('img'))  
 ELSE  
    SELECT @nextidentval = IDENT_SEED('img');  
SET IDENTITY_INSERT img OFF;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)   
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY &#40;fonction&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/identity-function-transact-sql.md)   
 [IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [SET IDENTITY_INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/set-identity-insert-transact-sql.md)   
 [Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  
