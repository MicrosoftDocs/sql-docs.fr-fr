---
title: Exécution de procédures stockées | Microsoft Docs
description: Une procédure stockée est un objet exécutable stocké dans une base de données. SQL Server prend en charge les procédures stockées et les procédures stockées étendues.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC, stored procedures
- stored procedures [ODBC], running
- SQL Server Native Client ODBC driver, stored procedures
- stored procedures [ODBC], executing
ms.assetid: 866b6dd3-2acd-4dfb-aeca-a0352b2d4c6a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 24f95a05032693773f5a9a21200ece29a96c7a73
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91867027"
---
# <a name="running-stored-procedures"></a>Exécution des procédures stockées
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Une procédure stockée est un objet exécutable stocké dans une base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge :  
  
-   Procédures stockées :  
  
     une ou plusieurs instructions SQL précompilées en une seule procédure exécutable.  
  
-   Les procédures stockées étendues :  
  
     Les DLL C ou C++ écrites dans l'API SQL Server Open Data Services pour les procédures stockées étendues. L'API Open Data Services étend les fonctions des procédures stockées pour inclure un code C ou C++.  
  
 Lorsque vous exécutez des instructions, l'appel d'une procédure stockée sur la source de données (au lieu d'exécuter ou de préparer directement une instruction dans l'application cliente) peut fournir les éléments suivants :  
  
-   Performances accrues  
  
     Les instructions SQL sont analysées et compilées lorsque les procédures sont créées. Cette charge est ensuite enregistrée lorsque les procédures sont exécutées.  
  
-   Charge réseau réduite  
  
     L'exécution d'une procédure au lieu d'envoyer des requêtes complexes à travers le réseau peut réduire le trafic réseau. Si une application ODBC utilise la syntaxe ODBC {CALL} pour exécuter une procédure stockée, le pilote ODBC procède à des optimisations supplémentaires qui éliminent le besoin de convertir les données des paramètres.  
  
-   Cohérence supérieure  
  
     Si les règles d'une organisation sont implémentées dans une ressource centrale, telle qu'une procédure stockée, elles peuvent être codées, testées et déboguées une fois. Les programmeurs individuels peuvent utiliser ensuite les procédures stockées testées au lieu de développer leurs propres implémentations.  
  
-   Exactitude supérieure  
  
     Comme les procédures stockées sont développées habituellement par des programmeurs expérimentés, elles tendent à être plus efficaces et à comporter moins d'erreurs qu'un code développé plusieurs fois par des programmeurs aux niveaux de compétence variables.  
  
-   Fonctionnalités supplémentaires  
  
     Les procédures stockées étendues peuvent utiliser les fonctionnalités C et C++ non disponibles dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
     Pour obtenir un exemple d’appel d’une procédure stockée, consultez [traiter les codes de retour et les paramètres de sortie &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Appel d’une procédure stockée](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)  
  
-   [Traitement par lot des appels de procédures stockées](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)  
  
-   [Traitement des résultats des procédures stockées](../../relational-databases/native-client-odbc-stored-procedures/processing-stored-procedure-results.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Rubriques de procédures relatives à l’exécution de procédures stockées &#40;ODBC&#41;](../native-client-odbc-how-to/running-stored-procedures-call-stored-procedures.md)  
  
