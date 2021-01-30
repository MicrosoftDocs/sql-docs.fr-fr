---
description: Informations de référence sur l’interface SPI ODBC
title: Informations de référence sur l’interface SPI (Service Provider Interface) ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: cdeffb4a-f344-4abe-97f3-be2ede1c8e59
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e4cbf20ed3d158fca4e7ae58854bf53778dc9db6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174631"
---
# <a name="odbc-service-provider-interface-spi-reference"></a>Informations de référence sur l’interface SPI ODBC
Traditionnellement, ODBC définissait une interface de programmation d’applications (API). Les fonctions de l’API peuvent être appelées par des applications et doivent être implémentées à la fois dans le gestionnaire de pilotes et dans le pilote.  
  
 Avec l’ajout de la fonctionnalité de regroupement de connexions prenant en charge les pilotes, ODBC introduit l’interface SPI (Service Provider Interface). Les fonctions du SPI sont utilisées pour la communication entre le gestionnaire de pilotes et le pilote. Les fonctions SPI sont implémentées par le pilote ; le gestionnaire de pilotes n’expose pas les fonctions SPI aux applications. Les applications ne doivent pas appeler ces fonctions directement.  
  
 Incluez sqlspi. h pour le développement du pilote ODBC.  
  
 Cette section contient les rubriques suivantes :  
  
-   [SQLCleanupConnectionPoolID](../../../odbc/reference/syntax/sqlcleanupconnectionpoolid-function.md)  
  
-   [SQLGetPoolID](../../../odbc/reference/syntax/sqlgetpoolid-function.md)  
  
-   [SQLPoolConnect](../../../odbc/reference/syntax/sqlpoolconnect-function.md)  
  
-   [SQLRateConnection](../../../odbc/reference/syntax/sqlrateconnection-function.md)  
  
-   [SQLSetConnectAttrForDbcInfo](../../../odbc/reference/syntax/sqlsetconnectattrfordbcinfo-function.md)  
  
-   [SQLSetConnectInfo](../../../odbc/reference/syntax/sqlsetconnectinfo-function.md)  
  
-   [SQLSetDriverConnectInfo](../../../odbc/reference/syntax/installation-and-configuration-wwi-oltp.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Développement Connection-Pool la sensibilisation à un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)   
 [Regroupement de connexions du gestionnaire de pilotes](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md)
