---
description: Types de données des fichiers texte
title: Types de données de fichier texte | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- text file driver [ODBC], data types
- ODBC desktop database drivers [ODBC], text file driver
- desktop database drivers [ODBC], text file driver
- text file data types [ODBC]
- Jet-based ODBC drivers [ODBC], text file driver
ms.assetid: e113112e-ae42-469e-8e4b-a365a10d9071
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b1e447d2f20c8efb5e0c0cddc793baf73384e556
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182542"
---
# <a name="text-file-data-types"></a>Types de données des fichiers texte
Le tableau suivant montre comment les types de données texte sont mappés aux types de données SQL ODBC. Notez que tous les types de données ODBC SQL ne sont pas pris en charge par le pilote texte ODBC.  
  
|Type de données text|Type de données ODBC|  
|--------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATETIME|SQL_TIMESTAMP|  
|FLOAT|SQL_DOUBLE|  
|INTEGER|SQL_INTEGER|  
|LONGCHAR|SQL_LONGVARCHAR|  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC. Toutes les conversions de l’annexe D de la *Référence du programmeur ODBC* sont prises en charge pour les types de données SQL répertoriés dans le tableau précédent.  
  
 Le tableau suivant présente les limitations relatives aux types de données texte.  
  
|Type de données|Description|  
|---------------|-----------------|  
|CHAR|La création d’une colonne CHAR de zéro ou d’une longueur non spécifiée retourne en fait une colonne 255 bits.<br /><br /> Dans les fichiers délimités, une colonne de type CHAR peut avoir ou non des délimiteurs de guillemet double au début et à la fin ; dans les fichiers de longueur fixe, les guillemets doubles ne sont pas utilisés comme délimiteurs.|  
|DATETIME|MM-JJ-AA (par exemple, 01-17-92)<br /><br /> MMM-JJ-AA (par exemple, Jan-17-92)<br /><br /> JJ-MMM-AA (par exemple, 17-Jan-92)<br /><br /> AAAA-MM-JJ (par exemple, 1992-01-17)<br /><br /> AAAA-MMM-JJ (par exemple, 1992-Jan-17)<br /><br /> Les séparateurs de date mixte ne sont pas autorisés dans une table.<br /><br /> Le texte ISAM met en forme un champ DATETIME au format États-Unis ou européen, en fonction du paramètre international dans le panneau de configuration Windows.|  
|FLOAT|La largeur maximale comprend le signe et la virgule décimale. Dans Schema.ini, la largeur est indiquée comme suit :<br /><br /> 14,083 est une largeur de virgule flottante de 6<br /><br /> -14,083 est une largeur à virgule flottante 7<br /><br /> + 14,083 est une largeur à virgule flottante 7<br /><br /> 14083. est une largeur à virgule flottante de 6<br /><br /> ODBC retourne toujours 8 pour les colonnes FLOAT.<br /><br /> Les colonnes FLOAT peuvent également être en notation scientifique, par exemple :<br /><br /> -3.04 e + 2 correspond à la largeur de flotte 8<br /><br /> 25E4 est de type float Width 4<br /><br /> **Remarque** Les notations Decimal et scientifique ne peuvent pas être mélangées dans une colonne.<br /><br /> Les valeurs NULL sont représentées par une chaîne remplie vide dans des fichiers de longueur fixe et sont omises dans les fichiers délimités.<br /><br /> Les données float peuvent être complétées par des espaces à gauche.|  
|INTEGER|Les valeurs valides pour les colonnes de type entier sont comprises entre 32767 et-32766.<br /><br /> Dans Schema.ini, la largeur est indiquée comme suit :<br /><br /> 14083 est une largeur d’entier de 5<br /><br /> 0 est la largeur entière 1<br /><br /> ODBC retourne toujours 4 pour les colonnes d’entiers.<br /><br /> La largeur maximale comprend un signe. La largeur maximale d’une colonne d’entiers est de 11, bien que la largeur puisse être supérieure en raison des espaces autorisés dans les tables de format fixe.|  
|LONGCHAR|La limite théorique de la largeur d’une colonne LONGCHAR dans une table de longueur fixe ou délimitée est 65500K. L’ISAM Text est plus susceptible de fournir une prise en charge fiable jusqu’à environ 32 Ko.|  
  
 Vous trouverez plus de restrictions sur les types de données dans limitations des types de [données](../../odbc/microsoft/data-type-limitations.md).
