---
description: Curseurs de bloc, curseurs avec défilement et compatibilité descendante
title: Curseurs de bloc, curseurs avec défilement et compatibilité descendante | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [ODBC]
- cursors [ODBC], backward compatibility
- compatibility [ODBC], cursors
- backward compatibility [ODBC], cursors
- block cursors [ODBC]
ms.assetid: d9d271f6-d2d9-49b9-a365-4909ca06caae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 32e21d7f066432d2ccd6cced3c326dfe35c76867
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212479"
---
# <a name="block-cursors-scrollable-cursors-and-backward-compatibility"></a>Curseurs de bloc, curseurs avec défilement et compatibilité descendante
L’existence de **SQLFetchScroll** et **SQLExtendedFetch** représente le premier fractionnement clair dans ODBC entre l’interface de programmation d’applications (API), qui est l’ensemble des fonctions que l’application appelle, et l’interface de fournisseur de services (SPI), qui est l’ensemble de fonctions implémentées par le pilote. Ce fractionnement est nécessaire pour que ODBC *3. x*, qui utilise **SQLFetchScroll**, s’aligne avec les normes et soit également compatible avec ODBC *2. x*, qui utilise **SQLExtendedFetch**.  
  
 L’API ODBC *3. x* , qui est l’ensemble des fonctions appelées par l’application, comprend **SQLFetchScroll** et les attributs d’instruction associés. Le SPI ODBC *3. x* , qui est l’ensemble des fonctions implémentées par le pilote, comprend **SQLFetchScroll**, **SQLExtendedFetch** et les attributs d’instruction associés. Dans la mesure où ODBC n’impose pas cette séparation entre l’API et le SPI, il est possible pour les applications ODBC *3. x* d’appeler **SQLExtendedFetch** et les attributs d’instruction associés. Toutefois, il n’y a aucune raison pour que l’application ODBC *3. x* le fasse. Pour plus d’informations sur les API et les SPI, consultez Introduction à [ODBC architecture](../../../odbc/reference/odbc-architecture.md).  
  
 Pour plus d’informations sur les fonctions et attributs d’instruction qu’une application ODBC *3. x* doit utiliser avec les curseurs de bloc et de défilement, consultez [curseurs de bloc, curseurs avec défilement et compatibilité descendante pour les applications ODBC 3. x](../../../odbc/reference/develop-app/block-cursors-scrollable-backward-compatibility-odbc-3-x-applications.md).  
  
 Cette section contient les rubriques suivantes :  
  
-   [Ce que fait le gestionnaire de pilotes](../../../odbc/reference/appendixes/what-the-driver-manager-does.md)  
  
-   [Ce que fait le pilote](../../../odbc/reference/appendixes/what-the-driver-does.md)
