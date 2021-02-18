---
title: Propriétés de SQL Server (onglet Avancé)
description: En savoir plus sur les options de l’onglet Avancé de la boîte de dialogue Propriétés de SQL Server, tels que le chemin d'accès aux données, l’ID de l’instance et les propriétés personnalisées.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 2ffd10fd-bac1-478f-9cff-96ed6c8b787f
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 3b62decb0bc8f350fbb1916e7a5fb8779c9878e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345121"
---
# <a name="sql-server-properties-advanced-tab"></a>Propriétés de SQL Server (onglet Avancé)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Les propriétés suivantes figurent par défaut dans l'onglet **Avancé** . Si des propriétés personnalisées sont définies, elles apparaissent également sous cet onglet, avec les valeurs correspondantes.  
  
## <a name="options"></a>Options  
 **Cluster**  
 Indique si ce service est installé en tant que ressource d'un serveur cluster.  
  
 **Commentaires client**  
 Indique si le contrôle SQM (Service Quality Monitoring) a été activé sur ce service. Pour plus d'informations sur Commentaires client, recherchez dans la documentation en ligne la rubrique « Paramètres des rapports d'erreurs et d'utilisation ».  
  
 **Chemin d'accès aux données**  
 Affiche le chemin des binaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour cette installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Répertoire de vidage**  
 Affiche l'emplacement où sont placés les vidages de mémoire en cas d'erreur.  
  
 **Rapport d'erreurs**  
 Quand cette option a pour valeur **Oui**, le programme Dr. Watson transfère les informations à [!INCLUDE[msCoName](../../includes/msconame-md.md)] ou au serveur d'erreur en cas de problème sérieux. Pour plus d'informations sur les rapports d'erreurs, recherchez dans la documentation en ligne la rubrique « Paramètres des rapports d'erreurs et d'utilisation ». Pour modifier cette valeur, dans l’Explorateur d’objets [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez avec le bouton droit sur votre serveur, cliquez sur **Propriétés**, puis sur la page **Paramètres divers du serveur**. Les options figurent dans la zone **Rapport d’information** .  
  
 **Version de fichier**  
 Affiche la version de l'exécutable [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Chemin d'accès à l'installation**  
 Affiche le chemin des binaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour cette installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **ID de l'instance**  
 Indique l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a utilisé ce service.  
  
 **Langage**  
 Affiche la langue par défaut des messages du serveur.  
  
 **Racine du Registre**  
 Affiche l'emplacement des clés de Registre utilisées par cette application.  
  
 **Niveau du Service Pack**  
 Affiche le niveau du Service Pack de cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Nom du SKU**  
 Affiche le SKU (stock keeping unit) du produit, parfois appelé « édition du produit ».  
  
 **Paramètres de démarrage**  
 Répertorie tous les paramètres de démarrage utilisés par cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les paramètres sont séparés par des points-virgules. Les paramètres par défaut comprennent les chemins d'accès au fichier de données de la base de données master (`master.mdf`), le fichier journal de la base de données master (`mastlog.ldf`) et le fichier journal des erreurs. Pour connaître la syntaxe des paramètres de démarrage, recherchez dans la documentation en ligne la rubrique **Utilisation des options de démarrage du service SQL Server.**  
  
 **Version**  
 Affiche le numéro SKU (stock keeping unit) du produit.  
  
 **Version**  
 Affiche le numéro de version de cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Nom du serveur virtuel**  
 **Nom du serveur virtuel** quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé sur un serveur cluster.  
  
  
