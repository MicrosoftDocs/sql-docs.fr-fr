---
title: Propriétés de SQL Server Integration Services (onglet Service)
description: En savoir plus sur les options de l’onglet Service de la boîte de dialogue Propriétés d’Integration Services, tels que le chemin d’accès binaire, le nom d’hôte et le mode de démarrage.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 37f0acd9-c96f-48fd-9b53-2ca0097af242
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4a24a62880a263dc6babd9708b9f4d7e0c3a8a05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345143"
---
# <a name="sql-server-integration-services-properties-service-tab"></a>Propriétés de SQL Server Integration Services (onglet Service)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilisez l’onglet **Service** de la boîte de dialogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] **Propriétés** pour afficher ou spécifier les options suivantes.  
  
## <a name="options"></a>Options  
 **Chemin d'accès binaire**  
 Affiche l'emplacement des fichiers programme utilisés par ce service.  
  
 **Contrôle d'erreurs**  
 1 indique `SERVICE_ERROR_NORMAL`. Si le lancement du service échoue pendant le démarrage de l'ordinateur, le programme de démarrage consigne l'erreur et affiche une boîte de message, mais poursuit l'opération de démarrage. Cette valeur ne peut pas être modifiée.  
  
 **Code de sortie**  
 Code d’erreur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows définissant tout problème rencontré au démarrage ou à l’arrêt du service. Cette propriété a pour valeur **ERROR_SERVICE_SPECIFIC_ERROR** (1066) quand l’erreur est unique pour le service représenté par cette classe, et les informations sur l’erreur sont disponibles dans la propriété **ServiceSpecificExitCode** . Le service définit cette valeur à NO_ERROR (0) lors du fonctionnement, et à nouveau lors d'une fin normale.  
  
 **Host Name**  
 Affiche le nom de l'ordinateur ou du cluster exécutant le service [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Nom**  
 Indique le nom d'affichage du service.  
  
 **ID de processus**  
 Affiche l'ID de processus Windows.  
  
 **Type de service SQL**  
 Affiche le type de service fourni aux processus appelants. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe plusieurs services.  
  
 **Mode de démarrage**  
 Les options disponibles pour ce service sont les suivantes :  
  
-   Manuel : ce service n'est pas automatiquement lancé au démarrage de l'ordinateur. Vous devez démarrer le service à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d'un autre outil.  
  
-   Automatique : ce service essaie de se lancer au démarrage de cet ordinateur.  
  
-   Désactivé : impossible de démarrer ce service.  
  
 **State**  
 Indique si ce service est en cours d'exécution, arrêté ou désactivé. « **…** » indique qu’un changement d’état est en attente.  
  
  
