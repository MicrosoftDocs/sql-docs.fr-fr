---
description: Implémenter des déclencheurs DDL
title: Implémenter des déclencheurs DDL | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- DDL triggers, implementing
ms.assetid: f44e5340-1d18-40e9-828e-0ffcca091ae3
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b6ad03a2ca062ecc93d8027d544ca37ef142cb74
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88472981"
---
# <a name="implement-ddl-triggers"></a>Implémenter des déclencheurs DDL
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique fournit des informations pour vous aider à créer, modifier, désactiver ou supprimer des déclencheurs DDL.  
  
## <a name="creating-ddl-triggers"></a>Création de déclencheurs DDL  
 Les déclencheurs DDL sont créés à l'aide de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE TRIGGER pour les déclencheurs DDL.  
  
 **Pour créer un déclencheur DDL**  
  
-   [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
  
> [!IMPORTANT]  
>  La fonctionnalité de renvoi des jeux de résultats à partir de déclencheurs sera éliminée dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les déclencheurs qui renvoient des jeux de résultats peuvent provoquer un comportement inattendu des applications qui ne sont pas conçues pour interagir avec eux. Évitez de renvoyer des jeux de résultats provenant de déclencheurs dans un nouveau travail de développement et prévoyez la modification des applications qui y recourent actuellement. Pour empêcher les déclencheurs de retourner des jeux de résultats dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], définissez l’option d’ [interdiction des résultats des déclencheurs](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) sur 1. Le paramètre par défaut de cette option sera 1 dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="modifying-ddl-triggers"></a>Modification de déclencheurs DDL  
 Si vous devez modifier la définition d'un déclencheur DDL, vous pouvez soit l'annuler, puis le recréer, soit redéfinir le déclencheur existant en une seule opération.  
  
 Si vous changez le nom d'un objet référencé par un déclencheur DDL, vous devez modifier le déclencheur pour que sa définition se réfère au nouveau nom de l'objet. Par conséquent, avant de renommer un objet, affichez les dépendances de l'objet pour savoir si des déclencheurs peuvent être concernés par la modification projetée.  
  
 Un déclencheur peut aussi être modifié pour en chiffrer la définition.  
  
 **Pour modifier un déclencheur**  
  
-   [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
 **Pour afficher les dépendances d'un déclencheur**  
  
-   [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
-   [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)  
  
-   [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
## <a name="disabling-and-dropping-ddl-triggers"></a>Désactivation et suppression de déclencheurs DDL  
 Vous pouvez désactiver ou supprimer un déclencheur DDL s'il ne vous est plus utile.  
  
 La désactivation d'un déclencheur DDL n'entraîne pas sa suppression. Le déclencheur existe toujours en tant qu'objet dans la base de données actuelle. Cependant, il ne se déclenchera pas lorsque des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sur lesquelles il a été programmé seront exécutées. Tout déclencheur DDL désactivé peut être réactivé. Quand un déclencheur est réactivé, il se déclenche de la même manière que lors de sa création. Les déclencheurs sont activés par défaut au moment de leur création.  
  
 La suppression d'un déclencheur DDL entraîne sa suppression définitive de la base de données actuelle. Les objets ou données faisant partie de la portée du déclencheur DDL ne sont pas affectés.  
  
 **Pour désactiver un déclencheur DDL**  
  
-   [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)  
  
-   [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
 **Pour activer un déclencheur DDL**  
  
-   [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)  
  
-   [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
 **Pour supprimer un déclencheur DDL**  
  
-   [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)  
  
  
