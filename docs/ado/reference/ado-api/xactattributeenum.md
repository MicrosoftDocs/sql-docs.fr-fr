---
description: XactAttributeEnum
title: XactAttributeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- XactAttributeEnum
helpviewer_keywords:
- XactAttributeEnum enumeration [ADO]
ms.assetid: e7dcecd3-7dc7-445c-b922-f700c3067fbc
author: rothja
ms.author: jroth
ms.openlocfilehash: e741ec0e5458218ddb6dc3a46c3a098acf482611
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056024"
---
# <a name="xactattributeenum"></a>XactAttributeEnum
Spécifie les attributs de transaction d’un objet de [connexion](./connection-object-ado.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adXactAbortRetaining**|262 144|Effectue des abandons de rétention en appelant [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) pour démarrer automatiquement une nouvelle transaction. Tous les fournisseurs ne prennent pas en charge ce comportement.|  
|**adXactCommitRetaining**|131 072|Effectue la conservation des validations en appelant [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) pour démarrer automatiquement une nouvelle transaction. Tous les fournisseurs ne prennent pas en charge ce comportement.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.XactAttribute.ABORTRETAINING|  
|AdoEnums.XactAttribute.COMMITRETAINING|  
  
## <a name="applies-to"></a>S'applique à  
 [Attributes, propriété (ADO)](./attributes-property-ado.md)