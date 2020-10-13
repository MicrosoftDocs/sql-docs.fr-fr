---
title: DDL pris en charge pour les modules T-SQL compilés en mode natif | Microsoft Docs
description: Apprenez-en davantage sur les constructions en langage de définition de données (DDL, Data Definition Language) prises en charge pour les modules T-SQL compilés en mode natif, telles que les procédures stockées, les fonctions définies par l’utilisateur (UDF) scalaires, les fonctions table (TVF) inline et les déclencheurs.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 6b21f47e-bceb-4054-8b3c-9d39bb9583c0
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9e7dc391e0f199ab6ab8855599506bb01365333a
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868475"
---
# <a name="supported-ddl-for-natively-compiled-t-sql-modules"></a>DDL pris en charge pour les modules T-SQL compilés en mode natif
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique répertorie les constructions DDL (langage de définition de données) prises en charge pour les modules T-SQL compilés en mode natif, telles que les procédures stockées, les fonctions définies par l’utilisateur (UDF) scalaires, les Fonctions table (TVF) inline et les déclencheurs.  
  
 Pour plus d’informations sur les fonctionnalités et la surface d’exposition de T-SQL qui peut être utilisée dans des modules de T-SQL compilés en mode natif, consultez [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
 Pour plus d’informations sur les constructions non prises en charge, consultez [Constructions Transact-SQL non prises en charge par l’OLTP en mémoire](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  
  
 Les constructions suivantes sont admises :  
  
-   [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
-   [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)  
  
-   [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)  
  
-   Instructions [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md) et INSERT SELECT  
  
-   SCHEMABINDING et BEGIN ATOMIC (requis pour les procédures stockées compilées en mode natif)  
  
     Pour plus d’informations, voir [Creating Natively Compiled Stored Procedures](../../relational-databases/in-memory-oltp/creating-natively-compiled-stored-procedures.md).  
  
-   NATIVE_COMPILATION  
  
     Pour plus d’informations, voir [Native Compilation of Tables and Stored Procedures](../../relational-databases/in-memory-oltp/native-compilation-of-tables-and-stored-procedures.md).  
  
-   Les paramètres et les variables peuvent être déclarés comme NOT NULL (uniquement disponibles pour les modules compilés en mode natif : procédures stockées compilées en mode natif et fonctions définies par l’utilisateur scalaires compilées en mode natif).  
  
-   Paramètres table.  
  
     Pour plus d’informations, consultez [Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
-   EXECUTE AS OWNER, SELF, CALLER et utilisateur.  
  
-   GRANT (accorder) et DENY (refuser) des autorisations sur les tables et les procédures.  
  
     Pour plus d’informations, consultez [Autorisations d’objet GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md) et [Autorisations d’objet DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
