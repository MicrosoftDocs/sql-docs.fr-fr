---
description: StreamWriteEnum
title: StreamWriteEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamWriteEnum
helpviewer_keywords:
- StreamWriteEnum enumeration [ADO]
ms.assetid: bdbf3405-a0bd-4f02-85d4-e3fe8da3f3f7
author: rothja
ms.author: jroth
ms.openlocfilehash: f0f102ec1457b0cb327ab563ee348cf4112c749a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056544"
---
# <a name="streamwriteenum"></a>StreamWriteEnum
Spécifie si un séparateur de ligne est ajouté à la chaîne écrite dans un objet de [flux](./stream-object-ado.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adWriteChar**|0|Par défaut. Écrit la chaîne de texte spécifiée (spécifiée par le paramètre de *données* ) dans l’objet de **flux** .|  
|**adWriteLine**|1|Écrit une chaîne de texte et un caractère de séparation de ligne dans un objet de **flux** . Si la propriété [LineSeparator](./lineseparator-property-ado.md) n’est pas définie, elle retourne une erreur d’exécution.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [WriteText, méthode](./writetext-method.md)