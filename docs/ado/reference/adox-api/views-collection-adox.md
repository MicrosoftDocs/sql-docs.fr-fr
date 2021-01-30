---
description: Views, collection (ADOX)
title: Views, collection (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog::Views
- Views
helpviewer_keywords:
- Views collection [ADOX]
ms.assetid: a55d380c-2b7b-4b57-af74-8ba0b3de0db9
author: rothja
ms.author: jroth
ms.openlocfilehash: 51a32bb1952e5c8100ed1d13cb7ba72b99e8994d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169099"
---
# <a name="views-collection-adox"></a>Views, collection (ADOX)
Contient tous les objets de [vue](./view-object-adox.md) d’un catalogue.  
  
## <a name="remarks"></a>Notes  
 La méthode [Append](./append-method-adox-views.md) d’une collection **views** est unique pour ADOX. Vous pouvez :  
  
-   Ajoutez une nouvelle vue à la collection à l’aide de la méthode **Append** .  
  
 Les propriétés et les méthodes restantes sont standard pour les collections ADO. Vous pouvez :  
  
-   Accédez à une vue de la collection à l’aide de la propriété [Item](../ado-api/item-property-ado.md) .  
  
-   Retourne le nombre de vues contenues dans la collection avec la propriété [Count](../ado-api/count-property-ado.md) .  
  
-   Supprimez une vue de la collection à l’aide de la méthode [Delete](./delete-method-adox-collections.md) .  
  
-   Mettez à jour les objets de la collection pour refléter le schéma de base de données actuel avec la méthode [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de la collection Views](./views-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Views et Fields, exemple de collections (VB)](./views-and-fields-collections-example-vb.md)   
 [Views, exemple de méthode Append (VB)](./views-append-method-example-vb.md)   
 [Views, collection, CommandText, exemple de propriété (VB)](./views-collection-commandtext-property-example-vb.md)   
 [Views, exemple de méthode Delete (VB)](./views-delete-method-example-vb.md)   
 [Views, exemple de méthode Refresh (VB)](./views-refresh-method-example-vb.md)   
 [Catalog, objet (ADOX)](./catalog-object-adox.md)   
 [View, objet (ADOX)](./view-object-adox.md)