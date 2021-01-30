---
description: Stream, propriété
title: Stream, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADOStreamConstruction::GetStream
- ADOStreamConstruction::PutStream
- ADOStreamConstruction::put_Stream
- ADOStreamConstruction::Stream
- ADOStreamConstruction::get_Stream
helpviewer_keywords:
- Stream property
ms.assetid: 4a44f9f6-0265-4c00-8def-d85b6af923b1
author: rothja
ms.author: jroth
ms.openlocfilehash: b65a3bf2d4664be84250a9923f0ed3908c07f179
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170142"
---
# <a name="stream-property"></a>Stream, propriété
Obtient ou définit un objet de **flux** OLE DB à partir de/sur un objet **ADOStreamConstruction** .  
  
 En lecture/écriture.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT get_Stream([out, retval] IUnknown** ppStream);  
HRESULT put_Stream([in] IUnknown* pStream);  
```  
  
## <a name="parameters"></a>Paramètres  
 *ppStream*  
 Pointeur vers un objet de **flux** OLE DB.  
  
 *pStream*  
 Objet de **flux** OLE DB.  
  
## <a name="return-values"></a>Valeurs de retour  
 Cette méthode de propriété retourne les valeurs HRESULT standard. Cela comprend les S_OK et les E_FAIL.  
  
## <a name="applies-to"></a>S'applique à  
 [ADOStreamConstruction, interface](./adostreamconstruction-interface.md)