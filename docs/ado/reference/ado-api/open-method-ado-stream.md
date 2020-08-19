---
description: Open, méthode (Stream ADO)
title: Open, méthode (objet Stream ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Stream::raw_Open
- _Stream::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: d26f48fb-904e-4932-a245-3b4332ca1600
author: rothja
ms.author: jroth
ms.openlocfilehash: baa3cb3eb5b2284a606362e31e0f185ab768c5f8
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88442951"
---
# <a name="open-method-ado-stream"></a>Open, méthode (Stream ADO)
Ouvre un objet de [flux](../../../ado/reference/ado-api/stream-object-ado.md) pour manipuler des flux de données binaires ou de texte.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.Open Source, Mode , OpenOptions, UserName, Password  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 facultatif. Valeur de **type Variant** qui spécifie la source de données pour le **flux**. La *source* peut contenir une chaîne d’URL absolue qui pointe vers un nœud existant dans une arborescence connue, telle qu’un message électronique ou un système de fichiers. Une URL doit être spécifiée à l’aide du mot clé URL (« URL =*Scheme*://*Server* / *Folder*»). Sinon, la *source* peut contenir une référence à un objet [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) déjà ouvert, ce qui ouvre le flux par défaut associé à l' **enregistrement**. Si la *source* n’est pas spécifiée, un **flux** est instancié et ouvert, mais il n’est associé à aucune source sous-jacente par défaut. Pour plus d’informations sur les schémas d’URL et leurs fournisseurs associés, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
 *Mode*  
 facultatif. Valeur de [ConnectModeEnum](../../../ado/reference/ado-api/connectmodeenum.md) qui spécifie le mode d’accès pour le **flux** résultant (par exemple, en lecture/écriture ou en lecture seule). La valeur par défaut est **adModeUnknown**. Pour plus d’informations sur les modes d’accès, consultez la propriété [mode](../../../ado/reference/ado-api/mode-property-ado.md) . Si le *mode* n’est pas spécifié, il est hérité par l’objet source. Par exemple, si l' **enregistrement** source est ouvert en mode lecture seule, le **flux** est également ouvert en mode lecture seule par défaut.  
  
 *OpenOptions*  
 facultatif. Valeur [StreamOpenOptionsEnum](../../../ado/reference/ado-api/streamopenoptionsenum.md) . La valeur par défaut est **adOpenStreamUnspecified**.  
  
 *UserName*  
 facultatif. Valeur de **chaîne** qui contient l’identification de l’utilisateur qui, si nécessaire, accède à l’objet de **flux** .  
  
 *Mot de passe*  
 facultatif. Valeur de **chaîne** qui contient le mot de passe qui, le cas échéant, accède à l’objet de **flux** .  
  
## <a name="remarks"></a>Notes  
 Lorsqu’un objet **Record** est transmis comme paramètre source, les paramètres *userid* et *Password* ne sont pas utilisés, car l’accès à l’objet **Record** est déjà disponible. De même, le [mode](../../../ado/reference/ado-api/mode-property-ado.md) de l’objet **Record** est transféré à l’objet **Stream** . Lorsque *source* n’est pas spécifié, le **flux** ouvert ne contient pas de données et sa [taille](../../../ado/reference/ado-api/size-property-ado-stream.md) est égale à zéro (0). Pour éviter de perdre les données écrites dans ce **flux** lorsque le **flux** est fermé, enregistrez le **flux** avec les méthodes [CopyTo](../../../ado/reference/ado-api/copyto-method-ado.md) ou [SaveToFile](../../../ado/reference/ado-api/savetofile-method.md) , ou enregistrez-le dans un autre emplacement de mémoire.  
  
 Une valeur *OpenOptions* de **adOpenStreamFromRecord** identifie le contenu du paramètre *source* comme un objet **enregistrement** déjà ouvert. Le comportement par défaut consiste à traiter la *source* comme une URL qui pointe directement vers un nœud dans une arborescence, tel qu’un fichier. Le flux par défaut associé à ce nœud est ouvert.  
  
 Si le **flux** n’est pas ouvert, il est possible de lire toutes les propriétés en lecture seule du **flux**. Si un **flux** est ouvert de façon asynchrone, toutes les opérations suivantes (autres que la vérification de l' [État](../../../ado/reference/ado-api/state-property-ado.md) et d’autres propriétés en lecture seule) sont bloquées jusqu’à la fin de l’opération d' **ouverture** .  
  
 En plus des options présentées précédemment, en ne spécifiant pas de *source*, vous pouvez créer une instance d’un objet de **flux** dans la mémoire sans l’associer à une source sous-jacente. Vous pouvez ajouter dynamiquement des données au flux en écrivant des données binaires ou de texte dans le **flux** avec [Write](../../../ado/reference/ado-api/write-method.md) ou [WRITETEXT](../../../ado/reference/ado-api/writetext-method.md), ou en chargeant des données à partir d’un fichier avec [LoadFromFile](../../../ado/reference/ado-api/loadfromfile-method-ado.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Open, méthode (connexion ADO)](../../../ado/reference/ado-api/open-method-ado-connection.md)   
 [Open, méthode (ADO record)](../../../ado/reference/ado-api/open-method-ado-record.md)   
 [Open, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [OpenSchema, méthode](../../../ado/reference/ado-api/openschema-method.md)   
 [SaveToFile, méthode](../../../ado/reference/ado-api/savetofile-method.md)
