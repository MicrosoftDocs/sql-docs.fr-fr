---
description: core.sp_update_data_source (Transact-SQL)
title: core.sp_update_data_source (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_data_source
- sp_update_data_source_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_data_source
- management data warehouse, data collector stored procedures
- core.sp_update_data_source stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 66b95f96-6df7-4657-9b3c-86a58c788ca5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45bfb2c042ea491516aa4d3caadff72c45451202
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180592"
---
# <a name="coresp_update_data_source-transact-sql"></a>core.sp_update_data_source (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour une ligne existante ou insère une nouvelle ligne dans la table core.source_info_internal de l'entrepôt de données de gestion. Cette procédure est appelée par le composant runtime du collecteur de données chaque fois qu'un package de téléchargement commence à télécharger des données dans l'entrepôt de données de gestion.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
core.sp_update_data_source [ @collection_set_uid = ] 'collection_set_uid'  
    ,[ @machine_name = ] 'machine_name'  
    , [ @named_instance = ] 'named_instance'  
    , [ @days_until_expiration = ] days_until_expiration  
    , [ @source_id = ] source_id OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
 [ @collection_set_uid =] '*collection_set_uid*'  
 GUID du jeu d'éléments de collecte. *collection_set_uid* est de type **uniqueidentifier**, sans valeur par défaut. Pour obtenir le GUID, interrogez la vue dbo.syscollector_collection_sets dans la base de données msdb.  
  
 [ @machine_name =] '*machine_name*'  
 Nom du serveur sur lequel réside le jeu d'éléments de collecte. *machine_name* est de **type sysname** et n’a pas de valeur par défaut.  
  
 [ @named_instance =] '*named_instance*'  
 Nom de l'instance pour le jeu d'éléments de collecte. *named_instance* est de **type sysname**, sans valeur par défaut.  
  
> [!NOTE]  
>  *named_instance* doit être le nom complet de l’instance, qui se compose du nom de l’ordinateur et du nom de l’instance, sous la forme *ComputerName* \\ *nom_instance*.  
  
 [ @days_until_expiration =] *days_until_expiration*  
 Nombre de jours restants dans la période de rétention des données d'instantanés. *days_until_expiration* est de type **smallint**.  
  
 [ @source_id =] *source_id*  
 Identificateur unique de la source de la mise à jour. *source_id* est de **type int** et est retourné en tant que sortie.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Chaque fois qu'un package de téléchargement commence à télécharger des données dans l'entrepôt de données de gestion, le composant runtime du collecteur de données appelle core.sp_update_data_source. La table core.source_info_internal est mise à jour si l'une des modifications suivantes a été apportée depuis le dernier téléchargement :  
  
-   Un nouveau jeu d'éléments de collecte a été ajouté.  
  
-   La valeur de days_until_expiration a changé.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **mdw_writer** (avec autorisation EXECUTE).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant met à jour la source de données (dans le cas présent, le jeu d'éléments de collecte Utilisation du disque), définit le nombre de jours avant l'expiration et retourne l'identificateur de la source. Dans l'exemple, l'instance par défaut est utilisée.  
  
```  
USE <management_data_warehouse>;  
GO  
DECLARE @source_id int;  
EXEC core.sp_update_data_source   
@collection_set_uid = '7B191952-8ECF-4E12-AEB2-EF646EF79FEF',   
@machine_name = '<computername>',  
@named_instance = 'MSSQLSERVER',  
@days_until_expiration = 10,  
@source_id = @source_id OUTPUT;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Entrepôt de données de gestion](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
