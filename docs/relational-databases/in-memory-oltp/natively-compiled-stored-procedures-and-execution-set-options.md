---
title: Procédures stockées compilées nativement et options Set
description: Les options SET d’une session n’affectent pas l’exécution d’une procédure stockée. Certaines options SET empêchent toutefois l’exécution des procédures stockées.
ms.custom: seo-dt-2019
ms.date: 10/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: c1869cf7-9030-4d18-85d6-0e419a4e9af7
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9bde2c1d3c91606df034d636ca2273c8d96978b8
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89545168"
---
# <a name="natively-compiled-stored-procedures-and-execution-set-options"></a>Procédures stockées compilées en mode natif et options SET d'exécution
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Les options de session sont fixes dans les blocs atomiques, comme décrit dans [Blocs atomiques](atomic-blocks-in-native-procedures.md). L’exécution d’une procédure stockée n’est pas affectée par les options SET d’une session, dans la mesure où les blocs atomiques sont requis. Toutefois, certaines options SET, telles que SET NOEXEC et SET SHOWPLAN_XML, empêchent l'exécution des procédures stockées (y compris des procédures stockées compilées en mode natif).   
  
 Lorsqu'une procédure stockée compilée en mode natif est exécutée avec une option STATISTICS activée, les statistiques sont collectées pour la procédure dans son ensemble, et non par instruction. Pour plus d’informations, consultez [SET STATISTICS IO &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-io-transact-sql.md), [SET STATISTICS PROFILE &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-profile-transact-sql.md), [SET STATISTICS TIME &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-time-transact-sql.md) et [SET STATISTICS XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-xml-transact-sql.md). Pour obtenir les statistiques d'exécution au niveau d'une instruction dans des procédures stockées compilées en mode natif, utilisez une session d'événements étendus dans l'événement sp_statement_completed, qui commence à l'issue de chaque requête individuelle exécutée dans les procédures stockées. Pour plus d’informations sur la création de sessions d’événements étendus, consultez [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md).  
  
 **SHOWPLAN_XML** est pris en charge pour les procédures stockées compilées en mode natif. **SHOWPLAN_ALL** et **SHOWPLAN_TEXT** ne sont pas pris en charge avec les procédures stockées compilées en mode natif.  
  
 **SET FMTONLY** n'est pas prise en charge dans les procédures stockées compilées en mode natif. Utilisez plutôt [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)  
  
  
