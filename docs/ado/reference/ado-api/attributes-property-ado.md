---
description: Attributes, propriété (ADO)
title: Attributes, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Attributes
- Field20::Attributes
- _Parameter::Attributes
helpviewer_keywords:
- Attributes property [ADO]
ms.assetid: acc15d40-68a6-4ba9-85bd-12d331aecaa6
author: rothja
ms.author: jroth
ms.openlocfilehash: 72cbd650b2ab3c66231383e89b048bb03c42b6f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164805"
---
# <a name="attributes-property-ado"></a>Attributes, propriété (ADO)
Indique une ou plusieurs caractéristiques d’un objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **type long** .  
  
 Pour un objet de [connexion](./connection-object-ado.md) , la propriété **attributes** est en lecture/écriture, et sa valeur peut être la somme d’une ou plusieurs valeurs [XactAttributeEnum](./xactattributeenum.md) . La valeur par défaut est zéro (0).  
  
 Pour un objet [Parameter](./parameter-object.md) , la propriété **attributes** est en lecture/écriture, et sa valeur peut être la somme d’une ou plusieurs valeurs [ParameterAttributesEnum](./parameterattributesenum.md) . La valeur par défaut est **adParamSigned**.  
  
 Pour un objet [Field](./field-object.md) , la propriété **attributes** peut être la somme d’une ou plusieurs valeurs [FieldAttributeEnum](./fieldattributeenum.md) . Il est généralement en lecture seule. Toutefois, pour les nouveaux objets de **champ** qui ont été ajoutés à la collection de [champs](./fields-collection-ado.md) d’un [enregistrement](./record-object-ado.md), les **attributs** sont en lecture/écriture uniquement une fois que la propriété [value](./value-property-ado.md) du **champ** a été spécifiée et que le nouveau **champ** a été ajouté avec succès par le fournisseur de données en appelant la méthode [Update](./update-method.md) de la collection **Fields** .  
  
 Pour un objet de [propriété](./property-object-ado.md) , la propriété **attributes** est en lecture seule et sa valeur peut être la somme d’une ou plusieurs valeurs [PropertyAttributesEnum](./propertyattributesenum.md) .  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **attributes** pour définir ou retourner des caractéristiques d’objets de **connexion** , d’objets de **paramètres** , d’objets de **champ** ou d’objets de **propriété** .  
  
 Lorsque vous définissez plusieurs attributs, vous pouvez additionner les constantes appropriées. Si vous définissez la valeur de la propriété sur une somme incluant des constantes non compatibles, une erreur se produit.  
  
> [!NOTE]
>  **Utilisation des services de données distants** Cette propriété n’est pas disponible sur un objet de **connexion** côté client.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Connection, objet (ADO)](./connection-object-ado.md)  
        [Objet Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Objet Parameter](./parameter-object.md)  
        [Property, objet (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Attributes et Name, exemple de propriétés (VB)](./attributes-and-name-properties-example-vb.md)   
 [Attributes et Name, exemple de propriétés (VC + +)](./attributes-and-name-properties-example-vc.md)   
 [AppendChunk, méthode (ADO)](./appendchunk-method-ado.md)   
 [BeginTrans, CommitTrans et RollbackTrans, méthodes (ADO)](./begintrans-committrans-and-rollbacktrans-methods-ado.md)   
 [GetChunk, méthode (ADO)](./getchunk-method-ado.md)