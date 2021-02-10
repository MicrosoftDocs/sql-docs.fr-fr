---
description: ParentRow, propriété (ADO)
title: ParentRow, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction::put_ParentRow
- ADORecordConstruction::ParentRow
- ADORecordConstruction::putParentRow
helpviewer_keywords:
- ParentRow property [ADO]
ms.assetid: 5ea8029b-eda4-490b-ae84-2ad036fb582f
author: rothja
ms.author: jroth
ms.openlocfilehash: 1dbe6152997431b95cfde76d8d45391c8465394e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041069"
---
# <a name="parentrow-property-ado"></a>ParentRow, propriété (ADO)
Définit le conteneur d’un objet OLE DB **Row** sur un objet **ADORecordConstruction** , de sorte que le parent de la ligne soit converti en objet ADO **Record** .  
  
 En écriture seule.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT put_ParentRow([in] IUnknown* pParent);  
```  
  
## <a name="parameters"></a>Paramètres  
 *pParent*  
 Conteneur d’une ligne.  
  
## <a name="return-values"></a>Valeurs de retour  
 Cette méthode de propriété retourne les valeurs HRESULT standard, y compris S_OK et E_FAIL.  
  
## <a name="applies-to"></a>S'applique à  
 [ADORecordConstruction, interface](./adorecordconstruction-interface.md)