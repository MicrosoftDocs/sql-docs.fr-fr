---
title: Lanceur de démon de filtre de texte intégral SQL (onglet Ouvrir une session)
description: En savoir plus sur les Lanceur de démon de filtre de texte intégral SQL, qu’utilise la recherche en texte intégral SQL Server. En savoir plus sur l’onglet Ouvrir une session de sa boîte de dialogue Propriétés.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 13e260f9-a75f-430b-88a3-959ddcead8fe
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 08224bf7ed6ec8a6ba2c5b4ccd8cc88bea4bd22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346044"
---
# <a name="sql-full-text-filter-daemon-launcher-log-on-tab"></a>Lanceur de démon de filtre de texte intégral SQL (onglet Ouvrir une session)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  À compter de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], le lanceur de démon de filtre de texte intégral SQL (service de lancement FDHOST) est utilisé par la recherche en texte intégral [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ce service doit être en cours d'exécution pendant que vous utilisez la recherche en texte intégral. Pour plus d'informations sur les processus hôte de démon de filtre, consultez « Architecture de la recherche en texte intégral » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 L'onglet **Ouvrir une session** de la boîte de dialogue **Propriétés de Lanceur de démon de filtre de texte intégral SQL** permet de spécifier le compte utilisé par le service de recherche en texte intégral [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , de modifier le mot de passe d'un compte, ainsi que de démarrer et d'arrêter ce service. La modification du mot de passe d'un compte prend effet après le redémarrage du service.  
  
> [!NOTE]  
>  Lorsque vous modifiez le nom du compte utilisé par un service sur une instance en cluster, soit le nouveau compte doit être un membre du groupe de domaine spécifié au cours de l'installation pour le service, soit vous devez disposer de l'autorisation d'ajouter des membres à ce groupe. Si vous ne disposez pas de l'autorisation de modifier l'appartenance aux groupes, contactez l'administrateur de domaine.  
>   
>  Pour plus d'informations sur la sélection d'un compte pour exécuter le service, consultez « Configuration des comptes de service Windows » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="options"></a>Options  
 **Compte intégré**  
 **Système local**  
 Spécifiez le compte système local. Ce compte ne nécessite pas de mot de passe. Toutefois, le compte système local peut empêcher le service d'interagir avec les autres serveurs, en fonction des privilèges accordés au compte.  
  
 **Service local**  
 Spécifiez le compte de service local. Ce compte ne nécessite pas de mot de passe. Cependant, le compte de service local peut empêcher le service d'interagir avec les autres serveurs, en fonction des privilèges accordés au compte.  
  
 **Service réseau**  
 Il est recommandé de ne pas utiliser le compte de service réseau pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les comptes d'utilisateur local et d'utilisateur de domaine conviennent mieux à ces services.  
  
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
 Suspend le service. Non disponible pour ce service.  
  
 **Reprendre**  
 Reprend un service suspendu.  
  
  
