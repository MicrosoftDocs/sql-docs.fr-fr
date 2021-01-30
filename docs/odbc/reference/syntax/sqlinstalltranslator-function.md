---
description: SQLInstallTranslator, fonction
title: Sqlinstalltranslator, fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallTranslator
helpviewer_keywords:
- SQLInstallTranslator function [ODBC]
ms.assetid: 453b21ff-3c2b-4069-8ff7-5c727f062d89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40f4c4a5df497c64d081a8d03d1765a4fe5e060a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189657"
---
# <a name="sqlinstalltranslator-function"></a>SQLInstallTranslator, fonction
**Conformité**  
 Version introduite : ODBC 2,5, déconseillé  
  
 **Résumé**  
 Dans ODBC 3,0, **sqlinstalltranslator,** a été remplacé par [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md). Les appels à **sqlinstalltranslator,** sont mappés à **SQLInstallTranslatorEx**. Pour plus d’informations, consultez **SQLInstallTranslatorEx**.  
  
 **Sqlinstalltranslator,** retourne la valeur false si une application l’appelle dans le gestionnaire de pilotes ODBC *3. x* avec l’argument *lpszInfFile* défini sur une valeur autre que NULL. Le fichier ODBC. inf utilisé dans ODBC *2. x* n’est plus pris en charge dans ODBC *3. x*, même pour la compatibilité descendante.
