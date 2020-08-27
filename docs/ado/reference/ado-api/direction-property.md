---
description: Direction, propriété
title: Direction, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Parameter::Direction
helpviewer_keywords:
- Direction property
ms.assetid: d5732578-3434-4dcd-a9f7-db1abd1b3b94
author: rothja
ms.author: jroth
ms.openlocfilehash: af002ff20ff3ad27ab2395529c533738ad797ea6
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88973870"
---
# <a name="direction-property"></a>Direction, propriété
Indique si le [paramètre](../../../ado/reference/ado-api/parameter-object.md) représente un paramètre d’entrée, un paramètre de sortie, une entrée et un paramètre de sortie, ou si le paramètre est la valeur de retour d’une procédure stockée.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur [ParameterDirectionEnum](../../../ado/reference/ado-api/parameterdirectionenum.md) .  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **direction** pour spécifier la façon dont un paramètre est passé à une procédure ou à partir de celle-ci. La propriété **direction** est en lecture/écriture ; Cela vous permet d’utiliser des fournisseurs qui ne retournent pas ces informations ou de définir ces informations lorsque vous ne voulez pas qu’ADO effectue un appel supplémentaire au fournisseur pour récupérer des informations de paramètres.  
  
 Tous les fournisseurs ne peuvent pas déterminer la direction des paramètres dans leurs procédures stockées. Dans ce cas, vous devez définir la propriété **direction** avant d’exécuter la requête.  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Parameter](../../../ado/reference/ado-api/parameter-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (VB)](../../../ado/reference/ado-api/activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (VC + +)](../../../ado/reference/ado-api/activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (JScript)](../../../ado/reference/ado-api/activeconnection-commandtext-timeout-type-size-example-jscript.md)
