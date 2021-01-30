---
description: Append, méthode (utilisateurs ADOX)
title: Append, méthode (utilisateurs ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Users::raw_Append
- Users::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: b80bc5d5-78ca-4f75-956b-2ac658029cc7
author: rothja
ms.author: jroth
ms.openlocfilehash: e321d3cf41e8edcd5a5bd29452050542257a908b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169567"
---
# <a name="append-method-adox-users"></a>Append, méthode (utilisateurs ADOX)
Ajoute un nouvel objet [utilisateur](./user-object-adox.md) à la collection [Users](./users-collection-adox.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Users.Append User[,Password]  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Utilisateur*  
 Valeur de **type Variant** qui contient l’objet **utilisateur** à ajouter ou le nom de l’utilisateur à créer et à ajouter.  
  
 *Mot de passe*  
 Facultatif. Valeur de **chaîne** qui contient le mot de passe de l’utilisateur. Le paramètre de *mot de passe* correspond à la valeur spécifiée par la méthode [ChangePassword](./changepassword-method-adox.md) d’un objet **utilisateur** .  
  
## <a name="remarks"></a>Notes  
 La collection d' **utilisateurs** d’un [catalogue](./catalog-object-adox.md) représente tous les utilisateurs du catalogue. Le regroupement **utilisateurs** pour un [groupe](./group-object-adox.md) représente uniquement les utilisateurs qui ont une appartenance au groupe spécifique.  
  
 Une erreur se produit si le fournisseur ne prend pas en charge la création d’utilisateurs.  
  
> [!NOTE]
>  Avant d’ajouter un objet **utilisateur** à la collection **Users** d’un objet **Group** , un objet **utilisateur** portant le même [nom](./name-property-adox.md) que celui à ajouter doit déjà exister dans la collection **Users** du **catalogue**.  
  
## <a name="applies-to"></a>S'applique à  
 [Users, collection (ADOX)](./users-collection-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Groups and Users Append, ChangePassword, exemple de méthodes (VB)](./groups-and-users-append-changepassword-methods-example-vb.md)   
 [Append, méthode (colonnes ADOX)](./append-method-adox-columns.md)   
 [Append, méthode (groupes ADOX)](./append-method-adox-groups.md)   
 [Append, méthode (Index ADOX)](./append-method-adox-indexes.md)   
 [Append, méthode (clés ADOX)](./append-method-adox-keys.md)   
 [Append, méthode (procédures ADOX)](./append-method-adox-procedures.md)   
 [Append, méthode (Tables ADOX)](./append-method-adox-tables.md)   
 [Append, méthode (vues ADOX)](./append-method-adox-views.md)