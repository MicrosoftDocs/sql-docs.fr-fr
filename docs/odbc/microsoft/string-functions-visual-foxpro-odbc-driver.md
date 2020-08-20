---
description: Fonctions de chaîne (pilote ODBC Visual FoxPro)
title: Fonctions de chaîne (pilote ODBC Visual FoxPro) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC string functions [ODBC]
- string functions [ODBC]
- Visual FoxPro ODBC driver [ODBC], string functions
- FoxPro ODBC driver [ODBC], string functions
ms.assetid: 1974fd26-ef0d-45d5-860b-298917c8e9c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4ab2ecefb604183b0387a561f72494028c62d15f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88471541"
---
# <a name="string-functions-visual-foxpro-odbc-driver"></a>Fonctions de chaîne (pilote ODBC Visual FoxPro)
Le tableau suivant répertorie les fonctions de manipulation de chaînes ODBC prises en charge par le pilote ODBC Visual FoxPro. Lorsque la grammaire Visual FoxPro pour la même fonction diffère de la syntaxe ODBC, l’équivalent Visual FoxPro est listé.  
  
|Grammaire ODBC|Grammaire de Visual FoxPro|  
|------------------|---------------------------|  
|ASCII *(string_exp)*|ASC *(string_exp)*|  
|CHAR *(code)*|CHR *(string_exp)*|  
|CONCAt *(string_exp1, string_exp2)*|*string_exp1 + string_exp2*|  
|DIFFÉRENCE *(string_exp1, string_exp2)*||  
|INSERTION *(string_exp1, début, longueur, string_exp2)*|Éléments *(string_exp1, début, longueur, string_exp2)*|  
|LCASE *(string_exp)*|INFÉRIEUR *(string_exp)*|  
|GAUCHE *(string_exp, nombre)*||  
|LONGUEUR *(string_exp)*|LEN *(string_exp)*|  
|LTRIM *(string_exp)*||  
|REPEAT *(string_exp, Count)*|Replicate *(string_exp, Count)*|  
|Replace *(string_exp1, string_exp2, string_exp3)*|STRTRAN *(string_exp1, string_exp2, string_exp3)*|  
|RIGHT *(string_exp, Count)*||  
|RTRIM *(string_exp)*||  
|SOUNDEX *(string_exp)*||  
|ESPACE *(nombre)*||  
|Substring *(string_exp, début, longueur)*|SUBSTR *(string_exp, début, longueur)*|  
|UCASE *(string_exp)*|UPPER *(string_exp)*|
