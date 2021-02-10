---
description: ConnectPromptEnum
title: ConnectPromptEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ConnectPromptEnum
helpviewer_keywords:
- ConnectPromptEnum enumeration [ADO]
ms.assetid: 21026e24-62b7-4cc9-8aef-62c1fc6cba75
author: rothja
ms.author: jroth
ms.openlocfilehash: 98a4e0d65ffb0596825557813cf12266b9fe79ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034619"
---
# <a name="connectpromptenum"></a>ConnectPromptEnum
Spécifie si une boîte de dialogue doit être affichée pour demander des paramètres manquants lors de l’ouverture d’une connexion à une source de données.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adPromptAlways**|1|Les invites s’affichent toujours.|  
|**adPromptComplete**|2|Demande si des informations supplémentaires sont requises.|  
|**adPromptCompleteRequired**|3|Demande si des informations supplémentaires sont requises, mais que les paramètres facultatifs ne sont pas autorisés.|  
|**adPromptNever**|4|Ne jamais demander.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.ConnectPrompt.ALWAYS|  
|AdoEnums.ConnectPrompt.COMPLETE|  
|AdoEnums.ConnectPrompt.COMPLETEREQUIRED|  
|AdoEnums.ConnectPrompt.NEVER|  
  
## <a name="applies-to"></a>S'applique à  
 [Prompt, propriété dynamique (ADO)](./prompt-property-dynamic-ado.md)