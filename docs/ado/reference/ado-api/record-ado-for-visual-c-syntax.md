---
description: Record (syntaxe ADO pour Visual C++)
title: Record (ADO pour la syntaxe Visual C++) | Microsoft Docs
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
- Record collection [ADO], ADO for Visual C++ syntax
ms.assetid: c4ce8532-a4d8-4f74-9488-9389b6695958
author: rothja
ms.author: jroth
ms.openlocfilehash: fdaf00ed44bdaefad2e0ffafb760b9538ff817c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170476"
---
# <a name="record-ado-for-visual-c-syntax"></a>Record (syntaxe ADO pour Visual C++)
## <a name="methods"></a>Méthodes  
  
```  
Cancel(void)  
Close(void)  
CopyRecord(BSTR Source, BSTR Destination, BSTR UserName, BSTR Password, CopyRecordOptionsEnum Options, VARIANT_BOOL Async, BSTR *pbstrNewURL)  
DeleteRecord(BSTR Source, VARIANT_BOOL Async)  
GetChildren(_ADORecordset **ppRSet)  
MoveRecord(BSTR Source, BSTR Destination, BSTR UserName, BSTR Password, MoveRecordOptionsEnum Options, VARIANT_BOOL Async, BSTR *pbstrNewURL)  
Open(VARIANT Source, VARIANT ActiveConnection, ConnectModeEnum Mode, RecordCreateOptionsEnum CreateOptions, RecordOpenOptionsEnum Options, BSTR UserName, BSTR Password)  
```  
  
## <a name="properties"></a>Propriétés  
  
```  
get_ActiveConnection(VARIANT *pvar)  
put_ActiveConnection(BSTR bstrConn)  
putref_ActiveConnection(_ADOConnection *Con)  
get_Fields(ADOFields **ppFlds)  
get_Mode(ConnectModeEnum *pMode)  
put_Mode(ConnectModeEnum Mode)  
get_ParentURL(BSTR *pbstrParentURL)  
get_RecordType(RecordTypeEnum *pType)  
get_Source(VARIANT *pvar)  
put_Source(BSTR Source)  
putref_Source(IDispatch *Source)  
get_State(ObjectStateEnum *pState)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Record, objet (ADO)](./record-object-ado.md)