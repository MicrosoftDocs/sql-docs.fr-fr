---
title: sys.fn_PageResCracker (Transact-SQL) | Microsoft Docs
description: En savoir plus sur la fonction système sys.fn_PageResCracker. Consultez des exemples et affichez des ressources disponibles supplémentaires.
ms.custom: ''
ms.date: 09/18/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_PageResCracker
- sys.fn_PageResCracker_TSQL
- fn_PageResCracker_TSQL
- sys.fn_PageResCracker
- sys.dm_db_page_info
dev_langs:
- TSQL
helpviewer_keywords:
- fn_PageResCracker function
- page_resource
- sys.fn_PageResCracker function
- sys.dm_db_page_info
- page info
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: a60b83d3b494ba705399effd9412ef0549f2f3e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198685"
---
# <a name="sysfn_pagerescracker-transact-sql"></a>sys.fn_PageResCracker (Transact-SQL)
[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Retourne `db_id` , `file_id` et `page_id` pour la `page_resource` valeur donnée. 
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
```  
sys.fn_PageResCracker ( page_resource )  
```  
  
## <a name="arguments"></a>Arguments  
*page_resource*    
Format hexadécimal sur 8 octets d’une ressource de page de base de données.
  
## <a name="tables-returned"></a>Tables retournées  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|db_id|**int**|ID de base de données|  
|file_id|**int**|ID de fichier|  
|page_id|**int**|ID de page|  
  
## <a name="remarks"></a>Notes  
`sys.fn_PageResCracker` est utilisé pour convertir la représentation hexadécimale sur 8 octets d’une page de base de données en un ensemble de lignes qui contient l’ID de base de données, l’ID de fichier et l’ID de page de la page.   

Vous pouvez obtenir une ressource de page valide à partir de la `page_resource` colonne de la [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) vue de gestion dynamique ou des [ processussys.sys&#40;vue système Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md) . Si une ressource de page non valide est utilisée, le retour est NULL.  
L’utilisation principale de `sys.fn_PageResCracker` a pour but de faciliter les jointures entre ces vues et le [sys.dm_db_page_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) fonction de gestion dynamique pour obtenir des informations sur la page, telles que l’objet auquel elle appartient.
  
## <a name="permissions"></a>Autorisations  
L’utilisateur a besoin `VIEW SERVER STATE` d’une autorisation sur le serveur.  
  
## <a name="examples"></a>Exemples  
La `sys.fn_PageResCracker` fonction peut être utilisée conjointement avec [sys.dm_db_page_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) pour résoudre les attentes et les blocages liés aux pages dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  Le script suivant est un exemple de la façon dont vous pouvez utiliser ces fonctions pour collecter des informations de page de base de données pour toutes les demandes actives qui attendent actuellement un type de ressource de page. 
  
```sql  
SELECT page_info.* 
FROM sys.dm_exec_requests AS d  
CROSS APPLY sys.fn_PageResCracker (d.page_resource) AS r  
CROSS APPLY sys.dm_db_page_info(r.db_id, r.file_id, r.page_id, 1) AS page_info
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_db_page_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)  
 [ Processus desys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
