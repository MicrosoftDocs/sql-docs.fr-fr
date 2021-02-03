---
description: SET LOCK_TIMEOUT (Transact-SQL)
title: SET LOCK_TIMEOUT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LOCK_TIMEOUT_TSQL
- SET_LOCK_TIMEOUT_TSQL
- SET LOCK_TIMEOUT
- LOCK_TIMEOUT
dev_langs:
- TSQL
helpviewer_keywords:
- timeout options [SQL Server], locks
- releasing locks
- LOCK_TIMEOUT option
- SET LOCK_TIMEOUT statement
- locking [SQL Server], time-outs
- wait time for lock releases [SQL Server]
ms.assetid: dd0c389e-956d-435e-bf71-e16624a0a215
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 037fe5a16b873f325c73f6c4b81a2ec0ab029015
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189474"
---
# <a name="set-lock_timeout-transact-sql"></a>SET LOCK_TIMEOUT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Spécifie le nombre de millisecondes qu’attend une instruction avant la libération d’un verrou.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SET LOCK_TIMEOUT timeout_period  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *timeout_period*  
 Nombre de millisecondes écoulées avant que [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne une erreur de verrouillage. La valeur -1 (par défaut) indique qu'il n'y a pas de délai d'expiration (et par conséquent une attente infinie).  
  
 Si le délai d'attente avant la libération d'un verrou est supérieur au délai d'expiration spécifié, une erreur est renvoyée. Une valeur égale à 0 signifie que l'instruction n'attend pas et qu'elle retourne un message dès qu'elle rencontre un verrou.  
  
## <a name="remarks"></a>Notes  
 Au début d'une connexion, la valeur de ce paramètre est -1 Une fois celle-ci modifiée, le nouveau paramètre reste en vigueur pour le reste de la durée de la connexion.  
  
 L'option SET LOCK_TIMEOUT est appliquée lors de l'exécution, et non pas lors de l'analyse.  
  
 L'indicateur de verrouillage READPAST est une alternative à cette option SET.  
  
 Les instructions CREATE DATABASE, ALTER DATABASE et DROP DATABASE ignorent le paramètre SET LOCK_TIMEOUT.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-set-the-lock-timeout-to-1800-milliseconds"></a>A : Définir le délai d’attente de verrou sur 1800 millisecondes  
 L'exemple suivant définit le délai d'attente de déverrouillage à `1800` millisecondes.  
  
```sql  
SET LOCK_TIMEOUT 1800;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-set-the-lock-timeout-to-wait-forever-for-a-lock-to-be-released"></a>B. Définir le délai d’attente de verrou pour attendre indéfiniment qu’un verrou soit libéré.  
 L’exemple suivant définit le délai d’attente de verrou pour attendre indéfiniment et ne jamais expirer. Il s’agit du comportement par défaut qui est déjà défini au début de chaque connexion.  
  
```sql  
SET LOCK_TIMEOUT -1;  
```  
  
 L'exemple suivant définit le délai d'attente de déverrouillage à `1800` millisecondes. Dans cette version, [!INCLUDE[ssDW](../../includes/ssdw-md.md)] analyse l’instruction avec succès, mais ignore la valeur 1800 et continue à utiliser le comportement par défaut.  
  
```sql  
SET LOCK_TIMEOUT 1800;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [@@LOCK_TIMEOUT &#40;Transact-SQL&#41;](../../t-sql/functions/lock-timeout-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

