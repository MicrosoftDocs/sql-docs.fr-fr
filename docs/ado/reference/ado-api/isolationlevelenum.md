---
description: IsolationLevelEnum
title: IsolationLevelEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- IsolationLevelEnum
helpviewer_keywords:
- IsolationLevelEnum enumeration [ADO]
ms.assetid: 8e17a7bc-b8a3-4ae2-b6c9-ce088ad31fdf
author: rothja
ms.author: jroth
ms.openlocfilehash: 03bd95a642f3942275e1ff9d32f1b2d1829b96d3
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88774688"
---
# <a name="isolationlevelenum"></a>IsolationLevelEnum
Spécifie le niveau d’isolation des transactions pour un objet de [connexion](./connection-object-ado.md) .  
  
|Constant|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adXactUnspecified**|-1|Indique que le fournisseur utilise un niveau d’isolation différent de celui spécifié, mais que le niveau ne peut pas être déterminé.|  
|**adXactChaos**|16|Indique que les modifications en attente de transactions hautement isolées ne peuvent pas être remplacées.|  
|**adXactBrowse**|256|Indique qu’à partir d’une transaction vous pouvez afficher les modifications non validées dans d’autres transactions.|  
|**adXactReadUncommitted**|256|Identique à **adXactBrowse**.|  
|**adXactCursorStability**|4096|Indique qu’à partir d’une transaction, vous pouvez afficher les modifications apportées aux autres transactions uniquement après qu’elles ont été validées.|  
|**adXactReadCommitted**|4096|Identique à **adXactCursorStability**.|  
|**adXactRepeatableRead**|65536|Indique qu’à partir d’une transaction, vous ne pouvez pas voir les modifications apportées dans d’autres transactions, mais cette dernière peut récupérer de nouveaux objets **Recordset** .|  
|**adXactIsolated**|1 048 576|Indique que les transactions sont effectuées de manière isolée des autres transactions.|  
|**adXactSerializable**|1 048 576|Identique à **adXactIsolated**.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constant|  
|--------------|  
|AdoEnums. IsolationLevel. non spécifié|  
|AdoEnums. IsolationLevel. CHAOS|  
|AdoEnums.IsolationLevel.BROWSE|  
|AdoEnums. IsolationLevel. READUNCOMMITTED|  
|AdoEnums.IsolationLevel.CURSORSTABILITY|  
|AdoEnums. IsolationLevel. READCOMMITTED|  
|AdoEnums. IsolationLevel. REPEATABLEREAD|  
|AdoEnums. IsolationLevel. ISOLATed|  
|AdoEnums. IsolationLevel. Serializable|  
  
## <a name="applies-to"></a>S'applique à  
 [IsolationLevel, propriété](./isolationlevel-property.md)