---
description: RecordOpenOptionsEnum
title: RecordOpenOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordOpenOptionsEnum
helpviewer_keywords:
- RecordOpenOptionsEnum enumeration [ADO]
ms.assetid: 9028aba4-90fc-4dfc-88e4-fa8a7b6fedee
author: rothja
ms.author: jroth
ms.openlocfilehash: 0ded43552086f814f9ea80fae65d6792e525d24b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170450"
---
# <a name="recordopenoptionsenum"></a>RecordOpenOptionsEnum
Spécifie les options d’ouverture d’un [enregistrement](./record-object-ado.md). Ces valeurs peuvent être combinées à l’aide de ou de.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adDelayFetchFields**|0x8000|Indique au fournisseur que les champs associés à l' **enregistrement** n’ont pas besoin d’être récupérés initialement, mais peuvent être récupérés lors de la première tentative d’accès au champ. Le comportement par défaut, indiqué par l’absence de cet indicateur, consiste à récupérer tous les champs d’objet **enregistrement** .|  
|**adDelayFetchStream**|0x4000|Indique au fournisseur que le flux par défaut associé à l' **enregistrement** n’a pas besoin d’être récupéré initialement. Le comportement par défaut, indiqué par l’absence de cet indicateur, consiste à récupérer le flux par défaut associé à l’objet **enregistrement** .|  
|**adOpenAsync**|0x1000|Indique que l’objet **Record** est ouvert en mode asynchrone.|  
|**adOpenExecuteCommand**|0x10000|Indique que la chaîne source contient le texte de la commande qui doit être exécuté. Cette valeur est équivalente à l’option **adCmdText** sur **Recordset. Open**.|  
|**adOpenRecordUnspecified**|-1|Par défaut. Indique qu'aucune option n'est spécifiée.|  
|**adOpenOutput**|0x800000|Indique que si la source pointe vers un nœud qui contient un script exécutable (tel qu’un. ASP), l' **enregistrement** ouvert contient les résultats du script exécuté. Cette valeur est valide uniquement avec les enregistrements qui ne sont pas des collections.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [Open, méthode (objet Record ADO)](./open-method-ado-record.md)