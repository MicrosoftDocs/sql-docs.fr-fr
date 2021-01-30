---
description: CancelUpdate, méthode (ADO)
title: CancelUpdate, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::CancelUpdate
helpviewer_keywords:
- CancelUpdate method [ADO]
ms.assetid: eaa856cc-c786-462e-890c-c896261b1741
author: rothja
ms.author: jroth
ms.openlocfilehash: 12ec3d30de9fc938d51ea342c468b522674b0086
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167794"
---
# <a name="cancelupdate-method-ado"></a>CancelUpdate, méthode (ADO)
Annule toutes les modifications apportées à la ligne actuelle ou nouvelle d’un objet [Recordset](./recordset-object-ado.md) , ou à la collection [Fields](./fields-collection-ado.md) d’un objet [Record](./record-object-ado.md) , avant d’appeler la méthode [Update](./update-method.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.CancelUpdaterecord.Fields.CancelUpdate  
```  
  
## <a name="remarks"></a>Notes  
  
## <a name="recordset"></a>Ensemble d'enregistrements  
 Utilisez la méthode **CancelUpdate** pour annuler les modifications apportées à la ligne actuelle ou pour ignorer une ligne qui vient d’être ajoutée. Vous ne pouvez pas annuler les modifications apportées à la ligne actuelle ou à une nouvelle ligne après avoir appelé la méthode **Update** , à moins que les modifications ne fassent partie d’une transaction que vous pouvez restaurer avec la méthode [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) , ou faire partie d’une mise à jour par lot. Dans le cas d’une mise à jour par lot, vous pouvez annuler la **mise à jour** avec la méthode **CancelUpdate** ou [CancelBatch](./cancelbatch-method-ado.md) .  
  
 Si vous ajoutez une nouvelle ligne lorsque vous appelez la méthode **CancelUpdate** , la ligne active devient la ligne qui était en cours avant l’appel [AddNew](./addnew-method-ado.md) .  
  
 Si vous êtes en mode édition et que vous souhaitez quitter l’enregistrement actif (par exemple, à l’aide des méthodes [Move](./move-method-ado.md), [NextRecordset](./nextrecordset-method-ado.md)ou [Close](./close-method-ado.md) ), vous pouvez utiliser **CancelUpdate** pour annuler les modifications en attente. Vous devrez peut-être effectuer cette opération si la mise à jour ne peut pas être publiée avec succès dans la source de données. Par exemple, une tentative de suppression qui échoue en raison de violations d’intégrité référentielle laisse le **Recordset** en mode édition après un appel à [Delete](./delete-method-ado-recordset.md).  
  
## <a name="record"></a>Enregistrer  
 La méthode **CancelUpdate** annule les insertions ou les suppressions en attente des objets [Field](./field-object.md) , et annule les mises à jour en attente des champs existants et les restaure à leurs valeurs d’origine. La propriété [Status](./status-property-ado-recordset.md) de tous les champs de la collection **Fields** a la valeur **adFieldOK**.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Fields, collection (ADO)](./fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Recordset, objet (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Update et CancelUpdate, exemple de méthodes (VB)](./update-and-cancelupdate-methods-example-vb.md)   
 [Update et CancelUpdate, exemple de méthodes (VC + +)](./update-and-cancelupdate-methods-example-vc.md)   
 [AddNew, méthode (ADO)](./addnew-method-ado.md)   
 [Cancel, méthode (ADO)](./cancel-method-ado.md)   
 [Cancel, méthode (RDS)](../rds-api/cancel-method-rds.md)   
 [Méthode CancelBatch (ADO)](./cancelbatch-method-ado.md)   
 [CancelUpdate, méthode (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [EditMode, propriété](./editmode-property.md)   
 [Update, méthode](./update-method.md)