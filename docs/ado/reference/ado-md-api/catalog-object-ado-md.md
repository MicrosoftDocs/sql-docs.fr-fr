---
description: Catalog, objet (ADO MD)
title: Objet Catalogue (ADO MD) | Microsoft Docs
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
- Catalog object [ADO MD]
ms.assetid: 11f6f896-d69c-44a4-94cd-d54c93140e4a
author: rothja
ms.author: jroth
ms.openlocfilehash: 834594c957c05d4e2acef78bb772afcbe83b685a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169968"
---
# <a name="catalog-object-ado-md"></a>Catalog, objet (ADO MD)
Contient des informations sur les schémas multidimensionnels (c’est-à-dire les cubes et les dimensions sous-jacentes, les hiérarchies, les niveaux et les membres) spécifiques à un fournisseur de données multidimensionnelles (MDP).  
  
## <a name="remarks"></a>Notes  
 Avec les collections et les propriétés d’un objet **catalogue** , vous pouvez effectuer les opérations suivantes :  
  
-   Ouvrez le catalogue en affectant à la propriété [ActiveConnection](./activeconnection-property-ado-md.md) un objet de [connexion](../ado-api/connection-object-ado.md) ADO standard ou une chaîne de connexion valide.  
  
-   Identifiez le **catalogue** à l’aide de la propriété [Name](./name-property-ado-md.md) .  
  
-   Itérer au sein des cubes dans un catalogue à l’aide de la collection [CubeDefs](./cubedefs-collection-ado-md.md) .  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements](./catalog-object-properties-methods-and-events-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de catalogue (VB)](./catalog-example-vb.md)   
 [Connection, objet (ADO)](../ado-api/connection-object-ado.md)   
 [CubeDefs, collection (ADO MD)](./cubedefs-collection-ado-md.md)