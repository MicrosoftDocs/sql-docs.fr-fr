---
description: Field (ADO - syntaxe WFC)
title: Field (syntaxe ADO-WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Field collection [ADO], ADO/WFC syntax
ms.assetid: 7e01cb24-2338-4f92-ad46-8d97248e1a4d
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d998deb2d09e77e54ba9c733e371cc60a439761
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034099"
---
# <a name="field-ado---wfc-syntax"></a>Field (ADO - syntaxe WFC)
## <a name="package-commswfcdata"></a>package com. ms. wfc. Data  
  
### <a name="methods"></a>Méthodes  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
public byte[] getByteChunk(int len)  
public char[] getCharChunk(int len)  
public String getStringChunk(int len)  
```  
  
### <a name="properties"></a>Propriétés  
  
```  
public int getActualSize()  
public int getAttributes()  
public void setAttributes(int pl)  
public com.ms.com.IUnknown getDataFormat()  
public void setDataFormat(com.ms.com.IUnknown format)  
```  
  
 (Pour plus d’informations, consultez la documentation de l’interface com. ms. wfc. Data. IDataFormat.)  
  
```  
public int getDefinedSize()  
public void setDefinedSize(int pl)  
public String getName()  
public int getNumericScale()  
public void setNumericScale(byte pbNumericScale)  
public Variant getOriginalValue()  
public int getPrecision()  
public void setPrecision(byte pbPrecision)  
public int getType()  
public void setType(int pDataType)  
public Variant getUnderlyingValue()  
public Variant getValue()  
public void setValue(Variant value)  
public AdoProperties getProperties()  
```  
  
### <a name="field-accessor-methods"></a>Méthodes d’accesseur de champ  
 La propriété [value](../../../ado/reference/ado-api/value-property-ado.md) d’un objet [Field](../../../ado/reference/ado-api/field-object.md) obtient ou définit le contenu de cet objet. Le contenu est représenté sous la forme d’une variante, d’un type d’objet auquel une valeur et l’un des différents types de données peuvent être attribués.  
  
 ADO/WFC implémente la propriété **value** avec la méthode **GetValue** , qui retourne un objet variant ; et la méthode **SetValue** , qui prend une variante comme argument. Les VARIANTEs sont très efficaces dans certains langages, tels que Microsoft Visual Basic.  
  
 En plus de la propriété **value** , ADO/WFC fournit des méthodes d' *accesseur* qui utilisent des types de données Java pour récupérer et définir le contenu des objets **Field** . La plupart de ces méthodes ont des noms au format **obtenir** le _type de données_ ou **définir** le type de _données_.  
  
 Il existe deux exceptions intéressantes : l’une des méthodes **GetObject** retourne un objet forcé dans une classe spécifiée. Il n’existe aucune propriété **getNull** ; au lieu de cela, il existe une propriété **IsNull** qui retourne une valeur booléenne indiquant si le champ est null.  
  
```  
public native boolean getBoolean();  
public void setBoolean(boolean v)  
public native byte getByte();  
public void setByte(byte v)  
public native byte[] getBytes();  
public void setBytes(byte[] v)  
public native double getDouble();  
public void setDouble(double v)  
public native float getFloat();  
public void setFloat(float v)  
public native int getInt();  
public void setInt(int v)  
public native long getLong();  
public void setLong(long v)  
public native short getShort();  
public void setShort(short v)  
public native String getString();  
public void setString(String v)  
public native boolean isNull();  
public void setNull()  
public Object getObject()  
public Object getObject(Class c)  
public void setObject(Object value)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Objet Field](../../../ado/reference/ado-api/field-object.md)
