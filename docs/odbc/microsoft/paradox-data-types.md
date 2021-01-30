---
description: Types de données Paradox
title: Types de données Paradox | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Paradox driver
- desktop database drivers [ODBC], Paradox driver
- Paradox data types [ODBC]
- Jet-based ODBC drivers [ODBC], Paradox driver
- data types [ODBC], Paradox driver
- Paradox driver [ODBC], data types
ms.assetid: 0c9e5d21-9321-49f8-a055-69459e1c9c85
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ee49ff2df24a57daa481ea650f57c09a7712400e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203507"
---
# <a name="paradox-data-types"></a>Types de données Paradox
Le pilote ODBC Paradox mappe les types de données Paradox aux types de données ODBC SQL. Le tableau suivant répertorie tous les types de données Paradox et indique les types de données SQL ODBC auxquels ils sont mappés.  
  
|Type de données Paradox|Type de données ODBC|  
|-----------------------|--------------------|  
|COMPORTANT|SQL_VARCHAR|  
|AUTOINCREMENT [1]|SQL_INTEGER|  
|BCD [1]|SQL_DOUBLE|  
|OCTETS [1]|SQL_BINARY|  
|DATE|SQL_DATE|  
|IMAGE [2]|SQL_LONGVARBINARY|  
|LOGIQUE [1]|SQL_BIT|  
|LONG [1]|SQL_INTEGER|  
|MÉMO [2]|SQL_LONGVARCHAR|  
|MONEY [1]|SQL_DOUBLE|  
|NUMBER|SQL_DOUBLE|  
|SHORT|SQL_SMALLINT|  
|HEURE [1]|SQL_TIMESTAMP|  
|HORODATEUR [1]|SQL_TIMESTAMP|  
  
 [1] valide uniquement pour les versions Paradox 5. *x*.  
  
 [2] valide uniquement pour Paradox versions 4. *x* et 5. *x*.  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC SQL. Toutes les conversions de l’annexe D de la *Référence du programmeur ODBC* sont prises en charge pour les types de données SQL ODBC répertoriés précédemment dans cette rubrique.  
  
 Le tableau suivant présente les limitations relatives aux types de données Paradox.  
  
|Type de données|Description|  
|---------------|-----------------|  
|COMPORTANT|La création d’une colonne alphanumérique de zéro ou d’une longueur non spécifiée renvoie en fait une colonne de 255 octets.|  
|BYTES|Si vous insérez la valeur NULL dans une colonne binaire avec le pilote Paradox5, elle est remplacée par la valeur 0.|  
|LONG|Valeur négative maximale prise en charge par le pilote Paradox pour le type de données long dans Paradox 5. *x* n’est pas-2 ^ 31 (-2147483648), car long est mappé au type de données ODBC SQL_INTEGER. La valeur négative maximale prise en charge pour long est en fait-2 ^ 31 + 1 (-2147483647).|  
|timestamp|Lorsqu’une valeur est insérée dans une colonne TIMESTAMP par le pilote Paradox, puis Récupérée à partir de la colonne, la valeur récupérée peut différer de la valeur insérée de 1 seconde en raison de l’arrondi.|  
  
 Vous trouverez plus de restrictions sur les types de données dans limitations des types de [données](../../odbc/microsoft/data-type-limitations.md).
