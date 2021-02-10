---
description: Type, propriété (ADO)
title: Type, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Type
- Field20::Type
helpviewer_keywords:
- Type property [ADO]
ms.assetid: 8a4c079f-9f4f-4545-801d-85983b8db71e
author: rothja
ms.author: jroth
ms.openlocfilehash: feb964fbc6cdd59be000e82c4a58b359cfc23d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056434"
---
# <a name="type-property-ado"></a>Type, propriété (ADO)
Indique le type opérationnel ou le type de données d’un [paramètre](./parameter-object.md), d’un [champ](./field-object.md)ou d’un objet de [propriété](./property-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur [DataTypeEnum](./datatypeenum.md) .  
  
## <a name="remarks"></a>Notes  
 Pour les objets de **paramètre** , la propriété de **type** est en lecture/écriture. Pour les nouveaux objets de **champ** qui ont été ajoutés à la collection de [champs](./fields-collection-ado.md) d’un [enregistrement](./record-object-ado.md), le **type** est lecture/écriture uniquement lorsque la propriété [valeur](./value-property-ado.md) du **champ** a été spécifiée et que le fournisseur de données a correctement ajouté le nouveau **champ** en appelant la méthode [Update](./update-method.md) de la collection **Fields** .  
  
 Pour tous les autres objets, la propriété **type** est en lecture seule.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Objet Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Objet Parameter](./parameter-object.md)  
    :::column-end:::
    :::column:::
        [Property, objet (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Type, exemple de propriété (Field) (VB)](./type-property-example-field-vb.md)   
 [Type, exemple de propriété (propriété) (VC + +)](./type-property-example-property-vc.md)   
 [RecordType, propriété (ADO)](./recordtype-property-ado.md)   
 [Type, propriété (objet Stream ADO)](./type-property-ado-stream.md)