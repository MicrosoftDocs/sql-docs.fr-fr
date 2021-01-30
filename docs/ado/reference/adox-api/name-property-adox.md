---
description: Name, propriété (ADOX)
title: Name, propriété (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Table::PutName
- _Table::GetName
- _Key::Name
- _Key::get_Name
- _Column::GetName
- _Index::Name
- _Index::put_Name
- _Column::PutName
- _Key::put_Name
- _Table::put_Name
- _User25::PutName
- _Index::get_Name
- _Column::get_Name
- _Group25::Name
- _Group25::get_Name
- _Column::Name
- _User25::get_Name
- _Table::Name
- _Group25::GetName
- _Index::PutName
- _Column::put_Name
- _Key::GetName
- _Table::get_Name
- _User25::Name
- _User25::put_Name
- _Index::GetName
- _User25::GetName
helpviewer_keywords:
- Name property [ADOX]
ms.assetid: 81b92baf-b6b9-4f4e-9f33-4503795518cd
author: rothja
ms.author: jroth
ms.openlocfilehash: 83ef51697cd3ad1ad1a01355baead5d90788813f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172002"
---
# <a name="name-property-adox"></a>Name, propriété (ADOX)
Indique le nom de l’objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **chaîne** .  
  
## <a name="remarks"></a>Notes  
 Les noms ne doivent pas nécessairement être uniques dans une collection.  
  
 La propriété de **nom** est en lecture/écriture sur les objets de [colonne](./column-object-adox.md), de [groupe](./group-object-adox.md), de [clé](./key-object-adox.md), d' [index](./index-object-adox.md), de [table](./table-object-adox.md)et d' [utilisateur](./user-object-adox.md) . La propriété **Name** est en lecture seule sur les objets [catalogue](./catalog-object-adox.md), [procédure](./procedure-object-adox.md)et [vue](./view-object-adox.md) .  
  
 Pour les objets en lecture/écriture (**colonne**, **Groupe**, **clé**, **index**, **table** et objets **utilisateur** ), la valeur par défaut est une chaîne vide ("").  
  
> [!NOTE]
>  Pour les clés, cette propriété est en lecture seule sur les objets **clés** déjà ajoutés à une collection. Pour les tables, cette propriété est en lecture seule pour les objets de **table** déjà ajoutés à une collection.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Column, objet (ADOX)](./column-object-adox.md)  
        [Group, objet (ADOX)](./group-object-adox.md)  
        [Index, objet (ADOX)](./index-object-adox.md)  
    :::column-end:::
    :::column:::
        [Key, objet (ADOX)](./key-object-adox.md)  
        [Procedure, objet (ADOX)](./procedure-object-adox.md)  
        [Property, objet (ADO)](../ado-api/property-object-ado.md)  
    :::column-end:::
    :::column:::
        [Table, objet (ADOX)](./table-object-adox.md)  
        [User, objet (ADOX)](./user-object-adox.md)  
        [View, objet (ADOX)](./view-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Columns et tables Append, méthodes, Name, exemple de propriété (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Méthodes Append des clés, type de clé, RelatedColumn, RelatedTable et UpdateRule, exemple de propriétés (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ParentCatalog, exemple de propriété (VB)](./parentcatalog-property-example-vb.md)