---
description: Stream (syntaxe ADO pour Visual C++)
title: Stream (syntaxe ADO pour Visual C++) | Microsoft Docs
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
- Stream collection [ADO]
ms.assetid: dddcceef-9296-4fb3-8eca-94b17d0148de
author: rothja
ms.author: jroth
ms.openlocfilehash: 4f6ee4566e977115e3b94a562f06a9a836235e80
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166430"
---
# <a name="stream-ado-for-visual-c-syntax"></a>Stream (syntaxe ADO pour Visual C++)
## <a name="methods"></a>Méthodes  
  
```  
Cancel(void)  
Close(void)  
CopyTo(_ADOStream *DestStream, LONG CharNumber = -1)  
Flush(void)  
LoadFromFile(BSTR FileName)  
Open(VARIANT Source, ConnectModeEnum Mode, StreamOpenOptionsEnum Options, BSTR UserName, BSTR Password)  
Read(long NumBytes, VARIANT *pVal)  
ReadText(long NumChars, BSTR *pbstr)  
SaveToFile(BSTR FileName, SaveOptionsEnum Options = adSaveCreateNotExist)  
SetEOS(void)  
SkipLine(void)  
Write(VARIANT Buffer)  
WriteText(BSTR Data, StreamWriteEnum Options = adWriteChar)  
```  
  
## <a name="properties"></a>Propriétés  
  
```  
get_Charset(BSTR *pbstrCharset)  
put_Charset(BSTR Charset)  
get_EOS(VARIANT_BOOL *pEOS)  
get_LineSeparator(LineSeparatorEnum *pLS)  
put_LineSeparator(LineSeparatorEnum LineSeparator)  
get_Mode(ConnectModeEnum *pMode)  
put_Mode(ConnectModeEnum Mode)  
get_Position(LONG *pPos)  
put_Position(LONG Position)  
get_Size(LONG *pSize)  
get_State(ObjectStateEnum *pState)  
get_Type(StreamTypeEnum *pType)  
put_Type(StreamTypeEnum Type)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Stream, objet (ADO)](./stream-object-ado.md)