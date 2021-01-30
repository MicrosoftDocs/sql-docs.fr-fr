---
description: Table, objet (ADOX)
title: Table, objet (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Table
helpviewer_keywords:
- Table object [ADOX]
ms.assetid: a6d74000-0828-49ba-850a-63da865f8802
author: rothja
ms.author: jroth
ms.openlocfilehash: ba2064096c36bf6490daa8d472801de7594cfdcc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164014"
---
# <a name="table-object-adox"></a>Table, objet (ADOX)
Représente une table de base de données, notamment des colonnes, des index et des clés.  
  
## <a name="remarks"></a>Notes  
 Le code suivant crée une nouvelle **table**:  
  
```  
Dim obj As New Table  
```  
  
 Avec les propriétés et les collections d’un objet **table** , vous pouvez :  
  
-   Identifiez la table à l’aide de la propriété nom de la [propriété (ADOX)](./name-property-adox.md) .  
  
-   Déterminez le type de table avec la propriété [type (table) (ADOX)](./type-property-table-adox.md) .  
  
-   Accédez aux colonnes de base de données de la table à l’aide de la collection [Columns collection (ADOX)](./columns-collection-adox.md) .  
  
-   Accédez aux index de la table à l’aide de la [collection d’index (ADOX)](./indexes-collection-adox.md).  
  
-   Accédez aux clés de la table à l’aide de la [collection Keys (ADOX)](./keys-collection-adox.md).  
  
-   Spécifiez le catalogue qui possède la table avec la propriété [ParentCatalog Property (ADOX)](./parentcatalog-property-adox.md) .  
  
-   Retournez les informations de date avec les propriétés [DateCreated Property (ADOX)](./datecreated-property-adox.md) et [DATEMODIFIED Property (ADOX)](./datemodified-property-adox.md) .  
  
-   Accédez aux propriétés de table spécifiques au fournisseur à l’aide de la collection de [Collections Properties (ADO)](../ado-api/properties-collection-ado.md) .  
  
> [!NOTE]
>  Votre fournisseur de données peut ne pas prendre en charge toutes les propriétés des objets de **table** . Une erreur se produit si vous avez défini une valeur pour une propriété que le fournisseur ne prend pas en charge. Pour les nouveaux objets de **table** , l’erreur se produit lorsque l’objet est ajouté à la collection. Pour les objets existants, l’erreur se produit lors de la définition de la propriété.  
>   
>  Lors de la création d’objets **table** , l’existence d’une valeur par défaut appropriée pour une propriété facultative ne garantit pas que votre fournisseur prenne en charge la propriété. Pour plus d’informations sur les propriétés prises en charge par votre fournisseur, consultez la documentation de votre fournisseur.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Table](./table-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété ActiveConnection de Catalog (VB)](./catalog-activeconnection-property-example-vb.md)   
 [Columns et tables Append, méthodes, Name, exemple de propriété (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Connection Close, méthode, table type, exemple de propriété (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Méthodes Append des clés, type de clé, RelatedColumn, RelatedTable et UpdateRule, exemple de propriétés (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ParentCatalog, exemple de propriété (VB)](./parentcatalog-property-example-vb.md)   
 [Columns, collection (ADOX)](./columns-collection-adox.md)   
 [Indexes, collection (ADOX)](./indexes-collection-adox.md)   
 [Keys, collection (ADOX)](./keys-collection-adox.md)   
 [Properties, collection (ADO)](../ado-api/properties-collection-ado.md)   
 [Tables, collection (ADOX)](./tables-collection-adox.md)