---
title: Update, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset15::Update
helpviewer_keywords:
- Update method [ADO]
ms.assetid: 6b2a9c31-1a7e-40db-8a53-30720d0f6cc1
author: rothja
ms.author: jroth
ms.openlocfilehash: aeeb7bcbb79231a2d6ed842459355a09e2889e4f
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87243170"
---
# <a name="update-method"></a>Update, méthode
Enregistre les modifications apportées à la ligne actuelle d’un objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) ou à la collection [Fields](../../../ado/reference/ado-api/fields-collection-ado.md) d’un objet [Record](../../../ado/reference/ado-api/record-object-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Update Fields, Values  
record.Fields.Update  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Fields*  
 facultatif. Valeur de **type Variant** qui représente un nom unique ou un tableau de **variants** qui représente des noms ou des positions ordinales du ou des champs que vous souhaitez modifier.  
  
 *Valeurs*  
 facultatif. **Variant** qui représente une valeur unique ou un tableau de **variants** qui représente des valeurs pour le ou les champs dans le nouvel enregistrement.  
  
## <a name="remarks"></a>Notes  
  
## <a name="recordset"></a>Ensemble d'enregistrements  
 Utilisez la méthode **Update** pour enregistrer les modifications apportées à l’enregistrement actuel d’un objet **Recordset** depuis l’appel de la méthode [AddNew](../../../ado/reference/ado-api/addnew-method-ado.md) ou depuis la modification des valeurs de champ dans un enregistrement existant. L’objet **Recordset** doit prendre en charge les mises à jour.  
  
 Pour définir des valeurs de champ, effectuez l’une des opérations suivantes :  
  
-   Assignez des valeurs à la propriété [value](../../../ado/reference/ado-api/value-property-ado.md) d’un objet [Field](../../../ado/reference/ado-api/field-object.md) et appelez la méthode **Update** .  
  
-   Transmettez un nom de champ et une valeur comme arguments avec l’appel de **mise à jour** .  
  
-   Passe un tableau de noms de champs et un tableau de valeurs avec l’appel de **mise à jour** .  
  
 Lorsque vous utilisez des tableaux de champs et de valeurs, il doit y avoir un nombre égal d’éléments dans les deux tableaux. En outre, l’ordre des noms de champs doit correspondre à l’ordre des valeurs de champ. Si le nombre et l’ordre des champs et des valeurs ne correspondent pas, une erreur se produit.  
  
 Si l’objet **Recordset** prend en charge la mise à jour par lot, vous pouvez mettre en cache plusieurs modifications dans un ou plusieurs enregistrements localement jusqu’à ce que vous appeliez la méthode [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) . Si vous modifiez l’enregistrement en cours ou si vous ajoutez un nouvel enregistrement lorsque vous appelez la méthode **UpdateBatch** , ADO appelle automatiquement la méthode **Update** pour enregistrer toutes les modifications en attente dans l’enregistrement en cours avant de transmettre les modifications par lot au fournisseur.  
  
 Si vous effectuez un déplacement à partir de l’enregistrement que vous ajoutez ou modifiez avant d’appeler la méthode **Update** , ADO appellera automatiquement **Update** pour enregistrer les modifications. Vous devez appeler la méthode [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) si vous voulez annuler les modifications apportées à l’enregistrement en cours ou ignorer un enregistrement qui vient d’être ajouté.  
  
 L’enregistrement actif reste actif après l’appel de la méthode **Update** .  
  
## <a name="record"></a>Enregistrement  
 La méthode **Update** finalise les ajouts, les suppressions et les mises à jour des champs de la collection [Fields](../../../ado/reference/ado-api/fields-collection-ado.md) d’un objet **Record** .  
  
 Par exemple, les champs supprimés à l’aide de la méthode **Delete** sont marqués pour suppression immédiatement, mais restent dans la collection. La méthode **Update** doit être appelée pour supprimer réellement ces champs de la collection du fournisseur.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Fields, collection (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Update et CancelUpdate, exemple de méthodes (VB)](../../../ado/reference/ado-api/update-and-cancelupdate-methods-example-vb.md)   
 [Update et CancelUpdate, exemple de méthodes (VC + +)](../../../ado/reference/ado-api/update-and-cancelupdate-methods-example-vc.md)   
 [AddNew, méthode (ADO)](../../../ado/reference/ado-api/addnew-method-ado.md)   
 [CancelUpdate, méthode (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [EditMode, propriété](../../../ado/reference/ado-api/editmode-property.md)   
 [UpdateBatch, méthode](../../../ado/reference/ado-api/updatebatch-method.md)
