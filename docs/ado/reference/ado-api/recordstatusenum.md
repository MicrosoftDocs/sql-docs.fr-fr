---
description: RecordStatusEnum
title: RecordStatusEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordStatusEnum
helpviewer_keywords:
- RecordStatusEnum enumeration [ADO]
ms.assetid: 506fdd70-4452-4e83-95d5-c94311988dfa
author: rothja
ms.author: jroth
ms.openlocfilehash: 4facc7f958c6fe8eab09c6889a54c63deb664ac0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051710"
---
# <a name="recordstatusenum"></a>RecordStatusEnum
Spécifie l' [État](./status-property-ado-recordset.md) d’un enregistrement en ce qui concerne les mises à jour par lots et les autres opérations en bloc.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adRecCanceled**|0x100|Indique que l’enregistrement n’a pas été enregistré parce que l’opération a été annulée.|  
|**adRecCantRelease**|0x400|Indique que le nouvel enregistrement n’a pas été enregistré parce que l’enregistrement existant a été verrouillé.|  
|**adRecConcurrencyViolation**|0x800|Indique que l’enregistrement n’a pas été enregistré, car l’accès concurrentiel optimiste était utilisé.|  
|**adRecDBDeleted**|0x40000|Indique que l’enregistrement a déjà été supprimé de la source de données.|  
|**adRecDeleted**|0x4|Indique que l’enregistrement a été supprimé.|  
|**adRecIntegrityViolation**|0x1000|Indique que l’enregistrement n’a pas été enregistré parce que l’utilisateur n’a pas respecté les contraintes d’intégrité.|  
|**adRecInvalid**|0x10|Indique que l’enregistrement n’a pas été enregistré, car son signet n’est pas valide.|  
|**adRecMaxChangesExceeded**|0x2000|Indique que l’enregistrement n’a pas été enregistré en raison d’un trop grand nombre de modifications en attente.|  
|**adRecModified**|0x2|Indique que l’enregistrement a été modifié.|  
|**adRecMultipleChanges**|0x40|Indique que l’enregistrement n’a pas été enregistré, car il aurait affecté plusieurs enregistrements.|  
|**adRecNew**|0x1|Indique que l’enregistrement est nouveau.|  
|**adRecObjectOpen**|0x4000|Indique que l’enregistrement n’a pas été enregistré en raison d’un conflit avec un objet de stockage ouvert.|  
|**adRecOK**|0|Indique que l’enregistrement a été correctement mis à jour.|  
|**adRecOutOfMemory**|0x8000|Indique que l’enregistrement n’a pas été enregistré, car l’ordinateur ne dispose pas de suffisamment de mémoire.|  
|**adRecPendingChanges**|0x80|Indique que l’enregistrement n’a pas été enregistré parce qu’il fait référence à une insertion en attente.|  
|**adRecPermissionDenied**|0x10000|Indique que l’enregistrement n’a pas été enregistré parce que l’utilisateur ne dispose pas des autorisations suffisantes.|  
|**adRecSchemaViolation**|0x20000|Indique que l’enregistrement n’a pas été enregistré, car il ne respecte pas la structure de la base de données sous-jacente.|  
|**adRecUnmodified**|0x8|Indique que l’enregistrement n’a pas été modifié.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 AdoEnums.RecordStatus.  
  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.RecordStatus.CANCELED|  
|AdoEnums.RecordStatus.CANTRELEASE|  
|AdoEnums.RecordStatus.CONCURRENCYVIOLATION|  
|AdoEnums.RecordStatus.DBDELETED|  
|AdoEnums.RecordStatus.DELETED|  
|AdoEnums.RecordStatus.INTEGRITYVIOLATION|  
|AdoEnums.RecordStatus.INVALID|  
|AdoEnums.RecordStatus.MAXCHANGESEXCEEDED|  
|AdoEnums.RecordStatus.MODIFIED|  
|AdoEnums.RecordStatus.MULTIPLECHANGES|  
|AdoEnums.RecordStatus.NEW|  
|AdoEnums.RecordStatus.OBJECTOPEN|  
|AdoEnums.RecordStatus.OK|  
|AdoEnums. RecordStatus. OUTOFMEMORY|  
|AdoEnums.RecordStatus.PENDINGCHANGES|  
|AdoEnums.RecordStatus.PERMISSIONDENIED|  
|AdoEnums.RecordStatus.SCHEMAVIOLATION|  
|AdoEnums.RecordStatus.UNMODIFIED|  
  
## <a name="applies-to"></a>S'applique à  
 [Status, propriété (objet Recordset ADO)](./status-property-ado-recordset.md)