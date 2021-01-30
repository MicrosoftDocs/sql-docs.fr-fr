---
description: Index, propriété
title: Index, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset21::Index
helpviewer_keywords:
- Index property
ms.assetid: 1c79e271-21ec-41a8-8163-c5e89f0001a7
author: rothja
ms.author: jroth
ms.openlocfilehash: 9e8c346ac48092d7a5dcaf09068bb64d63316a38
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170955"
---
# <a name="index-property"></a>Index, propriété
Indique le nom de l’index actuellement en vigueur pour un objet [Recordset](./recordset-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **chaîne** , qui est le nom de l’index.  
  
## <a name="remarks"></a>Notes  
 L’index nommé par la propriété **index** doit avoir été précédemment déclaré sur la table de base sous-jacente à l’objet **Recordset** . Autrement dit, l’index doit avoir été déclaré par programmation comme un objet d' [index](../adox-api/index-object-adox.md) ADOX ou lors de la création de la table de base.  
  
 Une erreur d’exécution se produit si l’index ne peut pas être défini. La propriété d' **index** ne peut pas être définie dans les conditions suivantes :  
  
-   Dans un gestionnaire d’événements [WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md) ou **RecordsetChangeComplete** .  
  
-   Si le **jeu d’enregistrements** est toujours en cours d’exécution d’une opération (ce qui peut être déterminé par la propriété [State](./state-property-ado.md) ).  
  
-   Si un filtre a été défini sur le **jeu d’enregistrements** avec la propriété [Filter](./filter-property.md) .  
  
 La propriété **index** peut toujours être définie correctement si le **jeu d’enregistrements** est fermé, mais que l' **objet Recordset** ne s’ouvre pas correctement, ou l’index ne peut pas être utilisé si le fournisseur sous-jacent ne prend pas en charge les index.  
  
 Si l’index peut être défini, la position de la ligne actuelle peut changer. Cela entraînera une mise à jour de la propriété [AbsolutePosition](./absoluteposition-property-ado.md) et déclenchera les événements **WillChangeRecordset**, **RecordsetChangeComplete**, [WillMove](./willmove-and-movecomplete-events-ado.md)et [MoveComplete](./willmove-and-movecomplete-events-ado.md) .  
  
 Si l’index peut être défini et si la propriété [LockType](./locktype-property-ado.md) est **adLockPessimistic** ou **adLockOptimistic**, une opération [UpdateBatch](./updatebatch-method.md) implicite est effectuée. Cela libère les groupes actuels et affectés. Tout filtre existant est libéré, et la position de ligne actuelle est remplacée par la première ligne de l’ensemble d' **enregistrements** réorganisé.  
  
 La propriété **index** est utilisée conjointement avec la méthode [Seek](./seek-method.md) . Si le fournisseur sous-jacent ne prend pas en charge la propriété d' **index** , et donc la méthode **Seek** , envisagez d’utiliser la méthode [Find](./find-method-ado.md) à la place. Déterminez si l’objet **Recordset** prend en charge les index avec la méthode [prend en charge](./supports-method.md)**(adIndex)** .  
  
 La propriété d' **index** intégrée n’est pas liée à la propriété d' [optimisation](./optimize-property-dynamic-ado.md) dynamique, bien qu’elle traite les deux index.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode Seek et index, exemple de propriété (VB)](./seek-method-and-index-property-example-vb.md)   
 [Index, objet (ADOX)](../adox-api/index-object-adox.md)   
 [Seek, méthode](./seek-method.md)