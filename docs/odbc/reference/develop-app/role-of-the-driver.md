---
description: Rôle du pilote
title: Rôle du pilote | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver error checking [ODBC]
- diagnostic information [ODBC], driver error checking
ms.assetid: cac64c24-a27d-4884-96c0-ea7988351711
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: afd8f058b12b30140bb193ded7a23e8daf365865
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88465645"
---
# <a name="role-of-the-driver"></a>Rôle du pilote
Le pilote vérifie toutes les erreurs et tous les avertissements non vérifiés par le gestionnaire de pilotes et les enregistrements d’état de commandes qu’il génère. (ODBC 2. le pilote *x* ne commande pas les enregistrements d’État.) Cela comprend les erreurs et les avertissements dans la troncation des données, la conversion des données, la syntaxe et certaines transitions d’État. Le pilote peut également vérifier les erreurs et les avertissements partiellement vérifiés par le gestionnaire de pilotes. Par exemple, bien que le gestionnaire de pilotes vérifie si la valeur de l' *opération* dans **SQLSetPos** est légale, le pilote doit vérifier s’il est pris en charge.  
  
 Le pilote mappe également les *erreurs natives* , c’est-à-dire les erreurs retournées par la source de données-aux sqlstates. Par exemple, le pilote peut mapper plusieurs erreurs natives différentes pour la syntaxe SQL illégale à SQLSTATE 42000 (erreur de syntaxe ou violation d’accès). Le pilote retourne le numéro d’erreur natif dans le champ SQL_DIAG_NATIVE de l’enregistrement d’État. La documentation du pilote doit indiquer comment les erreurs et les avertissements sont mappés de la source de données aux arguments dans **SQLGetDiagRec** et **SQLGetDiagField**.
