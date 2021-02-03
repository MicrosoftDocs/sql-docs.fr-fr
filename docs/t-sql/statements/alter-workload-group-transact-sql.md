---
description: ALTER WORKLOAD GROUP (Transact-SQL)
title: ALTER WORKLOAD GROUP (Transact-SQL)
ms.custom: ''
ms.date: 05/05/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_WORKLOAD_GROUP_TSQL
- ALTER WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER WORKLOAD GROUP statement
ms.assetid: 957addce-feb0-4e54-893e-5faca3cd184c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 8f387c9ab6d67f11211107b4e7c44d38d23702cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199378"
---
# <a name="alter-workload-group-transact-sql"></a>ALTER WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server et SQL Managed Instance

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server et SQL Managed Instance

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Modifie un groupe de charge de travail existant.

Pour plus d’informations sur la façon dont `ALTER WORKLOAD GROUP` se comporte sur un système avec des demandes en cours d’exécution et en file d’attente, consultez la section sur le comportement de `ALTER WORKLOAD GROUP` ci-dessous. 

Les restrictions en place pour [CREATE WORKLOAD GROUP](create-workload-group-transact-sql.md) s’appliquent également à `ALTER WORKLOAD GROUP`.  Avant de modifier des paramètres, interrogez [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md) pour vérifier que les valeurs se trouvent dans des plages acceptables.

## <a name="syntax"></a>Syntaxe

```syntaxsql
ALTER WORKLOAD GROUP group_name
 WITH
 ([       MIN_PERCENTAGE_RESOURCE = value ]
  [ [ , ] CAP_PERCENTAGE_RESOURCE = value ]
  [ [ , ] REQUEST_MIN_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ] 
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]
  ```

## <a name="arguments"></a>Arguments

group_name  
Nom du groupe de charge de travail défini par l’utilisateur existant en cours de modification.  group_name ne peut pas être modifié. 

MIN_PERCENTAGE_RESOURCE = valeur  
La valeur est une plage d’entiers comprise entre 0 et 100.  Lors de la modification de min_percentage_resource, la somme des valeurs min_percentage_resource de tous les groupes de charge de travail ne peut pas dépasser 100.  Quand vous modifiez min_percentage_resource, toutes les requêtes en cours d’exécution doivent être terminées dans le groupe de charge de travail pour que la commande aboutisse.  Pour plus d’informations, consultez la section sur le comportement de ALTER WORKLOAD GROUP dans ce document.

CAP_PERCENTAGE_RESOURCE = valeur  
La valeur est une plage d’entiers comprise entre 1 et 100.  La valeur de cap_percentage_resource ne peut pas être supérieure à min_percentage_resource.  Quand vous modifiez cap_percentage_resource, toutes les requêtes en cours d’exécution doivent être terminées dans le groupe de charge de travail pour que la commande aboutisse.  Pour plus d’informations, consultez la section sur le comportement de ALTER WORKLOAD GROUP dans ce document. 

REQUEST_MIN_RESOURCE_GRANT_PERCENT = valeur  
La valeur est un décimal compris entre 0,75 et 100,00.  La valeur de request_min_resource_grant_percent doit être un facteur de min_percentage_resource et être inférieure à cap_percentage_resource. 
  
REQUEST_MAX_RESOURCE_GRANT_PERCENT = valeur  
La valeur est un décimal et doit être supérieure à request_min_resource_grant_percent.

IMPORTANCE = { LOW \|  BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
Modifie l’importance par défaut d’une demande pour le groupe de charge de travail.

QUERY_EXECUTION_TIMEOUT_SEC = valeur  
Modifie la durée maximale (en secondes) pendant laquelle une requête peut s’exécuter avant d’être annulée. La valeur doit être égale à 0 ou être un entier positif. La valeur par défaut est 0, ce qui signifie qu’elle est illimitée.   

## <a name="permissions"></a>Autorisations

Exige l’autorisation CONTROL DATABASE

## <a name="example"></a>Exemple

L’exemple ci-dessous vérifie les valeurs de l’affichage catalogue pour wgDataLoads et change les valeurs.

```sql
SELECT *
FROM sys.workload_management_workload_groups  
WHERE [name] = 'wgDataLoads'

ALTER WORKLOAD GROUP wgDataLoads WITH
( MIN_PERCENTAGE_RESOURCE            = 40
 ,CAP_PERCENTAGE_RESOURCE            = 80
 ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 10 )
 ```

## <a name="alter-workload-group-behavior"></a>Comportement de ALTER WORKLOAD GROUP

À tout moment, le système compte 3 types de demandes.
- Les demandes qui n’ont pas encore été classifiées.
- Les demandes qui sont classifiées et en attente de verrous d’objets ou de ressources système.
- Les demandes qui sont classifiées et en cours d’exécution.

En fonction des propriétés d’un groupe de charge de travail en cours de modification, le moment auquel les paramètres prennent effet varie.

**Importance ou query_execution_timeout** Pour les propriétés importance et query_execution_timeout, les demandes non classifiées récupèrent les nouvelles valeurs de configuration.  Les demandes en attente et en cours d’exécution s’exécutent avec l’ancienne configuration.  La demande `ALTER WORKLOAD GROUP` s’exécute immédiatement, qu’il y ait ou non des requêtes en cours d’exécution dans le groupe de charge de travail.

**Request_min_resource_grant_percent ou request_max_resource_grant_percent** Pour request_min_resource_grant_percent et request_max_resource_grant_percent, les demandes en cours d’exécution s’exécutent avec l’ancienne configuration.  Les demandes en attente et celles non classifiées récupèrent les nouvelles valeurs de configuration.  La demande `ALTER WORKLOAD GROUP` s’exécute immédiatement, qu’il y ait ou non des requêtes en cours d’exécution dans le groupe de charge de travail.

**Min_percentage_resource ou cap_percentage_resource** Pour min_percentage_resource et cap_percentage_resource, les demandes en cours d’exécution s’exécutent avec l’ancienne configuration.  Les demandes en attente et celles non classifiées récupèrent les nouvelles valeurs de configuration. 

Le changement de min_percentage_resource et de cap_percentage_resource nécessite le drainage des demandes en cours d’exécution dans le groupe de charge de travail en cours de modification.  Si vous diminuez min_percentage_resource, les ressources libérées sont retournées au pool de partage et accessibles aux demandes d’autres groupes de charge de travail.  Inversement, l’augmentation de min_percentage_resource ne prend effet que lorsque les demandes utilisant uniquement les ressources nécessaires du pool partagé sont terminées.  L’opération ALTER WORKLOAD GROUP a un accès prioritaire aux ressources partagées par rapport aux autres demandes en attente d’exécution sur le pool partagé.  Si la somme de min_percentage_resource dépasse 100 %, la demande ALTER WORKLOAD GROUP échoue immédiatement. 

**Comportement de verrouillage** La modification d’un groupe de charge de travail nécessite un verrou global sur tous les groupes de charge de travail.  Une demande de modification d’un groupe de charge de travail est mise en file d’attente derrière les demandes de création ou de suppression de groupe de charge de travail déjà soumises.  Si plusieurs instructions ALTER sont soumises en même temps, elles sont traitées dans l’ordre de soumission.  

## <a name="see-also"></a>Voir aussi

- [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](create-workload-group-transact-sql.md)
- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](drop-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Démarrage rapide : Configurer l’isolation de la charge de travail à l’aide de T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
