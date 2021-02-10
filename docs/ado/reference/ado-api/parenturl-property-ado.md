---
description: ParentURL, propriété (ADO)
title: ParentURL, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::ParentURL
helpviewer_keywords:
- ParentURL property [ADO]
ms.assetid: 65120ce6-3900-4cd4-b322-3b9816d74737
author: rothja
ms.author: jroth
ms.openlocfilehash: fe54516f1721cdc24da9fbdeaf72688a6c0960a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041039"
---
# <a name="parenturl-property-ado"></a>ParentURL, propriété (ADO)
Indique une chaîne d’URL absolue qui pointe vers l' [enregistrement](./record-object-ado.md) parent de l’objet **enregistrement** actif.  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur de **chaîne** qui indique l’URL de l' **enregistrement** parent.  
  
## <a name="remarks"></a>Notes  
 La propriété **ParentURL** dépend de la source utilisée pour ouvrir l’objet **Record** . Par exemple, l' **enregistrement** peut être ouvert avec une source qui contient un nom de chemin d’accès relatif d’un répertoire référencé par la propriété [ActiveConnection](./activeconnection-property-ado.md) .  
  
 Supposons que « second » est un dossier contenu sous « First ». Ouvrez l’objet **Record** en utilisant la syntaxe suivante :  
  
```  
record.ActiveConnection = "https://first"  
record.Open "second"  
```  
  
 À présent, la valeur de la `the` propriété **ParentURL** est `"https://first"` , identique à **ActiveConnection**.  
  
 La source peut également être une URL absolue telle que, `"https://first/second"` . La propriété **ParentURL** est ensuite `"https://first"` le niveau ci-dessus `"second"` .  
  
 Cette propriété peut être une valeur null si :  
  
-   Il n’y a aucun parent pour l’objet actuel ; par exemple, si l’objet **Record** représente la racine d’un répertoire.  
  
-   L’objet **Record** représente une entité qui ne peut pas être spécifiée avec une URL.  
  
 Cette propriété est en lecture seule.  
  
> [!NOTE]
>  Cette propriété est uniquement prise en charge par les fournisseurs de sources de documents, tels que le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [enregistrements et champs de Provider-Supplied](../../guide/data/records-and-provider-supplied-fields.md).  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
> [!NOTE]
>  Si l’enregistrement actif contient un enregistrement de données d’un **jeu d’enregistrements** ADO, l’accès à la propriété **ParentURL** provoque une erreur d’exécution, indiquant qu’aucune URL n’est possible.  
  
## <a name="applies-to"></a>S'applique à  
 [Record, objet (ADO)](./record-object-ado.md)