---
description: Requery, méthode
title: Méthode Requery | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Requery
- Recordset15::raw_Requery
helpviewer_keywords:
- Requery method [ADO]
ms.assetid: d81ab76f-1aa8-4ccf-92ec-b65254dc3ea1
author: rothja
ms.author: jroth
ms.openlocfilehash: 21056d0e8f8e488bc5b80541ea4c397b3340c453
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166656"
---
# <a name="requery-method"></a>Requery, méthode
Met à jour les données dans un objet [Recordset](./recordset-object-ado.md) en exécutant à nouveau la requête sur laquelle l’objet est basé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Requery Options  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Options*  
 Facultatif. Masque de répercussion qui contient des valeurs [ExecuteOptionEnum](./executeoptionenum.md) et [CommandTypeEnum](./commandtypeenum.md) affectant cette opération.  
  
> [!NOTE]
>  Si *options* a la valeur **adAsyncExecute**, cette opération s’exécute de façon asynchrone et un événement [RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md) est émis lorsqu’elle se termine. Les valeurs **ExecuteOpenEnum** de **adExecuteNoRecords** ou **adExecuteStream** ne doivent pas être utilisées avec **Requery**.  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **Requery** pour actualiser la totalité du contenu d’un objet **Recordset** à partir de la source de données en réémettant la commande d’origine et en extrayant les données une deuxième fois. L’appel de cette méthode équivaut à appeler les méthodes [Close](./close-method-ado.md) et [Open](./open-method-ado-recordset.md) à la suite. Si vous modifiez l’enregistrement en cours ou ajoutez un nouvel enregistrement, une erreur se produit.  
  
 Lorsque l’objet **Recordset** est ouvert, les propriétés qui définissent la nature du curseur ([CursorType](./cursortype-property-ado.md), [LockType](./locktype-property-ado.md), [maxRecords](./maxrecords-property-ado.md), etc.) sont en lecture seule. Ainsi, la méthode **Requery** peut uniquement actualiser le curseur actuel. Pour modifier l’une des propriétés de curseur et afficher les résultats, vous devez utiliser la méthode [Close](./close-method-ado.md) afin que les propriétés soient à nouveau en lecture/écriture. Vous pouvez ensuite modifier les paramètres de propriété et appeler la méthode [Open](./open-method-ado-recordset.md) pour rouvrir le curseur.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Execute, Requery et Clear, exemples de méthodes (VB)](./execute-requery-and-clear-methods-example-vb.md)   
 [Execute, Requery et Clear, exemple de méthode (VBScript)](./execute-requery-and-clear-methods-example-vbscript.md)   
 [Execute, Requery et Clear, exemples de méthodes (VC + +)](./execute-requery-and-clear-methods-example-vc.md)   
 [CommandText, propriété (ADO)](./commandtext-property-ado.md)