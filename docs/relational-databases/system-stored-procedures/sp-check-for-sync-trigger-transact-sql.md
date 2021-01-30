---
description: sp_check_for_sync_trigger (Transact-SQL)
title: sp_check_for_sync_trigger (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- filter_TSQL
- sp_check_for_sync_trigger
helpviewer_keywords:
- sp_check_for_sync_trigger
ms.assetid: 54a1e2fd-c40a-43d4-ac64-baed28ae4637
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cb15d74c1ac4845b7ff60bd12983b922008b27c8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206566"
---
# <a name="sp_check_for_sync_trigger-transact-sql"></a>sp_check_for_sync_trigger (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Détermine si un déclencheur ou une procédure stockée définis par l'utilisateur sont appelés dans le contexte d'un déclencheur de réplication utilisé pour les abonnements avec mise à jour immédiate. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication ou sur la base de données d’abonnement de l’abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_check_for_sync_trigger [ @tabid = ] 'tabid'   
    [ , [ @trigger_op = ] 'trigger_output_parameters' OUTPUT ]  
    [ , [ @fonpublisher = ] fonpublisher ]  
```  
  
## <a name="arguments"></a>Arguments  
 [**@tabid =** *] « n »*  
 ID d'objet de la table dans laquelle les déclencheurs de mise à jour immédiate sont recherchés. *est de* **type int** , sans valeur par défaut.  
  
 sortie de [**@trigger_op =** ] '*trigger_output_parameters*'  
 Spécifie si le paramètre de sortie doit renvoyer le type du déclencheur à partir duquel il est appelé. *trigger_output_parameters* est de **type char (10)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Ceux**|Déclencheur INSERT.|  
|**Upd**|Déclencheur UPDATE.|  
|**Suppr**|Déclencheur DELETE.|  
|NULL (par défaut)||  
  
`[ @fonpublisher = ] fonpublisher` Spécifie l’emplacement où la procédure stockée est exécutée. *fonpublisher* est de **bits**, avec 0 comme valeur par défaut. Si la valeur est 0, l'exécution s'effectue sur l'Abonné et, si la valeur est 1, l'exécution s'effectue sur le serveur de publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 indique que la procédure stockée n'est pas appelée dans le contexte d'un déclencheur de mise à jour immédiate. 1 indique qu’il est appelé dans le contexte d’un déclencheur de mise à jour immédiate et est le type de déclencheur retourné dans *\@ trigger_op*.  
  
## <a name="remarks"></a>Notes  
 **sp_check_for_sync_trigger** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
 **sp_check_for_sync_trigger** est utilisé pour coordonner la réplication et les déclencheurs définis par l’utilisateur. Cette procédure stockée détermine si elle est appelée dans le contexte d'un déclencheur de réplication. Par exemple, vous pouvez appeler la procédure **sp_check_for_sync_trigger** dans le corps d’un déclencheur défini par l’utilisateur. Si **sp_check_for_sync_trigger** retourne la **valeur 0**, le déclencheur défini par l’utilisateur continue le traitement. Si **sp_check_for_sync_trigger** retourne la valeur **1**, le déclencheur défini par l’utilisateur se termine. Vous êtes ainsi assuré que le déclencheur défini par l'utilisateur ne s'active pas lorsque le déclencheur de réplication met à jour la table.  
  
## <a name="examples"></a>Exemples

### <a name="a-add-code-to-a-trigger-on-a-subscriber-table"></a>R. Ajouter du code à un déclencheur sur une table d’abonné
 L'exemple suivant contient du code que vous pouvez utiliser dans un déclencheur appliqué à une table de l'Abonné.  
  
```  
DECLARE @retcode int, @trigger_op char(10), @table_id int  
SELECT @table_id = object_id('tablename')  
EXEC @retcode = sp_check_for_sync_trigger @table_id, @trigger_op OUTPUT  
IF @retcode = 1  
RETURN  
```  
  
### <a name="b-add-code-to-a-trigger-on-a-publisher-table"></a>B. Ajouter du code à un déclencheur sur une table de serveur de publication
 Le code peut également être ajouté à un déclencheur sur une table sur le serveur de publication ; le code est similaire, mais l’appel à **sp_check_for_sync_trigger** comprend un paramètre supplémentaire.  
  
```  
DECLARE @retcode int, @trigger_op char(10), @table_id int, @fonpublisher int  
SELECT @table_id = object_id('tablename')  
SELECT @fonpublisher = 1  
EXEC @retcode = sp_check_for_sync_trigger @table_id, @trigger_op OUTPUT, @fonpublisher  
IF @retcode = 1  
RETURN  
```  
  
## <a name="permissions"></a>Autorisations  
 **sp_check_for_sync_trigger** procédure stockée peut être exécutée par n’importe quel utilisateur disposant d’autorisations SELECT dans la vue système [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) .  
  
## <a name="see-also"></a>Voir aussi  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)  
  
  
