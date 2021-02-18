---
title: Propriétés de SQL Server Browser (onglet Avancé)
description: En savoir plus sur les options de l’onglet Avancé de la boîte de dialogue Propriétés de SQL Server Browser, tels que le répertoire d’images mémoires et l’ID de l’instance.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ba79137a-cb72-4bf3-a650-e11d02cfce10
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: fc2075fe1875a8d74b101f79a691f5d88c26e04f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345218"
---
# <a name="sql-server-browser-properties-advanced-tab"></a>Propriétés de SQL Server Browser (onglet Avancé)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Le programme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser s'exécute sur le serveur en tant que service. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est à l’écoute des demandes entrantes pour les ressources [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et fournit des informations sur les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installées sur l’ordinateur.  
  
## <a name="options"></a>Options  
 **Cluster**  
 Indique si ce service est installé en tant que ressource d'un serveur cluster.  
  
 **Commentaires client**  
 Indique si le contrôle SQM (Service Quality Monitoring) a été activé sur ce service. Pour plus d'informations sur Commentaires client, recherchez dans la documentation en ligne la rubrique « Paramètres des rapports d'erreurs et d'utilisation ».  
  
 **Répertoire de vidage**  
 Emplacement où sont placés les vidages de mémoire en cas d'erreur.  
  
 **Rapport d'erreurs**  
 Quand cette option a pour valeur **Oui**, le programme Dr. Watson transfère les informations à [!INCLUDE[msCoName](../../includes/msconame-md.md)] ou au serveur d'erreur en cas de problème sérieux. Pour plus d'informations sur les rapports d'erreurs, recherchez dans la documentation en ligne la rubrique « Paramètres des rapports d'erreurs et d'utilisation ».  
  
 **ID de l'instance**  
 Indique l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a utilisé cette instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. L’instance par défaut est **MSSQL10_50.MSSQLSERVER**.  
  
## <a name="see-also"></a>Voir aussi  
 [Service SQL Server Browser](../../tools/configuration-manager/sql-server-browser-service.md)  
  
  
