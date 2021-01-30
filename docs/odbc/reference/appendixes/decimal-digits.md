---
description: Nombres décimaux
title: Chiffres décimaux | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- size of data types [ODBC]
- decimal digits of data types [ODBC]
- data types [ODBC], decimal digits
- SQL data types [ODBC], column characteristics
ms.assetid: 07f3d1fc-b4ee-4693-b342-330b2231b6d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f7a4d37601891c26ce4d59a9bfd788ce1c702ce5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194904"
---
# <a name="decimal-digits"></a>Nombres décimaux
Les *chiffres décimaux* des types de données decimal et numeric sont définis comme étant le nombre maximal de chiffres à droite de la virgule décimale, ou l’échelle des données. Pour les colonnes ou les paramètres de nombre à virgule flottante approximatifs, l’échelle n’est pas définie, car le nombre de chiffres à droite de la virgule décimale n’est pas fixe. Pour les données DateTime ou Interval qui contiennent un composant seconds, les chiffres décimaux sont définis en tant que nombre de chiffres à droite de la virgule décimale dans le composant seconds des données.  
  
 Pour les types de données SQL_DECIMAL et SQL_NUMERIC, l’échelle maximale est généralement identique à la précision maximale. Toutefois, certaines sources de données imposent une limite distincte à l’échelle maximale. Pour déterminer les échelles minimale et maximale autorisées pour un type de données, une application appelle **SQLGetTypeInfo**.  
  
 Les chiffres décimaux définis pour chaque type de données SQL concis sont présentés dans le tableau suivant.  
  
|Type SQL|des chiffres décimaux|  
|--------------|--------------------|  
|Tous les types caractère et binaire [a]|n/a|  
|SQL_DECIMAL<br />SQL_NUMERIC|Nombre défini de chiffres à droite de la virgule décimale. Par exemple, l’échelle d’une colonne définie en tant que valeur numérique (10, 3) est 3. Il peut s’agir d’un nombre négatif pour prendre en charge le stockage de très grands nombres sans utiliser la notation exponentielle. par exemple, « 12000 » peut être stocké sous la forme « 12 » avec une échelle de-3.|  
|Tous les types numériques exacts autres que SQL_DECIMAL et SQL_NUMERIC [a]|0|  
|Tous les types de données approximatifs [a]|n/a|  
|SQL_TYPE_DATE et tous les types d’intervalle sans composant de secondes [a]|n/a|  
|Tous les types DateTime sauf SQL_TYPE_DATE et tous les types d’intervalle avec un composant seconds|Nombre de chiffres à droite de la virgule décimale dans la partie des secondes de la valeur (fractions de seconde). Ce nombre ne peut pas être négatif.|  
|SQL_GUID|n/a|  
  
 [a] l’argument *DecimalDigits* de **SQLBindParameter** est ignoré pour ce type de données.  
  
 Les valeurs retournées pour les chiffres décimaux ne correspondent pas aux valeurs d’un champ de descripteur. Les valeurs peuvent provenir du champ SQL_DESC_SCALE ou SQL_DESC_PRECISION, selon le type de données, comme indiqué dans le tableau suivant.  
  
|Type SQL|Champ de descripteur correspondant à<br /><br /> chiffres décimaux|  
|--------------|----------------------------------------------------------|  
|Tous les types caractère et binaire|n/a|  
|Tous les types numériques exacts|SCALE|  
|SQL_BIT|n/a|  
|Tous les types numériques approximatifs|n/a|  
|Tous les types DateTime|PRECISION|  
|Tous les types d’intervalles avec un composant « secondes »|PRECISION|  
|Tous les types d’intervalle sans composant de secondes|n/a|
