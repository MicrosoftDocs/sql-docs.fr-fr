---
description: Catalog, objet (ADOX)
title: Catalog, objet (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog
helpviewer_keywords:
- Catalog object [ADOX]
ms.assetid: bb651639-a488-4e38-b6de-0ed99fa4dd92
author: rothja
ms.author: jroth
ms.openlocfilehash: 00e698ed85e739b2ea6a8dccebef7ce36e3be8e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054474"
---
# <a name="catalog-object-adox"></a>Catalog, objet (ADOX)
Contient des collections ([tables](./tables-collection-adox.md), [vues](./views-collection-adox.md), [utilisateurs](./users-collection-adox.md), [groupes](./groups-collection-adox.md)et [procédures](./procedures-collection-adox.md)) qui décrivent le catalogue de schémas d’une source de données.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez modifier l’objet **catalogue** en ajoutant ou en supprimant des objets ou en modifiant des objets existants. Certains fournisseurs peuvent ne pas prendre en charge tous les objets de **catalogue** ou ne prendre en charge que l’affichage des informations de schéma.  
  
 Avec les propriétés et les méthodes d’un objet **catalogue** , vous pouvez :  
  
-   Ouvrez le catalogue en affectant à la propriété [ActiveConnection](./activeconnection-property-adox.md) un objet de [connexion](../ado-api/connection-object-ado.md) ADO ou une chaîne de connexion valide.  
  
-   Créez un catalogue à l’aide de la méthode [Create](./create-method-adox.md) .  
  
-   Déterminez les propriétaires des objets dans un **catalogue** à l’aide des méthodes [GetObjectOwner](./getobjectowner-method-adox.md) et [SetObjectOwner](./setobjectowner-method.md) .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Catalog](./catalog-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété ActiveConnection de Catalog (VB)](./catalog-activeconnection-property-example-vb.md)   
 [Command et CommandText, exemples de propriétés (VB)](./command-and-commandtext-properties-example-vb.md)   
 [Connection Close, méthode, table type, exemple de propriété (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Create, exemple de méthode (VB)](./create-method-example-vb.md)   
 [Méthodes Append des clés, type de clé, RelatedColumn, RelatedTable et UpdateRule, exemple de propriétés (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Parameters (collection), Command, exemple de propriété (VB)](./parameters-collection-command-property-example-vb.md)   
 [ParentCatalog, exemple de propriété (VB)](./parentcatalog-property-example-vb.md)   
 [Procedures, exemple de méthode Append (VB)](./procedures-append-method-example-vb.md)   
 [Procedures, exemple de méthode Delete (VB)](./procedures-delete-method-example-vb.md)   
 [Procedures, exemple de méthode Refresh (VB)](./procedures-refresh-method-example-vb.md)   
 [Views et Fields, exemple de collections (VB)](./views-and-fields-collections-example-vb.md)   
 [Views, exemple de méthode Append (VB)](./views-append-method-example-vb.md)   
 [Views, collection, CommandText, exemple de propriété (VB)](./views-collection-commandtext-property-example-vb.md)   
 [Views, exemple de méthode Delete (VB)](./views-delete-method-example-vb.md)   
 [Views, exemple de méthode Refresh (VB)](./views-refresh-method-example-vb.md)   
 [Groups, collection (ADOX)](./groups-collection-adox.md)   
 [Procedures, collection (ADOX)](./procedures-collection-adox.md)   
 [Tables, collection (ADOX)](./tables-collection-adox.md)   
 [Users, collection (ADOX)](./users-collection-adox.md)   
 [Views, collection (ADOX)](./views-collection-adox.md)