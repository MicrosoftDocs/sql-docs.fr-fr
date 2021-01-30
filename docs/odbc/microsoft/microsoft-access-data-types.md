---
description: Types de données Microsoft Access
title: Types de données Microsoft Access | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Access driver
- Jet-based ODBC drivers [ODBC], Access driver
- desktop database drivers [ODBC], Access driver
- Access driver [ODBC], data types
- access data types [ODBC]
- data types [ODBC], Access driver
ms.assetid: b537348a-bea0-4bd6-84a4-52a75292957f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0e7a55c6929d63bf036c9f1585d2eb039f6fec77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205705"
---
# <a name="microsoft-access-data-types"></a>Types de données Microsoft Access
Le tableau suivant répertorie les types de données Microsoft Access, les types de données utilisés pour créer des tables et les types de données ODBC SQL.  
  
|Type de données Microsoft Access|Type de données (CREATETABLE)|Type de données SQL ODBC|  
|--------------------------------|-------------------------------|------------------------|  
|BIGBINARY [1]|AUTORISE|SQL_LONGVARBINARY|  
|BINARY|BINARY|SQL_BINARY|  
|BIT|BIT|SQL_BIT|  
|)|)|SQL_INTEGER|  
|DEVISE|DEVISE|SQL_NUMERIC|  
|DATE/HEURE|DATETIME|SQL_TIMESTAMP|  
|GUID|GUID|SQL_GUID|  
|BINAIRE LONG|AUTORISE|SQL_LONGVARBINARY|  
|TEXTE LONG|LONGTEXT|SQL_WLONGVARCHAR DE SQL_LONGVARCHAR [2] [3]|  
|CHAMPS|LONGTEXT|SQL_WLONGVARCHAR DE SQL_LONGVARCHAR [2] [3]|  
|NUMBER (FieldSize = SINGLE)|SINGLE|SQL_REAL|  
|NUMBER (TailleChamp = DOUBLE)|DOUBLE|SQL_DOUBLE|  
|NUMBER (TailleChamp = octet)|OCTET NON SIGNÉ|SQL_TINYINT|  
|NUMBER (TailleChamp = entier)|SHORT|SQL_SMALLINT|  
|NUMBER (FieldSize = entier LONG)|LONG|SQL_INTEGER|  
|NUMERIC|NUMERIC|SQL_NUMERIC|  
|OLE|AUTORISE|SQL_LONGVARBINARY|  
|TEXT|VARCHAR|SQL_VARCHAR [1] SQL_WVARCHAR [2]|  
|VARBINARY|VARBINARY|SQL_VARBINARY|  
  
 [1] accéder aux applications 4,0 uniquement. Longueur maximale de 4000 octets. Comportement similaire à LONGBINARY.  
  
 [2] applications ANSI uniquement.  
  
 [3] applications Unicode et Access 4,0 uniquement.  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC. Elle ne retourne pas tous les types de données Microsoft Access si plusieurs types Microsoft Access sont mappés au même type de données SQL ODBC. Toutes les conversions de l’annexe D de la *Référence du programmeur ODBC* sont prises en charge pour les types de données SQL répertoriés dans le tableau précédent.  
  
 Le tableau suivant présente les limitations sur les types de données Microsoft Access.  
  
|Type de données|Description|  
|---------------|-----------------|  
|BINARY, VARBINARY et VARCHAR|La création d’une colonne BINARY, VARBINARY ou VARCHAR de zéro ou d’une longueur non spécifiée retourne en fait une colonne de 510 octets.|  
|BYTE|Bien qu’un champ de numéro Microsoft Access avec un Field égal à BYTE soit non signé, un nombre négatif peut être inséré dans le champ lors de l’utilisation du pilote Microsoft Access.|  
|CHAR, LONGVARCHAR et VARCHAR|Un littéral de chaîne de caractères peut contenir n’importe quel caractère ANSI (1-255 décimal). Utilisez deux guillemets simples consécutifs (' ') pour représenter un guillemet simple (').<br /><br /> Les procédures doivent être utilisées pour passer des données de caractères lors de l’utilisation d’un caractère spécial dans une colonne de type de données character.|  
|DATE|Les valeurs de date doivent être délimitées selon le format de date canonique ODBC ou délimitée par le délimiteur DateTime (« # »). Dans le cas contraire, Microsoft Access traitera la valeur comme une expression arithmétique et ne déclenchera pas d’avertissement ni d’erreur.<br /><br /> Par exemple, la date « 5 mars 1996 » doit être représentée sous la forme {d' 1996-03-05 '} ou #03/05/1996 #; dans le cas contraire, si seulement 03/05/1993 est envoyé, Microsoft Access évalue cela comme suit : 3 divisé par 5 divisé par 1996. Cette valeur est arrondie à l’entier 0, et puisque le jour zéro est mappé à 1899-12-31, il s’agit de la date utilisée.<br /><br /> Un caractère de barre verticale (&#124;) ne peut pas être utilisé dans une valeur de date, même s’il est placé entre guillemets.|  
|GUID|Type de données limité à Microsoft Access 4,0.|  
|NUMERIC|Type de données limité à Microsoft Access 4,0.|  
  
 Vous trouverez plus de restrictions sur les types de données dans limitations des types de [données](../../odbc/microsoft/data-type-limitations.md).
