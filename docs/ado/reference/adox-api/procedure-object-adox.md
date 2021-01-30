---
description: Procedure, objet (ADOX)
title: PROCEDURE, objet (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Procedure
helpviewer_keywords:
- Procedure object [ADOX]
ms.assetid: 927bcf3e-32f5-4a80-98d3-600779f0732e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2b1c3734ad88b72a2f7779b37c97c4b7c4863c12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164098"
---
# <a name="procedure-object-adox"></a>Procedure, objet (ADOX)
Représente une procédure stockée. Lorsqu’il est utilisé conjointement avec l’objet de [commande](../ado-api/command-object-ado.md) ADO, l’objet de **procédure** peut être utilisé pour ajouter, supprimer ou modifier des procédures stockées.  
  
## <a name="remarks"></a>Notes  
 L’objet **procedure** vous permet de créer une procédure stockée sans avoir à connaître ou à utiliser la syntaxe « CREATE PROCEDURE » du fournisseur.  
  
 Avec les propriétés d’un objet de **procédure** , vous pouvez :  
  
-   Identifiez la procédure avec la propriété [Name](./name-property-adox.md) .  
  
-   Spécifiez l’objet de **commande** ADO qui peut être utilisé pour créer ou exécuter la procédure avec la propriété [Command](./command-property-adox.md) .  
  
-   Retourne les informations de date avec les propriétés [DateCreated](./datecreated-property-adox.md) et [DateModified](./datemodified-property-adox.md) .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Procedure](./procedure-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Command et CommandText, exemples de propriétés (VB)](./command-and-commandtext-properties-example-vb.md)   
 [Parameters (collection), Command, exemple de propriété (VB)](./parameters-collection-command-property-example-vb.md)   
 [Procedures, exemple de méthode Append (VB)](./procedures-append-method-example-vb.md)   
 [Procedures, exemple de méthode Delete (VB)](./procedures-delete-method-example-vb.md)   
 [Procedures, collection (ADOX)](./procedures-collection-adox.md)