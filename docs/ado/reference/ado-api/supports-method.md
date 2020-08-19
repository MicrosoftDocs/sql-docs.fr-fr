---
description: Supports, méthode
title: Prend en charge la méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset15::raw_Supports
- Recordset15::Supports
helpviewer_keywords:
- Supports method [ADO]
ms.assetid: 298fc41c-0b55-42fc-b373-c5133b4da6a5
author: rothja
ms.author: jroth
ms.openlocfilehash: 8b0c05118b0f4b8f952b933bc2474bd1f0879865
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88441751"
---
# <a name="supports-method"></a>Supports, méthode
Détermine si un objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) spécifié prend en charge un type particulier de fonctionnalité.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
boolean = recordset.Supports(CursorOptions )  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur **booléenne** qui indique si toutes les fonctionnalités identifiées par l’argument *CursorOptions* sont prises en charge par le fournisseur.  
  
#### <a name="parameters"></a>Paramètres  
 *CursorOptions*  
 Expression **longue** qui se compose d’une ou de plusieurs valeurs [CursorOptionEnum](../../../ado/reference/ado-api/cursoroptionenum.md) .  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **prend en charge** pour déterminer les types de fonctionnalités qu’un objet **Recordset** prend en charge. Si l’objet **Recordset** prend en charge les fonctionnalités dont les constantes correspondantes se trouvent dans *CursorOptions*, la méthode **supports** retourne la **valeur true**. Sinon, elle retourne **false**.  
  
> [!NOTE]
>  Bien que la méthode **supports** puisse retourner la **valeur true** pour une fonctionnalité donnée, elle ne garantit pas que le fournisseur peut rendre la fonctionnalité disponible dans toutes les circonstances. La méthode **supports** retourne simplement une valeur indiquant si le fournisseur peut prendre en charge les fonctionnalités spécifiées, en supposant que certaines conditions sont remplies. Par exemple, la méthode **prend en charge** peut indiquer qu’un objet **Recordset** prend en charge les mises à jour, même si le curseur est basé sur une jointure de tables multiples, certaines colonnes de qui ne peuvent pas être mises à jour.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Supported, exemple de méthode (VB)](../../../ado/reference/ado-api/supports-method-example-vb.md)   
 [Supported, exemple de méthode (VC + +)](../../../ado/reference/ado-api/supports-method-example-vc.md)   
 [CursorType, propriété (ADO)](../../../ado/reference/ado-api/cursortype-property-ado.md)
