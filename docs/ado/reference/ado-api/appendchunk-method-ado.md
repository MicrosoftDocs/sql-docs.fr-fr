---
description: AppendChunk, méthode (ADO)
title: AppendChunk, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::AppendChunk
- Field20::AppendChunk
helpviewer_keywords:
- AppendChunk method [ADO]
ms.assetid: c648b5a8-d4f1-4d16-836e-3957feb03617
author: rothja
ms.author: jroth
ms.openlocfilehash: b7abcebce9f587260d7147745ab99c2b4cecc78a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167876"
---
# <a name="appendchunk-method-ado"></a>AppendChunk, méthode (ADO)
Ajoute des données à un [champ](./field-object.md)de données de texte ou binaires volumineux, ou à un objet de [paramètre](./parameter-object.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.AppendChunk Data  
```  
  
#### <a name="parameters"></a>Paramètres  
 *object*  
 **Champ** ou objet de **paramètre** .  
  
 *Données*  
 **Variant** qui contient les données à ajouter à l’objet.  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **AppendChunk** sur un objet **champ** ou **paramètre** pour la remplir avec des données de type binaire ou caractère. Dans les situations où la mémoire système est limitée, vous pouvez utiliser la méthode **AppendChunk** pour manipuler des valeurs longues dans des parties plutôt que dans leur intégralité.  
  
## <a name="field"></a>Champ  
 Si le **bit adFldLong** dans la propriété [attributes](./attributes-property-ado.md) d’un objet de **champ** a la valeur **true**, vous pouvez utiliser la méthode **AppendChunk** pour ce champ.  
  
 Le premier appel de **AppendChunk** sur un objet de **champ** écrit des données dans le champ, en remplaçant toutes les données existantes. Les appels **AppendChunk** suivants sont ajoutés aux données existantes. Si vous ajoutez des données à un champ, puis que vous définissez ou lisez la valeur d’un autre champ dans l’enregistrement actif, ADO suppose que vous avez terminé d’ajouter des données au premier champ. Si vous rappelez la méthode **AppendChunk** sur le premier champ, ADO interprète l’appel comme une nouvelle opération **AppendChunk** et remplace les données existantes. L’accès aux champs d’autres objets [Recordset](./recordset-object-ado.md) qui ne sont pas des clones du premier objet **Recordset** n’interrompt pas les opérations **AppendChunk** .  
  
 S’il n’y a pas d’enregistrement actif quand vous appelez **AppendChunk** sur un objet de **champ** , une erreur se produit.  
  
> [!NOTE]
>  La méthode **AppendChunk** ne fonctionne pas sur les objets **Field** d’un objet [Record Object (ADO)](./record-object-ado.md) . Elle n’exécute aucune opération et génère une erreur d’exécution.  
  
## <a name="parameter"></a>Paramètre  
 Si le **bit adParamLong** dans la propriété **attributes** d’un objet **Parameter** a la valeur **true**, vous pouvez utiliser la méthode **AppendChunk** pour ce paramètre.  
  
 Le premier appel de **AppendChunk** sur un objet **Parameter** écrit des données dans le paramètre, en remplaçant toutes les données existantes. Les appels **AppendChunk** suivants sur un objet **Parameter** sont ajoutés aux données de paramètres existantes. Un appel de **AppendChunk** qui transmet une valeur null ignore toutes les données de paramètre.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Objet Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Objet Parameter](./parameter-object.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [AppendChunk et GetChunk, exemple de méthodes (VB)](./appendchunk-and-getchunk-methods-example-vb.md)   
 [AppendChunk et GetChunk, exemple de méthodes (VC + +)](./appendchunk-and-getchunk-methods-example-vc.md)   
 [Attributes, propriété (ADO)](./attributes-property-ado.md)   
 [GetChunk, méthode (ADO)](./getchunk-method-ado.md)