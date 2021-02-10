---
description: DataSpace (ADO - syntaxe WFC)
title: DataSpace (syntaxe ADO-WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- DataSpace collection [ADO], ADO/WFC syntax
ms.assetid: 950d45d8-07de-467b-b255-f9a7b997204c
author: rothja
ms.author: jroth
ms.openlocfilehash: 40207634f4111852bc280823826c108c49fde070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034439"
---
# <a name="dataspace-ado---wfc-syntax"></a>DataSpace (ADO - syntaxe WFC)
La méthode **CreateObject** de la classe **DataSpace** spécifie à la fois un objet métier pour traiter les requêtes d’application cliente (*ProgID*) et le protocole de communication et le serveur (*connexion*). **CreateObject** retourne un objet [ObjectProxy](../../../ado/reference/ado-api/objectproxy-ado-wfc-syntax.md) qui représente le serveur.  
  
## <a name="package-commswfcdata"></a>package com. ms. wfc. Data  
  
### <a name="constructor"></a>Constructeur  
  
```  
public DataSpace()  
```  
  
### <a name="methods"></a>Méthodes  
  
```  
public static ObjectProxy DataSpace.createObject(String  
    progid, String connection)  
```  
  
### <a name="properties"></a>Propriétés  
  
```  
public static int getInternetTimeout()  
public static void setInternetTimeout(int plInetTimeout)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DataSpace, objet (RDS)](../../../ado/reference/rds-api/dataspace-object-rds.md)
