---
description: 'RecordsetEvents (index de la syntaxe Visual C++ avec #import)'
title: 'RecordsetEvents (index de la syntaxe Visual C++ avec #import) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- RecordsetEvents collection [ADO]
ms.assetid: b7021f11-8242-4e9f-92e9-1a4472673fb1
author: rothja
ms.author: jroth
ms.openlocfilehash: 8086814a0baf03d9329b79bd04a698cd5ee64cf2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170422"
---
# <a name="recordsetevents-visual-c-syntax-index-with-import"></a>RecordsetEvents (index de la syntaxe Visual C++ avec #import)
## <a name="events"></a>Événements  
  
```  
HRESULT WillChangeField( long cFields, const  
    _variant_t & Fields, enum     EventStatusEnum * adStatus, struct  
    _Recordset * pRecordset );  
  
HRESULT FieldChangeComplete( long cFields, const  
    _variant_t & Fields,     struct Error * pError, enum EventStatusEnum  
    * adStatus, struct     _Recordset * pRecordset );  
  
HRESULT WillChangeRecord( enum EventReasonEnum  
    adReason, long cRecords,     enum EventStatusEnum * adStatus, struct  
    _Recordset * pRecordset );  
  
HRESULT RecordChangeComplete( enum EventReasonEnum  
    adReason, long     cRecords, struct Error * pError, enum  
    EventStatusEnum * adStatus,     struct _Recordset * pRecordset );  
  
HRESULT WillChangeRecordset( enum EventReasonEnum  
    adReason, enum     EventStatusEnum * adStatus, struct _Recordset *  
    pRecordset );  
  
HRESULT RecordsetChangeComplete( enum  
    EventReasonEnum adReason, struct     Error * pError, enum  
    EventStatusEnum * adStatus, struct _Recordset *     pRecordset );  
  
HRESULT WillMove( enum EventReasonEnum adReason, enum  
    EventStatusEnum *     adStatus, struct _Recordset * pRecordset );  
  
HRESULT MoveComplete( enum EventReasonEnum adReason, struct  
    Error *     pError, enum EventStatusEnum * adStatus, struct  
    _Recordset * pRecordset );  
  
HRESULT EndOfRecordset( VARIANT_BOOL * fMoreData, enum  
    EventStatusEnum *     adStatus, struct _Recordset * pRecordset );  
  
HRESULT FetchProgress( long Progress, long MaxProgress,  
    enum     EventStatusEnum * adStatus, struct _Recordset * pRecordset );  
  
HRESULT FetchComplete( struct Error * pError, enum  
    EventStatusEnum *     adStatus, struct _Recordset * pRecordset );  
```
