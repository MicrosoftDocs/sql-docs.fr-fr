---
description: sp_addmergepartition (Transact-SQL)
title: sp_addmergepartition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepartition
- sp_addmergepartition_TSQL
helpviewer_keywords:
- sp_addmergepartition
ms.assetid: 02a5f46b-e5ff-4932-a3ff-7f0fd82d0981
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5129bcd9ee63b0c1e074294c8b69f997432240f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192328"
---
# <a name="sp_addmergepartition-transact-sql"></a>sp_addmergepartition (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée une partition filtrée dynamiquement pour un abonnement qui est filtrée par les valeurs de [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) ou [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) au niveau de l’abonné. Cette procédure stockée, exécutée sur le serveur de publication dans la base de données en cours de publication, est utilisée pour générer manuellement des partitions.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergepartition [ @publication = ] 'publication'  
        , [ @suser_sname = ] 'suser_sname'  
        , [ @host_name = ] 'host_name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Publication de fusion sur laquelle la partition est créée. *publication* est de **type sysname**, sans valeur par défaut. Si *SUSER_SNAME* est spécifié, la valeur du *nom d’hôte* doit être null.  
  
`[ @suser_sname = ] 'suser_sname'` Valeur utilisée lors de la création de la partition pour un abonnement filtré par la valeur de la fonction [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) sur l’abonné. *SUSER_SNAME* est de **type sysname**, sans valeur par défaut.  
  
`[ @host_name = ] 'host_name'` Valeur utilisée lors de la création de la partition pour un abonnement filtré par la valeur de la fonction [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) sur l’abonné. *HOST_NAME* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergepartition** est utilisé dans la réplication de fusion.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-addmergepartition-tra_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addmergepartition**.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un instantané pour une publication de fusion avec des filtres paramétrés](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [Filtres de lignes paramétrés](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
