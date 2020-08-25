---
description: Stream, propriété
title: Stream, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
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
ms.openlocfilehash: 97f378e1be87be95682bf2eeb05e112abfb96f9d
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88777208"
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