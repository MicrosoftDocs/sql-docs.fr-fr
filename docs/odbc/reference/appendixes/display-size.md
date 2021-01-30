---
description: Taille d’affichage
title: Taille d’affichage | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- display size of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- SQL data types [ODBC], column characteristics
ms.assetid: 9f7f766f-2492-463c-aab7-f2476e222042
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a39a0f986164cf6bc14e8be6e221232dd936e4bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194873"
---
# <a name="display-size"></a>Taille d’affichage
La taille d’affichage d’une colonne est le nombre maximal de caractères nécessaires pour afficher les données sous forme de caractère. Le tableau suivant définit la taille d’affichage pour chaque type de données SQL ODBC.  
  
|Identificateur de type SQL|Taille de l’écran|  
|-------------------------|------------------|  
|Tous les types de caractères [a]|Nombre de caractères définis (pour les types fixes) ou maximum (pour les types de variable) nécessaires à l’affichage des données sous forme de caractère.|  
|SQL_DECIMAL SQL_NUMERIC|Précision de la colonne plus 2 (signe, chiffres de *précision* et virgule décimale). Par exemple, la taille d’affichage d’une colonne définie en tant que valeur numérique (10, 3) est 12.|  
|SQL_BIT|1 (1 chiffre).|  
|SQL_TINYINT|4 s’il est signé (signe et 3 chiffres) ou 3 Si non signé (3 chiffres).|  
|SQL_SMALLINT|6 s’ils sont signés (signe et 5 chiffres) ou 5 s’ils ne sont pas signés (5 chiffres).|  
|SQL_INTEGER|11 si signé (signe et 10 chiffres) ou 10 si non signé (10 chiffres).|  
|SQL_BIGINT|20 (signe et 19 chiffres s’ils sont signés ou 20 chiffres s’ils ne sont pas signés).|  
|SQL_REAL|14 (signe, 7 chiffres, virgule décimale, lettre *E*, signe et 2 chiffres).|  
|SQL_FLOAT SQL_DOUBLE|24 (signe, 15 chiffres, virgule décimale, lettre *E*, signe et 3 chiffres).|  
|Tous les types binaires [a]|La longueur définie ou maximale (pour les types de variable) de la colonne Times 2. (Chaque octet binaire est représenté par un nombre hexadécimal à 2 chiffres.)|  
|SQL_TYPE_DATE|10 (une date au format *aaaa-mm-jj*).|  
|SQL_TYPE_TIME|8 (une heure au format *hh : mm : SS*)<br /><br /> - ou -<br /><br /> 9 + *s* (une heure au format *hh : mm : SS*[. fff...], où *s* est la précision en fractions de seconde).|  
|SQL_TYPE_TIMESTAMP|19 (pour un horodateur au format *aaaa-mm-jj hh : mm : SS* )<br /><br /> - ou -<br /><br /> 20 + *s* (pour un horodateur au format *aaaa-mm-jj hh : mm : SS*[. fff...], où *s* est la précision en fractions de seconde).|  
|Tous les types de données Interval|Consultez [longueur des types de données d’intervalle](../../../odbc/reference/appendixes/interval-data-type-length.md).|  
|SQL_GUID|36 (nombre de caractères au format *aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee*|  
  
 [a] si le pilote ne peut pas déterminer la longueur de la colonne ou du paramètre des types de variable, il retourne SQL_NO_TOTAL.
