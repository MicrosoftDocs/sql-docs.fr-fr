---
description: GetString, méthode (ADO)
title: GetString, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset20::raw_GetString
- Recordset20::GetString
helpviewer_keywords:
- GetString method [ADO]
ms.assetid: 92452940-b2a7-456e-94fc-3780c71da33c
author: rothja
ms.author: jroth
ms.openlocfilehash: 8a972edd11c419c1990c78635d42c44d8c06db2c
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88774908"
---
# <a name="getstring-method-ado"></a>GetString, méthode (ADO)
Retourne le [Recordset](./recordset-object-ado.md) sous forme de chaîne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Variant = recordset.GetString(StringFormat, NumRows, ColumnDelimiter, RowDelimiter, NullExpr)  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne le **Recordset** sous la forme d’une **variante** de type chaîne (BSTR).  
  
#### <a name="parameters"></a>Paramètres  
 *StringFormat*  
 Valeur [StringFormatEnum](./stringformatenum.md) qui spécifie la façon dont l' **objet Recordset** doit être converti en chaîne. Les paramètres *RowDelimiter*, *ColumnDelimiter*et *NullExpr* sont utilisés uniquement avec un *StringFormat* de **adClipString**.  
  
 *NumRows*  
 facultatif. Nombre de lignes à convertir dans le **Recordset**. Si la valeur *numRows* n’est pas spécifiée, ou si elle est supérieure au nombre total de lignes dans le **Recordset**, toutes les lignes de l’ensemble d' **enregistrements** sont converties.  
  
 *ColumnDelimiter*  
 facultatif. Séparateur utilisé entre les colonnes, si elles sont spécifiées, sinon le caractère de tabulation.  
  
 *RowDelimiter*  
 facultatif. Séparateur utilisé entre les lignes, si elles sont spécifiées, sinon le caractère de retour chariot.  
  
 *NullExpr*  
 facultatif. Expression utilisée à la place d’une valeur null, si elle est spécifiée, sinon la chaîne vide.  
  
## <a name="remarks"></a>Remarques  
 Les données de ligne, mais pas de données de schéma, sont enregistrées dans la chaîne. Par conséquent, un **jeu d’enregistrements** ne peut pas être rouvert à l’aide de cette chaîne.  
  
 Cette méthode est équivalente à la méthode **GETCLIPSTRING** RDO.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [GetString, exemple de méthode (VB)](./getstring-method-example-vb.md)