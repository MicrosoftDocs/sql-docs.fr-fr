---
description: sp_monitor (Transact-SQL)
title: sp_monitor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_monitor_TSQL
- sp_monitor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_monitor
ms.assetid: cb628496-2f9b-40e4-b018-d0831c4cb018
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f046c3e6fe81c7dac489fcdd88237ecb2d3747d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195468"
---
# <a name="sp_monitor-transact-sql"></a>sp_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche des statistiques sur [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_monitor  
```  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|**last_run**|Heure de la dernière exécution de **sp_monitor** .|  
|**current_run**|L’heure de **sp_monitor** est en cours d’exécution.|  
|**secondes**|Nombre de secondes écoulées depuis l’exécution de **sp_monitor** .|  
|**cpu_busy**|Nombre de secondes que l'UC de l'ordinateur serveur a consacrées à des tâches [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**io_busy**|Nombre de secondes que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a consacrées aux opérations d'entrée et de sortie.|  
|**périodes**|Nombre de secondes pendant lesquelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été inactif.|  
|**packets_received**|Nombre de paquets entrants lus par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**packets_sent**|Nombre de paquets sortants écrits par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**packet_errors**|Nombre d'erreurs détectées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la lecture ou de l'écriture des paquets.|  
|**total_read**|Nombre de lectures effectuées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**total_write**|Nombre d'écritures effectuées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**total_errors**|Nombre d'erreurs détectées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors des opérations de lecture et d'écriture.|  
|**connexions**|Nombre de connexions ou de tentatives de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="remarks"></a>Notes  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assure le suivi, par le biais d'une série de fonctions, du volume de son activité. L’exécution de **sp_monitor** affiche les valeurs actuelles retournées par ces fonctions et indique leur nombre modifié depuis la dernière exécution de la procédure.  
  
 Pour chaque colonne, les statistiques sont imprimées au format *Number*(*Number*)-*Number*% ou *Number*(*Number*). Le premier *nombre* fait référence au nombre de secondes (pour **cpu_busy**, **io_busy** et **inactif**) ou au nombre total (pour les autres variables) depuis le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] redémarrage de. Le *nombre* entre parenthèses fait référence au nombre de secondes ou au nombre total depuis la dernière exécution de **sp_monitor** . Le pourcentage est le pourcentage de temps écoulé depuis la dernière exécution de **sp_monitor** . Par exemple, si le rapport indique **cpu_busy** en tant que 4250 (215)-68%, le processeur est occupé 4250 secondes depuis [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le dernier démarrage de, 215 secondes depuis la dernière exécution de **sp_monitor** et 68% du temps total depuis la dernière exécution de **sp_monitor** .  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant fournit des informations sur le niveau d'activité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```console
USE master  
EXEC sp_monitor  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```console
last_run       current_run                   seconds
-----------    --------------------------    ---------
Mar 29 1998    11:55AM Apr 4 1998 2:22 PM    561

cpu_busy           io_busy     idle
---------------    ---------   --------------
190(0)-0%          187(0)-0%   148(556)-99%

packets_received       packets_sent    packet_errors
----------------       ------------    -------------
16(1)                  20(2)           0(0)

total_read     total_write   total_errors    connections
-----------    -----------   -------------   -----------
141(0)         54920(127)    0(0)            4(0)
```
  
## <a name="see-also"></a>Voir aussi  
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
