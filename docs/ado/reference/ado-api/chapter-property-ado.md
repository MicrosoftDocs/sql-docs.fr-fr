---
description: Chapter, propriété (ADO)
title: Chapter, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordsetConstruction::Chapter
- ADORecordsetConstruction::put_Chapter
- ADORecordsetConstruction::get_Chapter
helpviewer_keywords:
- Chapter property [ADO]
ms.assetid: 8aa90cb0-f588-4141-9dc9-3b22918394ee
author: rothja
ms.author: jroth
ms.openlocfilehash: e4d156c6e117d5597569918496c161729cf6fc41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164729"
---
# <a name="chapter-property-ado"></a>Chapter, propriété (ADO)
Obtient ou définit un objet OLE DB **chapitre** à partir de/sur un objet d' [interface ADORecordsetConstruction](./adorecordsetconstruction-interface.md) . Lorsque vous utilisez **put_Chapter** pour définir l’objet **Chapter** , un sous-ensemble de lignes est converti en objet [objet Recordset](./recordset-object-ado.md) ADO. Cela définit le chapitre actuel de l’objet **rowset**. Cette propriété est en lecture/écriture.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT get_Chapter([out, retval] long* plChapter);  
HRESULT put_Chapter([in] long lChapter);  
```  
  
## <a name="parameters"></a>Paramètres  
 *plChapter*  
 Pointeur vers le handle d’un chapitre.  
  
 *LChapter*  
 Handle d’un chapitre.  
  
## <a name="return-values"></a>Valeurs de retour  
 Cette méthode de propriété retourne les valeurs HRESULT standard, y compris S_OK et E_FAIL.  
  
## <a name="applies-to"></a>S'applique à  
 [ADORecordsetConstruction, interface](./adorecordsetconstruction-interface.md)