---
description: ObjectProxy (ADO - syntaxe WFC)
title: ObjectProxy (syntaxe ADO-WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- ObjectProxy collection [ADO]
ms.assetid: f68f58bc-ad28-46cc-9fb3-099e1a678397
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c52f9253ced985ed6a53af87c95ff7fe2eee3fa
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88990400"
---
# <a name="objectproxy-ado---wfc-syntax"></a>ObjectProxy (ADO - syntaxe WFC)
Un objet **ObjectProxy** représente un serveur et est retourné par la méthode **CreateObject** de l’objet [DataSpace](../rds-api/dataspace-object-rds.md) . La classe ObjectProxy a une méthode, **Call**, qui peut appeler une méthode sur le serveur et retourner un objet résultant de cet appel.  
  
 **package com. ms. wfc. Data**  
  
## <a name="methods"></a>Méthodes  
  
### <a name="call-method-adowfc-syntax"></a>Call, méthode (syntaxe ADO/WFC)  
 Appelle une méthode sur le serveur représenté par l’objet ObjectProxy. Éventuellement, les arguments de méthode peuvent être passés en tant que tableau d’objets.  
  
#### <a name="syntax"></a>Syntaxe  
  
```  
public Object ObjectProxy.( String method )  
public Object ObjectProxy.( String method, Object[] args)  
```  
  
#### <a name="returns"></a>Retours  
 Object  
 Objet qui résulte de l’appel de la méthode.  
  
#### <a name="parameters"></a>Paramètres  
 *ObjectProxy*  
 Objet **ObjectProxy** qui représente le serveur.  
  
 *method*  
 Chaîne contenant le nom de la méthode à appeler sur le serveur.  
  
 *args*  
 facultatif. Tableau d’objets qui sont des arguments de la méthode sur le serveur. Les types de données Java sont automatiquement convertis en types de données pouvant être utilisés sur le serveur.