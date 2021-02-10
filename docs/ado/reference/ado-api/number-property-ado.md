---
description: Number, propriété (ADO)
title: Number, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::Number
- Error::GetNumber
- Error::get_Number
helpviewer_keywords:
- number property [ADO]
ms.assetid: f92323c5-dd11-4a63-a505-d9014a0f067f
author: rothja
ms.author: jroth
ms.openlocfilehash: c39192c6158dfdd180c6f3235668157b9750e1e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041559"
---
# <a name="number-property-ado"></a>Number, propriété (ADO)
Indique le nombre qui identifie de façon unique un objet d' [erreur](./error-object.md) .  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur de **type long** qui peut correspondre à l’une des constantes [ErrorValueEnum](./errorvalueenum.md) .  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Number** pour déterminer l’erreur qui s’est produite. La valeur de la propriété est un nombre unique qui correspond à la condition d’erreur.  
  
 La collection [Errors](./errors-collection-ado.md) renvoie un HRESULT au format hexadécimal (par exemple, 0x80004005) ou une valeur long (par exemple, 2147467259). Ces HRESULT peuvent être déclenchés par des composants sous-jacents, tels que OLE DB ou même OLE lui-même. Pour plus d’informations sur ces nombres, consultez [Erreurs (OLE DB)](/previous-versions/windows/desktop/ms724533(v=vs.85)) dans le [Guide de référence du programmeur OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85))*.*  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Error](./error-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Description, HelpContext, HelpFile, NativeError, Number, source et SQLState, exemples de propriétés (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description, HelpContext, HelpFile, NativeError, Number, source et SQLState, exemples de propriétés (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description, propriété](./description-property.md)   
 [HelpContext, HelpFile, propriétés](./helpcontext-helpfile-properties.md)   
 [Source, propriété (objet Error ADO)](./source-property-ado-error.md)