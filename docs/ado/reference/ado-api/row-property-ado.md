---
description: Row, propriété (ADO)
title: Row, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction::PutRow
- ADORecordConstruction::GetRow
- ADORecordConstruction::get_Row
- ADORecordConstruction::Row
- ADORecordConstruction::put_Row
helpviewer_keywords:
- Row property [ADO]
ms.assetid: 21019d89-2dd1-4a26-ac6f-384b81d66949
author: rothja
ms.author: jroth
ms.openlocfilehash: e59240d9cb6419153e6f717521748ad6beca8e2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040739"
---
# <a name="row-property-ado"></a>Row, propriété (ADO)
Obtient ou définit un objet OLE DB **ligne** à partir de ou sur un objet d' [interface ADORecordConstruction](./adorecordconstruction-interface.md) . Lorsque vous utilisez **put_Row** pour définir un objet **Row** , une ligne est transformée en objet **Record** ADO.  
  
## <a name="readwritesyntax"></a>Read/write.Syntax  
  
```  
HRESULT get_Row([out, retval] IUnknown** ppRow);  
HRESULT put_Row([in] IUnknown* pRow);  
```  
  
## <a name="parameters"></a>Paramètres  
 *ppRow*  
 Pointeur vers un objet OLE DB **Row** .  
  
 *PRow*  
 Objet OLE DB **Row** .  
  
## <a name="return-values"></a>Valeurs de retour  
 Cette méthode de propriété retourne les valeurs HRESULT standard, y compris S_OK et E_FAIL.  
  
## <a name="applies-to"></a>S'applique à  
 [ADORecordConstruction, interface](./adorecordconstruction-interface.md)