---
description: Méthode getPortNumber (SQLServerDataSource)
title: Méthode getPortNumber (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDataSource.getPortNumber
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e5dc38d0-4340-4ad7-a56e-1d2a0f0fd846
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ded903bb6696c3dfefd51979d37605af899b6d6d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88435011"
---
# <a name="getportnumber-method-sqlserverdatasource"></a>Méthode getPortNumber (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne le numéro de port actuel utilisé pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int getPortNumber()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur **int** qui contient le numéro de port actuel.  
  
## <a name="remarks"></a>Notes  
 Le numéro de port est le numéro de port TCP/IP utilisé lors de l’ouverture d’une connexion de socket à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si la propriété portNumber n'est pas définie, la méthode getPortNumber retourne la valeur par défaut de 1433.  
  
> [!NOTE]  
>  La méthode [setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md) n'effectue aucune vérification de plage sur la valeur de port transmise. Vous pouvez transmettre des numéros de port qui ne sont pas valides, tels que 99999, sans déclencher d'erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
