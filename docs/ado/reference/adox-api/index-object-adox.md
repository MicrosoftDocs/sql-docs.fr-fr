---
description: Index, objet (ADOX)
title: Index, objet (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Index
helpviewer_keywords:
- Index object [ADOX]
ms.assetid: 6b9578c0-bc94-46b9-b801-c18e14b04b31
author: rothja
ms.author: jroth
ms.openlocfilehash: a11389324337d4e5a9deea347dda8ecd5cea3016
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172036"
---
# <a name="index-object-adox"></a>Index, objet (ADOX)
Représente un index à partir d’une table de base de données.  
  
## <a name="remarks"></a>Notes  
 Le code suivant crée un nouvel **index**:  
  
```  
Dim obj As New Index  
```  
  
 Avec les propriétés et les collections d’un objet **index** , vous pouvez :  
  
-   Identifiez l’index avec la propriété [Name](./name-property-adox.md) .  
  
-   Accédez aux colonnes de base de données de l’index avec la collection [Columns](./columns-collection-adox.md) .  
  
-   Spécifiez si les clés d’index doivent être uniques avec la propriété [unique](./unique-property-adox.md) .  
  
-   Spécifie si l’index est la clé primaire d’une table avec la propriété [PrimaryKey](./primarykey-property-adox.md) .  
  
-   Spécifiez si les enregistrements qui ont des valeurs NULL dans leurs champs d’index ont des entrées d’index avec la propriété [IndexNulls](./indexnulls-property-adox.md) .  
  
-   Spécifiez si l’index est ordonné en clusters avec la propriété [Clustered](./clustered-property-adox.md) .  
  
-   Accédez aux propriétés d’index spécifiques au fournisseur avec la collection [Properties](../ado-api/properties-collection-ado.md) .  
  
> [!NOTE]
>  Une erreur se produit lors de l’ajout d’une [colonne](./column-object-adox.md) à la collection **Columns** d’un **index** si la **colonne** n’existe pas dans un objet [table](./table-object-adox.md) déjà ajouté à la collection [tables](./tables-collection-adox.md) .  
  
> [!NOTE]
>  Votre fournisseur de données peut ne pas prendre en charge toutes les propriétés des objets **index** . Une erreur se produit si vous avez défini une valeur pour une propriété qui n’est pas prise en charge par le fournisseur. Pour les nouveaux objets **index** , l’erreur se produit lorsque l’objet est ajouté à la collection. Pour les objets existants, l’erreur se produit lors de la définition de la propriété.  
  
> [!NOTE]
>  Lors de la création d’objets **index** , l’existence d’une valeur par défaut appropriée pour une propriété facultative ne garantit pas que votre fournisseur prenne en charge la propriété. Pour plus d’informations sur les propriétés prises en charge par votre fournisseur, consultez la documentation de votre fournisseur.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Index](./index-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Indexes, exemple de méthode Append (VB)](./indexes-append-method-example-vb.md)   
 [IndexNulls, exemple de propriété (VB)](./indexnulls-property-example-vb.md)   
 [PrimaryKey et unique, exemple de propriétés (VB)](./primarykey-and-unique-properties-example-vb.md)   
 [SortOrder, exemple de propriété (VB)](./sortorder-property-example-vb.md)   
 [Columns, collection (ADOX)](./columns-collection-adox.md)   
 [Indexes, collection (ADOX)](./indexes-collection-adox.md)   
 [Properties, collection (ADO)](../ado-api/properties-collection-ado.md)