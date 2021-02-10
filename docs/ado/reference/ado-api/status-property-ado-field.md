---
description: Status, propriété (objet Field ADO)
title: Status, propriété (champ ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field::Status
- Field::get_Status
- Field::GetStatus
helpviewer_keywords:
- Status property [ADO Field]
ms.assetid: 8cd1f7f4-0a3a-4f07-b8ba-6582e70140ad
author: rothja
ms.author: jroth
ms.openlocfilehash: d8ec88ad85b4f0432ce4740e8f54647640f2e7bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040609"
---
# <a name="status-property-ado-field"></a>Status, propriété (objet Field ADO)
Indique l’état d’un objet de [champ](./field-object.md) .  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur [FieldStatusEnum](./fieldstatusenum.md) . La valeur par défaut est **adFieldOK**.  
  
## <a name="remarks"></a>Notes  
  
## <a name="record-field-status"></a>État du champ d’enregistrement  
 Les modifications apportées à la valeur d’un objet **champ** dans la collection Fields d’un objet [Record](./record-object-ado.md) sont mises en cache jusqu’à ce que la méthode [Update](./update-method.md) de l’objet soit appelée. À ce stade, si la modification de la valeur du champ est à l’origine d’une erreur, OLE DB génère l’erreur **DB_E_ERRORSOCCURRED** (2147749409). La propriété Status de l’un des objets **Field** de la collection **Fields** à l’origine de l’erreur contient une valeur du [FieldStatusEnum](./fieldstatusenum.md) décrivant la cause du problème.  
  
 Pour améliorer les performances, les ajouts et les suppressions dans les collections [Fields](./fields-collection-ado.md) de l’objet **Record** sont mis en cache jusqu’à ce que la méthode **Update** soit appelée, puis les modifications sont apportées dans une mise à jour optimiste par lot. Si la méthode **Update** n’est pas appelée, le serveur n’est pas mis à jour. Si des mises à jour échouent, une erreur de fournisseur de OLE DB (DB_E_ERRORSOCCURRED) est retournée et la propriété **Status** indique les valeurs combinées de l’opération et du code d’état d’erreur. Par exemple, **ADFIELDPENDINGINSERT ou adFieldPermissionDenied**. La propriété **Status** de chaque **champ** peut être utilisée pour déterminer la raison pour laquelle le **champ** n’a pas été ajouté, modifié ou supprimé.  
  
 De nombreux types de problèmes rencontrés lors de l’ajout, de la modification ou de la suppression d’un **champ** sont signalés via la propriété **Status** . Par exemple, si l’utilisateur supprime un **champ**, il est marqué pour suppression de la collection de **champs** . Si la **mise à jour** suivante retourne une erreur parce que l’utilisateur a tenté de supprimer un **champ** pour lequel il n’a pas d’autorisation, le **champ** aura l' **État** **adFieldPermissionDenied ou adFieldPendingDelete**. L’appel de la méthode [CancelUpdate](./cancelupdate-method-ado.md) restaure les valeurs d’origine et définit l' **État** sur **adFieldOK**.  
  
 De même, la méthode **Update** peut retourner une erreur, car un nouveau **champ** a été ajouté et une valeur incorrecte lui est affectée. Dans ce cas, le nouveau **champ** se trouve dans la collection de **champs** et a l’état **AdFieldPendingInsert** et peut-être **adFieldCantCreate** (selon votre fournisseur). Vous pouvez fournir une valeur appropriée pour le nouveau **champ** et appeler **à nouveau Update** .  
  
## <a name="recordset-field-status"></a>État du champ du Recordset  
 Les modifications apportées à la valeur d’un objet **champ** dans la collection Fields d’un [Recordset](./recordset-object-ado.md) sont mises en cache jusqu’à ce que la méthode [Update](./update-method.md) de l’objet soit appelée. À ce stade, si la modification de la valeur du champ est à l’origine d’une erreur, OLE DB génère l’erreur **DB_E_ERRORSOCCURRED** (2147749409). La propriété Status de l’un des objets **Field** de la collection **Fields** à l’origine de l’erreur contient une valeur du [FieldStatusEnum](./fieldstatusenum.md) décrivant la cause du problème.  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Field](./field-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Status, exemple de propriété (Field) (VB)](./status-property-example-field-vb.md)   
 [Status, exemple de propriété (VC++)](./status-property-example-vc.md)