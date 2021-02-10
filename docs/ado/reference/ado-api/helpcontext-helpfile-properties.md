---
description: HelpContext, HelpFile, propriétés
title: HelpContext, HelpFile, propriétés | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::GetHelpContext
- Error::GetHelpFile
- Error::get_HelpFile
- Error::get_HelpContext
- Error::HelpContext
- Error::HelpFile
helpviewer_keywords:
- HelpContext property [ADO]
- HelpFile property [ADO]
ms.assetid: 2b9ef441-993c-44d4-8f87-fac0979dac1d
author: rothja
ms.author: jroth
ms.openlocfilehash: ee683a07b0a581788657af2233944dccf5ab05b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020786"
---
# <a name="helpcontext-helpfile-properties"></a>HelpContext, HelpFile, propriétés
Indique le fichier d’aide et la rubrique associés à un objet d' [erreur](./error-object.md) .  
  
## <a name="return-values"></a>Valeurs de retour  
  
-   **HelpContextID** Retourne un ID de contexte, sous la forme d’une valeur **long** , pour une rubrique dans un fichier d’aide.  
  
-   **HelpFile** Retourne une valeur de **chaîne** qui prend la valeur d’un chemin d’accès à un fichier d’aide entièrement résolu.  
  
## <a name="remarks"></a>Notes  
 Si un fichier d’aide est spécifié dans la propriété **HelpFile** , la propriété **HelpContext** est utilisée pour afficher automatiquement la rubrique d’aide qu’il identifie. Si aucune rubrique d’aide pertinente n’est disponible, la propriété **HelpContext** retourne la valeur zéro et la propriété **HelpFile** retourne une chaîne de longueur nulle ("").  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Error](./error-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Description, HelpContext, HelpFile, NativeError, Number, source et SQLState, exemples de propriétés (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description, HelpContext, HelpFile, NativeError, Number, source et SQLState, exemples de propriétés (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description, propriété](./description-property.md)   
 [Number, propriété (ADO)](./number-property-ado.md)   
 [Source, propriété (objet Error ADO)](./source-property-ado-error.md)