---
description: MoveRecord, méthode (ADO)
title: MoveRecord, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::MoveRecord
- _Record::raw_MoveRecord
helpviewer_keywords:
- MoveRecord method [ADO]
ms.assetid: 6d2807b0-b861-4583-bcaf-fb0b82e0f2d0
author: rothja
ms.author: jroth
ms.openlocfilehash: d55a2a2aacc281271789d2c456ea5bdc8de44499
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167079"
---
# <a name="moverecord-method-ado"></a>MoveRecord, méthode (ADO)
Déplace l’entité représentée par un [enregistrement](./record-object-ado.md) à un autre emplacement.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Record.MoveRecord (Source, Destination, UserName, Password, Options, Async)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 Facultatif. Valeur de **chaîne** qui contient une URL identifiant l' **enregistrement** à déplacer. Si la *source* est omise ou qu’elle spécifie une chaîne vide, l’objet représenté par cet **enregistrement** est déplacé. Par exemple, si l' **enregistrement** représente un fichier, le contenu du fichier est déplacé vers l’emplacement spécifié par *destination*.  
  
 *Destination*  
 Facultatif. Valeur de **chaîne** qui contient une URL spécifiant l’emplacement où la *source* sera déplacée.  
  
 *UserName*  
 Facultatif. Valeur de **chaîne** qui contient l’ID d’utilisateur qui, le cas échéant, autorise l’accès à la *destination*.  
  
 *Mot de passe*  
 Facultatif. **Chaîne** qui contient le mot de passe qui, le cas échéant, vérifie le *nom d’utilisateur*.  
  
 *Options*  
 Facultatif. Valeur [MoveRecordOptionsEnum](./moverecordoptionsenum.md) dont la valeur par défaut est **adMoveUnspecified**. Spécifie le comportement de cette méthode.  
  
 *Async*  
 Facultatif. Valeur **booléenne** qui, lorsque la **valeur est true**, spécifie que cette opération doit être asynchrone.  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur de **chaîne**. En règle générale, la valeur de *destination* est retournée. Toutefois, la valeur exacte retournée est dépendante du fournisseur.  
  
## <a name="remarks"></a>Notes  
 Les valeurs de *source* et de *destination* ne doivent pas être identiques ; dans le cas contraire, une erreur d’exécution se produit. Au moins le nom du serveur, du chemin d’accès et des ressources doit être différent.  
  
 Pour les fichiers déplacés à l’aide du fournisseur de publication Internet, cette méthode met à jour tous les liens hypertexte dans les fichiers déplacés, sauf indication contraire dans les *options*. Cette méthode échoue si la *destination* identifie un objet existant (par exemple, un fichier ou un répertoire), sauf si **adMoveOverWrite** est spécifié.  
  
> [!NOTE]
>  Utilisez l’option **adMoveOverWrite** judicieusement. Par exemple, si vous spécifiez cette option lors du déplacement d’un fichier vers un répertoire, le répertoire est supprimé et remplacé par le fichier.  
  
 Certains attributs de l’objet **Record** , tels que la propriété [ParentURL](./parenturl-property-ado.md) , ne sont pas mis à jour une fois cette opération terminée. Actualisez les propriétés de l’objet **Record** en fermant l' **enregistrement**, puis en le réouvrant avec l’URL de l’emplacement où le fichier ou le répertoire a été déplacé.  
  
 Si cet **enregistrement** a été obtenu à partir d’un [jeu d’enregistrements](./recordset-object-ado.md), le nouvel emplacement du fichier ou du répertoire déplacé ne sera pas reflété immédiatement dans le **jeu d’enregistrements**. Actualisez le **Recordset** en le fermant et en le réouvrant.  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Record, objet (ADO)](./record-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Move, méthode (ADO)](./move-method-ado.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (ADO)](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (RDS)](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)