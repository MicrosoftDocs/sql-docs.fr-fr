---
description: User, objet (ADOX)
title: User, objet (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- User
helpviewer_keywords:
- User object [ADOX]
ms.assetid: f68e32ce-ef7c-407d-bdb5-d280947ae0e2
author: rothja
ms.author: jroth
ms.openlocfilehash: 6e83bd40c226f1d5b5948c6475b259c799907866
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88983049"
---
# <a name="user-object-adox"></a>User, objet (ADOX)
Représente un compte d’utilisateur qui dispose d’autorisations d’accès au sein d’une base de données sécurisée.  
  
## <a name="remarks"></a>Notes  
 La collection d' [utilisateurs](./users-collection-adox.md) d’un [catalogue](./catalog-object-adox.md) représente tous les utilisateurs du catalogue. La collection d' **utilisateurs** d’un [groupe](./group-object-adox.md) représente uniquement les utilisateurs du groupe spécifique.  
  
 Avec les propriétés, collections et méthodes d’un objet **utilisateur** , vous pouvez :  
  
-   Identifiez l’utilisateur à l’aide de la propriété [Name](./name-property-adox.md) .  
  
-   Modifiez le mot de passe d’un utilisateur à l’aide de la méthode [ChangePassword](./changepassword-method-adox.md) .  
  
-   Déterminez si un utilisateur dispose d’autorisations de lecture, d’écriture ou de suppression avec les méthodes [GetPermissions](./getpermissions-method-adox.md) et [SetPermissions](./setpermissions-method-adox.md) .  
  
-   Accédez aux groupes auxquels appartient un utilisateur avec la collection de [groupes](./groups-collection-adox.md) .  
  
-   Accédez aux propriétés spécifiques au fournisseur avec la collection [Properties](../ado-api/properties-collection-ado.md) .  
  
-   Déterminez le [ParentCatalog](./parentcatalog-property-adox.md) d’un utilisateur.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet User](./user-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [GetPermissions et SetPermissions, exemples de méthodes (VB)](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [Groups, collection (ADOX)](./groups-collection-adox.md)   
 [Users, collection (ADOX)](./users-collection-adox.md)