---
description: Users, collection (ADOX)
title: Users, collection (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Group::Users
- Users
- Catalog::Users
helpviewer_keywords:
- Users collection [ADOX]
ms.assetid: 0a30fa74-6f10-4410-bd70-882e7c43cd46
author: rothja
ms.author: jroth
ms.openlocfilehash: e69ecbf642982d6465c12e225f45199a0c1b33e0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88439371"
---
# <a name="users-collection-adox"></a>Users, collection (ADOX)
Contient tous les objets [utilisateur](../../../ado/reference/adox-api/user-object-adox.md) stockés d’un [catalogue](../../../ado/reference/adox-api/catalog-object-adox.md) ou d’un [groupe](../../../ado/reference/adox-api/group-object-adox.md).  
  
## <a name="remarks"></a>Notes  
 La collection d' **utilisateurs** d’un [catalogue](../../../ado/reference/adox-api/catalog-object-adox.md) représente tous les utilisateurs du catalogue. Le regroupement **utilisateurs** pour un [groupe](../../../ado/reference/adox-api/group-object-adox.md) représente uniquement les utilisateurs qui ont une appartenance au groupe spécifique.  
  
 La méthode [Append](../../../ado/reference/adox-api/append-method-adox-users.md) d’une collection **Users** est unique pour ADOX. Vous pouvez :  
  
-   Ajoutez un nouvel utilisateur à la collection à l’aide de la méthode **Append** .  
  
 Les propriétés et les méthodes restantes sont standard pour les collections ADO. Vous pouvez :  
  
-   Accédez à un utilisateur dans la collection avec la propriété [Item](../../../ado/reference/ado-api/item-property-ado.md) .  
  
-   Retourne le nombre d’utilisateurs contenus dans la collection avec la propriété [Count](../../../ado/reference/ado-api/count-property-ado.md) .  
  
-   Supprimer un utilisateur de la collection à l’aide de la méthode [Delete](../../../ado/reference/adox-api/delete-method-adox-collections.md) .  
  
-   Mettez à jour les objets de la collection pour refléter le schéma de la base de données actuelle avec la méthode [Refresh](../../../ado/reference/ado-api/refresh-method-ado.md) .  
  
> [!NOTE]
>  Avant d’ajouter un objet **utilisateur** à la collection **Users** d’un objet **Group** , un objet **utilisateur** portant le même [nom](../../../ado/reference/adox-api/name-property-adox.md) que celui à ajouter doit déjà exister dans la collection **Users** du **catalogue**.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de la collection Users](../../../ado/reference/adox-api/users-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [GetPermissions et SetPermissions, exemples de méthodes (VB)](../../../ado/reference/adox-api/getpermissions-and-setpermissions-methods-example-vb.md)   
 [Catalog, objet (ADOX)](../../../ado/reference/adox-api/catalog-object-adox.md)   
 [User, objet (ADOX)](../../../ado/reference/adox-api/user-object-adox.md)
