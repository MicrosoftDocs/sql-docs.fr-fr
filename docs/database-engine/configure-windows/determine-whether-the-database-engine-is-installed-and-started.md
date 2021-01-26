---
title: Déterminer si le moteur de base de données est installé et démarré | Microsoft Docs
description: Apprenez comment déterminer si le moteur de base de données est installé et démarré. Découvrez comment utiliser le Gestionnaire de configuration SQL Server pour vérifier les composants installés.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server, determining if installed
- verifying Database Engine installation
- viewing Database Engine installation
- installed Database Engine verification [SQL Server]
ms.assetid: babb02e4-3521-4b75-b5df-e09205594375
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 03aa96f16ee200846dae726851c51a1dc1d04e80
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783000"
---
# <a name="determine-whether-the-database-engine-is-installed-and-started"></a>Déterminer si le moteur de base de données est installé et démarré
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Lorsqu'elle est réussie, l'installation du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] installe des fichiers dans le système de fichiers, crée des entrées dans le Registre et installe plusieurs outils. Cette rubrique explique comment déterminer si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est installé et démarré dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="how-to-view-and-start-the-database-engine-by-using-sql-server-configuration-manager"></a>Comment afficher et démarrer le moteur de base de données à l'aide du Gestionnaire de configuration SQL Server  
  
1.  Cliquez sur **Démarrer**, pointez sur **Tous les programmes**, pointez sur **Microsoft SQL Server**, pointez sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
     Si ces éléments ne sont pas visibles dans le menu **Démarrer** , cela signifie que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas correctement installé. Exécutez le programme d'installation pour installer le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
2.  Dans le **Gestionnaire de configuration SQL Server**, dans le volet gauche, cliquez sur **Services SQL Server**. Le volet droit répertorie plusieurs services associés à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est installé, le service [!INCLUDE[ssDE](../../includes/ssde-md.md)] apparaît sous le nom **SQL Server (MSSQLSERVER)** s'il s'agit de l'instance par défaut ou sous la forme **SQL Server (** \<*instance_name*> **)**  si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est installé en tant qu'instance nommée. Sauf modification du nom de l’instance, [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] s’installe en tant qu’instance nommée sous le nom **SQLEXPRESS**. Si l'icône du service est un triangle vert, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est en cours d'exécution. Par contre, si l'icône est un carré rouge, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est arrêté.  
  
3.  Pour démarrer le [!INCLUDE[ssDE](../../includes/ssde-md.md)],dans le volet droit, cliquez avec le bouton droit sur le [!INCLUDE[ssDE](../../includes/ssde-md.md)], puis cliquez sur **Démarrer**.  
  
> [!NOTE]  
>  Au cours de l'installation, l'utilisateur peut choisir l'emplacement où les fichiers programmes et les fichiers de base de données vont être installés. S’il accepte l’emplacement par défaut, les fichiers sont installés dans [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)] et C:\Program Files\Microsoft SQL Server\MSSQL.*x*, où *x* est un nombre.  
  
  
