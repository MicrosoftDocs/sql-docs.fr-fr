---
description: Types de données dBASE
title: Types de données dBASE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- Jet-based ODBC drivers [ODBC], DBasedriver
- desktop database drivers [ODBC], DBasedriver
- DBase driver [ODBC], data types
- data types [ODBC], DBase driver
- dbase data types [ODBC]
- ODBC desktop database drivers [ODBC], DBasedriver
ms.assetid: a0e31e6b-d02b-4ee2-9b37-5baf6a11c0a6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4ef5b42157a011bf719614b7378cd73733e28afc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165426"
---
# <a name="dbase-data-types"></a>Types de données dBASE
Le tableau suivant montre comment les types de données dBASE sont mappés aux types de données ODBC SQL. Notez que tous les types de données ODBC SQL ne sont pas pris en charge.  
  
|type de données dBASE|Type de données ODBC|  
|---------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATE|SQL_DATE|  
|FLOAT [1]|SQL_DOUBLE|  
|LOGICAL|SQL_BIT|  
|CHAMPS|SQL_LONGVARCHAR|  
|NUMÉRIQUE (BCD)|SQL_DOUBLE|  
|OLEOBJECT [1]|SQL_LONGBINARY|  
  
 [1] valide uniquement pour dBASE version 5. *x*  
  
 La précision dans dBASE III autorise les nombres avec des exposants à deux chiffres au maximum et dans les nombres dBASE IV avec des exposants à trois chiffres au maximum. Étant donné que les nombres sont stockés sous forme de texte, ils sont convertis en nombres. Si le nombre à convertir ne tient pas dans un champ, des résultats inexpliqués peuvent se produire.  
  
 Tandis que dBASE permet de spécifier une précision et une échelle à l’aide d’un type de données numérique, il n’est pas pris en charge par le pilote ODBC dBASE. Le pilote ODBC dBASE retourne toujours une précision de 15 et une échelle de 0 pour un type de données numérique.  
  
 Une colonne créée avec le type de données numérique à l’aide du pilote ODBC dBASE correspond au type de données ODBC SQL_DOUBLE. Les données de cette colonne sont donc sujettes à l’arrondi. Ce comportement n’est pas le même que celui du type de données numérique dans dBASE (type N), qui correspond au code binaire décimal (BCD).  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC SQL. Toutes les conversions de l’annexe D de la *Référence du programmeur ODBC* sont prises en charge pour les types de données SQL ODBC répertoriés précédemment dans cette rubrique.  
  
 Le tableau suivant présente les limitations relatives aux types de données dBASE.  
  
|Type de données|Description|  
|---------------|-----------------|  
|CHAR|La création d’une colonne CHAR de zéro ou d’une longueur non spécifiée retourne en fait une colonne de 254 octets.|  
|Données chiffrées|Le pilote dBASE ne prend pas en charge les tables dBASE chiffrées.|  
|LOGICAL|Le pilote dBASE ne peut pas créer d’index sur une colonne logique.|  
|CHAMPS|La longueur maximale d’une colonne de type Mémo est de 65 500 octets.|  
  
 Vous trouverez plus de restrictions sur les types de données dans limitations des types de [données](../../odbc/microsoft/data-type-limitations.md).
