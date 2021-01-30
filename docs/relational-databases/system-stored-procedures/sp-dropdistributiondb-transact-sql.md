---
title: sp_dropdistributiondb (Transact-SQL) | Microsoft Docs
description: Supprime une base de données de distribution et les fichiers qu’elle utilise s’ils ne sont pas utilisés par une autre base de données. Cette procédure stockée s’exécute sur le serveur de distribution sur n’importe quelle base de données.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistributiondb_TSQL
- sp_dropdistributiondb
helpviewer_keywords:
- sp_dropdistributiondb
ms.assetid: b6dd1846-2259-4d29-93af-a70a5d25a0c5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2de544361c4bcd9befb80e5172e0b4887a984a9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191198"
---
# <a name="sp_dropdistributiondb-transact-sql"></a>sp_dropdistributiondb (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Supprime une base de données de distribution. Supprime les fichiers physiques utilisés par la base de données s'ils ne sont pas utilisés par une autre base de données. Cette procédure stockée est exécutée sur n’importe quelle base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropdistributiondb [ @database= ] 'database'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @database = ] 'database'` Base de données à supprimer. *Database est de* **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropdistributiondb** est utilisé dans tous les types de réplications.  
  
 Cette procédure stockée doit être exécutée avant de supprimer le serveur de distribution en exécutant **sp_dropdistributor**.  
  
 **sp_dropdistributiondb** supprime également un travail agent de lecture de la file d’attente pour la base de données de distribution, le cas échéant.  
  
 Pour désactiver la distribution, la base de données remplissant cette fonction doit se trouver en ligne. Également, si un instantané existe pour la base de données de distribution, elle doit être supprimée avant la désactivation de la distribution. Un instantané de base de données est une copie en lecture seule hors ligne d'une base de données et n'est pas lié à un instantané de réplication. Pour plus d’informations, consultez [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistributiondb-tr_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_dropdistributiondb**.  
  
## <a name="see-also"></a>Voir aussi  
 [Désactiver la publication et la distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)   
 [sp_changedistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_helpdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
