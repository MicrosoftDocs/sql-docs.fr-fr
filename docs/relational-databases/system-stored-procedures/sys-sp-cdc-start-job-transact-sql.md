---
description: sys.sp_cdc_start_job (Transact-SQL)
title: sys.sp_cdc_start_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_start_job
- sp_cdc_start_job_TSQL
- sys.sp_cdc_start_job_TSQL
- sys.sp_cdc_start_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_start_job
ms.assetid: cf443a67-7705-4799-9f39-0e3a6a8a0708
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 336d71974ee9294ec3a23c245f68eb547c163f4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159753"
---
# <a name="syssp_cdc_start_job-transact-sql"></a>sys.sp_cdc_start_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Démarre un nettoyage de capture de données modifiées ou un travail de capture pour la base de données actuelle.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_start_job [ [ @job_type = ] 'job_type' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ [ @job_type = ] 'job_type' ]` Type de travail à ajouter. *job_type* est de type **nvarchar (20)** avec **capture** comme valeur par défaut. Les entrées valides sont **capture** et **Cleanup**.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 sys.sp_cdc_start_job peut être utilisée par un administrateur pour démarrer de manière explicite le travail de capture ou le travail de nettoyage.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-starting-a-capture-job"></a>R. Démarrage d'un travail de capture  
 L'exemple suivant démarre le travail de capture pour la base de données `AdventureWorks2012`. La spécification d’une valeur pour *job_type* n’est pas nécessaire, car le type de travail par défaut est **capture**.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_start_job;  
GO  
```  
  
### <a name="b-starting-a-cleanup-job"></a>B. Démarrage d'un travail de nettoyage  
 L'exemple suivant démarre un travail de nettoyage pour la base de données `AdventureWorks2012`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_start_job @job_type = N'cleanup';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_stop_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-stop-job-transact-sql.md)  
  
  
