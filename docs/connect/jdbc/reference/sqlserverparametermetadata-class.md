---
description: Classe SQLServerParameterMetaData
title: Classe SQLServerParameterMetaData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 546290e0-9411-4a2b-aa36-61251e70e9cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9a8bbbc8ad213d9cbfbf7218558223a6ad37b803
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88354355"
---
# <a name="sqlserverparametermetadata-class"></a>Classe SQLServerParameterMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Représente les métadonnées pour les paramètres d'instructions préparées.  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Étend :** java.lang.Object  
  
 **Implémente :** java.sql.ParameterMetaData  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public class SQLServerParameterMetaData  
```  
  
## <a name="remarks"></a>Notes  
 Pour récupérer des métadonnées de paramètre, des instructions préparées sont exécutées avec SET FMT ONLY. Les instructions pouvant être appelées appellent sp_sproc_columns pour récupérer les noms et les métadonnées des paramètres de procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerParameterMetaData, membres](../../../connect/jdbc/reference/sqlserverparametermetadata-members.md)   
 [Informations de référence sur l'API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
