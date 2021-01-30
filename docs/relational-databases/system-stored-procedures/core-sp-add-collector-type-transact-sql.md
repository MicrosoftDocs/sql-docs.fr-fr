---
description: core.sp_add_collector_type (Transact-SQL)
title: core.sp_add_collector_type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_collector_type
- sp_add_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- core.sp_add_collector_type stored procedure
- management data warehouse, data collector stored procedures
- sp_add_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 1d981037-2147-464e-a456-7d8e479bce89
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0db58f15641ab57d7eb35782c62334106a4bcdf7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181327"
---
# <a name="coresp_add_collector_type-transact-sql"></a>core.sp_add_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute une nouvelle entrée à la vue core.supported_collector_types de la base de données de l'entrepôt de données de gestion. La procédure doit être exécutée dans le contexte de la base de données d'entrepôt de données de gestion.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
core.sp_add_collector_type [ @collector_type_uid = ] 'collector_type_uid'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @collector_type_uid =] '*collector_type_uid*'  
 GUID pour le type de collecteur. *collector_type_uid* est de type **uniqueidentifier**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **mdw_admin** (avec autorisation EXECUTE).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ajoute le type de collecteur Requête T-SQL générique à la vue core.supported_collector_types. Par défaut, le type de collecteur Requête T-SQL générique existe déjà. Par conséquent, si vous exécutez ce code sur une installation par défaut, vous recevrez un message indiquant que le type de collecteur existe déjà.  
  
 Ce code s'exécuterait correctement si vous aviez supprimé le type de collecteur Requête T-SQL générique à l'aide de la procédure stockée core.sp_remove_collector_type et si vous aviez voulu ensuite l'ajouter à nouveau en tant que type de collecteur inscrit capable de télécharger des données dans l'entrepôt de données de gestion.  
  
```  
USE <management_data_warehouse>;  
GO  
DECLARE @RC int;  
DECLARE @collector_type_uid uniqueidentifier;  
SELECT @collector_type_uid = (SELECT collector_type_uid FROM msdb.dbo.syscollector_collector_types WHERE name = N'Generic T-SQL Query Collector Type');  
EXECUTE @RC = core.sp_add_collector_type @collector_type_uid;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Entrepôt de données de gestion](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
