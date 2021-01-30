---
description: SQLInstallTranslator, mappage
title: Mappage Sqlinstalltranslator, | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLInstallTranslator function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLInstallTranslator
ms.assetid: bcd9ba4f-7834-4bc4-876e-c7478998e524
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b063768fed57adffd4a6e5051e5383a2ad32a943
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202710"
---
# <a name="sqlinstalltranslator-mapping"></a>SQLInstallTranslator, mappage
Quand une application ODBC *2. x* appelle **sqlinstalltranslator,** via un pilote ODBC *3. x* , le gestionnaire de pilotes mappe l’appel à **SQLInstallTranslatorEx**. Une application ne doit pas appeler **sqlinstalltranslator,** dans le gestionnaire de pilotes ODBC *3. x* avec l’argument *lpszInfFile* défini sur une valeur autre que NULL. ODBC. Le fichier INF utilisé dans ODBC *2. x* n’est plus pris en charge dans ODBC *3. x*, même pour la compatibilité descendante.
