---
description: Delete, méthode (collections ADOX)
title: Delete, méthode (collections ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Views::Delete
- Groups::Delete
- Indexes::raw_Delete
- Columns::raw_Delete
- Tables::Delete
- Keys::Delete
- Users::Delete
- Users::raw_Delete
- Keys::raw_Delete
- Procedures::raw_Delete
- Views::raw_Delete
- Indexes::Delete
- Procedures::Delete
- Groups::raw_Delete
- Tables::raw_Delete
- Columns::Delete
helpviewer_keywords:
- delete method [ADOX]
ms.assetid: e6b6e3a4-8952-4d79-81f4-51019c338374
author: rothja
ms.author: jroth
ms.openlocfilehash: 8a7646288bfb2736671f58a2b158f6047476dbc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172108"
---
# <a name="delete-method-adox-collections"></a>Delete, méthode (collections ADOX)
Supprime un objet d’une collection.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Collection.Delete Name  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Nom*  
 **Variant** qui spécifie le nom ou la position ordinale (index) de l’objet à supprimer.  
  
## <a name="remarks"></a>Notes  
 Une erreur se produit si le *nom* n’existe pas dans la collection.  
  
 Pour les collections de [tables](./tables-collection-adox.md) et d' [utilisateurs](./users-collection-adox.md) , une erreur se produit si le fournisseur ne prend pas en charge la suppression de tables ou d’utilisateurs, respectivement. Pour les collections de [procédures](./procedures-collection-adox.md) et de [vues](./views-collection-adox.md) , la **suppression** échoue si le fournisseur ne prend pas en charge les commandes persistantes.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Columns, collection (ADOX)](./columns-collection-adox.md)  
        [Groups, collection (ADOX)](./groups-collection-adox.md)  
        [Indexes, collection (ADOX)](./indexes-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Keys, collection (ADOX)](./keys-collection-adox.md)  
        [Procedures, collection (ADOX)](./procedures-collection-adox.md)  
        [Tables, collection (ADOX)](./tables-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Users, collection (ADOX)](./users-collection-adox.md)  
        [Views, collection (ADOX)](./views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Procedures, exemple de méthode Delete (VB)](./procedures-delete-method-example-vb.md)   
 [Views, exemple de méthode Delete (VB)](./views-delete-method-example-vb.md)