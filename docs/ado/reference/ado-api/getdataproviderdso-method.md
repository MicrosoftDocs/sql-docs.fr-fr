---
description: GetDataProviderDSO, méthode
title: Méthode GetDataProviderDSO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- GetDataProviderDSO Method [ADO]
ms.assetid: 5a4c6bd5-0c79-4f81-a977-0561392d8d50
author: rothja
ms.author: jroth
ms.openlocfilehash: c784e5c3951f78ef28bfd0d0008570d1b1e68ca8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167276"
---
# <a name="getdataproviderdso-method"></a>GetDataProviderDSO, méthode
Récupère l’objet source de données OLE DB sous-jacent à partir du fournisseur Shape.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT GetDataProviderDSO(  
      IUnknown **ppDataProviderDSOIUnknown  
);  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ppDataProviderDSOIUnknown*  
 à  Pointeur vers un pointeur qui retourne l’IUnknown de l’objet OLE DB source de données sous-jacent.  
  
## <a name="remarks"></a>Notes  
 Cette méthode ne fait pas de AddRef avec le pointeur d’interface. Si l’appelant envisage de conserver le pointeur, l’appelant doit faire les AddRef et Release requis.  
  
## <a name="applies-to"></a>S’applique à  
 [IDSOShapeExtensions, interface](./idsoshapeextensions-interface.md)