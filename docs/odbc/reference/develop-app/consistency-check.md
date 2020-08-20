---
description: Vérification de la cohérence
title: Vérification de la cohérence | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- descriptors [ODBC], consistency checks
- consistency checks [ODBC]
ms.assetid: deb80efa-ad1f-4ea5-b334-9817cd279e5c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad1ffe0cb7819447a3819d73cd6605347b756ede
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88465901"
---
# <a name="consistency-check"></a>Vérification de la cohérence
Une vérification de cohérence est effectuée automatiquement par le pilote chaque fois qu’une application définit le champ SQL_DESC_DATA_PTR du APD, ARD ou IPD. Chaque fois que ce champ est défini, le pilote vérifie que la valeur du champ SQL_DESC_TYPE et les valeurs applicables au champ SQL_DESC_TYPE dans le même enregistrement sont valides et cohérentes.  
  
 Le champ SQL_DESC_DATA_PTR d’une IPD n’est normalement pas défini ; Toutefois, une application peut le faire pour forcer une vérification de cohérence des champs IPD. La valeur dont le champ SQL_DESC_DATA_PTR de l’IPD est défini n’est pas réellement stockée et ne peut pas être récupérée par un appel à **SQLGetDescField** ou **SQLGetDescRec**; le paramètre est défini uniquement pour forcer la vérification de cohérence. Une vérification de cohérence ne peut pas être effectuée sur un IRD.  
  
 Pour plus d’informations sur la vérification de cohérence, consultez [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).
