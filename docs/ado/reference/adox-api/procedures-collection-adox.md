---
description: Procedures, collection (ADOX)
title: Procedures, collection (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Procedures
- Catalog::Procedures
helpviewer_keywords:
- Procedures collection [ADOX]
ms.assetid: dc7a38e1-93b9-4034-9af2-ff419e8fb2a3
author: rothja
ms.author: jroth
ms.openlocfilehash: c77c3eccd6cc2e61898b1619ec6255f65227384b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169243"
---
# <a name="procedures-collection-adox"></a>Procedures, collection (ADOX)
Contient tous les objets de [procédure](./procedure-object-adox.md) d’un catalogue.  
  
## <a name="remarks"></a>Notes  
 La méthode [Append](./append-method-adox-procedures.md) d’une collection **procedures** est unique pour ADOX. Vous pouvez :  
  
-   Ajoutez une nouvelle procédure à la collection à l’aide de la méthode **Append** .  
  
 Les propriétés et les méthodes restantes sont standard pour les collections ADO. Vous pouvez :  
  
-   Accédez à une procédure dans la collection à l’aide de la propriété [Item](../ado-api/item-property-ado.md) .  
  
-   Retourne le nombre de procédures contenues dans la collection avec la propriété [Count](../ado-api/count-property-ado.md) .  
  
-   Supprimez une procédure de la collection à l’aide de la méthode [Delete](./delete-method-adox-collections.md) .  
  
-   Mettez à jour les objets de la collection pour refléter le schéma de base de données actuel avec la méthode [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de la collection Indexes](./indexes-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Command et CommandText, exemples de propriétés (VB)](./command-and-commandtext-properties-example-vb.md)   
 [Parameters (collection), Command, exemple de propriété (VB)](./parameters-collection-command-property-example-vb.md)   
 [Procedures, exemple de méthode Append (VB)](./procedures-append-method-example-vb.md)   
 [Procedures, exemple de méthode Delete (VB)](./procedures-delete-method-example-vb.md)   
 [Procedures, exemple de méthode Refresh (VB)](./procedures-refresh-method-example-vb.md)   
 [Procédures, propriétés, méthodes et événements de la collection procedures](./procedures-collection-properties-methods-and-events.md)   
 [Catalog, objet (ADOX)](./catalog-object-adox.md)   
 [Procedure, objet (ADOX)](./procedure-object-adox.md)