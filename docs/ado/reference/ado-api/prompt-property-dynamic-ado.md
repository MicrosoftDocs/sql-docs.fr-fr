---
description: Prompt, propriété dynamique (ADO)
title: Property-Dynamic prompt (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Prompt property [ADO]
ms.assetid: c4f001b5-8d16-4d39-a42e-c0e2faaaceaf
author: rothja
ms.author: jroth
ms.openlocfilehash: b5b7e99548e156126455a66be7fdf73a9c00ebbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166830"
---
# <a name="prompt-property-dynamic-ado"></a>Prompt, propriété dynamique (ADO)
Spécifie si le fournisseur de OLE DB doit inviter l’utilisateur à fournir des informations d’initialisation.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit et retourne une valeur [ConnectPromptEnum](./connectpromptenum.md) .  
  
## <a name="remarks"></a>Notes  
 **Prompt** est une propriété dynamique, qui peut être ajoutée à la collection de [Propriétés](./properties-collection-ado.md) de l’objet de [connexion](./connection-object-ado.md) par le fournisseur OLE DB. Pour demander des informations d’initialisation, OLE DB fournisseurs affichent généralement une boîte de dialogue à l’utilisateur.  
  
 Les propriétés dynamiques d’un objet de [connexion](./connection-object-ado.md) sont perdues lorsque la **connexion** est fermée. La propriété **prompt** doit être réinitialisée avant la réouverture de la **connexion** pour utiliser une valeur autre que la valeur par défaut.  
  
> [!NOTE]
>  Ne spécifiez pas que le fournisseur doit inviter l’utilisateur dans les scénarios où l’utilisateur ne sera pas en mesure de répondre à la boîte de dialogue. Par exemple, l’utilisateur ne peut pas répondre si l’application s’exécute sur un système serveur plutôt que sur le client de l’utilisateur, ou si l’application s’exécute sur un système sans utilisateur connecté. Dans ce cas, l’application attend indéfiniment une réponse et semble être verrouillée.  
  
## <a name="usage"></a>Usage  
  
```  
Set cn = New Connection  
cn.Provider = "SQLOLEDB"  
cn.Properties("Prompt") = adPromptNever    ' do not prompt the user  
```  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](./connection-object-ado.md)