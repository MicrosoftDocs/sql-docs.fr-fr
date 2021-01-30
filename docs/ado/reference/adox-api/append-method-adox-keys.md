---
description: Append, méthode (clés ADOX)
title: Append, méthode (clés ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Keys::Append
- Keys::raw_Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 215a5391-f422-42ec-99ea-4e6fbb5d3d64
author: rothja
ms.author: jroth
ms.openlocfilehash: c4690fba27f42da95eb354b5074f9eb9954902a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172296"
---
# <a name="append-method-adox-keys"></a>Append, méthode (clés ADOX)
Ajoute un nouvel objet [clé](./key-object-adox.md) à la collection de [clés](./keys-collection-adox.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Keys.Append Key [,KeyType] [,Column] [,RelatedTable] [,RelatedColumn]  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Clé*  
 Objet **clé** à ajouter ou nom de la clé à créer et à ajouter.  
  
 *KeyType*  
 Facultatif. Valeur de type **long** qui spécifie le type de clé. Le paramètre de *clé* correspond à la propriété de [type](./type-property-key-adox.md) d’un objet **clé** .  
  
 *Colonne*  
 Facultatif. Valeur de **chaîne** qui spécifie le nom de la colonne à indexer. Le paramètre *Columns* correspond à la valeur de la propriété [Name](./name-property-adox.md) d’un objet [Column](./column-object-adox.md) .  
  
 *RelatedTable*  
 Facultatif. Valeur de **chaîne** qui spécifie le nom de la table associée. Le paramètre *RelatedTable* correspond à la valeur de la propriété **Name** d’un objet [table](./table-object-adox.md) .  
  
 *; RelatedColumn*  
 Facultatif. Valeur de **chaîne** qui spécifie le nom de la colonne associée pour une clé étrangère. Le paramètre *RelatedColumn* correspond à la valeur de la propriété **Name** d’un objet [Column](./column-object-adox.md) .  
  
## <a name="remarks"></a>Notes  
 Le paramètre *Columns* peut prendre soit le nom d’une colonne, soit un tableau de noms de colonnes.  
  
## <a name="applies-to"></a>S'applique à  
 [Keys, collection (ADOX)](./keys-collection-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes Append des clés, type de clé, RelatedColumn, RelatedTable et UpdateRule, exemple de propriétés (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Append, méthode (colonnes ADOX)](./append-method-adox-columns.md)   
 [Append, méthode (groupes ADOX)](./append-method-adox-groups.md)   
 [Append, méthode (Index ADOX)](./append-method-adox-indexes.md)   
 [Append, méthode (procédures ADOX)](./append-method-adox-procedures.md)   
 [Append, méthode (Tables ADOX)](./append-method-adox-tables.md)   
 [Append, méthode (utilisateurs ADOX)](./append-method-adox-users.md)   
 [Append, méthode (vues ADOX)](./append-method-adox-views.md)