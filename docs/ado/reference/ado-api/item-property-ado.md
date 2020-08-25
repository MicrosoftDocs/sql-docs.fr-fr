---
description: Item, propriété (ADO)
title: Item, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Parameters::GetItem
- Indexes::GetItem
- Parameters::Item
- Tables::Item
- Procedures::Item
- Users::GetItem
- Tables::GetItem
- Procedures::GetItem
- Users::get_Item
- Users::Item
- Views::GetItem
- Groups::Item
- Groups::get_Item
- Columns::Item
- Indexes::Item
- Fields15::GetItem
- Columns::GetItem
- Fields::Item
- Indexes::get_Item
- Columns::get_Item
- Fields15::Item
- Views::get_Item
- Groups::GetItem
- Errors::get_Item
- Fields15::get_Item
- Tables::get_Item
- Views::Item
- Errors::GetItem
- Parameters::get_Item
- Errors::Item
- Procedures::get_Item
helpviewer_keywords:
- Item property [ADO]
ms.assetid: e11484bb-c5c7-42d8-9bb8-21572125d727
author: rothja
ms.author: jroth
ms.openlocfilehash: 6bea35ef148aa2f5646420a1c2b46197ce66f0d6
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88774678"
---
# <a name="item-property-ado"></a>Item, propriété (ADO)
Indique un membre spécifique d’une collection, par nom ou numéro ordinal.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
Set object = collection.Item ( Index )  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une référence d’objet.  
  
## <a name="parameters"></a>Paramètres  
 *Index*  
 Expression **Variant** qui prend pour valeur le nom ou le numéro ordinal d’un objet dans une collection.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Item** pour retourner un objet spécifique dans une collection. Si l' **élément** ne peut pas trouver un objet dans la collection correspondant à l’argument d' *index* , une erreur se produit. En outre, certaines collections ne prennent pas en charge les objets nommés ; pour ces collections, vous devez utiliser des références de nombre ordinal.  
  
 La propriété **Item** est la propriété par défaut pour toutes les collections ; par conséquent, les formes de syntaxe suivantes sont interchangeables :  
  
```  
collection.Item (Index)  
collection (Index)  
```  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Axes, collection (ADO MD)](../ado-md-api/axes-collection-ado-md.md)  
        [Columns, collection (ADOX)](../adox-api/columns-collection-adox.md)  
        [CubeDefs, collection (ADO MD)](../ado-md-api/cubedefs-collection-ado-md.md)  
        [Dimensions, collection (ADO MD)](../ado-md-api/dimensions-collection-ado-md.md)  
        [Errors, collection (ADO)](./errors-collection-ado.md)  
        [Fields, collection (ADO)](./fields-collection-ado.md)  
        [Groups, collection (ADOX)](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Hierarchies, collection (ADO MD)](../ado-md-api/hierarchies-collection-ado-md.md)  
        [Indexes, collection (ADOX)](../adox-api/indexes-collection-adox.md)  
        [Keys, collection (ADOX)](../adox-api/keys-collection-adox.md)  
        [Levels, collection (ADO MD)](../ado-md-api/levels-collection-ado-md.md)  
        [Members, collection (ADO MD)](../ado-md-api/members-collection-ado-md.md)  
        [Parameters, collection (ADO)](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Positions, collection (ADO MD)](../ado-md-api/positions-collection-ado-md.md)  
        [Procedures, collection (ADOX)](../adox-api/procedures-collection-adox.md)  
        [Properties, collection (ADO)](./properties-collection-ado.md)  
        [Tables, collection (ADOX)](../adox-api/tables-collection-adox.md)  
        [Users, collection (ADOX)](../adox-api/users-collection-adox.md)  
        [Views, collection (ADOX)](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Item, exemple de propriété (VB)](./item-property-example-vb.md)   
 [Item, exemple de propriété (VC++)](./item-property-example-vc.md)