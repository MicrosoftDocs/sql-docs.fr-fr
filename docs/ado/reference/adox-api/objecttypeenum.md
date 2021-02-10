---
description: ObjectTypeEnum
title: ObjectTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ObjectTypeEnum
helpviewer_keywords:
- ObjectTypeEnum enumeration [ADOX]
ms.assetid: 3fdecfca-aa91-4596-ad98-610f1b7f840b
author: rothja
ms.author: jroth
ms.openlocfilehash: 9a5ff3f235a01bb84a05b9c4efdff5c1fd15b7c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049889"
---
# <a name="objecttypeenum"></a>ObjectTypeEnum
Spécifie le type d’objet de base de données pour lequel définir des autorisations ou la propriété.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adPermObjColumn**|2|L’objet est une colonne.|  
|**adPermObjDatabase**|3|L’objet est une base de données.|  
|**adPermObjProcedure**|4|L’objet est une procédure.|  
|**adPermObjProviderSpecific**|-1|L’objet est un type défini par le fournisseur. Une erreur se produit si le paramètre *ObjectType* est **adPermObjProviderSpecific** et qu’un *ObjectTypeId* n’est pas fourni.|  
|**adPermObjTable**|1|L’objet est une table.|  
|**adPermObjView**|5|L’objet est une vue.|  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [GetObjectOwner, méthode (ADOX)](./getobjectowner-method-adox.md)  
        [GetPermissions, méthode (ADOX)](./getpermissions-method-adox.md)  
    :::column-end:::
    :::column:::
        [SetObjectOwner, méthode](./setobjectowner-method.md)  
        [SetPermissions, méthode (ADOX)](./setpermissions-method-adox.md)  
    :::column-end:::
:::row-end:::