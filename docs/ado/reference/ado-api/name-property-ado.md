---
description: Name, propriété (ADO)
title: Name, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Name
- Field20::Name
helpviewer_keywords:
- Name property [ADO]
ms.assetid: cfd0e29c-8310-44ab-85c3-5761184b865d
author: rothja
ms.author: jroth
ms.openlocfilehash: 384c1e96eb735d8cf9569039f45c2d420679ce0e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167065"
---
# <a name="name-property-ado"></a>Name, propriété (ADO)
Indique le nom d’un objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **chaîne** qui indique le nom d’un objet.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Name** pour attribuer un nom à une **commande**, une **propriété**, un **champ** ou un objet **Parameter** ou pour en récupérer le nom.  
  
 La valeur est en lecture/écriture sur un objet **Command** et en lecture seule sur un objet **Property** .  
  
 Pour un objet **Field** , **Name** est généralement en lecture seule. Toutefois, pour les nouveaux objets de **champ** qui ont été ajoutés à la collection de [champs](./fields-collection-ado.md) d’un [enregistrement](./record-object-ado.md), le **nom** est en lecture/écriture uniquement après la spécification de la propriété [value](./value-property-ado.md) du **champ** et que le fournisseur de données a correctement ajouté le nouveau **champ** en appelant la méthode [Update](./update-method.md) de la collection **Fields** .  
  
 Pour les objets de **paramètre** qui n’ont pas encore été ajoutés à la collection [Parameters](./parameters-collection-ado.md) , la propriété **Name** est en lecture/écriture. Pour les objets de **paramètre** ajoutés et tous les autres objets, la propriété **Name** est en lecture seule. Les noms ne doivent pas nécessairement être uniques dans une collection.  
  
 Vous pouvez récupérer la propriété **Name** d’un objet à l’aide d’une référence ordinale, après laquelle vous pouvez faire référence à l’objet directement par son nom. Par exemple, si `rstMain.Properties(20).Name` donne `Updatability` , vous pouvez ensuite faire référence à cette propriété en tant que `rstMain.Properties("Updatability")` .  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Command, objet (ADO)](./command-object-ado.md)  
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