---
description: Status, propriété (objet Recordset ADO)
title: Status, propriété (objet Recordset ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset15::GetStatus
- Recordset15::Status
helpviewer_keywords:
- Status property [ADO Recordset]
ms.assetid: 41d70d89-880f-4850-9d17-19d9790cc8eb
author: rothja
ms.author: jroth
ms.openlocfilehash: 7869ff91269d033b14a7f77e014da70962a1c1d5
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88441931"
---
# <a name="status-property-ado-recordset"></a>Status, propriété (objet Recordset ADO)
Indique l’état de l’enregistrement en cours par rapport aux mises à jour par lots ou à d’autres opérations en bloc.  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne la somme d’une ou plusieurs valeurs [RecordStatusEnum](../../../ado/reference/ado-api/recordstatusenum.md) .  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Status** pour voir les modifications en attente pour les enregistrements modifiés lors de la mise à jour par lot. Vous pouvez également utiliser la propriété **Status** pour afficher l’état des enregistrements qui échouent pendant les opérations en bloc, par exemple lorsque vous appelez les méthodes [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)ou [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) sur un objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) , ou lorsque vous affectez à la propriété [Filter](../../../ado/reference/ado-api/filter-property.md) d’un objet **Recordset** la valeur d’un tableau de signets. Avec cette propriété, vous pouvez déterminer le mode d’échec d’un enregistrement donné et le résoudre en conséquence.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Status, exemple de propriété (Recordset) (VB)](../../../ado/reference/ado-api/status-property-example-recordset-vb.md)   
 [Status, exemple de propriété (VC++)](../../../ado/reference/ado-api/status-property-example-vc.md)   
