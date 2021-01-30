---
description: Limitations des chaînes
title: Limitations de chaîne | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC]
- desktop database drivers [ODBC]
ms.assetid: ec1da65f-c69d-415d-bf75-8fda8aa2b39f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12d02a6e69b34c13219e4bb5f00aeacfa9945cfb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188931"
---
# <a name="string-limitations"></a>Limitations des chaînes
La longueur maximale d’une chaîne d’instruction SQL est de 65 000 caractères.  
  
 Lorsque le pilote Microsoft Access est utilisé, seules les constantes de chaîne SQL-92 (avec des guillemets simples, et non des guillemets doubles) sont prises en charge.  
  
 Le caractère de barre verticale (&#124;) ne peut pas être utilisé dans une chaîne, que le caractère soit placé entre guillemets ou non.  
  
 Pour une interopérabilité maximale, les applications doivent passer des chaînes dans les paramètres, plutôt que de passer des chaînes entre guillemets.
