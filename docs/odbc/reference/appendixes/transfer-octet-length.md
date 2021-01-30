---
description: Longueur en octets du transfert
title: Longueur de l’octet de transfert | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- data types [ODBC], transfer octet length
ms.assetid: 9fdc9762-e203-4cff-9212-54f450bf18d9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b910f544be952a18ae2961e9939960820e3805bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202485"
---
# <a name="transfer-octet-length"></a>Longueur en octets du transfert
La longueur d’octet de transfert d’une colonne est le nombre maximal d’octets renvoyés à l’application lorsque les données sont transférées à son type de données C par défaut. Pour les données de type caractère, la longueur de l’octet de transfert n’inclut pas d’espace pour le caractère de fin null. La longueur d’octet de transfert d’une colonne peut être différente du nombre d’octets requis pour stocker les données sur la source de données.  
  
 La longueur d’octet de transfert définie pour chaque type de données SQL ODBC est indiquée dans le tableau suivant.  
  
|Identificateur de type SQL|Longueur|  
|-------------------------|------------|  
|Tous les types de caractères [a]|La longueur définie ou maximale (pour le type de variable) de la colonne en octets. Il s’agit de la même valeur que le champ de descripteur SQL_DESC_OCTET_LENGTH.|  
|SQL_DECIMAL<br />SQL_NUMERIC|Nombre d’octets requis pour contenir la représentation sous forme de caractère de ces données si le jeu de caractères est ANSI, et deux fois ce nombre si le jeu de caractères est UNICODE. Il s’agit du nombre maximal de chiffres plus deux, car les données sont retournées sous la forme d’une chaîne de caractères et les caractères sont nécessaires pour les chiffres, un signe et une virgule décimale. Par exemple, la longueur de transfert d’une colonne définie en tant que valeur numérique (10, 3) est égale à 12.|  
|SQL_TINYINT|1|  
|SQL_SMALLINT|2|  
|SQL_INTEGER|4|  
|SQL_BIGINT| 8 |  
|SQL_REAL|4|  
|SQL_FLOAT|8|  
|SQL_DOUBLE|8|  
|SQL_BIT|1|  
|Tous les types binaires [a]|Nombre d’octets requis pour contenir le nombre de caractères définis (pour les types fixes) ou maximal (pour les types de variable).|  
|SQL_TYPE_DATE<br />SQL_TYPE_TIME|6 (taille du SQL_DATE_STRUCT ou de la structure SQL_TIME_STRUCT).|  
|SQL_TYPE_TIMESTAMP|16 (la taille de la structure SQL_TIMESTAMP_STRUCT).|  
|Tous les types de données Interval|34 (taille de la structure de l’intervalle).|  
|SQL_GUID|16 (la taille de la structure GUID).|  
| &nbsp; | &nbsp; |

 [a] si le pilote ne peut pas déterminer la longueur de la colonne ou du paramètre pour les types de variable, il retourne SQL_NO_TOTAL.
