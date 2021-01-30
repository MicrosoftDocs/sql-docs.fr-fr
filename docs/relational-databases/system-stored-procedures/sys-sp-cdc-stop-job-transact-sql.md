---
description: sys.sp_cdc_stop_job (Transact-SQL)
title: sys.sp_cdc_stop_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_stop_job_TSQL
- sys.sp_cdc_stop_job
- sp_cdc_stop_job_TSQL
- sp_cdc_stop_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_stop_job
ms.assetid: 421fc21c-c7a4-407c-8b31-359273b68c63
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c2a738af88e947528f4c8d2cadf871db7cd8334a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159748"
---
# <a name="syssp_cdc_stop_job-transact-sql"></a>sys.sp_cdc_stop_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Arrête un nettoyage de capture de données modifiées ou un travail de capture pour la base de données actuelle.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_stop_job [ [ @job_type = ] 'job_type' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ [ @job_type = ] 'job_type_' ]` Type de travail à ajouter. *job_type* est de type **nvarchar (20)** avec **capture** comme valeur par défaut. Les entrées valides sont **capture** et **Cleanup**.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 sys.sp_cdc_stop_job peut être utilisée par un administrateur pour arrêter de manière explicite le travail de capture ou le travail de nettoyage.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant arrête le travail de nettoyage pour la base de données `AdventureWorks2012`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_stop_job @job_type = N'capture';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-start-job-transact-sql.md)  
  
  
