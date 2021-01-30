---
title: Description, propriété | Microsoft Docs
description: En savoir plus sur la propriété Description de l’objet d’erreur dans ADO qui retourne une valeur de chaîne contenant une description de l’erreur.
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::Description
- Error::GetDescription
- Error::get_Description
helpviewer_keywords:
- Description property
ms.assetid: 4b5d6790-6c29-42aa-bf78-d9cfb8ad7965
author: rothja
ms.author: jroth
ms.openlocfilehash: e2a982c21cbab3a93db6c6c64cec997d9f3bb4fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167527"
---
# <a name="description-property"></a>Description, propriété
Décrit un objet d' [erreur](../../../ado/reference/ado-api/error-object.md) .  
  
## <a name="return-value"></a>Valeur renvoyée  
 Retourne une valeur de **chaîne** qui contient une description de l’erreur.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Description** pour obtenir une brève description de l’erreur. Affichez cette propriété pour alerter l’utilisateur en cas d’erreur que vous ne pouvez pas ou ne souhaitez pas gérer. La chaîne provient de ADO ou d’un fournisseur.  
  
 Les fournisseurs sont responsables du passage du texte d’erreur spécifique à ADO. ADO ajoute un objet [Error](../../../ado/reference/ado-api/error-object.md) à la collection **Errors** pour chaque erreur de fournisseur ou avertissement qu’il reçoit. Énumérez la collection **Errors** pour tracer les erreurs que le fournisseur passe.  
  
## <a name="applies-to"></a>S'applique à  
 [Error, objet](../../../ado/reference/ado-api/error-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Description, HelpContext, HelpFile, NativeError, Number, source et SQLState, exemples de propriétés (VB)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description, HelpContext, HelpFile, NativeError, Number, source et SQLState, exemples de propriétés (VC + +)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [HelpContext, HelpFile, propriétés](../../../ado/reference/ado-api/helpcontext-helpfile-properties.md)   
 [Number, propriété (ADO)](../../../ado/reference/ado-api/number-property-ado.md)   
 [Source, propriété (objet Error ADO)](../../../ado/reference/ado-api/source-property-ado-error.md)
