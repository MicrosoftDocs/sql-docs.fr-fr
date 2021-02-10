---
description: Delete, méthode (collection Fields ADO)
title: Delete, méthode (collection Fields ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Fields20::Delete
- Fields20::raw_Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 25bedc25-c51c-4cab-96ce-930b959965d9
author: rothja
ms.author: jroth
ms.openlocfilehash: 9cfc2784e0a6019e4a3aaee68cb18c54b0e52fc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034409"
---
# <a name="delete-method-ado-fields-collection"></a>Delete, méthode (collection Fields ADO)
Supprime un objet de la collection de [champs](../../../ado/reference/ado-api/fields-collection-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Fields.Delete Field  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Champ*  
 **Variante** qui désigne l’objet de [champ](../../../ado/reference/ado-api/field-object.md) à supprimer. Ce paramètre peut être le nom de l’objet de **champ** ou la position ordinale de l’objet de **champ** lui-même.  
  
## <a name="remarks"></a>Notes  
 L’appel de la méthode **Fields. Delete** sur un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) ouvert provoque une erreur au moment de l’exécution.  
  
## <a name="applies-to"></a>S'applique à  
 [Fields, collection (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Delete, méthode (collection Parameters ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [Delete, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [DeleteRecord, méthode (ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
