---
description: Parameter (syntaxe ADO pour Visual C++)
title: Parameter (ADO pour la syntaxe Visual C++) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- Parameter collection [ADO], ADO for Visual C++ syntax
ms.assetid: 74801dc1-cf0f-4a6e-960b-5990fe55e30d
author: rothja
ms.author: jroth
ms.openlocfilehash: 157124ac59400a208b108f4a6fa61491f77bd948
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041179"
---
# <a name="parameter-ado-for-visual-c-syntax"></a>Parameter (syntaxe ADO pour Visual C++)
## <a name="methods"></a>Méthodes  
  
```  
AppendChunk(VARIANT Val)  
```  
  
## <a name="properties"></a>Propriétés  
  
```  
get_Attributes(LONG *plParmAttribs)  
put_Attributes(LONG lParmAttribs)  
get_Direction(ParameterDirectionEnum *plParmDirection)  
put_Direction(ParameterDirectionEnum lParmDirection)  
get_Name(BSTR *pbstr)  
put_Name(BSTR bstr)  
get_NumericScale(BYTE *pbScale)  
put_NumericScale(BYTE bScale)  
get_Precision(BYTE *pbPrecision)  
put_Precision(BYTE bPrecision)  
get_Size(long *pl)  
put_Size(long l)  
get_Type(DataTypeEnum *psDataType)  
put_Type(DataTypeEnum sDataType)  
get_Value(VARIANT *pvar)  
put_Value(VARIANT val)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Objet Parameter](./parameter-object.md)