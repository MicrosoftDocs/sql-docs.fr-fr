---
title: Superviser et dépanner PolyBase | Microsoft Docs
description: Pour résoudre les problèmes de PolyBase, utilisez ces vues et vues de gestion dynamique. Affichez le plan de requête PolyBase, supervisez les nœuds dans un groupe PolyBase et configurez la haute disponibilité des nœuds de nom Hadoop.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
f1_keywords:
- PolyBase, monitoring
- PolyBase, performance monitoring
helpviewer_keywords:
- PolyBase, troubleshooting
ms.assetid: f119e819-c3ae-4e0b-a955-3948388a9cfe
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-2016'
ms.openlocfilehash: 5945f88320f01f6ce431bea79483528bf8dbeb64
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351753"
---
# <a name="monitor-and-troubleshoot-polybase"></a>Superviser et dépanner PolyBase

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Pour résoudre les problèmes de PolyBase, utilisez les techniques indiquées dans cette rubrique.

## <a name="catalog-views"></a>Affichages catalogue

Utilisez les affichages catalogue répertoriés ici pour gérer les opérations de PolyBase.

|Affichage|Description|  
|-|-|  
|[sys.external_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)|Identifie les tables externes.|  
|[sys.external_data_sources &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)|Identifie les sources de données externes.|  
|[sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)|Identifie les formats de fichiers externes.|  

## <a name="dynamic-management-views"></a>Vues de gestion dynamique  

:::row:::
    :::column:::
        [sys.dm_exec_compute_node_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_compute_node_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_distributed_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_exec_distributed_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-requests-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_distributed_sql_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-sql-requests-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_exec_dms_services &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-services-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_dms_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_exec_external_operations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-external-operations-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_external_work &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-external-work-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Les requêtes PolyBase se décomposent en une série d’étapes dans sys.dm_exec_distributed_request_steps. Le tableau suivant fournit un mappage du nom de l’étape à la vue DMV associée.

|Étape PolyBase|Vue DMV associée|  
|-|-|
|HadoopJobOperation | sys.dm_exec_external_operations|
|RandomIdOperation | sys.dm_exec_distributed_request_steps|
|HadoopRoundRobinOperation | sys.dm_exec_dms_workers|
|StreamingReturnOperation | sys.dm_exec_dms_workers|
|OnOperation | sys.dm_exec_distributed_sql_requests |

## <a name="to-monitor-polybase-queries-using-dmvs"></a>Pour surveiller les requêtes PolyBase à l’aide des vues DMV

Surveillez et résolvez les problèmes des requêtes PolyBase à l’aide des vues DMV suivantes.

1. **Rechercher les requêtes de plus longue durée**  

   Enregistrez l’ID d’exécution de la requête la plus longue.

   ```sql
    -- Find the longest running query  
   SELECT execution_id, st.text, dr.total_elapsed_time  
   FROM sys.dm_exec_distributed_requests  dr  
         cross apply sys.dm_exec_sql_text(sql_handle) st  
   ORDER BY total_elapsed_time DESC;  
   ```

1. **Rechercher l’étape d’exécution la plus longue de la requête distribuée**  

   Utilisez l’ID d’exécution enregistrée à l’étape précédente. Enregistrez l’index d’étape de l’étape d’exécution la plus longue.

   Vérifiez l’élément location_type de l’étape d’exécution la plus longue :  

   - Head ou Compute : implique une opération SQL. Passez à l’étape 3a.

      - DMS : implique une opération de Service de déplacement de données PolyBase. Passez à l’étape 3b.

      ```sql  
      -- Find the longest running step of the distributed query plan  
      SELECT execution_id, step_index, operation_type, distribution_type,   
      location_type, status, total_elapsed_time, command   
      FROM sys.dm_exec_distributed_request_steps   
      WHERE execution_id = 'QID4547'   
      ORDER BY total_elapsed_time DESC;  
      ```  

1. **Rechercher la progression de l’exécution de l’étape d’exécution la plus longue**  

   1. **Rechercher la progression de l’exécution d’une étape SQL**  

      Utilisez l’ID d’exécution et l’index d’étape enregistrés dans les étapes précédentes. Utilisez l’ID d’exécution et l’index d’étape enregistrés dans les étapes précédentes.

      ```sql  
      -- Find the execution progress of SQL step    
      SELECT execution_id, step_index, distribution_id, status,   
      total_elapsed_time, row_count, command   
      FROM sys.dm_exec_distributed_sql_requests   
      WHERE execution_id = 'QID4547' and step_index = 1;  
      ```  

   1. **Rechercher la progression de l’exécution d’une étape DMS**

      Utilisez l’ID d’exécution et l’index d’étape enregistrés dans les étapes précédentes.

      ```sql  
      -- Find the execution progress of DMS step    
      SELECT execution_id, step_index, dms_step_index, status,   
      type, bytes_processed, total_elapsed_time  
      FROM sys.dm_exec_dms_workers   
      WHERE execution_id = 'QID4547'   
      ORDER BY total_elapsed_time DESC;
      ```  

1. **Rechercher les informations sur les opérations DMS externes**  

   Utilisez l’ID d’exécution et l’index d’étape enregistrés dans les étapes précédentes.

   ```sql  
   SELECT execution_id, step_index, dms_step_index, compute_node_id,   
   type, input_name, length, total_elapsed_time, status   
   FROM sys.dm_exec_external_work   
   WHERE execution_id = 'QID4547' and step_index = 7   
   ORDER BY total_elapsed_time DESC;  
   ```  

## <a name="to-view-the--polybase-query-plan-to-be-changed"></a>Pour voir le plan de requête PolyBase (à modifier) 

1. Dans SSMS, activez **Inclure le plan d’exécution réel** (Ctrl + M) et exécutez la requête.

2. Cliquez sur l’onglet **Plan d’exécution** .

   ![Plan de requête PolyBase](../../relational-databases/polybase/media/polybase-query-plan.png "Plan de requête PolyBase")  

3. Cliquez avec le bouton droit sur **l’opérateur Remote Query** et sélectionnez **Propriétés**.

4. Copiez et collez la valeur Remote Query dans un éditeur de texte pour afficher le plan de requête distante XML. Voici un exemple.

   ```xml  

   <dsql_query number_nodes="1" number_distributions="8" number_distributions_per_node="8">  
      <sql>ExecuteMemo explain query</sql>  
      <dsql_operations total_cost="0" total_number_operations="6">  
        <dsql_operation operation_type="RND_ID">  
          <identifier>TEMP_ID_74</identifier>  
        </dsql_operation>  
        <dsql_operation operation_type="ON">  
          <location permanent="false" distribution="AllDistributions" />  
          <sql_operations>  
            <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_74] ([SensorKey] INT NOT NULL, [CustomerKey] INT NOT NULL, [GeographyKey] INT, [Speed] FLOAT(53) NOT NULL, [YearMeasured] INT NOT NULL ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>  
          </sql_operations>  
        </dsql_operation>  
        <dsql_operation operation_type="ON">  
          <location permanent="false" distribution="AllDistributions" />  
          <sql_operations>  
            <sql_operation type="statement">EXEC [tempdb].[sys].[sp_addextendedproperty] @name=N'IS_EXTERNAL_STREAMING_TABLE', @value=N'true', @level0type=N'SCHEMA', @level0name=N'dbo', @level1type=N'TABLE', @level1name=N'TEMP_ID_74'</sql_operation>  
          </sql_operations>  
        </dsql_operation>  
        <dsql_operation operation_type="ON">  
          <location permanent="false" distribution="AllDistributions" />  
          <sql_operations>  
            <sql_operation type="statement">UPDATE STATISTICS [tempdb].[dbo].[TEMP_ID_74] WITH ROWCOUNT = 2401, PAGECOUNT = 7</sql_operation>  
          </sql_operations>  
        </dsql_operation>  
        <dsql_operation operation_type="MULTI">  
          <dsql_operation operation_type="STREAMING_RETURN">  
            <operation_cost cost="1" accumulative_cost="1" average_rowsize="24" output_rows="5762.1" />  
            <location distribution="AllDistributions" />  
            <select>SELECT [T1_1].[SensorKey] AS [SensorKey],  
             [T1_1].[CustomerKey] AS [CustomerKey],  
             [T1_1].[GeographyKey] AS [GeographyKey],  
             [T1_1].[Speed] AS [Speed],  
             [T1_1].[YearMeasured] AS [YearMeasured]  
      FROM   (SELECT [T2_1].[SensorKey] AS [SensorKey],  
                     [T2_1].[CustomerKey] AS [CustomerKey],  
                     [T2_1].[GeographyKey] AS [GeographyKey],  
                     [T2_1].[Speed] AS [Speed],  
                     [T2_1].[YearMeasured] AS [YearMeasured]  
              FROM   [tempdb].[dbo].[TEMP_ID_74] AS T2_1  
              WHERE  ([T2_1].[Speed] > CAST (6.50000000000000000E+001 AS FLOAT))) AS T1_1</select>  
          </dsql_operation>  
          <dsql_operation operation_type="ExternalRoundRobinMove">  
            <operation_cost cost="16.594848" accumulative_cost="17.594848" average_rowsize="24" output_rows="19207" />  
            <external_uri>hdfs://10.193.26.177:8020/Demo/car_sensordata.tbl/</external_uri>  
            <destination_table>[TEMP_ID_74]</destination_table>  
          </dsql_operation>  
        </dsql_operation>  
        <dsql_operation operation_type="ON">  
          <location permanent="false" distribution="AllDistributions" />  
          <sql_operations>  
            <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_74]</sql_operation>  
          </sql_operations>  
        </dsql_operation>  
      </dsql_operations>  
   </dsql_query>  
   ```  

## <a name="to-monitor-nodes-in-a-polybase-group"></a>Pour surveiller des nœuds dans un groupe PolyBase  

Après avoir configuré un ensemble d’ordinateurs dans le cadre d’un groupe de scale-out PolyBase, vous pouvez surveiller l’état des ordinateurs. Pour plus d’informations sur la création d’un groupe de scale-out, consultez [Groupes de scale-out PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).

1. Connectez-vous à SQL Server sur le nœud principal d’un groupe.

2. Exécutez la DMV [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md) pour afficher tous les nœuds dans le groupe PolyBase.

3. Exécutez la DMV [sys.dm_exec_compute_node_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql.md) pour afficher l’état de tous les nœuds dans le groupe PolyBase.

## <a name="hadoop-name-node-high-availability"></a>Haute disponibilité des nœuds de nom Hadoop

De nos jours, PolyBase n’interagit pas avec les services de haute disponibilité des nœuds de nom tels que Zookeeper ou Knox. Toutefois, vous pouvez utiliser une solution de contournement éprouvée pour fournir cette fonctionnalité.

Solution de contournement : utiliser le nom DNS pour rediriger les connexions vers le nom de nœud actif. Pour ce faire, vous devez vous assurer que la source de données externe utilise un nom DNS pour communiquer avec le nom de nœud. Quand le basculement du nom de nœud se produit, vous devez changer l’adresse IP associée au nom DNS utilisé dans la définition de la source de données externe. Cette opération redirige toutes les nouvelles connexions vers le nœud de nom correct. Les connexions existantes échouent quand le basculement se produit. Pour automatiser ce processus, une « pulsation » peut exécuter un ping sur le nom de nœud actif. Si la pulsation échoue, il est à supposer qu’un basculement s’est produit avec un basculement automatiquement vers l’adresse IP des bases de données secondaires.

## <a name="error-messages-and-possible-solutions"></a>Messages d’erreur et solutions possibles

Pour résoudre les erreurs de table externe, consultez le blog de Murshed Zaman [https://blogs.msdn.microsoft.com/sqlcat/2016/06/21/polybase-setup-errors-and-possible-solutions/](/archive/blogs/sqlcat/polybase-setup-errors-and-possible-solutions "Erreurs de configuration de PolyBase et solutions possibles").

## <a name="see-also"></a>Voir aussi

[Résoudre les problèmes de connectivité de PolyBase Kerberos](polybase-troubleshoot-connectivity.md)