---
description: View, objet (ADOX)
title: View, objet (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- View
helpviewer_keywords:
- View object [ADOX]
ms.assetid: 653421ce-7b94-43d0-9bc6-4900f8f2af45
author: rothja
ms.author: jroth
ms.openlocfilehash: 227a52fb1ee89777d1e198e96e253ef8dd5bb979
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049659"
---
# <a name="view-object-adox"></a>View, objet (ADOX)
Représente un jeu d’enregistrements filtré ou une table virtuelle. Lorsqu’il est utilisé conjointement avec l’objet [Command](../ado-api/command-object-ado.md) ADO, l’objet **View** peut être utilisé pour ajouter, supprimer ou modifier des vues.  
  
## <a name="remarks"></a>Notes  
 Une vue est une table virtuelle créée à partir d’autres tables ou vues de base de données. L’objet **View** vous permet de créer une vue sans avoir à connaître ou à utiliser la syntaxe « CREATE VIEW » du fournisseur.  
  
 Avec les propriétés d’un objet **View** , vous pouvez :  
  
-   Identifiez la vue à l’aide de la propriété [Name](./name-property-adox.md) .  
  
-   Spécifiez l’objet de **commande** ADO qui peut être utilisé pour ajouter, supprimer ou modifier des vues avec la propriété [Command](./command-property-adox.md) .  
  
-   Retourne les informations de date avec les propriétés [DateCreated](./datecreated-property-adox.md) et [DateModified](./datemodified-property-adox.md) .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet View](./view-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Views et Fields, exemple de collections (VB)](./views-and-fields-collections-example-vb.md)   
 [Views, exemple de méthode Append (VB)](./views-append-method-example-vb.md)   
 [Views, collection, CommandText, exemple de propriété (VB)](./views-collection-commandtext-property-example-vb.md)   
 [Views, exemple de méthode Delete (VB)](./views-delete-method-example-vb.md)   
 [Views, collection (ADOX)](./views-collection-adox.md)