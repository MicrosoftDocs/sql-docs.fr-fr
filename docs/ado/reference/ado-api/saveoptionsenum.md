---
description: SaveOptionsEnum
title: SaveOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- SaveOptionsEnum
helpviewer_keywords:
- SaveOptionsEnum enumeration [ADO]
ms.assetid: 59339100-6e29-48d1-aea3-6873796d186b
author: rothja
ms.author: jroth
ms.openlocfilehash: 56e16ff6ca93dde78531394dc474a2e5cc817348
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88989310"
---
# <a name="saveoptionsenum"></a>SaveOptionsEnum
Spécifie si un fichier doit être créé ou remplacé lors de l’enregistrement à partir d’un objet de [flux](./stream-object-ado.md) . Les valeurs peuvent être **adSaveCreateNotExist** ou **adSaveCreateOverWrite**.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adSaveCreateNotExist**|1|Par défaut. Crée un nouveau fichier si le fichier spécifié par le paramètre de *nom* de fichier n’existe pas déjà.|  
|**adSaveCreateOverWrite**|2|Remplace le fichier par les données de l’objet de **flux** actuellement ouvert, si le fichier spécifié par le paramètre *filename* existe déjà. Si le fichier spécifié par le paramètre *filename* n’existe pas, un nouveau fichier est créé.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [SaveToFile, méthode](./savetofile-method.md)