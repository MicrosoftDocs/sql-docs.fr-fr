---
description: sp_helparticledts (Transact-SQL)
title: sp_helparticledts (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helparticledts
- sp_helparticledts_TSQL
helpviewer_keywords:
- sp_helparticledts
ms.assetid: cd1aed60-e056-4ff3-86ee-62b19433d890
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e819ecd2e452f4a8a1be8e8199df6f9a6cf753d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176471"
---
# <a name="sp_helparticledts-transact-sql"></a>sp_helparticledts (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Permet d'obtenir des informations sur les noms de tâches personnalisés corrects à utiliser lors de la création d'un abonnement de transformation avec [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helparticledts [ @publication = ] 'publication', [ @article = ] 'article'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom d’un article de la publication. *article* est de **type sysname** et n’a pas de valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pre_script_ignore_error_task_name**|**sysname**|Nom de la tâche de programmation intervenant avant la copie des données d'instantané. L'exécution du programme doit se poursuivre si une erreur de script est détectée.|  
|**pre_script_task_name**|**sysname**|Nom de la tâche de programmation intervenant avant la copie des données d'instantané. L'exécution du programme s'arrête si une erreur survient.|  
|**transformation_task_name**|**sysname**|Nom de la tâche de programmation lors de l'utilisation d'une tâche Requête contrôlée par les données.|  
|**post_script_ignore_error_task_name**|**sysname**|Nom de la tâche de programmation intervenant après la copie des données d'instantané. L'exécution du programme doit se poursuivre si une erreur de script est détectée.|  
|**post_script_task_name**|**sysname**|Nom de la tâche de programmation intervenant après la copie des données d'instantané. L'exécution du programme s'arrête si une erreur survient.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helparticledts** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
 Il existe des conventions de nom, requises par les agents de réplication, qui doivent être respectées lors de l'affectation de noms aux tâches dans un programme DTS (Data Transformation Services) de réplication. Dans le cas des tâches personnalisées, telles qu'une tâche d'exécution de requête SQL, le nom est une chaîne concaténée comprenant le nom de l'article, un préfixe et une partie facultative. Lors de l'écriture du code, si vous ne connaissez pas les noms de tâche à utiliser, vous pouvez les trouver dans l'ensemble de résultats.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** et du rôle de base de données fixe **db_owner** peuvent exécuter **sp_helparticledts**.  
  
  
