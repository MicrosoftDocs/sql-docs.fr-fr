---
description: Configurer WMI pour afficher l'état du serveur dans les outils SQL Server
title: Configurer WMI pour afficher l'état du serveur dans les outils SQL Server
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- WMI Provider for Server Events, setting permissions
- WMI permissions [SQL Server]
ms.assetid: 7e97197b-ed4d-40d1-9a52-9ab1d92401d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a39ef785c7f0208d34f10bf31d5749f403e63723
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344198"
---
# <a name="configure-wmi-to-show-server-status-in-sql-server-tools"></a>Configurer WMI pour afficher l'état du serveur dans les outils SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Cette rubrique explique comment configurer WMI pour afficher l'état du serveur dans les outils SQL Server dans [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]. Lors de la connexion aux serveurs, les composants Serveurs inscrits et Explorateur d'objets de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], ainsi que le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , utilisent WMI (Windows Management Instrumentation) pour obtenir l'état des services [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (MSSQLSERVER) et [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent (MSSQLSERVER). Pour afficher l'état du service, l'utilisateur doit disposer des droits d'accéder à distance à l'objet WMI. Cette autorisation peut être configurée seulement si WMI a été installé sur le serveur.  
  
## <a name="to-configure-wmi-permission"></a><a name="SSMSProcedure"></a>Pour configurer l'autorisation WMI  
  
1.  Sur le serveur distant, dans le menu **Démarrer** , cliquez sur **Exécuter**.  
  
2.  Dans la zone **Ouvrir** , tapez **wmimgmt.msc** et cliquez sur **OK**.  
  
3.  Dans le programme **Windows Management Infrastructure** , cliquez avec le bouton droit sur **Contrôle WMI (local)**, puis cliquez sur **Propriétés**.  
  
4.  Dans la boîte de dialogue **Propriétés de WMI Control (Local)** , sous l’onglet **Sécurité** , développez **Root**, puis cliquez sur **CIMV2**.  
  
5.  Cliquez sur **Sécurité** pour ouvrir la boîte de dialogue **Sécurité pour ROOT\CIMV2** .  
  
6.  Ajoutez un groupe ou un utilisateur à la zone **Noms de groupes ou d’utilisateurs** et sélectionnez-le.  
  
7.  Dans la zone **Autorisations pour** _<group or user>_ , sélectionnez la colonne **Autoriser** correspondant à l’autorisation **Appel à distance autorisé** , pour les utilisateurs dont vous souhaitez détecter à distance l’état de service.  
  
## <a name="see-also"></a>Voir aussi  
[Démarrer, arrêter ou suspendre le service SQL Server Agent](../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  
