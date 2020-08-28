---
description: StayInSync, propriété
title: StayInSync, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset20::StayInSync
- Recordset20::put_StayInSync
- Recordset20::PutStayInSync
- Recordset20::get_StayInSync
- Recordset20::GetStayInSync
helpviewer_keywords:
- StayInSync property
ms.assetid: 502d69b5-dc9a-455d-b115-a03bd39a552b
author: rothja
ms.author: jroth
ms.openlocfilehash: 9677a776273e83ad31fecde3a7ea436138d3784e
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88988620"
---
# <a name="stayinsync-property"></a>StayInSync, propriété
Indique, dans un objet [Recordset](./recordset-object-ado.md) hiérarchique, si la référence aux enregistrements enfants sous-jacents (autrement dit, le *chapitre*) change lorsque la position de la ligne parente change.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur **booléenne** . La valeur par défaut est **True**. Si la **valeur est true**, le chapitre sera mis à jour si l’objet du **jeu d’enregistrements** parent change de position de ligne ; Si la **valeur est false**, le chapitre continuera à faire référence aux données du chapitre précédent, même si l’objet **Recordset** parent a modifié la position de ligne.  
  
## <a name="remarks"></a>Notes  
 Cette propriété s’applique aux recordsets hiérarchiques, tels que ceux pris en charge par le [service de mise en forme de données Microsoft pour OLE DB](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md), et doit être défini sur le **jeu d’enregistrements** parent avant que le **jeu d’enregistrements** enfant soit récupéré. Cette propriété simplifie la navigation dans les jeux d’enregistrements hiérarchiques.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [StayInSync, exemple de propriété (VB)](./stayinsync-property-example-vb.md)   
 [Microsoft Data Shaping Service pour OLE DB (fournisseur de services ADO)](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)