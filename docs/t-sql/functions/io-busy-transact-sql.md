---
description: '&#x40;&#x40;IO_BUSY (Transact-SQL)'
title: '@@IO_BUSY (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@IO_BUSY'
- '@@IO_BUSY_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- ticks [SQL Server]
- I/O [SQL Server], time spent performing operations
- '@@IO_BUSY function'
- output operations [SQL Server]
- input operations [SQL Server]
- time [SQL Server], I/O operations
ms.assetid: 3c26770c-41ae-4e34-8c82-7bef920ffbca
author: cawrites
ms.author: chadam
ms.openlocfilehash: dbcd33b3ccc8079a8bf6c23aab582d38b3bf66e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102560"
---
# <a name="x40x40io_busy-transact-sql"></a>&#x40;&#x40;IO_BUSY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne le temps que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a consacré aux opérations d'entrée et de sortie depuis le dernier démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les résultats sont exprimés en incréments de temps processeur (« cycles ») et cumulés pour tous les processeurs, aussi peuvent-ils dépasser le temps réel écoulé. Pour convertir cette valeur en microsecondes, multipliez-la par @@TIMETICKS.  
  
> [!NOTE]  
>  Si le temps renvoyé dans @@CPU_BUSY ou @@IO_BUSY dépasse approximativement 49 jours de temps processeur cumulé, vous recevez un avertissement de dépassement arithmétique. Dans ce cas, la valeur des variables @@CPU_BUSY, @@IO_BUSY et @@IDLE n’est pas précise.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
@@IO_BUSY  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **integer**  
  
## <a name="remarks"></a>Remarques  
 Pour afficher un état contenant plusieurs statistiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez sp_monitor.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne le nombre de millisecondes pendant lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a effectué des opérations d'entrée/sortie entre l'heure de début et l'heure actuelle. Pour éviter un dépassement arithmétique lors de la conversion de la valeur en microsecondes, l’exemple convertit l’une des valeurs en type de données **float**.  
  
```sql  
SELECT @@IO_BUSY*@@TIMETICKS AS 'IO microseconds',   
   GETDATE() AS 'as of';  
```  
  
 Jeu de résultats généralement obtenu :  
  
```  
  
IO microseconds as of                   
--------------- ----------------------  
4552312500      12/5/2006 10:23:00 AM   
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [@@CPU_BUSY &#40;Transact-SQL&#41;](../../t-sql/functions/cpu-busy-transact-sql.md)   
 [sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [Fonctions statistiques système &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
