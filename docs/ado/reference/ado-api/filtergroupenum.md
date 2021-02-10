---
description: FilterGroupEnum
title: FilterGroupEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FilterGroupEnum
helpviewer_keywords:
- FilterGroupEnum enumeration [ADO]
ms.assetid: b22e725e-84bd-4286-a070-290c278c3783
author: rothja
ms.author: jroth
ms.openlocfilehash: 27b8b217287f3c623fc626de1cb46dcc2eed5394
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024595"
---
# <a name="filtergroupenum"></a>FilterGroupEnum
Spécifie le groupe d’enregistrements à filtrer à partir d’un [jeu d’enregistrements](./recordset-object-ado.md).  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adFilterAffectedRecords**|2|Filtres permettant d’afficher uniquement les enregistrements affectés par le dernier appel [Delete](./delete-method-ado-recordset.md), [Resync](./resync-method.md), [UpdateBatch](./updatebatch-method.md)ou [CancelBatch](./cancelbatch-method-ado.md) .|  
|**adFilterConflictingRecords**|5|Filtres permettant d’afficher les enregistrements qui ont échoué lors de la dernière mise à jour par lot.|  
|**adFilterFetchedRecords**|3|Filtres permettant d’afficher les enregistrements dans le cache actuel, autrement dit, les résultats du dernier appel pour récupérer des enregistrements de la base de données.|  
|**adFilterNone**|0|Supprime le filtre en cours et restaure tous les enregistrements à afficher.|  
|**adFilterPendingRecords**|1|Filtres permettant d’afficher uniquement les enregistrements qui ont été modifiés mais qui n’ont pas encore été envoyés au serveur. S’applique uniquement au mode de mise à jour par lot.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. FilterGroup. AFFECTEDRECORDS|  
|AdoEnums.FilterGroup.CONFLICTINGRECORDS|  
|AdoEnums. FilterGroup. FETCHEDRECORDS|  
|AdoEnums.FilterGroup.NONE|  
|AdoEnums.FilterGroup.PENDINGRECORDS|  
  
## <a name="applies-to"></a>S'applique à  
 [Filter, propriété](./filter-property.md)