---
description: sys.fn_virtualservernodes (Transact-SQL)
title: sys.fn_virtualservernodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_virtualservernodes
- fn_virtualservernodes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- nodes [Faillover Clustering], virtual servers
- nodes [Faillover Clustering]
- virtual servers [Faillover Clustering]
- failover clustering [SQL Server], nodes
- fn_virtualservernodes function
- sys.fn_virtualservernodes function
ms.assetid: 257f3b8d-93c0-4444-87f1-ea211bd8cad0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1623ad4fe9de10eb8ad896b62a7fc698df2f9f0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187386"
---
# <a name="sysfn_virtualservernodes-transact-sql"></a>sys.fn_virtualservernodes (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Retourne la liste des nœuds d'instance de cluster de basculement sur lesquels une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être exécutée. Ce type d'informations est utile dans les environnements de clustering de basculement.  
  
> [!IMPORTANT]
>  Cette [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] fonction système est incluse à des fins de compatibilité descendante. Nous vous recommandons d’utiliser à la place [sys.dm_os_cluster_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_virtualservernodes()  
```  
  
## <a name="tables-returned"></a>Tables retournées  
 Si le serveur actuel est un serveur en cluster, **fn_virtualservernodes** retourne une liste de nœuds d’instance de cluster de basculement sur lesquels cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été définie.  
  
 Si l’instance de serveur actuelle n’est pas un serveur en cluster, **fn_virtualservernodes** retourne un ensemble de lignes vide.  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit disposer de l’autorisation VIEW SERVER STATE pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise `fn_virtualservernodes` pour effectuer une requête sur une instance de serveur cluster :  
  
```  
SELECT * FROM fn_virtualservernodes();  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 NodeName  
  
 -------\-  
  
 SS3-CLUSN1  
  
 SS3-CLUSN2  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_os_cluster_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.fn_servershareddrives &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-servershareddrives-transact-sql.md)  
  
  
