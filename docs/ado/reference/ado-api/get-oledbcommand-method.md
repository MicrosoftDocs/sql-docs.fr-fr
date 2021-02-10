---
description: get_OLEDBCommand, méthode
title: Méthode get_OLEDBCommand | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- get_OLEDBCommand method [ADO]
ms.assetid: 23d551f5-3d5b-434b-ade6-fef15f1710e7
author: rothja
ms.author: jroth
ms.openlocfilehash: f287d19a4a34e9c71f06aab63910465ade3982d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024193"
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
 [IADOCommandConstruction](/previous-versions/windows/desktop/aa965677(v=vs.85))