---
description: SQLCleanupConnectionPoolID, fonction
title: SQLCleanupConnectionPoolID fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- SQLCleanupConnectionPoolID function [ODBC]
ms.assetid: 1fc61908-e003-4587-b91a-32f40569fb99
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12046405d10c41796b8ad989f746aaac242f430d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88448839"
---
# <a name="sqlcleanupconnectionpoolid-function"></a>SQLCleanupConnectionPoolID, fonction
**Conformité**  
 Version introduite : ODBC 3,81 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLCleanupConnectionPoolID** informe un pilote qu’un ID de pool a expiré. Un ID de pool peut expirer chaque fois que toutes les connexions d’un pool associé à cet ID de pool ont expiré. Pour plus d’informations sur le délai d’expiration [de la connexion, consultez regroupement dans les composants d’accès aux données Microsoft](https://msdn.microsoft.com/library/ms810829.aspx) .  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
  
SQLRETURN  SQLCleanupConnectionPoolID (  
                SQLHENV    EnvironmentHandle  
                SQLPOOLID  PoolID );  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 Entrée Handle d’environnement du pool.  
  
 *PoolID*  
 Entrée Pool associé à l’ID de pool qui a expiré.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Le gestionnaire de pilotes ne traitera pas les informations de diagnostic retournées par **SQLCleanupConnectionPoolID**.  
  
 Une application ne peut pas recevoir le message d’erreur retourné par le pilote.  
  
## <a name="remarks"></a>Notes  
 **SQLCleanupConnectionPoolID** peut être appelé à tout moment, mais le gestionnaire de pilotes garantit qu’aucun autre thread n’appelle simultanément **SQLGetPoolID** et qu’aucun autre thread n’appelle simultanément **SQLRateConnection** et **SQLPoolConnect** avec un jeton d’informations de connexion affecté avec cet ID de pool. Par conséquent, le pilote doit s’assurer que cette fonction est thread-safe.  
  
 Un pilote peut nettoyer les ressources associées à l’ID du pool.  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Incluez sqlspi. h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
