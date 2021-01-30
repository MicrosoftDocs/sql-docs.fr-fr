---
description: Limitations des types de données
title: Limitations des types de données | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], data types
- data types [ODBC], desktop database drivers
- desktop database drivers [ODBC], data types
ms.assetid: 81c4eab7-1f6b-47a0-b940-89d6c6a14dae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9081f52e7cf79613b9021ce7b883a1720da468c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176526"
---
# <a name="data-type-limitations"></a>Limitations des types de données
Les pilotes de base de données Microsoft ODBC Desktop imposent les limitations suivantes sur les types de données :  
  
|Type de données|Description|  
|---------------|-----------------|  
|Tous les types de données|Les échecs de conversion de type peuvent entraîner la définition de la colonne affectée avec la valeur NULL.|  
|BINARY|La création d’une colonne binaire de longueur nulle retourne en fait une colonne binaire de 255 octets.|  
|DATE|Le type de données DATE ne peut pas être converti en un autre type de données (ou lui-même) par la fonction CONVERT.|  
|DÉCIMAL (numérique exact)|Non pris en charge.|  
|Types de données Floating-Point|Le nombre de décimales dans un nombre à virgule flottante peut être limité par le format de nombre défini dans la section international du panneau de configuration Windows.|  
|NUMERIC|Prend en charge la précision maximale et une échelle de 28.|  
|timestamp|Le type de données TIMESTAMP ne peut pas être converti en lui-même par la fonction CONVERT.|  
|TINYINT|Les valeurs TINYINT sont toujours non signées.|  
|Chaînes Zero-Length|Lorsqu’un dBASE, Microsoft Excel, Paradox ou TextDriver est utilisé, l’insertion d’une chaîne de longueur nulle dans une colonne insère en fait une valeur NULL à la place.|
