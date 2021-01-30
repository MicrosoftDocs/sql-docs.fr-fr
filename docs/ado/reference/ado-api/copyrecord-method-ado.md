---
description: CopyRecord, méthode (ADO)
title: CopyRecord, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_CopyRecord
- _Record::CopyRecord
helpviewer_keywords:
- CopyRecord method [ADO]
ms.assetid: b9bcf272-3c74-479f-95dd-0229a32e98fc
author: rothja
ms.author: jroth
ms.openlocfilehash: d2f6cd1b8bb40b4c62ad2c75fef63fd641201e9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171410"
---
# <a name="copyrecord-method-ado"></a>CopyRecord, méthode (ADO)
Copie une entité représentée par un [enregistrement](./record-object-ado.md) à un autre emplacement.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Record.CopyRecord (Source, Destination, UserName, Password, Options, Async)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 Facultatif. Valeur de **chaîne** qui contient une URL spécifiant l’entité à copier (par exemple, un fichier ou un répertoire). Si la *source* est omise ou qu’elle spécifie une chaîne vide, le fichier ou le répertoire représenté par l' [enregistrement](./record-object-ado.md) actif sera copié.  
  
 *Destination*  
 Facultatif. Valeur de **chaîne** qui contient une URL spécifiant l’emplacement où la *source* sera copiée.  
  
 *UserName*  
 Facultatif. Valeur de **chaîne** qui contient l’ID d’utilisateur qui, le cas échéant, autorise l’accès à la *destination*.  
  
 *Mot de passe*  
 Facultatif. Valeur de **chaîne** qui contient le mot de passe qui, le cas échéant, vérifie le *nom d’utilisateur*.  
  
 *Options*  
 Facultatif. Valeur [CopyRecordOptionsEnum](./copyrecordoptionsenum.md) qui a une valeur par défaut de **adCopyUnspecified**. Spécifie le comportement de cette méthode.  
  
 *Async*  
 Facultatif. Valeur **booléenne** qui, lorsqu’elle a la **valeur true**, spécifie que cette opération doit être asynchrone.  
  
## <a name="return-value"></a>Valeur renvoyée  
 Valeur de **chaîne** qui retourne généralement la valeur de *destination*. Toutefois, la valeur exacte retournée est dépendante du fournisseur.  
  
## <a name="remarks"></a>Notes  
 Les valeurs de *source* et de *destination* ne doivent pas être identiques ; dans le cas contraire, une erreur d’exécution se produit. Au moins un des noms de serveur, de chemin d’accès ou de ressource doit être différent.  
  
 Tous les enfants (par exemple, les sous-répertoires) de la *source* sont copiés de manière récursive, sauf si **adCopyNonRecursive** est spécifié. Dans une opération récursive, la *destination* ne doit pas être un sous-répertoire de la *source*; dans le cas contraire, l’opération ne se termine pas.  
  
 Cette méthode échoue si la *destination* identifie une entité existante (par exemple, un fichier ou un répertoire), sauf si **adCopyOverwrite** est spécifié.  
  
> [!IMPORTANT]
>  Utilisez l’option **adCopyOverwrite** judicieusement. Par exemple, si vous spécifiez cette option lors de la copie d’un fichier dans un répertoire, le répertoire est *supprimé* et remplacé par le fichier.  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Record, objet (ADO)](./record-object-ado.md)