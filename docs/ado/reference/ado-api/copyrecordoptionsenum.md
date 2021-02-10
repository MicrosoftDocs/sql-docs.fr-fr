---
description: CopyRecordOptionsEnum
title: CopyRecordOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CopyRecordOptionsEnum
helpviewer_keywords:
- CopyRecordOptionsEnum enumeration [ADO]
ms.assetid: 2fa4eec5-d50b-4fd3-8ae7-40af441ba12b
author: rothja
ms.author: jroth
ms.openlocfilehash: 2ea414447624d8b1aa7c27e250a92dc227020304
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034609"
---
# <a name="copyrecordoptionsenum"></a>CopyRecordOptionsEnum
Spécifie le comportement de la méthode [CopyRecord](./copyrecord-method-ado.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adCopyAllowEmulation**|4|Indique que le fournisseur *source* tente de simuler la copie à l’aide des opérations de téléchargement et de téléchargement si cette méthode échoue en raison de la *destination* sur un autre serveur ou par un fournisseur différent de la *source*. Notez que des fonctionnalités de fournisseur différentes peuvent nuire aux performances ou perdre des données.|  
|**adCopyNonRecursive**|2|Copie le répertoire actif, mais aucun de ses sous-répertoires, vers la destination. L’opération de copie n’est pas récursive.|  
|**adCopyOverWrite**|1|Remplace le fichier ou le répertoire si la *destination* pointe vers un fichier ou un répertoire existant.|  
|**adCopyUnspecified**|-1|Par défaut. Effectue l’opération de copie par défaut : l’opération échoue si le fichier ou le répertoire de destination existe déjà et que l’opération copie de manière récursive.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [CopyRecord, méthode (ADO)](./copyrecord-method-ado.md)