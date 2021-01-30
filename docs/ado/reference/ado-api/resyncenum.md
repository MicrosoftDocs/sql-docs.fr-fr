---
description: ResyncEnum
title: ResyncEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ResyncEnum
helpviewer_keywords:
- ResyncEnum enumeration [ADO]
ms.assetid: d3df2c90-e570-4c40-a79a-25b3448a009c
author: rothja
ms.author: jroth
ms.openlocfilehash: c20965a348227583580bb75a807e792f74a9e967
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166624"
---
# <a name="resyncenum"></a>ResyncEnum
Spécifie si les valeurs sous-jacentes sont remplacées par un appel à [Resync](./resync-method.md).  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adResyncAllValues**|2|Par défaut. Remplace les données, et les mises à jour en attente sont annulées.|  
|**adResyncUnderlyingValues**|1|Ne remplace pas les données et les mises à jour en attente ne sont pas annulées.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.Resync.ALLVALUES|  
|AdoEnums.Resync.UNDERLYINGVALUES|  
  
## <a name="applies-to"></a>S'applique à  
 [Resync, méthode](./resync-method.md)