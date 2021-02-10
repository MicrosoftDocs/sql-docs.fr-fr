---
description: Erreurs (ADO)
title: Erreurs (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 8ae6611b-3069-4155-b014-c0c9da37be39
author: rothja
ms.author: jroth
ms.openlocfilehash: b26899566d428a1b04c918c2552972448d996597
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037509"
---
# <a name="errors-ado"></a>Erreurs (ADO)
Toute opération impliquant des objets ADO peut générer une ou plusieurs erreurs de fournisseur. À mesure que chaque erreur se produit, un ou plusieurs objets **Error** sont placés dans la collection **Errors** de l’objet **Connection** . Pour plus d’informations sur la gestion des avertissements et des erreurs dans votre application ADO, consultez [gestion des erreurs](./error-handling.md).  
  
 Les erreurs d’application peuvent être déclenchées par un mécanisme distinct. Par exemple, dans Visual Basic, l’objet **Err** contient des erreurs au niveau de l’application.