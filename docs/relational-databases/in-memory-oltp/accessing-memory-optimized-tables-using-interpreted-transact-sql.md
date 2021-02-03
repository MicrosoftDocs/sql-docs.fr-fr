---
title: Tables à mémoire optimisée basées sur le T-SQL interprété
description: En savoir plus sur l’accès aux tables à mémoire optimisées à l’aide de Transact-SQL interprété (lots Transact-SQL ou procédures stockées dans SQL Server).
ms.custom: seo-dt-2019
ms.date: 05/31/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 92a44d4d-0e53-4fb0-b890-de264c65c95a
author: MightyPen
ms.author: genemi
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3bce064f00aa3f16a9fc4710fc9dd7dcd53e1534
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236869"
---
# <a name="accessing-memory-optimized-tables-using-interpreted-transact-sql"></a>Accès aux tables optimisées en mémoire à l’aide du Transact-SQL interprété
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

 À quelques exceptions près, vous accédez aux tables mémoire optimisées à l'aide d'une requête [!INCLUDE[tsql](../../includes/tsql-md.md)] ou d'une opération DML (sélectionner, insérer, mettre à jour ou supprimer), de lots ad hoc et de modules SQL, tels que des procédures stockées, des fonctions table, des déclencheurs et des vues.  
  
Le [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété fait référence à des lots ou des procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] autres qu'une procédure stockée compilée en mode natif. L'accès par [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété aux tables mémoire optimisées est appelé un accès d'interopérabilité.  

À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], les requêtes interprétées dans [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent analyser en parallèle des tables optimisées en mémoire, au lieu de les analyser en série.

Les tables mémoire optimisées sont également accessibles à l'aide d'une procédure stockée compilée en mode natif. Les procédures stockées compilées en mode natif sont recommandées pour les opérations OLTP critiques pour les performances.  
  
L'accès en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété est recommandé dans les scénarios suivants :  
  
- Requêtes ad hoc et tâches d'administration.  
  
- Requêtes de rapports qui utilisent généralement des constructions non disponibles dans les procédures stockées compilées en mode natif (telles que les fonctions de *fenêtre* , parfois désignées sous le terme de fonctions [OVER](../../t-sql/queries/select-over-clause-transact-sql.md) ).  
  
- Pour migrer des parties de votre application qui ont un impact sur les performances vers des tables mémoire optimisées, avec des modifications de code minimes (ou sans aucune modification du code). Potentiellement, vous constaterez de meilleures performances en migrant des tables. Si vous migrez ensuite des procédures stockées vers des procédures stockées compilées en mode natif, vous constaterez des améliorations de performances accrues.  
  
- Lorsqu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] n'est pas disponible pour les procédures stockées compilées en mode natif.  
  
Cependant, les constructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes ne sont pas prises en charge dans les procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] interprétées qui accèdent aux données dans une table mémoire optimisée.  
  
|Domaine|Non pris en charge|  
|----------|-----------------|  
|Accès aux tables|TRUNCATE TABLE<br /><br /> MERGE (table mémoire optimisée comme cible).<br /><br /> Curseurs DYNAMIC et KEYSET (dégradés automatiquement en STATIC).<br /><br /> Accédez aux modules CLR à l'aide de la connexion contextuelle.<br /><br /> Référencement d'une table mémoire optimisée à partir d'une vue indexée.|  
|Bases de données croisées|Requêtes de bases de données croisées<br /><br /> Transactions de bases de données croisées<br /><br /> Serveurs liés|  
  
## <a name="table-hints"></a>Indicateurs de table

Pour plus d'informations sur les indicateurs de table, consultez. [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md). L'isolement SNAPSHOT a été ajouté pour prendre en charge [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  
  
Les indicateurs de table suivants ne sont pas pris en charge lors de l'accès à une table de mémoire optimisée à l'aide du [!INCLUDE[tsql](../../includes/tsql-md.md)]interprété.  

:::row:::
    :::column:::
        HOLDLOCK

        PAGLOCK

        READUNCOMMITTED

        TABLOCKXX
    :::column-end:::
    :::column:::
        IGNORE_CONSTRAINTS

        READCOMMITTED

        ROWLOCK

        UPDLOCK
    :::column-end:::
    :::column:::
        IGNORE_TRIGGERS

        READCOMMITTEDLOCK

        SPATIAL_WINDOW_MAX_CELLS = *integer*

        XLOCK
    :::column-end:::
    :::column:::
        NOWAIT

        READPAST

        TABLOCK
    :::column-end:::
:::row-end:::

Lorsque vous accédez à une table optimisée en mémoire à partir d’une transaction explicite ou implicite à l’aide du [!INCLUDE[tsql](../../includes/tsql-md.md)]interprété, vous devez au moins effectuer l’une des opérations suivantes :  
  
- Spécifiez un [indicateur de table de niveau d’isolation](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md) tel que SNAPSHOT, REPEATABLEREAD ou SERIALIZABLE.  
  
- Définissez l’option de base de données [MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT](../../t-sql/statements/alter-database-transact-sql-set-options.md) sur ON.  
  
Un indicateur de table de niveau d’isolation n’est pas obligatoire pour les tables optimisées en mémoire accessibles par des requêtes qui s’exécutent en [mode de validation automatique](../../odbc/reference/develop-app/auto-commit-mode.md).  
  
## <a name="see-also"></a>Voir aussi

[Prise en charge de Transact-SQL pour OLTP en mémoire](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)   

[Migration vers OLTP en mémoire](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)