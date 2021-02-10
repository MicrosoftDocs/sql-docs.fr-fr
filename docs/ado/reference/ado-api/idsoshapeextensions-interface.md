---
description: IDSOShapeExtensions, interface
title: Interface IDSOShapeExtensions | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- IDSOShapeExtensions interface [ADO]
ms.assetid: ad4ba313-1161-4bc7-b8f6-4083305bc81e
author: rothja
ms.author: jroth
ms.openlocfilehash: 329c7b3193bd86ad05c757229477b5934d6e650d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020796"
---
# <a name="idsoshapeextensions-interface"></a>IDSOShapeExtensions, interface
Obtient l’objet source de données OLE DB sous-jacent pour le fournisseur de formes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
interface IDSOShapeExtensions: public IUnknown {  
public:  
      HRESULT  GetDataProviderDSO(  
            IUnknown **ppDataProviderDSOIUnknown  
      );  
};  
```  
  
## <a name="methods"></a>Méthodes  
  
|Méthode|Description|  
|-|-|  
|[GetDataProviderDSO, méthode](./getdataproviderdso-method.md)|Récupère l’objet source de données OLE DB sous-jacent à partir du fournisseur Shape.|  
  
## <a name="requirements"></a>Spécifications  
 **Version :** ADO 2,0 et versions ultérieures  
  
 **Bibliothèque :** msado15.dll  
  
 **UUID :** 00000283-0000-0010-8000-00AA006D2EA4