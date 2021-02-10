---
description: Supports, méthode
title: Prend en charge la méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Supports
- Recordset15::Supports
helpviewer_keywords:
- Supports method [ADO]
ms.assetid: 298fc41c-0b55-42fc-b373-c5133b4da6a5
author: rothja
ms.author: jroth
ms.openlocfilehash: 474d2d1f076188b22224e62f0e487872440c6eb5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056464"
---
# <a name="supports-method"></a>Supports, méthode
Détermine si un objet [Recordset](./recordset-object-ado.md) spécifié prend en charge un type particulier de fonctionnalité.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
boolean = recordset.Supports(CursorOptions )  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur **booléenne** qui indique si toutes les fonctionnalités identifiées par l’argument *CursorOptions* sont prises en charge par le fournisseur.  
  
#### <a name="parameters"></a>Paramètres  
 *CursorOptions*  
 Expression **longue** qui se compose d’une ou de plusieurs valeurs [CursorOptionEnum](./cursoroptionenum.md) .  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **prend en charge** pour déterminer les types de fonctionnalités qu’un objet **Recordset** prend en charge. Si l’objet **Recordset** prend en charge les fonctionnalités dont les constantes correspondantes se trouvent dans *CursorOptions*, la méthode **supports** retourne la **valeur true**. Sinon, elle retourne **false**.  
  
> [!NOTE]
>  Bien que la méthode **supports** puisse retourner la **valeur true** pour une fonctionnalité donnée, elle ne garantit pas que le fournisseur peut rendre la fonctionnalité disponible dans toutes les circonstances. La méthode **supports** retourne simplement une valeur indiquant si le fournisseur peut prendre en charge les fonctionnalités spécifiées, en supposant que certaines conditions sont remplies. Par exemple, la méthode **prend en charge** peut indiquer qu’un objet **Recordset** prend en charge les mises à jour, même si le curseur est basé sur une jointure de tables multiples, certaines colonnes de qui ne peuvent pas être mises à jour.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Supported, exemple de méthode (VB)](./supports-method-example-vb.md)   
 [Supported, exemple de méthode (VC + +)](./supports-method-example-vc.md)   
 [CursorType, propriété (ADO)](./cursortype-property-ado.md)