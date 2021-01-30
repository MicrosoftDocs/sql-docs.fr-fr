---
description: core.sp_purge_data (Transact-SQL)
title: core.sp_purge_data (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_purge_data_TSQL
- sp_purge_data
dev_langs:
- TSQL
helpviewer_keywords:
- sp_purge_data
- management data warehouse, data collector stored procedures
- core.sp_purge_data stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 056076c3-8adf-4f51-8a1b-ca39696ac390
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a4f198f3aa51d42b3dbc22d040fe9b3325f4361f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190053"
---
# <a name="coresp_purge_data-transact-sql"></a>core.sp_purge_data (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime des données de l'entrepôt de données de gestion en fonction d'une stratégie de rétention. Cette procédure est exécutée quotidiennement par le travail de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mdw_purge_data contre l'entrepôt de données de gestion associé à l'instance spécifiée. Vous pouvez utiliser cette procédure stockée pour effectuer une suppression à la demande des données dans l'entrepôt de données de gestion.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
core.sp_purge_data  
    [ [ @retention_days = ] retention_days ]  
    [ , [ @instance_name = ] 'instance_name' ]  
    [ , [ @collection_set_uid = ] 'collection_set_uid' ]  
    [ , [ @duration = ] duration ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @retention_days =] *retention_days*  
 Nombre de jours pendant lesquels conserver des données dans les tables de l'entrepôt de données de gestion. Les données avec un horodatage antérieur à *retention_days* sont supprimées. *retention_days* est de type **smallint**, avec NULL comme valeur par défaut. Si elle est spécifiée, la valeur doit être positive. Lorsqu'elle est NULL, la valeur dans la colonne valid_through de la vue core.snapshots détermine les lignes qui sont éligibles pour la suppression.  
  
 [ @instance_name =] '*instance_name*'  
 Nom de l'instance pour le jeu d'éléments de collecte. *instance_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 *instance_name* doit être le nom complet de l’instance, qui se compose du nom de l’ordinateur et du nom de l’instance, sous la forme *ComputerName* \\ *nom_instance*. Lorsque la valeur est NULL, l'instance par défaut sur le serveur local est utilisée.  
  
 [ @collection_set_uid =] '*collection_set_uid*'  
 GUID du jeu d'éléments de collecte. *collection_set_uid* est de type **uniqueidentifier**, avec NULL comme valeur par défaut. Lorsque la valeur est NULL, les lignes éligibles de tous les jeux d'éléments de collecte sont supprimées. Pour obtenir cette valeur, interrogez l'affichage catalogue syscollector_collection_sets.  
  
 [ @duration =] *durée*  
 Nombre maximal de minutes pendant lesquelles l'opération de vidage doit s'exécuter. *Duration* est de type **smallint**, avec NULL comme valeur par défaut. Si elle est spécifiée, la valeur doit être zéro ou un entier positif. Lorsque NULL, l'opération s'exécute jusqu'à ce que toutes les lignes éligibles soient supprimées ou que l'opération soit arrêtée manuellement.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Cette procédure sélectionne des lignes dans la vue core.snapshots qui sont éligibles pour la suppression en fonction d'une période de rétention. Toutes les lignes éligibles pour la suppression sont supprimées de la table core.snapshots_internal. La suppression des lignes précédentes déclenche une action de suppression en cascade dans toutes les tables de l'entrepôt de données de gestion. Cette opération est effectuée en utilisant la clause ON DELETE CASCADE, qui est définie pour toutes les tables stockant des données collectées.  
  
 Chaque instantané et ses données associées sont supprimés dans une transaction explicite, puis validés. Par conséquent, si l’opération de vidage est arrêtée manuellement ou si la valeur spécifiée pour @duration est dépassée, seules les données non validées sont conservées. Ces données peuvent être supprimées lors de la prochaine exécution du travail.  
  
 La procédure doit être exécutée dans le contexte de la base de données d'entrepôt de données de gestion.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **mdw_admin** (avec autorisation EXECUTE).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-running-sp_purge_data-with-no-parameters"></a>R. Exécution de sp_purge_data sans paramètres  
 L'exemple suivant exécute core.sp_purge_data sans spécifier de paramètres. Par conséquent, la valeur par défaut NULL est utilisée pour tous les paramètres, avec le comportement associé.  
  
```  
USE <management_data_warehouse>;  
EXECUTE core.sp_purge_data;  
GO  
```  
  
### <a name="b-specifying-retention-and-duration-values"></a>B. Spécification de valeurs de rétention et de durée  
 L'exemple suivant supprime les données de l'entrepôt de données de gestion antérieures à 7 jours. En outre, le @duration paramètre est spécifié afin que l’opération ne s’exécute pas plus de 5 minutes.  
  
```  
USE <management_data_warehouse>;  
EXECUTE core.sp_purge_data @retention_days = 7, @duration = 5;  
GO  
```  
  
### <a name="c-specifying-an-instance-name-and-collection-set"></a>C. Spécification d'un nom d'instance et d'un jeu d'éléments de collecte  
 L'exemple suivant supprime des données de l'entrepôt de données de gestion pour un jeu d'éléments de collecte donné sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Étant donné que @retention_days n’est pas spécifié, la valeur dans la colonne valid_through de la vue Core. Snapshots est utilisée pour déterminer les lignes du jeu d’collections pouvant être supprimées.  
  
```  
USE <management_data_warehouse>;  
GO  
-- Get the collection set unique identifier for the Disk Usage system collection set.  
DECLARE @disk_usage_collection_set_uid uniqueidentifier = (SELECT collection_set_uid   
    FROM msdb.dbo.syscollector_collection_sets WHERE name = N'Disk Usage');   
  
EXECUTE core.sp_purge_data @instance_name = @@SERVERNAME, @collection_set_uid = @disk_usage_collection_set_uid;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)  
  
  
