---
description: Entrées de Registre pour les sources de données
title: Entrées de Registre pour les sources de données | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- subkeys [ODBC]
- data sources [ODBC], registry entries
- registry entries for data sources [ODBC], about registry entries
- data sources [ODBC], configuring
- registry entries for data sources [ODBC]
ms.assetid: 78aaa3d3-d081-4550-80e3-720c910d5996
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 82759a988a0a2ff290d67406a1450ec9cb228a82
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88461301"
---
# <a name="registry-entries-for-data-sources"></a>Entrées de Registre pour les sources de données
> [!NOTE]  
>  À compter de Windows XP et de Windows Server 2003, ODBC est inclus dans le système d’exploitation Windows. Vous devez uniquement installer explicitement ODBC sur les versions antérieures de Windows.  
  
 La DLL du programme d’installation gère les informations dans le Registre relatives à chaque source de données. Dans Microsoft Windows NT/Windows 2000 et Microsoft Windows 95/98, ces informations sont stockées dans des sous-clés sous l’une des deux clés suivantes dans le registre :  

 ```console
 HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\Odbc.ini  
 ```

 ```console
 HKEY_CURRENT_USER\SOFTWARE\ODBC\Odbc.ini
 ```

 La clé utilisée varie selon que la source de données est une *source de données système,* accessible à tous les utilisateurs ou à une *source de données utilisateur,* qui est disponible uniquement pour l’utilisateur actuel. Les sources de données système sont stockées dans l’arborescence HKEY_LOCAL_MACHINE, et les sources de données utilisateur sont stockées dans l’arborescence HKEY_CURRENT_USER. Dans tous les autres aspects, les sources de données système et les sources de données utilisateur sont identiques.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Sous-clé des sources de données ODBC](../../../odbc/reference/install/odbc-data-sources-subkey.md)  
  
-   [Sous-clés de spécification de source de données](../../../odbc/reference/install/data-source-specification-subkeys.md)  
  
-   [Sous-clé par défaut](../../../odbc/reference/install/default-subkey.md)  
  
-   [Sous-clé ODBC](../../../odbc/reference/install/odbc-subkey.md)
