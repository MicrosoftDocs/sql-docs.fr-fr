---
description: Columns, collection (ADOX)
title: Columns, collection (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Index::Columns
- Table::Columns
- Key::Columns
- Columns
helpviewer_keywords:
- Columns collection [ADOX]
ms.assetid: 23b9fea8-4f76-4a51-95ce-1a6ce4560b34
author: rothja
ms.author: jroth
ms.openlocfilehash: 81a7e0c521b96b737c44e3c88f2b14be42176a31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050269"
---
# <a name="columns-collection-adox"></a>Columns, collection (ADOX)
Contient tous les objets [Column](./column-object-adox.md) d’une table, d’un index ou d’une clé.  
  
## <a name="remarks"></a>Notes  
 La méthode [Append](./append-method-adox-columns.md) d’une collection **Columns** est unique pour ADOX. Vous pouvez :  
  
-   Ajoutez une nouvelle colonne à la collection à l’aide de la méthode **Append** .  
  
 Les propriétés et les méthodes restantes sont standard pour les collections ADO. Vous pouvez :  
  
-   Accédez à une colonne de la collection avec la propriété [Item](../ado-api/item-property-ado.md) .  
  
-   Retourne le nombre de colonnes contenues dans la collection avec la propriété [Count](../ado-api/count-property-ado.md) .  
  
-   Supprimez une colonne de la collection à l’aide de la méthode [Delete](./delete-method-adox-collections.md) .  
  
-   Mettez à jour les objets de la collection pour refléter le schéma de la base de données actuelle avec la méthode [Refresh](../ado-api/refresh-method-ado.md) .  
  
> [!NOTE]
>  Une erreur se produit lors de l’ajout d’une **colonne** à la collection **Columns** d’un [index](./index-object-adox.md) si la **colonne** n’existe pas dans une [table](./table-object-adox.md) qui est déjà ajoutée à la collection [tables](./tables-collection-adox.md) .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de la collection Columns](./columns-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Columns et tables Append, méthodes, Name, exemple de propriété (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Connection Close, méthode, table type, exemple de propriété (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Méthodes Append des clés, type de clé, RelatedColumn, RelatedTable et UpdateRule, exemple de propriétés (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ParentCatalog, exemple de propriété (VB)](./parentcatalog-property-example-vb.md)   
 [SortOrder, exemple de propriété (VB)](./sortorder-property-example-vb.md)   
 [Column, objet (ADOX)](./column-object-adox.md)