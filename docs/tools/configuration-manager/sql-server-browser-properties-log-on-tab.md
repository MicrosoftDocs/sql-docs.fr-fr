---
title: Propriétés de SQL Server Browser (onglet Ouvrir une session)
description: En savoir plus sur l’onglet Ouvrir une session de la boîte de dialogue Propriétés SQL Server Browser. Découvrez comment utiliser cet onglet pour spécifier un compte et démarrer ou arrêter le service.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: c77871ec-c2f4-4e4a-9a10-5aeb4ae70507
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b26b1c78890a0af4425da74236c5e2fb4d10de44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345215"
---
# <a name="sql-server-browser-properties-log-on-tab"></a>Propriétés de SQL Server Browser (onglet Ouvrir une session)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Le programme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser s'exécute sur le serveur en tant que service. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est à l’écoute des demandes entrantes pour les ressources [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et fournit des informations sur les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installées sur l’ordinateur.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est à l’écoute d’un port UDP et accepte les demandes non authentifiées à l’aide du protocole SSRP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Resolution Protocol).  
  
 La modification du mot de passe d'un compte prend effet immédiatement sans redémarrer le service.  
  
## <a name="options"></a>Options  
 **Compte système local**  
 Exécutez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser dans le contexte de sécurité du compte système local. Quand cela est possible, utilisez à la place de celui-ci un compte à faible niveau d'autorisation.  
  
 **Ce compte**  
 Spécifiez un compte d'utilisateur local ou de domaine qui utilise l'authentification Windows. Nous vous recommandons d'utiliser un compte d'utilisateur de domaine doté d'autorisations minimales pour les services. Pour plus d'informations sur la sélection d'un compte, consultez « Configuration des comptes de service Windows » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Parcourir**  
 Recherchez un principal de sécurité utilisateur ou intégré.  
  
> [!IMPORTANT]  
>  Utilisez un compte à faible niveau d'autorisation. Pour plus d’informations sur les autorisations nécessaires pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser, consultez la section relative à la sécurité de la rubrique [Service SQL Server Browser](../../tools/configuration-manager/sql-server-browser-service.md).  
  
 **Mot de passe**  
 Entrez le mot de passe du principal de sécurité.  
  
 **Confirmer le mot de passe**  
 Confirmez le mot de passe du principal de sécurité.  
  
 **État du service**  
 Indique si ce service est en cours d'exécution, arrêté ou désactivé. « **…** » indique qu’un changement d’état est en attente.  
  
 **Start**  
 Démarre le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser.  
  
 **Stop**  
 Arrête le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser.  
  
 **Suspendre**  
 Interrompt le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser.  
  
 **Reprendre**  
 Reprend un service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser suspendu.  
  
## <a name="see-also"></a>Voir aussi  
 [Service SQL Server Browser](../../tools/configuration-manager/sql-server-browser-service.md)  
  
  
