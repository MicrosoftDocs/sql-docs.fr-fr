---
description: Key, objet (ADOX)
title: Key, objet (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Key
helpviewer_keywords:
- Key object [ADOX]
ms.assetid: 55f116fe-4d56-4892-bffe-0cdd6fc727c9
author: rothja
ms.author: jroth
ms.openlocfilehash: b0e2cdd3dcb1d6d0772e1b7508bee8063089a4e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054074"
---
# <a name="key-object-adox"></a>Key, objet (ADOX)
Représente un champ clé primaire, étrangère ou unique d’une table de base de données.  
  
## <a name="remarks"></a>Notes  
 Le code suivant crée une nouvelle **clé**:  
  
```  
Dim obj As New Key  
```  
  
 Avec les propriétés et les collections d’un objet **clé** , vous pouvez :  
  
-   Identifiez la clé avec la propriété [Name](./name-property-adox.md) .  
  
-   Déterminez si la clé est primaire, étrangère ou unique avec la propriété [type](./type-property-key-adox.md) .  
  
-   Accédez aux colonnes de base de données de la clé avec la collection [Columns](./columns-collection-adox.md) .  
  
-   Spécifiez le nom de la table associée à l’aide de la propriété [RelatedTable](./relatedtable-property-adox.md) .  
  
-   Déterminez l’action effectuée lors de la suppression ou de la mise à jour d’une clé primaire avec les propriétés [DeleteRule](./deleterule-property-adox.md) et [UpdateRule](./updaterule-property-adox.md) .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Key](./key-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes Append des clés, type de clé, RelatedColumn, RelatedTable et UpdateRule, exemple de propriétés (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Columns, collection (ADOX)](./columns-collection-adox.md)   
 [Keys, collection (ADOX)](./keys-collection-adox.md)