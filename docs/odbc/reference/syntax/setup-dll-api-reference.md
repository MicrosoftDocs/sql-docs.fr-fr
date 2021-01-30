---
description: Informations de référence sur l’API de la DLL de configuration
title: Informations de référence sur l’API DLL d’installation | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC drivers [ODBC], driver setup DLL
- driver setup DLL [ODBC]
ms.assetid: f9d03f17-1c0d-4e7c-9c04-8c316e07ef25
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d80d6315227013d2de149d6847eb0ed5b8fa223a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174609"
---
# <a name="setup-dll-api-reference"></a>Informations de référence sur l’API de la DLL de configuration
Cette section décrit la syntaxe de l’API DLL d’installation du pilote, qui se compose de deux fonctions (**ConfigDriver** et **ConfigDSN**). **ConfigDriver** et **ConfigDSN** peuvent être dans la dll du pilote ou dans une DLL d’installation distincte.  
  
 En outre, cette section décrit la syntaxe de l’API DLL d’installation du traducteur, qui se compose d’une fonction unique (**ConfigTranslator**). **ConfigTranslator** peut être dans la dll du traducteur ou dans une DLL d’installation distincte.  
  
 Chaque fonction est étiquetée avec la version ODBC dans laquelle elle a été introduite.  
  
 Cette section contient les rubriques suivantes :  
  
-   [ConfigDriver, fonction](../../../odbc/reference/syntax/configdriver-function.md)  
  
-   [ConfigDSN, fonction](../../../odbc/reference/syntax/configdsn-function.md)  
  
-   [ConfigTranslator, fonction](../../../odbc/reference/syntax/configtranslator-function.md)
