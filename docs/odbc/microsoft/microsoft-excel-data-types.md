---
description: Types de données Microsoft Excel
title: Types de données Microsoft Excel | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], Excel driver
- Excel data types [ODBC]
- Jet-based ODBC drivers [ODBC], Excel driver
- desktop database drivers [ODBC], Excel driver
- ODBC desktop database drivers [ODBC], Excel driver
- Excel driver [ODBC], data types
ms.assetid: 7b44c8e5-0bc3-4912-8a5d-56f4d5562fe6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a6342706e2ab9b66db9d0410b31573d89bdf1d7c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212131"
---
# <a name="microsoft-excel-data-types"></a>Types de données Microsoft Excel
Le tableau suivant montre comment les types de données du pilote Microsoft Excel sont mappés aux types de données ODBC SQL. Le pilote Microsoft Excel attribue ces types de données aux colonnes des tableaux Microsoft Excel en fonction des données de la colonne.  
  
|Type de données Microsoft Excel|Type de données ODBC|  
|-------------------------------|--------------------|  
|DEVISE|SQL_NUMERIC|  
|DATETIME|SQL_TIMESTAMP|  
|LOGICAL|SQL_BIT|  
|NUMBER|SQL_DOUBLE|  
|TEXT|SQL_VARCHAR|  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC SQL. Toutes les conversions de l’annexe D de la *Référence du programmeur ODBC* sont prises en charge pour les types de données SQL ODBC répertoriés précédemment dans cette rubrique.  
  
 Le tableau suivant présente les limitations relatives aux types de données Microsoft Excel.  
  
|Type de données|Description|  
|---------------|-----------------|  
|Données chiffrées|Le pilote Microsoft Excel ne peut pas lire les données chiffrées.|  
|Chaînes d’erreur|Le pilote Microsoft Excel ne peut pas retourner une chaîne de caractères pour les valeurs d’erreur Microsoft Excel (#N/A !, #VALUE !, #REF !, #DIV/0 !, #NUM !, #NAME ? et #NULL !), mais retourne une valeur NULL à la place.|  
|LOGICAL|La valeur d’une colonne logique est retournée dans une mémoire tampon SQL_C_CHAR en tant que 0 ou 1.|  
|NUMBER|Si une colonne d’entiers est créée, les nombres trop grands pour le type de données Integer peuvent être entrés, et les données contenant des valeurs non entières peuvent être insérées, avec pour résultat que la colonne peut être convertie en SQL_DOUBLE.|  
|TEXT|Lorsque les lignes d’une colonne contiennent plusieurs types de données Microsoft Excel, le pilote ODBC Microsoft Excel affecte le type de données SQL_VARCHAR à la colonne. Il existe une exception à cela : si la colonne contient seulement deux ou trois des types de données DateTime (DATE, TIME et DATETIME), le pilote ODBC de Microsoft Excel affecte le type de données SQL_TIMESTAMP à la colonne.<br /><br /> La création d’une colonne de texte de zéro ou d’une longueur non spécifiée retourne en fait une colonne de 255 octets.<br /><br /> Un littéral de chaîne de caractères peut contenir n’importe quel caractère ANSI (1-255 décimal). Utilisez deux guillemets simples consécutifs (") pour représenter un guillemet simple (').<br /><br /> L’insertion d’une valeur NULL dans une colonne avec un type de données autre que SQL_VARCHAR entraîne la modification du type de données de la colonne en SQL_VARCHAR.|  
  
 Vous trouverez plus de restrictions sur les types de données dans limitations des types de [données](../../odbc/microsoft/data-type-limitations.md).
