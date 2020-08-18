---
description: Configuration matérielle et logicielle requise (ODBC)
title: Configuration matérielle et logicielle requise (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- hardware requirements [ODBC], desktop database drivers
- software requirements [ODBC], desktop database drivers
- system requirements [ODBC], desktop database drivers
- requirements [ODBC], desktop database drivers
ms.assetid: 6df2e9cd-de10-4629-97bd-32f2782616c7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a0f88c8877161122c11fb65bcdb62fd4e7b684c3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88412485"
---
# <a name="hardware-and-software-requirements-odbc"></a>Configuration matérielle et logicielle requise (ODBC)
Cette rubrique répertorie la configuration requise pour l’utilisation des pilotes de base de données de bureau ODBC.  
  
## <a name="hardware-requirements"></a>Configuration matérielle  
 Pour utiliser les pilotes de base de données de bureau ODBC, vous devez disposer des éléments suivants :  
  
-   Un ordinateur personnel compatible IBM.  
  
-   Un disque dur avec 6 Mo d’espace disque libre.  
  
-   Au moins 16 Mo de mémoire vive (RAM).  
  
## <a name="software-requirements"></a>Configuration logicielle  
 Pour accéder aux données avec un pilote ODBC, vous devez disposer des éléments suivants :  
  
-   Pilote ODBC.  
  
-   Le gestionnaire de pilotes ODBC 32 bits, version 3,51 ou ultérieure (Odbc32.dll).  
  
-   Microsoft Windows 95 ou version ultérieure, ou Windows NT 4,0 ou Windows 2000.  
  
-   Taille de la pile d’au moins 20 Ko pour une application utilisant un pilote Microsoft ODBC.  
  
 Lorsque vous utilisez Microsoft Windows NT 4,0 ou Windows 2000, le pilote 32 bits est thread-safe, mais uniquement via l’utilisation d’un sémaphore global qui contrôle l’accès au pilote. L’utilisation simultanée du pilote est très limitée sous Windows NT. Tout accès à la couche ISAM jet sera monothread pour toutes les applications utilisant le moteur Microsoft Jet.  
  
 Lors de l’exécution de plusieurs applications 16 bits sur Windows sur Windows (WOW) sur Microsoft Windows NT 4,0, les applications doivent être exécutées dans des espaces mémoire distincts. (Vous ne pouvez pas utiliser le même espace mémoire, car ODBC ne prend pas en charge plusieurs environnements dans le même processus.) Pour exécuter une application dans un espace mémoire séparé, sélectionnez l’icône de l’application dans le gestionnaire de programmes, ouvrez le menu **fichier** , cliquez sur **Propriétés**, puis cliquez sur **exécuter dans un espace mémoire séparé**.  
  
 L’utilisation de ces pilotes par des applications 16 bits sur Windows 95 n’est pas prise en charge.  
  
## <a name="driver-specific-hardware-and-software-requirements"></a>Configuration matérielle et logicielle requise spécifique au pilote  
  
-   MicrosoftAccess et dBASEdrivers peuvent nécessiter des modifications dans les fichiers Autoexec.bat ou Config.sys.
