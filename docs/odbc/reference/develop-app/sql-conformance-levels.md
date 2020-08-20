---
description: Niveaux de conformité SQL
title: Niveaux de conformité SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- conformance levels [ODBC], SQL
- SQL conformance levels [ODBC]
- data sources [ODBC], conformance levels
- ODBC drivers [ODBC], conformance levels
ms.assetid: 3529df2c-a09b-4c16-9c60-eae7a06d903a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 525cda9094213e6decf30643c23a7db623511e14
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88499802"
---
# <a name="sql-conformance-levels"></a>Niveaux de conformité SQL
Le niveau de grammaire SQL-92 pris en charge par un pilote est indiqué par la valeur retournée par un appel à **SQLGetInfo** avec le type d’informations SQL_SQL_CONFORMANCE. Cela indique si le pilote est conforme aux niveaux d’entrée, de transition FIPS, intermédiaire ou complète définis dans SQL-92.  
  
 Tous les pilotes ODBC doivent prendre en charge la grammaire SQL minimale décrite dans [grammaire minimale SQL](../../../odbc/reference/appendixes/sql-minimum-grammar.md) dans annexe C : grammaire SQL. Cette grammaire est un sous-ensemble du niveau d’entrée de SQL-92. Les pilotes peuvent prendre en charge des SQL supplémentaires et être conformes à l’entrée SQL-92, au niveau intermédiaire ou complet, ou au niveau de transition FIPS 127-2. Les pilotes qui se conforment à un niveau donné de SQL-92 ou FIPS 127-2 peuvent prendre en charge des fonctionnalités supplémentaires dans n’importe quel niveau supérieur, mais ne sont pas entièrement conformes à ce niveau. Pour déterminer si une fonctionnalité est prise en charge, une application doit appeler **SQLGetInfo** avec le type d’informations approprié. Le niveau de conformité d’une fonctionnalité SQL est décrit dans le type d’informations correspondant. (Consultez la description de la fonction [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) .)
