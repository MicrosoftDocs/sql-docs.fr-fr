---
description: EditMode, propriété
title: EditMode, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::EditMode
helpviewer_keywords:
- EditMode property
ms.assetid: a1b04bb2-8c8b-47f9-8477-bfd0368b6f68
author: rothja
ms.author: jroth
ms.openlocfilehash: 5854dce12ddb2303077111f3129274861a340ebd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171238"
---
# <a name="editmode-property"></a>EditMode, propriété
Indique l’état de modification de l’enregistrement en cours.  
  
## <a name="return-value"></a>Valeur renvoyée  
 Retourne une valeur [EditModeEnum](../../../ado/reference/ado-api/editmodeenum.md) .  
  
## <a name="remarks"></a>Notes  
 ADO gère un tampon d’édition associé à l’enregistrement actif. Cette propriété indique si des modifications ont été apportées à cette mémoire tampon ou si un nouvel enregistrement a été créé. Utilisez la propriété **EditMode** pour déterminer l’état de modification de l’enregistrement en cours. Vous pouvez tester les modifications en attente si un processus de modification a été interrompu et déterminer si vous devez utiliser la méthode [Update](../../../ado/reference/ado-api/update-method.md) ou [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) .  
  
 En *mode de mise à jour immédiate* , la propriété **EditMode** est réinitialisée à **adEditNone** après l’appel d’un appel à la méthode **Update** . Lorsqu’un appel à [Delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md) ne supprime pas correctement l’enregistrement ou les enregistrements dans la source de données (par exemple, en raison de violations d’intégrité référentielle), le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) reste en mode édition (**EditMode**  =  **adEditInProgress**). Par conséquent, **CancelUpdate** doit être appelé avant de quitter l’enregistrement actif (par exemple, avec [Move](../../../ado/reference/ado-api/move-method-ado.md), [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)ou [Close](../../../ado/reference/ado-api/close-method-ado.md) ).  
  
 En *mode de mise à jour par lot* (dans lequel le fournisseur met en cache plusieurs modifications et les écrit dans la source de données sous-jacente uniquement lorsque vous appelez la méthode [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) ), la valeur de la propriété **EditMode** est modifiée lorsque la première opération est exécutée et n’est pas réinitialisée par un appel à la méthode **Update** . Les opérations ultérieures ne modifient pas la valeur de la propriété **EditMode** , même si différentes opérations sont effectuées. Par exemple, si la première opération consiste à ajouter un nouvel enregistrement et que la seconde modifie un enregistrement existant, la propriété de **EditMode** est toujours **adEditAdd**. La propriété **EditMode** n’est pas réinitialisée à **adEditNone** tant que l’appel à **UpdateBatch** n’a pas été effectué. Pour déterminer les opérations qui ont été effectuées, définissez la propriété [Filter](../../../ado/reference/ado-api/filter-property.md) sur [adFilterPending](../../../ado/reference/ado-api/filtergroupenum.md) afin que seuls les enregistrements avec des modifications en attente soient visibles et examinez la propriété [Status](../../../ado/reference/ado-api/status-property-ado-recordset.md) de chaque enregistrement pour déterminer les modifications apportées aux données.  
  
> [!NOTE]
>  **EditMode** peut retourner une valeur valide uniquement s’il existe un enregistrement en cours. **EditMode** retourne une erreur si [BOF ou EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) a la valeur true ou si l’enregistrement en cours a été supprimé.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CursorType, LockType et EditMode, exemple de propriétés (VB)](../../../ado/reference/ado-api/cursortype-locktype-and-editmode-properties-example-vb.md)   
 [CursorType, LockType et EditMode, exemples de propriétés (VC + +)](../../../ado/reference/ado-api/cursortype-locktype-and-editmode-properties-example-vc.md)   
 [AddNew, méthode (ADO)](../../../ado/reference/ado-api/addnew-method-ado.md)   
 [Delete, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [CancelUpdate, méthode (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [Update, méthode](../../../ado/reference/ado-api/update-method.md)
