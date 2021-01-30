---
description: sp_addtabletocontents (Transact-SQL)
title: sp_addtabletocontents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addtabletocontents_TSQL
- sp_addtabletocontents
helpviewer_keywords:
- sp_addtabletocontents
ms.assetid: 2ea27001-74f4-463e-bf1b-b6b5a86b9219
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 469d9255813e1a072539e6aa8ca7035378ce2b92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198408"
---
# <a name="sp_addtabletocontents-transact-sql"></a>sp_addtabletocontents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Insère des références dans les tables de suivi de fusion pour toutes les lignes d'une table source qui ne sont pas actuellement incluses dans les tables de suivi. Utilisez cette option si vous avez chargé en masse une grande quantité de données à l’aide de **BCP**, ce qui n’entraîne pas l’activation des déclencheurs de suivi de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addtabletocontents [ @table_name = ] 'table_name'  
    [ , [ @owner_name = ] 'owner_name' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table_name = ] 'table_name'` Nom de la table. *table_name* est de **type sysname**, sans valeur par défaut.  
  
`[ @owner_name = ] 'owner_name'` Nom du propriétaire de la table. *owner_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @filter_clause = ] 'filter_clause'` Spécifie une clause de filtre qui contrôle les lignes des données récemment chargées à ajouter aux tables de suivi de fusion. *filter_clause* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut. Si *filter_clause* a la **valeur null**, toutes les lignes chargées en bloc sont ajoutées.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addtabletocontents** est utilisé uniquement dans la réplication de fusion.  
  
 Les lignes de la *table_name* sont référencées par leur **ROWGUIDCOL** et les références sont ajoutées aux tables de suivi de fusion. **sp_addtabletocontents** doit être utilisé après la copie en bloc de données dans une table qui est publiée à l’aide de la réplication de fusion. La procédure stockée commence le suivi des lignes qui ont été copiées et garantit que les nouvelles lignes seront incluses lors de la prochaine synchronisation.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addtabletocontents**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
