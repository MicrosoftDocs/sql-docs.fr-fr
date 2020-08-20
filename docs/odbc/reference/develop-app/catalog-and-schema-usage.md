---
description: Utilisation des catalogues et des schémas
title: Utilisation du catalogue et du schéma | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- SQL statements [ODBC], interoperability
- interoperability of SQL statements [ODBC], catalog names
- catalog names [ODBC]
- interoperability of SQL statements [ODBC], schema names
- schema names in SQL statements [ODBC]
ms.assetid: 84f7ef61-1ef1-46f3-9678-b087aa8e8e34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2506810c477e9d75e1d3b38f38185d22edf9d010
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88494689"
---
# <a name="catalog-and-schema-usage"></a>Utilisation des catalogues et des schémas
Les sources de données ne prennent pas nécessairement en charge les noms de catalogues et de schémas en tant qu’identificateurs de noms d’objets dans toutes les instructions SQL. Les sources de données peuvent prendre en charge les noms de catalogue et de schéma dans une ou plusieurs des classes d’instructions SQL suivantes : instructions DML (Data Manipulation Language), appels de procédure, instructions de définition de table, instructions de définition d’index et instructions de définition de privilège. Pour déterminer les classes d’instructions SQL dans lesquelles les noms de catalogue et de schéma peuvent être utilisés, une application appelle **SQLGetInfo** avec les options SQL_CATALOG_USAGE et SQL_SCHEMA_USAGE.
