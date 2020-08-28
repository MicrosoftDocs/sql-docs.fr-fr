---
description: Clear, méthode (ADO)
title: Clear, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Errors::raw_Clear
- Errors::Clear
helpviewer_keywords:
- Clear method [ADO]
ms.assetid: 0a61ba7a-20b8-426a-91a0-9040e7c5a98a
author: rothja
ms.author: jroth
ms.openlocfilehash: d78980c1baee5aed1280f69c0d5224622a217ea0
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88975470"
---
# <a name="clear-method-ado"></a>Clear, méthode (ADO)
Supprime tous les objets [Error](./error-object.md) de la collection [Errors](./errors-collection-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Errors.Clear  
```  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **Clear** de la collection [Errors](./errors-collection-ado.md) pour supprimer tous les objets d' [erreur](./error-object.md) existants de la collection. Lorsqu’une erreur se produit, ADO efface automatiquement la collection d' **Erreurs** et la remplit avec des objets d' **erreur** en fonction de la nouvelle erreur.  
  
 Certaines propriétés et méthodes retournent des avertissements qui s’affichent en tant qu’objets d' **erreur** dans la collection d' **Erreurs** , mais n’arrêtent pas l’exécution d’un programme. Avant d’appeler les méthodes [Resync](./resync-method.md), [UpdateBatch](./updatebatch-method.md)ou [CancelBatch](./cancelbatch-method-ado.md) sur un objet [Recordset](./recordset-object-ado.md) ; la méthode [Open](./open-method-ado-connection.md) sur un objet [Connection](./connection-object-ado.md) ; ou définissez la propriété [Filter](./filter-property.md) sur un objet **Recordset** , appelez la méthode **Clear** sur la collection **Errors** . De cette façon, vous pouvez lire la propriété [Count](./count-property-ado.md) de la collection **Errors** pour tester les avertissements retournés.  
  
## <a name="applies-to"></a>S'applique à  
 [Errors, collection (ADO)](./errors-collection-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Execute, Requery et Clear, exemples de méthodes (VB)](./execute-requery-and-clear-methods-example-vb.md)   
 [Execute, Requery et Clear, exemple de méthode (VBScript)](./execute-requery-and-clear-methods-example-vbscript.md)   
 [Execute, Requery et Clear, exemples de méthodes (VC + +)](./execute-requery-and-clear-methods-example-vc.md)   
 [Méthode CancelBatch (ADO)](./cancelbatch-method-ado.md)   
 [Delete, méthode (collection Fields ADO)](./delete-method-ado-fields-collection.md)   
 [Delete, méthode (collection Parameters ADO)](./delete-method-ado-parameters-collection.md)   
 [Delete, méthode (objet Recordset ADO)](./delete-method-ado-recordset.md)   
 [Filter (propriété)](./filter-property.md)   
 [Resync, méthode](./resync-method.md)   
 [UpdateBatch, méthode](./updatebatch-method.md)