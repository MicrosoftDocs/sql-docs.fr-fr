---
description: get_OLEDBCommand, méthode
title: Méthode get_OLEDBCommand | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- get_OLEDBCommand method [ADO]
ms.assetid: 23d551f5-3d5b-434b-ade6-fef15f1710e7
author: rothja
ms.author: jroth
ms.openlocfilehash: 562b10fa67b04926e512833248c99ecb7b55a1fd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88443601"
---
# <a name="get_oledbcommand-method"></a>get_OLEDBCommand, méthode
Retourne la commande de OLE DB sous-jacente, en propageant tout d’abord les informations sur les paramètres définis sur la commande ADO à la commande OLE DB.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT get_OLEDBCommand(  
      IUnknown **ppOLEDBCommand  
);  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ppOLEDBCommand*  
 à Pointeur vers un emplacement de pointeur où le pointeur IUnknown pour la commande de OLE DB sous-jacente sera écrit.  
  
## <a name="applies-to"></a>S'applique à  
 [IADOCommandConstruction](https://msdn.microsoft.com/d8e54333-00eb-4b72-bf4a-ca92c7ca5f86)
