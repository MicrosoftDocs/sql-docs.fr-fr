---
description: GetPermissions, méthode (ADOX)
title: GetPermissions, méthode (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _User25::GetPermissions
- _Group25::raw_GetPermissions
- _Group25::GetPermissions
- _User25::raw_GetPermissions
helpviewer_keywords:
- GetPermissions method [ADOX]
ms.assetid: df201c1f-c76a-465d-98f0-83b7fc36e6e3
author: rothja
ms.author: jroth
ms.openlocfilehash: 4c4d96176e4bdad70cbd5c09a59adcade6f99f81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169391"
---
# <a name="getpermissions-method-adox"></a>GetPermissions, méthode (ADOX)
Retourne les autorisations pour un [groupe](./group-object-adox.md) ou un [utilisateur](./user-object-adox.md) sur un objet ou un conteneur d’objets.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ReturnValue=GroupOrUser.GetPermissions(Name, ObjectType    [,ObjectTypeId])  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur de **type long** qui spécifie un masque de caractères contenant les autorisations dont dispose le groupe ou l’utilisateur sur l’objet. Cette valeur peut être une ou plusieurs des constantes [RightsEnum](./rightsenum.md) .  
  
#### <a name="parameters"></a>Paramètres  
 *Nom*  
 Valeur de **type Variant** qui spécifie le nom de l’objet pour lequel les autorisations doivent être définies. Affectez une valeur null au *nom* si vous souhaitez obtenir les autorisations pour le conteneur d’objets.  
  
 *ObjectType*  
 Valeur de type **long** qui peut être l’une des constantes [ObjectTypeEnum](./objecttypeenum.md) , qui spécifie le type de l’objet pour lequel obtenir des autorisations.  
  
 *ObjectTypeId*  
 Facultatif. Valeur de **type Variant** qui spécifie le GUID pour un type d’objet fournisseur non défini par la spécification OLE DB. Ce paramètre est obligatoire si *ObjectType* a la valeur **adPermObjProviderSpecific**; dans le cas contraire, il n’est pas utilisé.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Group, objet (ADOX)](./group-object-adox.md)  
    :::column-end:::
    :::column:::
        [User, objet (ADOX)](./user-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [GetPermissions et SetPermissions, exemples de méthodes (VB)](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [Name, propriété (ADOX)](./name-property-adox.md)   
 [SetPermissions, méthode (ADOX)](./setpermissions-method-adox.md)