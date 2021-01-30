---
description: SeekEnum
title: SeekEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SeekEnum
helpviewer_keywords:
- SeekEnum enumeration [ADO]
ms.assetid: f0ec0c92-8253-47c6-9a14-e5dbccbad219
author: rothja
ms.author: jroth
ms.openlocfilehash: f61e05965b1750c4f8e6b48f092eeb059d1e19d3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166561"
---
# <a name="seekenum"></a>SeekEnum
Spécifie le type de [recherche](./seek-method.md) à exécuter.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adSeekFirstEQ**|1|Recherche la première clé égale à *KeyValues*.|  
|**adSeekLastEQ**|2|Recherche la dernière clé égale à *KeyValues*.|  
|**adSeekAfterEQ**|4|Recherche une clé égale à *KeyValues* ou juste après l’emplacement où cette correspondance s’est produite.|  
|**adSeekAfter**|8|Recherche une clé juste après l’endroit où une correspondance avec *KeyValues* s’est produite.|  
|**adSeekBeforeEQ**|16|Recherche une clé égale à *KeyValues* ou juste avant l’emplacement où cette correspondance s’est produite.|  
|**adSeekBefore**|32|Recherche une clé juste avant l’endroit où une correspondance avec *KeyValues* s’est produite.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.Seek.FIRSTEQ|  
|AdoEnums.Seek.LASTEQ|  
|AdoEnums.Seek.AFTEREQ|  
|AdoEnums. Seek. après|  
|AdoEnums. Seek. BEFOREEQ|  
|AdoEnums.Seek.BEFORE|  
  
## <a name="applies-to"></a>S'applique à  
 [Seek, méthode](./seek-method.md)