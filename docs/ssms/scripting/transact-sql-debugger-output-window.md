---
title: Fenêtre Sortie
description: Découvrez comment utiliser la fenêtre Sortie pour afficher les messages d’état et autres sorties du débogueur SQL Server Management Studio ainsi que d’autres outils.
titleSuffix: T-SQL Debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Output Window [Transact-SQL]
- Output Window [SQL Server Management Studio]
ms.assetid: 9808e00c-c8f6-45cc-896e-192b8420f747
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6866f5720d6e3343a9019595280897b5a275d27c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341014"
---
# <a name="transact-sql-debugger---output-window"></a>Débogueur Transact-SQL - Fenêtre Sortie

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette fenêtre contient les messages d'état de plusieurs fonctionnalités de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. La sortie est acheminée vers des volets spéciaux de la fenêtre **Sortie** du débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] , des fonctionnalités d’outils externes ou les commandes exécutées dans la **Fenêtre Commande** du débogueur. La sortie généralement affichée dans la fenêtre d'invite de commande des outils externes (notamment les fichiers .bat ou .com) est également disponible.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
 **Pour accéder à la fenêtre Sortie**  
  
-   Dans le menu **Affichage** , cliquez sur **Autres fenêtres**, puis sur **Sortie**.  
  
## <a name="options"></a>Options  
 **Liste des volets de sortie**  
 Affiche une liste des volets de sortie qui peuvent être consultés. Plusieurs volets d’informations peuvent être disponibles en fonction des outils qui ont utilisé la fenêtre **Sortie** pour fournir des informations à l’utilisateur.  
  
 **Volet de sortie**  
 Affiche la sortie du volet sélectionné dans la **liste du volet de sortie**.  
  
## <a name="see-also"></a>Voir aussi  
 [Débogueur Transact-SQL](./transact-sql-debugger.md)