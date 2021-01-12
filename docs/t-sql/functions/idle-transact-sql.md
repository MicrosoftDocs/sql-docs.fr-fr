---
description: '&#x40;&#x40;IDLE (Transact-SQL)'
title: '@@IDLE (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@IDLE_TSQL'
- '@@IDLE'
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], idle
- ticks [SQL Server]
- '@@IDLE function'
- status information [SQL Server], idle time
- idle time [SQL Server]
ms.assetid: 8f49c62a-8da5-4afd-a5eb-4df8ef8be755
author: cawrites
ms.author: chadam
ms.openlocfilehash: 96fd2277d19f8610b265c16942196585951eb5a9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101104"
---
# <a name="x40x40idle-transact-sql"></a>&#x40;&#x40;IDLE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne le temps pendant lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été inactif depuis le dernier démarrage. Le résultat est exprimé en incréments de temps processeur ou « graduations », et est cumulé pour tous les processeurs. Par conséquent, il peut être supérieur au temps écoulé actuel. Pour convertir cette valeur en microsecondes, multipliez-la par @@TIMETICKS.  
  
> [!NOTE]  
>  Si le temps renvoyé dans @@CPU_BUSY ou @@IO_BUSY dépasse approximativement 49 jours de temps processeur cumulé, vous recevez un avertissement de dépassement arithmétique. Dans ce cas, la valeur des variables @@CPU_BUSY, @@IO_BUSY et @@IDLE n’est pas précise.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
@@IDLE  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **integer**  
  
## <a name="remarks"></a>Remarques  
 Pour afficher un rapport contenant plusieurs statistiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez **sp_monitor**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre le nombre de millisecondes pendant lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est resté inactif entre l'heure de début et l'heure actuelle. Pour éviter un dépassement arithmétique lors de la conversion de la valeur en microsecondes, l'exemple convertit l'une des valeurs en type de données `float`.  
  
```sql  
SELECT @@IDLE * CAST(@@TIMETICKS AS float) AS 'Idle microseconds',  
   GETDATE() AS 'as of';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
I  
Idle microseconds  as of                   
----------------- ----------------------  
8199934           12/5/2006 10:23:00 AM   
```  
  
## <a name="see-also"></a>Voir aussi  
 [@@CPU_BUSY &#40;Transact-SQL&#41;](../../t-sql/functions/cpu-busy-transact-sql.md)   
 [sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [@@IO_BUSY &#40;Transact-SQL&#41;](../../t-sql/functions/io-busy-transact-sql.md)   
 [Fonctions statistiques système &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
