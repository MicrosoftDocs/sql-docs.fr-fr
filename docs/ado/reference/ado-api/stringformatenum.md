---
description: StringFormatEnum
title: StringFormatEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StringFormatEnum
helpviewer_keywords:
- StringFormatEnum enumeration [ADO]
ms.assetid: 28f7d1ec-092b-4323-a39d-d3f882c6c81a
author: rothja
ms.author: jroth
ms.openlocfilehash: b23a6bc4354f4c67c07bcdc1f4b4d463fff072f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056504"
---
# <a name="stringformatenum"></a>StringFormatEnum
Spécifie le format lors de la récupération d’un [jeu d’enregistrements](./recordset-object-ado.md) sous forme de chaîne.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adClipString**|2|Délimite les lignes par *RowDelimiter*, les colonnes par *ColumnDelimiter* et les valeurs NULL par *NullExpr*. Ces trois paramètres de la méthode [GetString](./getstring-method-ado.md) sont valides uniquement avec un *StringFormat* de **adClipString**.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.StringFormat.CLIPSTRING|  
  
## <a name="applies-to"></a>S'applique à  
 [GetString, méthode (ADO)](./getstring-method-ado.md)