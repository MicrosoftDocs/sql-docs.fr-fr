---
description: SaveToFile, méthode
title: SaveToFile, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_SaveToFile
- _Stream::SaveToFile
helpviewer_keywords:
- SaveToFile method [ADO]
ms.assetid: 8a8594f2-422b-4d2e-94f8-7fe337445900
author: rothja
ms.author: jroth
ms.openlocfilehash: febf5627d46bbb464ff01c41a0eee23b2242c801
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040699"
---
# <a name="savetofile-method"></a>SaveToFile, méthode
Enregistre le contenu binaire d’un [flux](./stream-object-ado.md) dans un fichier.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.SaveToFile FileName, SaveOptions  
```  
  
#### <a name="parameters"></a>Paramètres  
 *FileName*  
 Valeur de **chaîne** qui contient le nom qualifié complet du fichier dans lequel le contenu du **flux** sera enregistré. Vous pouvez enregistrer dans n’importe quel emplacement local valide, ou à tout emplacement auquel vous avez accès par le biais d’une valeur UNC.  
  
 *SaveOptions*  
 Valeur [SaveOptionsEnum](./saveoptionsenum.md) qui spécifie si un nouveau fichier doit être créé par **SaveToFile**, s’il n’existe pas déjà. La valeur par défaut est **adSaveCreateNotExists**. Ces options vous permettent de spécifier qu’une erreur se produit si le fichier spécifié n’existe pas. Vous pouvez également spécifier que la **SaveToFile** remplace le contenu actuel d’un fichier existant.  
  
> [!NOTE]
>  Si vous remplacez un fichier existant (lorsque **adSaveCreateOverwrite** est défini), **SaveToFile** tronque tous les octets du fichier existant d’origine qui suivent le nouveau [EOS](./eos-property.md).  
  
## <a name="remarks"></a>Notes  
 La **SaveToFile** peut être utilisée pour copier le contenu d’un objet de **flux** dans un fichier local. Il n’y a aucune modification dans le contenu ou les propriétés de l’objet de **flux** . L’objet de **flux** doit être ouvert avant d’appeler **SaveToFile**.  
  
 Cette méthode ne modifie pas l’Association de l’objet de **flux** à sa source sous-jacente. L’objet de **flux** est toujours associé à l’URL ou à l' **enregistrement** d’origine qui était sa source lors de son ouverture.  
  
 Après une opération **SaveToFile** , la position actuelle ([position](./position-property-ado.md)) dans le flux est définie au début du flux (0).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Open, méthode (objet Stream ADO)](./open-method-ado-stream.md)   
 [Save, méthode](./save-method.md)