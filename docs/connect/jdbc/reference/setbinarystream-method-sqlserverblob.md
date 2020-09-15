---
description: Méthode setBinaryStream (SQLServerBlob)
title: Méthode setBinaryStream (SQLServerBlob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerBlob.setBinaryStream
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: abcec31f-1a60-4765-9725-8cf7e9f1f8ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dc75a5c48e9e1ce4efeefe646bceeb0d601b0f3d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88432431"
---
# <a name="setbinarystream-method-sqlserverblob"></a>Méthode setBinaryStream (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère un flux qui permet d'écrire sur la valeur du BLOB.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.io.OutputStream setBinaryStream(long pos)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Pos*  
  
 Position dans la valeur BLOB à laquelle démarrer l'écriture.  
  
## <a name="return-value"></a>Valeur de retour  
 Flux de sortie.  
  
## <a name="exceptions"></a>Exceptions  
 java.sql.SQLException  
  
## <a name="remarks"></a>Notes  
 Cette méthode setBinaryStream est spécifiée par la méthode setBinaryStream de l’interface java.sql.Blob.  
  
 Les données dans le BLOB sont remplacées par le flux de sortie à partir de la position spécifiée et peuvent dépasser la longueur initiale du BLOB. Si vous spécifiez une valeur position+1, des octets sont ajoutés. Le passage d'une valeur position+2 ou supérieure (ou inférieure ou égale à zéro) génère une erreur de position.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerBlob, méthodes](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob, membres](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob, classe](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
