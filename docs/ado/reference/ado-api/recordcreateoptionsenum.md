---
description: RecordCreateOptionsEnum
title: RecordCreateOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- RecordCreateOptionsEnum
helpviewer_keywords:
- RecordCreateOptionsEnum enumeration [ADO]
ms.assetid: 6d746670-0850-4065-9cd4-168dea1d3ea9
author: rothja
ms.author: jroth
ms.openlocfilehash: 2fba315867e49935557d7bd6b3abe04c942e5a0a
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88772448"
---
# <a name="recordcreateoptionsenum"></a>RecordCreateOptionsEnum
Spécifie si un **enregistrement existant doit** être ouvert ou si un nouvel **enregistrement** a été créé pour la méthode d' [ouverture](./open-method-ado-record.md) de l’objet [Record](./record-object-ado.md) . Les valeurs peuvent être combinées avec un opérateur AND.  
  
|Constant|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adCreateCollection**|0x2000|Crée un **enregistrement** au niveau du nœud spécifié par le paramètre *source* , au lieu d’ouvrir un **enregistrement**existant. Si la source pointe vers un nœud existant, une erreur d’exécution se produit, sauf si **adCreateCollection** est combiné avec **adOpenIfExists** ou **adCreateOverwrite**.|  
|**adCreateNonCollection**|0|Crée un nouvel **enregistrement** de type [adSimpleRecord](./recordtypeenum.md).|  
|**adCreateOverwrite**|0x4000000|Modifie les indicateurs de création **adCreateCollection**, **adCreateNonCollection**et **adCreateStructDoc**. Lorsque ou est utilisé avec cette valeur et l’une des valeurs d’indicateur de création, si l’URL source pointe vers un nœud ou un **enregistrement**existant, l' **enregistrement** existant est remplacé et un nouvel enregistrement est créé à la place. Cette valeur ne peut pas être utilisée avec **adOpenIfExists**.|  
|**adCreateStructDoc**|0x80000000|Crée un nouvel **enregistrement** de type [adStructDoc](./recordtypeenum.md), au lieu d’ouvrir un **enregistrement**existant.|  
|**adFailIfNotExists**|-1|Par défaut. Génère une erreur au moment de l’exécution si la *source* pointe vers un nœud inexistant.|  
|**adOpenIfExists**|0x2000000|Modifie les indicateurs de création **adCreateCollection**, **adCreateNonCollection**et **adCreateStructDoc**. Lorsque ou est utilisé avec cette valeur et l’une des valeurs d’indicateur de création, si l’URL source pointe vers un objet de nœud ou d' **enregistrement** existant, le fournisseur doit ouvrir l' **enregistrement** existant au lieu d’en créer un nouveau. Cette valeur ne peut pas être utilisée avec **adCreateOverwrite**.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [Open, méthode (objet Record ADO)](./open-method-ado-record.md)