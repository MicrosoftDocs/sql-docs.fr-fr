---
description: ActiveCommand, propriété (ADO)
title: ActiveCommand, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset20::ActiveCommand
helpviewer_keywords:
- ActiveCommand property [ADO]
ms.assetid: fb4088d5-5968-42d6-aeaa-3955046bb4da
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e8c969c8e611c8e2bff76dc045a28a9c6d6ab96
ms.sourcegitcommit: c4d564435c008e2c92035efd2658172f20f07b2b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88759939"
---
# <a name="activecommand-property-ado"></a>ActiveCommand, propriété (ADO)
Indique l’objet de [commande](./command-object-ado.md) qui a créé l’objet [Recordset](./recordset-object-ado.md) associé.  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur de **type Variant** qui contient un objet **Command** . La valeur par défaut est une référence d’objet null.  
  
## <a name="remarks"></a>Notes  
 La propriété **ActiveCommand** est en lecture seule.  
  
 Si un objet **Command** n’a pas été utilisé pour créer le **Recordset**actuel, une référence d’objet **null** est retournée.  
  
 Utilisez cette propriété pour Rechercher l’objet de **commande** associé lorsque vous n’avez donné que l’objet **Recordset** résultant.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveCommand, exemple de propriété (VB)](./activecommand-property-example-vb.md)   
 [ActiveCommand, exemple de propriété (JScript)](./activecommand-property-example-jscript.md)   
 [ActiveCommand, exemple de propriété (VC + +)](./activecommand-property-example-vc.md)   
 [Command, objet (ADO)](./command-object-ado.md)