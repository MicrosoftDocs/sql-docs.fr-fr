---
title: Mémoires tampons en anneau pour les informations d’intégrité sur les groupes de disponibilité
description: Obtenez certaines informations de diagnostic sur les groupes de disponibilité Always On à partir des mémoires tampons en anneau SQL Server.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 47bb7a1a-c0a5-473c-a7db-d9f4bf3ee650
author: rothja
ms.author: jroth
ms.openlocfilehash: 1feb461ad05167b4669da2ee53c17342bba98195
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641433"
---
# <a name="use-ring-buffers-to-obtain-health-information-about-always-on-availability-groups"></a>Utiliser des mémoires tampons en anneau pour obtenir des informations sur l’intégrité des groupes de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Vous pouvez obtenir certaines informations de diagnostic sur les groupes de disponibilité Always On à partir des mémoires tampons en anneau SQL Server ou de la vue de gestion dynamique (DMV) sys.dm_os_ring_buffers. Les mémoires tampons en anneau sont créées durant le démarrage de SQL Server et enregistrent les alertes générées au sein du système SQL Server à des fins de diagnostic interne. Même si elles ne sont pas prises en charge, vous pouvez toujours en déduire des informations utiles pour la résolution des problèmes. Ces mémoires tampons en anneau fournissent une autre source de diagnostics quand SQL Server plante ou se bloque.  
  
 La requête T-SQL (Transact-SQL) suivante récupère tous les enregistrements d’événement des mémoires tampons en anneau des groupes de disponibilité.  
  
```sql  
SELECT * FROM sys.dm_os_ring_buffers WHERE ring_buffer_type LIKE '%HADR%'  
```  
  
 Pour gérer plus facilement les données, filtrez les données par date et type de mémoire tampon en anneau. La requête suivante récupère à partir de la mémoire tampon en anneau spécifiée les enregistrements qui se sont produits aujourd’hui.  
  
```sql  
DECLARE @runtime datetime  
SET @runtime = GETDATE()  
SELECT CONVERT (varchar(30), @runtime, 121) as data_collection_runtime,   
DATEADD (ms, -1 * (inf.ms_ticks - ring.[timestamp]), GETDATE()) AS ring_buffer_record_time,   
ring.[timestamp] AS record_timestamp, inf.ms_ticks AS cur_timestamp, ring.*   
FROM sys.dm_os_ring_buffers ring  
CROSS JOIN sys.dm_os_sys_info inf where ring_buffer_type='<RING_BUFFER_TYPE>'  
```  
  
 La colonne Record dans chaque enregistrement contient des données de diagnostic au format XML. Les données XML varient selon le type de mémoire tampon en anneau. Pour plus d’informations sur chaque type de mémoire tampon en anneau, consultez [Types de mémoires tampons en anneau des groupes de disponibilité](#BKMK_RingBufferTypes). Pour améliorer la lisibilité des données XML, vous devez personnaliser votre requête T-SQL de manière à extraire les éléments XML qui vous intéressent. Par exemple, la requête suivante récupère tous les événements de la mémoire tampon en anneau RING_BUFFER_HADRDBMGR_API et met en forme les données XML dans des colonnes de table distinctes.  
  
```sql  
WITH hadr(ts, type, record) AS  
(  
  SELECT timestamp AS ts, ring_buffer_type AS type, CAST(record AS XML) AS record   
  FROM sys.dm_os_ring_buffers WHERE ring_buffer_type = 'RING_BUFFER_HADRDBMGR_API'  
)  
SELECT   
  ts,  
  type,  
  record.value('(./Record/@id)[1]','bigint') AS [Record ID],  
  record.value('(./Record/@time)[1]','bigint') AS [Time],  
  record.value('(./Record/HadrDbMgrAPI/dbId)[1]', 'bigint') AS [DBID],  
  record.value('(/Record/HadrDbMgrAPI/API)[1]', 'varchar(50)') AS [API],  
  record.value('(/Record/HadrDbMgrAPI/Action)[1]', 'varchar(50)') AS [Action],  
  record.value('(/Record/HadrDbMgrAPI/role)[1]', 'int') AS [Role],  
  record.value('(/Record/Stack)[1]', 'varchar(100)') AS [Call Stack]  
FROM hadr  
ORDER BY record.value('(./Record/@time)[1]','bigint') DESC  
GO  
```  
  
##  <a name="availability-groups-ring-buffer-types"></a><a name="BKMK_RingBufferTypes"></a> Types de mémoires tampons en anneau des groupes de disponibilité  
 Il existe quatre mémoires tampons de groupes de disponibilité dans sys.dm_os_ring_buffers. Le tableau ci-dessous décrit les types de mémoires tampons en anneau et fournit un exemple du contenu de la colonne Record pour chaque type de mémoire tampon en anneau.  
  
 **RING_BUFFER_HADRDBMGR_API**  
  
 Enregistre les transitions d’état qui ont eu lieu ou qui sont en cours. Quand vous examinez les transitions d’état, soyez attentif aux valeurs objectType.  
  
```xml  
<Record id="11" type="RING_BUFFER_HADRDBMGR_STATE" time="860243">  
  <HadrDbMgrState>  
    <objectType>HadrUsers</objectType>  
    <currentState>HDbMState_Starting</currentState>  
    <proposedState>HDbMState_Started</proposedState>  
    <targetState>HDbMState_Started</targetState>  
    <legalTransition>Y</legalTransition>  
    <role>1</role>  
  </HadrDbMgrState>  
</Record>  
```  
  
 **RING_BUFFER_HADRDBMGR_STATE**  
  
 Enregistre les appels de méthode ou de fonction internes associés à l’activité Always On. Elle peut afficher des informations sur la suspension, la reprise ou les changements de rôle, notamment les points d’entrée et de sortie.  
  
```xml  
<Record id="45" type="RING_BUFFER_HADRDBMGR_STATE" time="1723487912">  
  <HadrDbMgrState>  
    <dbId>5</dbId>  
    <objectType>HadrDbMgr</objectType>  
    <currentState>HDbMState_Starting</currentState>  
    <proposedState>HDbMState_Started</proposedState>  
    <targetState>HDbMState_Started</targetState>  
    <legalTransition>Y</legalTransition>  
    <role>2</role>  
  </HadrDbMgrState>  
</Record>  
```  
  
 **RING_BUFFER_HADRDBMGR_COMMIT**  
  
```xml  
<Record id="0" type="RING_BUFFER_HADRDBMGR_COMMIT" time="1723475368">  
  <HadrDbMgrCommitPolicy>  
    <dbId>5</dbId>  
    <replicaId>883a18f5-97d5-450f-8f8f-9983a4fa5299</replicaId>  
    <dbHardenPolicy>KillAll</dbHardenPolicy>  
    <dbSyncConfig>0x0</dbSyncConfig>  
    <syncPartnerCount>0</syncPartnerCount>  
    <minSyncPartnerConfig>0</minSyncPartnerConfig>  
    <partnerHardenPolicy>KillAll</partnerHardenPolicy>  
    <partnerSyncConfig>0x0</partnerSyncConfig>  
    <logBlock>0x0000000000000000</logBlock>  
    <leaseExpired>Y</leaseExpired>  
    <partnerChange>N</partnerChange>  
    <role>2</role>  
  </HadrDbMgrCommitPolicy>  
</Record>  
```  
  
 **RING_BUFFER_HADR_TRANSPORT_STATE**  
  
```xml  
<Record id="3" type="RING_BUFFER_HADR_TRANSPORT_STATE" time="1723485399">  
  <HadrTransportState>  
    <agId>08264B79-D10B-412F-B38D-CA07B08E9BD8</agId>  
    <localArId>883A18F5-97D5-450F-8F8F-9983A4FA5299</localArId>  
    <targetArId>628D6349-72DD-4D18-A6E1-1272645660BA</targetArId>  
    <currentState>HadrSession_Configuring</currentState>  
    <targetState>HadrSession_Connected</targetState>  
    <legalTransition>Y</legalTransition>  
  </HadrTransportState>  
</Record>  
```  
  
## <a name="parse-xml-data-from-a-ring-buffer"></a>Analyser les données XML à partir d’une mémoire tampon en anneau  
 Vous pouvez analyser le champ Record de la mémoire tampon en anneau que vous inspectez en utilisant [value&#40;&#41; Method &#40;xml Data Type&#41;](~/t-sql/xml/value-method-xml-data-type.md) dans votre requête. Pour utiliser cette méthode, vous devez d’abord effectuer un [CAST](~/t-sql/functions/cast-and-convert-transact-sql.md) de la colonne Record de la mémoire tampon en anneau en XML. Par exemple, la requête ci-dessous montre comment analyser RING_BUFFER_HADRDBMGR_API dans un format lisible à l’aide de cette méthode.  
  
```sql 
WITH hadr(ts, type, record) AS  
   (SELECT timestamp AS ts, ring_buffer_type AS type, CAST(record AS XML) AS record   
FROM sys.dm_os_ring_buffers   
WHERE ring_buffer_type = 'RING_BUFFER_HADRDBMGR_API')  
SELECT ts,  
type,  
record.value('(./Record/@id)[1]','bigint') AS [Record id],  
record.value('(./Record/@time)[1]','bigint') AS [Time],  
record.value('(./Record/HadrDbMgrAPI/dbId)[1]', 'bigint') AS [dbid],  
record.value('(/Record/HadrDbMgrAPI/API)[1]', 'varchar(50)') AS [API],  
record.value('(/Record/HadrDbMgrAPI/Action)[1]', 'varchar(50)') AS [Action],  
record.value('(/Record/HadrDbMgrAPI/role)[1]', 'int') AS [Role],  
record.value('(/Record/Stack)[1]', 'varchar(100)') AS [Call Stack]  
FROM hadr  
ORDER BY record.value('(./Record/@time)[1]','bigint') DESC  
GO  
```  
  
  
