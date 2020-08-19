---
description: SQLInstallTranslator, mappage
title: Mappage Sqlinstalltranslator, | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- SQLInstallTranslator function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLInstallTranslator
ms.assetid: bcd9ba4f-7834-4bc4-876e-c7478998e524
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fc571e305070e4afba6dc7c647d18a6790c011f0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88448968"
---
# <a name="sqlinstalltranslator-mapping"></a>SQLInstallTranslator, mappage
Quand une application ODBC *2. x* appelle **sqlinstalltranslator,** via un pilote ODBC *3. x* , le gestionnaire de pilotes mappe l’appel à **SQLInstallTranslatorEx**. Une application ne doit pas appeler **sqlinstalltranslator,** dans le gestionnaire de pilotes ODBC *3. x* avec l’argument *lpszInfFile* défini sur une valeur autre que NULL. ODBC. Le fichier INF utilisé dans ODBC *2. x* n’est plus pris en charge dans ODBC *3. x*, même pour la compatibilité descendante.
