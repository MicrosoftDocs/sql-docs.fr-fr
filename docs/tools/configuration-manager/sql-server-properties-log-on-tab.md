---
title: Propriétés de SQL Server (onglet Ouvrir une session)
description: En savoir plus sur l’onglet Ouvrir une session de la boîte de dialogue Propriétés SQL Server. Utilisez cet onglet pour spécifier le compte utilisé par SQL Server et pour démarrer ou arrêter le service.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 405073fc-eaa3-43c6-bf82-2cd58cacc1c3
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 63e71e0a0e4c53b77b3c1f11225f49abb250c585
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345979"
---
# <a name="sql-server-properties-log-on-tab"></a>Propriétés de SQL Server (onglet Ouvrir une session)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  L'onglet **Ouvrir une session** de la boîte de dialogue **Propriétés de SQL Server** permet de spécifier le compte utilisé par le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , de modifier le mot de passe d'un compte, ainsi que de démarrer et d'arrêter ce service. La modification du mot de passe d'un compte prend effet immédiatement.  
  
> [!NOTE]  
>  Lorsque vous modifiez le nom du compte utilisé par un service sur une instance cluster, le nouveau compte doit être un membre du groupe de domaine spécifié au cours de l'installation pour le service modifié ou vous devez disposer de l'autorisation d'ajouter des membres à ce groupe. Si vous ne disposez pas de l'autorisation de modifier l'appartenance aux groupes, contactez l'administrateur de domaine.  
>   
>  Pour plus d'informations sur la sélection d'un compte pour exécuter le service, consultez « Configuration des comptes de service Windows » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="options"></a>Options  
 **Compte intégré**  
 **Système local**  
 -   Spécifiez le compte système local. Ce compte ne nécessite pas de mot de passe. Toutefois, le compte système local peut empêcher le service d'interagir avec les autres serveurs, en fonction des privilèges accordés au compte.  
  
 **Service local**  
 -   Spécifiez le compte de service local. Ce compte ne nécessite pas de mot de passe. Cependant, le compte de service local peut empêcher le service d'interagir avec les autres serveurs, en fonction des privilèges accordés au compte.  
  
 **Service réseau**  
 -   Il est recommandé de ne pas utiliser le compte de service réseau pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Les comptes d'utilisateur local et d'utilisateur de domaine conviennent mieux à ces services.  
  
 **Ce compte**  
 Spécifiez un compte d'utilisateur local ou de domaine qui utilise l'authentification Windows. Nous vous recommandons d'utiliser un compte d'utilisateur de domaine doté d'autorisations minimales pour les services.  
  
 **Nom du compte**  
 Spécifie le nom de compte d'utilisateur local ou de domaine.  
  
 **Mot de passe**  
 Tapez le mot de passe du compte.  
  
 **Confirmer le mot de passe**  
 Tapez de nouveau le mot de passe du compte.  
  
 **Start**  
 Démarrez le service.  
  
 **Stop**  
 Arrête le service.  
  
 **Suspendre**  
 Suspend le service.  
  
 **Reprendre**  
 Reprend un service suspendu.  
  
> [!IMPORTANT]  
>  Par défaut, seuls les membres du groupe des administrateurs locaux peuvent démarrer, arrêter, interrompre, reprendre ou redémarrer un service. Pour accorder aux non-administrateurs la capacité de gérer des services, consultez [Comment accorder aux utilisateurs des droits de gestion des services dans Windows Server 2003](https://support.microsoft.com/kb/325349). (le processus est semblable sur d'autres versions de Windows).  
  
> [!NOTE]  
>  Lors du démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une erreur WMI contenant l'expression « non implémenté [0x80004001] » peut indiquer que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas installé sur l'ordinateur cible.  
  
  
