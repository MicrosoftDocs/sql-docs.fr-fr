---
description: GetChildren, méthode (ADO)
title: GetChildren, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_GetChildren
- _Record::GetChildren
helpviewer_keywords:
- GetChildren method [ADO]
ms.assetid: b3f09bac-4f66-49f6-aa5a-6fbb4fb28338
author: rothja
ms.author: jroth
ms.openlocfilehash: cb49de0d3613cca9d24991e67ee8787c0d863b8c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167288"
---
# <a name="getchildren-method-ado"></a>GetChildren, méthode (ADO)
Retourne un [jeu d’enregistrements](./recordset-object-ado.md) dont les lignes représentent les enfants d’un [enregistrement](./record-object-ado.md)de collection.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set recordset = record.GetChildren  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Objet **Recordset** pour lequel chaque ligne représente un enfant de l’objet **enregistrement** actif. Par exemple, les enfants d’un **enregistrement** qui représente un répertoire sont les fichiers et les sous-répertoires contenus dans le répertoire parent.  
  
## <a name="remarks"></a>Notes  
 Le fournisseur détermine les colonnes qui existent dans le **jeu d’enregistrements** retourné. Par exemple, un fournisseur de source de document retourne toujours un **jeu d’enregistrements** de ressources.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Record, objet (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Recordset, objet (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::