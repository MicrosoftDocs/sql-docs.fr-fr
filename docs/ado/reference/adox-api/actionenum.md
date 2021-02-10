---
description: ActionEnum
title: ActionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ActionEnum
helpviewer_keywords:
- ActionEnum enumeration [ADOX]
ms.assetid: f948febd-c885-4621-823b-421e116fec4e
author: rothja
ms.author: jroth
ms.openlocfilehash: fd2104a3b334da02e9bceee3ef191baac1d66cab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050790"
---
# <a name="actionenum"></a>ActionEnum
Spécifie le type d’action à effectuer lorsque l’opération [SetPermissions](./setpermissions-method-adox.md) est appelée.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adAccessDeny**|3|Les autorisations spécifiées sont refusées au groupe ou à l’utilisateur.|  
|**adAccessGrant**|1|Le groupe ou l’utilisateur aura au moins les autorisations demandées.|  
|**adAccessRevoke**|4|Tous les droits d’accès explicites du groupe ou de l’utilisateur seront révoqués.|  
|**adAccessSet**|2|Le groupe ou l’utilisateur aura exactement les autorisations demandées.|