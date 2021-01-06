---
description: CREATE WORKLOAD GROUP (Transact-SQL)
title: CREATE WORKLOAD GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/27/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD GROUP
- WORKLOAD_GROUP_TSQL
- CREATE WORKLOAD GROUP
- CREATE_WORKLOAD_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD GROUP statement
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 1ad781bd554d7bcf506371ab71e3441e38f91f8c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643966"
---
# <a name="create-workload-group-transact-sql"></a>CREATE WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server et SQL Managed Instance

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server et SQL Managed Instance

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Crée un groupe de charge de travail. Les groupes de charge de travail sont les conteneurs d’un ensemble de requêtes. Ils déterminent la façon dont la gestion des charges de travail est configurée sur un système. Les groupes de charge de travail permettent de réserver des ressources pour l’isolation de la charge de travail. Ils contiennent des ressources, définissent les ressources par requête et adhèrent aux règles d’exécution. Une fois l’instruction exécutée, les paramètres sont activés.

:::image type="icon" source="../../database-engine/configure-windows/media/topic-link.gif"::: [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

```syntaxsql
CREATE WORKLOAD GROUP group_name
 WITH
 (   MIN_PERCENTAGE_RESOURCE = value 
   , CAP_PERCENTAGE_RESOURCE = value 
   , REQUEST_MIN_RESOURCE_GRANT_PERCENT = value
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH } ]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]

```

*group_name*</br>
Spécifie le nom qui identifie le groupe de charge de travail. *group_name* est un sysname. Il peut comporter jusqu’à 128 caractères et doit être unique dans l’instance.

*MIN_PERCENTAGE_RESOURCE* = valeur</br>
Pour ce groupe de charge de travail, spécifie une allocation de ressources minimale garantie qui n’est pas partagée avec d’autres groupes de charge de travail. La *valeur* est une plage d’entiers comprise entre 0 et 100. La somme des min_percentage_resource de tous les groupes de charge de travail ne peut pas dépasser 100. La valeur de min_percentage_resource ne peut pas être supérieure à cap_percentage_resource. Il existe des valeurs minimales effectives autorisées pour chaque niveau de service. Pour plus d’informations, consultez [Valeurs effectives](#effective-values).

*CAP_PERCENTAGE_RESOURCE* = valeur</br>
Spécifie l’utilisation maximale des ressources pour toutes les requêtes d’un groupe de charge de travail. La plage d’entiers autorisée pour la valeur est comprise entre 1 et 100. La valeur de cap_percentage_resource ne peut pas être supérieure à min_percentage_resource. La valeur effective de cap_percentage_resource peut être réduite si min_percentage_resource est supérieur à zéro dans d’autres groupes de charge de travail.

*REQUEST_MIN_RESOURCE_GRANT_PERCENT* = valeur</br>
Définit le nombre minimal de ressources allouées par requête. La *valeur* est un paramètre obligatoire avec une plage décimale comprise entre 0,75 et 100,00. La valeur de request_min_resource_grant_percent doit être un multiple de 0,25, être un facteur de min_percentage_resource et être inférieur à cap_percentage_resource. Il existe des valeurs minimales effectives autorisées pour chaque niveau de service. Pour plus d’informations, consultez [Valeurs effectives](#effective-values).

Par exemple :

```sql
CREATE WORKLOAD GROUP wgSample 
WITH
  ( MIN_PERCENTAGE_RESOURCE = 26                -- integer value
    , REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3.25 -- factor of 26 (guaranteed a minimum of 8 concurrency)
    , CAP_PERCENTAGE_RESOURCE = 100 )
```

Aidez-vous des valeurs utilisées avec les classes de ressources pour request_min_resource_grant_percent.  Le tableau ci-dessous contient les allocations des ressources pour Gen2.

|Classe de ressource|Pourcentage de ressources|
|---|---|
|Smallrc|3 %|
|Mediumrc|10 %|
|Largerc|22 %|
|Xlargerc|70 %|
|||

*REQUEST_MAX_RESOURCE_GRANT_PERCENT* = valeur</br>         
Définit le nombre maximal de ressources allouées par requête. La *valeur* est un paramètre décimal facultatif avec une valeur par défaut égale à celle de request_min_resource_grant_percent. La *valeur* doit être supérieure ou égale à request_min_resource_grant_percent. Lorsque la valeur de request_max_resource_grant_percent est supérieure à request_min_resource_grant_percent et que des ressources système sont disponibles, des ressources supplémentaires sont allouées à une requête.

*IMPORTANCE* = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }</br>        
Spécifie l’importance par défaut d’une requête pour le groupe de charge de travail. Le paramètre Importance peut avoir les valeurs suivantes, NORMAL étant la valeur par défaut :

- LOW
- BELOW_NORMAL
- NORMAL (valeur par défaut)
- ABOVE_NORMAL
- HIGH

L’importance définie pour le groupe de charge de travail est l’importance par défaut de toutes les requêtes du groupe de charge de travail. Un utilisateur peut également définir l’importance au niveau du classifieur, ce qui peut remplacer le paramètre d’importance du groupe de charge de travail. Cela permet de différencier l’importance des requêtes au sein d’un groupe de charge de travail afin d’accéder plus rapidement aux ressources non réservées. Lorsque la somme des min_percentage_resource des différents groupes de charge de travail est inférieure à 100, des ressources non réservées sont affectées selon l’importance.

*QUERY_EXECUTION_TIMEOUT_SEC* = valeur</br>     
Spécifie la durée maximale (en secondes) pendant laquelle une requête peut s’exécuter avant d’être annulée. *value* doit être égal à 0 ou un entier positif. Le paramètre par défaut de la valeur est 0, ce qui signifie que la requête n’expire jamais. Le délai QUERY_EXECUTION_TIMEOUT_SEC démarre une fois que la requête est en cours d’exécution, pas quand elle est mise en file d’attente.

## <a name="remarks"></a>Notes

Les groupes de charge de travail correspondant aux classes de ressources sont créés automatiquement pour permettre une compatibilité descendante. Ces groupes de charge de travail définis par le système ne peuvent pas être supprimés. Il est possible de créer 8 autres groupes de charge de travail définis par l’utilisateur.

Si un groupe de charge de travail est créé avec une `min_percentage_resource` supérieure à zéro, l’instruction `CREATE WORKLOAD GROUP` est mise en file d’attente tant qu’il n’y a pas suffisamment de ressources pour créer le groupe de charge de travail.

## <a name="effective-values"></a>Valeurs effectives

Les paramètres `min_percentage_resource`, `cap_percentage_resource`, `request_min_resource_grant_percent` et `request_max_resource_grant_percent` ont des valeurs effectives qui sont ajustées dans le contexte du niveau de service actuel et de la configuration d’autres groupes de charge de travail.

Le paramètre `request_min_resource_grant_percent` a une valeur effective, car les ressources minimales nécessaires par requête dépendent du niveau de service.  Par exemple, au niveau de service le plus bas, DW100c, un minimum de 25 % des ressources par demande est nécessaire.  Si le groupe de charge de travail est configuré avec 3 % pour `request_min_resource_grant_percent` et `request_max_resource_grant_percent`, les valeurs effectives pour les deux paramètres s’ajustent à 25 % lorsque l’instance est démarrée.  Si l’instance est mise à l’échelle à DW1000c, les valeurs configurées et effectives pour les deux paramètres sont de 3 %, car 3 % est la valeur minimale prise en charge à ce niveau de service.  Si la mise à l’échelle de l’instance est supérieure à Dw1000c, les valeurs configurées et effectives pour les deux paramètres resteront à 3 %.  Consultez le tableau ci-dessous pour plus d’informations sur les valeurs effectives pour différents niveaux de service.

|Niveau de service|Valeur effective la plus faible pour REQUEST_MIN_RESOURCE_GRANT_PERCENT|Nombre maximal de requêtes simultanées|
|---|---|---|
|DW100c|25 %|4|
|DW200c|12,5 %|8|
|DW300c|8 %|12|
|DW400c|6,25 %|16|
|DW500c|5 %|20|
|DW1000c|3 %|32|
|DW1500c|3 %|32|
|DW2000c|2 %|48|
|DW2500c|2 %|48|
|DW3000c|1,5 %|64|
|DW5000c|1,5 %|64|
|DW6000c|0,75 %|128|
|DW7500c|0,75 %|128|
|DW10000c|0,75 %|128|
|DW15000c|0,75 %|128|
|DW30000c|0,75 %|128|
||||

Le paramètre `min_percentage_resource` doit être supérieur ou égal au paramètre `request_min_resource_grant_percent`effectif. Un groupe de charge de travail avec `min_percentage_resource` inférieur à la valeur effective de `min_percentage_resource` a sa valeur définie sur zéro au moment de l’exécution. Quand cela se produit, les ressources configurées pour `min_percentage_resource` peuvent être partagées entre tous les groupes de charge de travail. Par exemple, le groupe de charge de travail `wgAdHoc` avec un `min_percentage_resource` de 10 % exécuté avec DW1000c aura un `min_percentage_resource` effectif de 10 % (3 % est la valeur minimale prise en charge par DW1000c). `wgAdhoc` avec DW100c aura un min_percentage_resource effectif de 0 %. Les 10 % configurés pour `wgAdhoc` seront partagés entre tous les groupes de charge de travail.

Le paramètre `cap_percentage_resource` a également une valeur effective. Si un groupe de charge de travail `wgAdhoc` est configuré avec un `cap_percentage_resource` de 100 % et qu’un autre groupe de charge de travail `wgDashboards` est créé avec un `min_percentage_resource` de 25 %, le `cap_percentage_resource` effectif pour `wgAdhoc` devient 75 %.

Le moyen le plus simple de comprendre les valeurs d’exécution de vos groupes de charge de travail consiste à interroger la vue système [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md).

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation `CONTROL DATABASE`

## <a name="see-also"></a>Voir aussi

- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](drop-workload-group-transact-sql.md)
- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](alter-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Démarrage rapide : Configurer l’isolation de la charge de travail à l’aide de T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
