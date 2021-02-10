---
description: Users, collection (ADOX)
title: Users, collection (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
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
ms.openlocfilehash: 781f9eb90c621cf8daf4afb0cdf8dc4f1f7b47b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049669"
---
# <a name="users-collection-adox"></a>Users, collection (ADOX)
Contient tous les objets [utilisateur](./user-object-adox.md) stockés d’un [catalogue](./catalog-object-adox.md) ou d’un [groupe](./group-object-adox.md).  
  
## <a name="remarks"></a>Notes  
 La collection d' **utilisateurs** d’un [catalogue](./catalog-object-adox.md) représente tous les utilisateurs du catalogue. Le regroupement **utilisateurs** pour un [groupe](./group-object-adox.md) représente uniquement les utilisateurs qui ont une appartenance au groupe spécifique.  
  
 La méthode [Append](./append-method-adox-users.md) d’une collection **Users** est unique pour ADOX. Vous pouvez :  
  
-   Ajoutez un nouvel utilisateur à la collection à l’aide de la méthode **Append** .  
  
 Les propriétés et les méthodes restantes sont standard pour les collections ADO. Vous pouvez :  
  
-   Accédez à un utilisateur dans la collection avec la propriété [Item](../ado-api/item-property-ado.md) .  
  
-   Retourne le nombre d’utilisateurs contenus dans la collection avec la propriété [Count](../ado-api/count-property-ado.md) .  
  
-   Supprimer un utilisateur de la collection à l’aide de la méthode [Delete](./delete-method-adox-collections.md) .  
  
-   Mettez à jour les objets de la collection pour refléter le schéma de la base de données actuelle avec la méthode [Refresh](../ado-api/refresh-method-ado.md) .  
  
> [!NOTE]
>  Avant d’ajouter un objet **utilisateur** à la collection **Users** d’un objet **Group** , un objet **utilisateur** portant le même [nom](./name-property-adox.md) que celui à ajouter doit déjà exister dans la collection **Users** du **catalogue**.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de la collection Users](./users-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [GetPermissions et SetPermissions, exemples de méthodes (VB)](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [Catalog, objet (ADOX)](./catalog-object-adox.md)   
 [User, objet (ADOX)](./user-object-adox.md)