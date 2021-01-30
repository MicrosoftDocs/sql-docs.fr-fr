---
description: CommandType, propriété (ADO)
title: CommandType, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::CommandType
helpviewer_keywords:
- CommandType property [ADO]
ms.assetid: ca44809c-8647-48b6-a7fb-0be70a02f53e
author: rothja
ms.author: jroth
ms.openlocfilehash: debd8c2630ba9fa7369b356950f979a64eee59d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155715"
---
# <a name="commandtype-property-ado"></a>CommandType, propriété (ADO)
Indique le type d’un objet [Command](./command-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une ou plusieurs valeurs [CommandTypeEnum](./commandtypeenum.md) .  
  
> [!NOTE]
>  N’utilisez pas les valeurs **CommandTypeEnum** de **adCmdFile** ou **adCmdTableDirect** avec **CommandType**. Ces valeurs ne peuvent être utilisées qu’en tant qu’options avec les méthodes d' [ouverture](./open-method-ado-recordset.md) et de [rerequête](./requery-method.md) d’un [Recordset](./recordset-object-ado.md).  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **CommandType** pour optimiser l’évaluation de la propriété [CommandText](./commandtext-property-ado.md) .  
  
 Si la valeur de la propriété **CommandType** est définie sur la valeur par défaut, **adCmdUnknown**, vous pouvez constater une baisse des performances, car ADO doit effectuer des appels au fournisseur pour déterminer si la propriété **CommandText** est une instruction SQL, une procédure stockée ou un nom de table. Si vous connaissez le type de commande que vous utilisez, la définition de la propriété **CommandType** demande à ADO d’accéder directement au code approprié. Si la propriété **CommandType** ne correspond pas au type de commande dans la propriété **CommandText** , une erreur se produit lorsque vous appelez la méthode [Execute](./execute-method-ado-command.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Command, objet (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)