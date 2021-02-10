---
description: Append, méthode (index ADOX)
title: Append, méthode (Index ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Indexes::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 6695769f-275b-4b70-81bd-1a5f7d74926c
author: rothja
ms.author: jroth
ms.openlocfilehash: feb259809798e3260e4e201c4658bfbc1455f168
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050540"
---
# <a name="append-method-adox-indexes"></a>Append, méthode (index ADOX)
Ajoute un nouvel objet [index](./index-object-adox.md) à la collection d' [index](./indexes-collection-adox.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Indexes.Append Index [,Columns]  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Index*  
 Objet d' **index** à ajouter ou nom de l’index à créer et à ajouter.  
  
 *Colonnes*  
 facultatif. Valeur de **type Variant** qui spécifie le ou les noms de la ou des colonnes à indexer. Le paramètre *Columns* correspond à la ou aux valeurs de la propriété [Name](./name-property-adox.md) d’un ou plusieurs objets [Column](./column-object-adox.md) .  
  
## <a name="remarks"></a>Notes  
 Le paramètre *Columns* peut prendre soit le nom d’une colonne, soit un tableau de noms de colonnes.  
  
 Une erreur se produit si le fournisseur ne prend pas en charge la création d’index.  
  
## <a name="applies-to"></a>S'applique à  
 [Indexes, collection (ADOX)](./indexes-collection-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Indexes, exemple de méthode Append (VB)](./indexes-append-method-example-vb.md)   
 [Append, méthode (colonnes ADOX)](./append-method-adox-columns.md)   
 [Append, méthode (groupes ADOX)](./append-method-adox-groups.md)   
 [Append, méthode (clés ADOX)](./append-method-adox-keys.md)   
 [Append, méthode (procédures ADOX)](./append-method-adox-procedures.md)   
 [Append, méthode (Tables ADOX)](./append-method-adox-tables.md)   
 [Append, méthode (utilisateurs ADOX)](./append-method-adox-users.md)   
 [Append, méthode (vues ADOX)](./append-method-adox-views.md)