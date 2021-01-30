---
description: Collections (ADO - syntaxe WFC)
title: Collections (ADO-syntaxe WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- syntax indexes [ADO], ADO/WFC
- collections [ADO], ADO/WFC syntax
- ADO/WFC syntax index [ADO]
ms.assetid: 073f9a0e-c755-42dd-9f71-4647d68e331a
author: rothja
ms.author: jroth
ms.openlocfilehash: d8fca0d29736fb9a57d9a8f72703d3870db15087
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171506"
---
# <a name="collections-ado---wfc-syntax"></a>Collections (ADO - syntaxe WFC)
**package com. ms. wfc. Data**  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="methods"></a>Méthodes  
  
```  
public void append(com.ms.wfc.data.Parameter param)  
public void delete(int n)  
public void delete(String s)  
public void refresh()  
public Parameter getItem(int n)  
public Parameter getItem(String s)  
```  
  
### <a name="properties"></a>Propriétés  
  
```  
public int getCount()  
```  
  
## <a name="fields"></a>Champs  
  
### <a name="methods"></a>Méthodes  
  
```  
public void append(String name, int type)  
public void append(String name, int type, int definedSize)  
public void append(String name, int type, int definedSize, int attrib)  
public void delete(int n)  
public void delete(String s)  
public void refresh()  
public com.ms.wfc.data.Field getItem(int n)  
public com.ms.wfc.data.Field getItem(String s)  
```  
  
### <a name="properties"></a>Propriétés  
  
```  
public int getCount()  
```  
  
## <a name="errors"></a>Erreurs  
  
### <a name="methods"></a>Méthodes  
  
```  
public void clear()  
public void refresh()  
public com.ms.wfc.data.Error getItem(int n)  
public com.ms.wfc.data.Error getItem(String s)  
```  
  
### <a name="properties"></a>Propriétés  
  
```  
public int getCount()  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Errors, collection (ADO)](./errors-collection-ado.md)   
 [Fields, collection (ADO)](./fields-collection-ado.md)   
 [Parameters, collection (ADO)](./parameters-collection-ado.md)