---
description: Append, méthode (groupes ADOX)
title: Append, méthode (groupes ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Groups::raw_Append
- Groups::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 56b94fc6-7ef0-4e4a-82a3-033b94c46036
author: rothja
ms.author: jroth
ms.openlocfilehash: 846475567fb15e549f96094ca55a1881d8888ce5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050530"
---
# <a name="append-method-adox-groups"></a>Append, méthode (groupes ADOX)
Ajoute un nouvel objet de [groupe](./group-object-adox.md) à la collection de [groupes](./groups-collection-adox.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Groups.Append Group  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Groupe*  
 Objet de **groupe** à ajouter ou nom du groupe à créer et à ajouter.  
  
## <a name="remarks"></a>Notes  
 La collection **groups** d’un [catalogue](./catalog-object-adox.md) représente tous les comptes de groupe du catalogue. La collection **groups** pour un [utilisateur](./user-object-adox.md) représente uniquement le groupe auquel appartient l’utilisateur.  
  
 Une erreur se produit si le fournisseur ne prend pas en charge la création de groupes.  
  
> [!NOTE]
>  Avant d’ajouter un objet **groupe** à la collection **groups** d’un objet **User** , un objet **Group** portant le même [nom](./name-property-adox.md) que celui à ajouter doit déjà exister dans la collection **groups** du **catalogue**.  
  
## <a name="applies-to"></a>S'applique à  
 [Groups, collection (ADOX)](./groups-collection-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Groups and Users Append, ChangePassword, exemple de méthodes (VB)](./groups-and-users-append-changepassword-methods-example-vb.md)   
 [Append, méthode (colonnes ADOX)](./append-method-adox-columns.md)   
 [Append, méthode (Index ADOX)](./append-method-adox-indexes.md)   
 [Append, méthode (clés ADOX)](./append-method-adox-keys.md)   
 [Append, méthode (procédures ADOX)](./append-method-adox-procedures.md)   
 [Append, méthode (Tables ADOX)](./append-method-adox-tables.md)   
 [Append, méthode (utilisateurs ADOX)](./append-method-adox-users.md)   
 [Append, méthode (vues ADOX)](./append-method-adox-views.md)