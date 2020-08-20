---
description: sp_replcounters (Transact-SQL)
title: sp_replcounters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_replcounters
- sp_replcounters_TSQL
helpviewer_keywords:
- sp_replcounters
ms.assetid: fe585b1f-edda-421f-81d6-8a03a3a535d2
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: c4646b4062a0557a4c0b8a3086eef10f3e1a38a2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88485732"
---
# <a name="sp_replcounters-transact-sql"></a>sp_replcounters (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Renvoie les statistiques de réplication relatives à la latence, au débit de traitement et au nombre de transactions pour chaque base de données publiée. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replcounters  
  
```  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**Sauvegarde de la base de données**|**sysname**|Nom de la base de données.|  
|**Replicated transactions**|**int**|Nombre de transactions qui attendent dans le journal d'être remises à la base de données de distribution.|  
|**Replication rate trans/sec**|**float**|Nombre moyen de transactions transmises par seconde à la base de données de distribution.|  
|**Latence de la réplication**|**float**|Temps moyen (en secondes) que les transactions restent dans le journal avant d'être distribuées.|  
|**Replbeginlsn**|**binary(10)**|Numéro séquentiel dans le journal (LSN) du point de troncature courant dans le journal.|  
|**Replnextlsn**|**binary(10)**|Numéro de séquence d'enregistrement (LSN) de l'enregistrement validé suivant en attente de remise à la base de données de distribution.|  
  
## <a name="remarks"></a>Notes  
 **sp_replcounters** est utilisé dans la réplication transactionnelle.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** ou au rôle serveur fixe **sysadmin** .  
  
## <a name="see-also"></a>Voir aussi  
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
