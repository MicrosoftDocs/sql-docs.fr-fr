---
title: SetPermissions, méthode (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- User25::SetPermissions
- User25::raw_SetPermissions
- _Group25::SetPermissions
- _Group25::raw_SetPermissions
helpviewer_keywords:
- SetPermissions method [ADOX]
ms.assetid: b7f925d7-b05c-4376-bb49-f8d2c17b8b24
author: rothja
ms.author: jroth
ms.openlocfilehash: 4287e836dd46c579ee81a4baf0e175e8b49acf67
ms.sourcegitcommit: 591bbf4c7e4e2092f8abda6a2ffed263cb61c585
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86942637"
---
# <a name="setpermissions-method-adox"></a>SetPermissions, méthode (ADOX)
Spécifie les autorisations pour un [groupe](../../../ado/reference/adox-api/group-object-adox.md) ou un [utilisateur](../../../ado/reference/adox-api/user-object-adox.md) sur un objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
GroupOrUser.SetPermissions Name, ObjectType, Action, Rights [, Inherit] [, ObjectTypeId]  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Nom*  
 Valeur de **chaîne** qui spécifie le nom de l’objet pour lequel les autorisations doivent être définies.  
  
 *ObjectType*  
 Valeur de type **long** qui peut être l’une des constantes [ObjectTypeEnum](../../../ado/reference/adox-api/objecttypeenum.md) , qui spécifie le type de l’objet pour lequel obtenir des autorisations.  
  
 *Action*  
 Valeur de type **long** qui peut être l’une des constantes [ActionEnum](../../../ado/reference/adox-api/actionenum.md) qui spécifient le type d’action à effectuer lors de la définition des autorisations.  
  
 *Droits*  
 Valeur de **type long** qui peut être un masque de caractères d’une ou plusieurs des constantes [RightsEnum](../../../ado/reference/adox-api/rightsenum.md) , qui indique les droits à définir.  
  
 *Être*  
 Optionnel. Valeur de **type long** qui peut être l’une des constantes [InheritTypeEnum](../../../ado/reference/adox-api/inherittypeenum.md) , qui spécifie comment les objets hériteront de ces autorisations. La valeur par défaut est **adInheritNone**.  
  
 *ObjectTypeId*  
 Optionnel. Valeur de **type Variant** qui spécifie le GUID pour un type d’objet fournisseur qui n’est pas défini par la spécification OLE DB. Ce paramètre est obligatoire si *ObjectType* a la valeur **adPermObjProviderSpecific**; dans le cas contraire, il n’est pas utilisé.  
  
## <a name="remarks"></a>Notes  
 Une erreur se produit si le fournisseur ne prend pas en charge la définition des droits d’accès pour les groupes ou les utilisateurs.  
  
> [!NOTE]
>  Lors de l’appel de **SetPermissions**, la définition des actions sur **adAccessRevoke** remplace tous les paramètres du paramètre *Rights* . Ne définissez pas d' *actions* sur **adAccessRevoke** si vous souhaitez que les droits spécifiés dans le paramètre *Rights* prennent effet.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Group, objet (ADOX)](../../../ado/reference/adox-api/group-object-adox.md)  
    :::column-end:::
    :::column:::
        [User, objet (ADOX)](../../../ado/reference/adox-api/user-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [GetPermissions et SetPermissions, exemples de méthodes (VB)](../../../ado/reference/adox-api/getpermissions-and-setpermissions-methods-example-vb.md)   
 [GetPermissions, méthode (ADOX)](../../../ado/reference/adox-api/getpermissions-method-adox.md)   
 [Name, propriété (ADOX)](../../../ado/reference/adox-api/name-property-adox.md)
