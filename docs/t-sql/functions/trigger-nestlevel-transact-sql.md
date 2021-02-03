---
description: TRIGGER_NESTLEVEL (Transact-SQL)
title: TRIGGER_NESTLEVEL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TRIGGER_NESTLEVEL
- TRIGGER_NESTLEVEL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- triggers [SQL Server], number executed
- number of triggers
- TRIGGER_NESTLEVEL function
ms.assetid: 6a33e74a-0cf9-4ae1-a1e4-4a137a3ea39d
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 9ccd96f5df38256b6394aaf02ff31c450201d660
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195733"
---
# <a name="trigger_nestlevel-transact-sql"></a>TRIGGER_NESTLEVEL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie le nombre de déclencheurs exécutés pour l'instruction qui a activé le déclencheur. TRIGGER_NESTLEVEL est utilisé dans les déclencheurs DML et DDL pour déterminer le niveau d'imbrication actuel.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TRIGGER_NESTLEVEL ( [ object_id ] , [ 'trigger_type' ] , [ 'trigger_event_category' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *object_id*  
 Identificateur d'objet d'un déclencheur. Si l’argument *object_id* est spécifié, la valeur renvoyée est le nombre d’exécutions du déclencheur indiqué pour l’instruction. Si *object_id* n’est pas spécifié, c'est le nombre d’exécutions de tous les déclencheurs pour l’instruction qui est renvoyé.  
  
 **'** *trigger_type* **'**  
 Spécifie si TRIGGER_NESTLEVEL doit s'appliquer aux déclencheurs AFTER ou aux déclencheurs INSTEAD OF. Spécifiez **AFTER** pour les déclencheurs AFTER. Spécifiez **IOT** pour les déclencheurs INSTEAD OF. Si *trigger_type* est spécifié, *trigger_event_category* doit également être spécifié.  
  
 **'** *trigger_event_category* **'**  
 Spécifie si TRIGGER_NESTLEVEL doit s'appliquer aux déclencheurs DML ou DDL. Spécifiez **DML** pour les déclencheurs DML. Spécifiez **DDL** pour les déclencheurs DDL. Si *trigger_event_category* est spécifié, *trigger_type* doit également être spécifié. Notez que seule la valeur **AFTER** peut être spécifiée avec **DDL**, car les déclencheurs DDL ne peuvent être que des déclencheurs AFTER.  
  
## <a name="remarks"></a>Remarques  
 Si aucun paramètre n'est spécifié, TRIGGER_NESTLEVEL renvoie le nombre total de déclencheurs sur la pile des appels. Elle est comprise elle-même dans ce décompte. Les paramètres peuvent être omis lorsqu'un déclencheur exécute des commandes entraînant l'activation d'un autre déclencheur ou provoque l'activation d'une succession de déclencheurs.  
  
 Pour retourner le nombre total de déclencheurs sur la pile des appels pour un type de déclencheur et une catégorie d’événement déterminés, spécifiez *object_id* = 0.  
  
 TRIGGER_NESTLEVEL renvoie la valeur 0 si son exécution se produit à l'extérieur d'un déclencheur et si aucun paramètre n'a la valeur NULL.  
  
 Si des paramètres sont explicitement spécifiés avec la valeur NULL, une valeur NULL est renvoyée, que TRIGGER_NESTLEVEL ait été utilisé à l'intérieur ou à l'extérieur d'un déclencheur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-testing-the-nesting-level-of-a-specific-dml-trigger"></a>R. Test du niveau d'imbrication d'un déclencheur DML spécifique  
  
```sql
IF ( (SELECT TRIGGER_NESTLEVEL( OBJECT_ID('xyz') , 'AFTER' , 'DML' ) ) > 5 )  
   RAISERROR('Trigger xyz nested more than 5 levels.',16,-1)  
```  
  
### <a name="b-testing-the-nesting-level-of-a-specific-ddl-trigger"></a>B. Test du niveau d'imbrication d'un déclencheur DDL spécifique  
  
```sql
IF ( ( SELECT TRIGGER_NESTLEVEL ( ( SELECT object_id FROM sys.triggers  
WHERE name = 'abc' ), 'AFTER' , 'DDL' ) ) > 5 )  
   RAISERROR ('Trigger abc nested more than 5 levels.',16,-1)  
```  
  
### <a name="c-testing-the-nesting-level-of-all-triggers-executed"></a>C. Test du niveau d'imbrication de tous les déclencheurs exécutés  
  
```sql
IF ( (SELECT trigger_nestlevel() ) > 5 )  
   RAISERROR  
      ('This statement nested over 5 levels of triggers.',16,-1)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
  
  
