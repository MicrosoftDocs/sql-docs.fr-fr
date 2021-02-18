---
title: TuningOptions, élément (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément TuningOptions contient les options de paramétrage pour une session de paramétrage spécifique.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- TuningOptions element
ms.assetid: 58a22ba1-8e03-411f-bd46-85e4540f217a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 5ee768da96546cfcc4619098f5f0c962205047d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351114"
---
# <a name="tuningoptions-element-dta"></a>TuningOptions, élément (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contient les options de paramétrage pour une session de paramétrage spécifique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<DTAInput>  
    <Server>  
...code removed...  
    <Workload>...</Workload>  
    <TuningOptions>...</TuningOptions>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|Aucun.|  
|**Valeur par défaut**|Aucun.|  
|**Occurrence**|facultatif. Si cet élément est utilisé, il ne peut être utilisé qu'une seule fois pour l'élément **DTAInput** .|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|[DTAInput, élément &#40;DTA&#41;](../../tools/dta/dtainput-element-dta.md)|  
|**Éléments enfants**|Élément **ReportSet** . Pour plus d'informations, consultez l'article [Database Engine Tuning Advisor XML schema](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Élément **TuningLogTable** . Pour plus d'informations, consultez l'article [Database Engine Tuning Advisor XML schema](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Élément **NumberOfEvents** . Pour plus d'informations, consultez l'article [Database Engine Tuning Advisor XML schema](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> [TuningTimeInMin, élément &#40;DTA&#41;](../../tools/dta/tuningtimeinmin-element-dta.md)<br /><br /> [StorageBoundInMB, élément &#40;DTA&#41;](../../tools/dta/storageboundinmb-element-dta.md)<br /><br /> Élément **MaxKeyColumnsInIndex** . Pour plus d'informations, consultez l'article [Database Engine Tuning Advisor XML schema](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Élément **MaxColumnsInIndex** . Pour plus d'informations, consultez l'article [Database Engine Tuning Advisor XML schema](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Élément **MinPercentageImprovement** . Pour plus d'informations, consultez le [schéma XML de l'Assistant Paramétrage du moteur de base de données](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> [TestServer, élément &#40;DTA&#41;](../../tools/dta/testserver-element-dta.md)<br /><br /> [FeatureSet, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/featureset-element-dta.md)<br /><br /> [Partitioning, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/partitioning-element-dta.md)<br /><br /> [DropOnlyMode, élément &#40;DTA&#41;](../../tools/dta/droponlymode-element-dta.md)<br /><br /> [KeepExisting, élément &#40;DTA&#41;](../../tools/dta/keepexisting-element-dta.md)<br /><br /> [OnlineIndexOperation, élément &#40;DTA&#41;](../../tools/dta/onlineindexoperation-element-dta.md)<br /><br /> [DatabaseToConnect, élément &#40;DTA&#41;](../../tools/dta/databasetoconnect-element-dta.md)<br /><br /> Élément **IgnoreConstantsInWorkload** . Pour plus d'informations, consultez l'article [Database Engine Tuning Advisor XML schema](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Élément **RetainShellDB** . Pour plus d'informations, consultez l'article [Database Engine Tuning Advisor XML schema](https://go.microsoft.com/fwlink/?linkid=43100).|  
  
## <a name="example"></a>Exemple  
 Pour obtenir des exemples de l’élément **TuningOptions**, consultez les [exemples de fichiers d’entrée XML &#40;DTA&#41;](../../tools/dta/xml-input-file-samples-dta.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
