---
title: Modifier les comptes de service contrôleurs et clients
titleSuffix: SQL Server Distributed Replay
description: Découvrez comment modifier les comptes de service Distributed Replay Controller et Distributed Replay Client, puis à réappliquer les listes de contrôle d’accès.
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 85a79a7af034f0a35f64f6d9a0cafeb2233e8418
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839287"
---
# <a name="modify-the-controller-and-client-services-accounts"></a>Modifier les comptes de service contrôleurs et clients

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Dans cette rubrique, vous apprendrez à modifier les comptes de service Distributed Replay Controller et Distributed Replay Client, puis à réappliquer les listes de contrôle d'accès (ACL).  
  
### <a name="to-start-or-stop-the-distributed-replay-services-using-computer-management"></a>Pour démarrer ou arrêter les services Distributed Replay à l'aide de Gestion de l'ordinateur  
  
1.  Sur l’ordinateur sur lequel les services Distributed Replay sont installés, depuis l’invite de commandes, tapez **dcomcnfg**.  
  
2.  Double-cliquez sur **Services**, faites défiler et cliquez avec le bouton de droite sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay \<service name>** , puis cliquez sur **Démarrer** ou **Arrêter**.  
  
### <a name="to-modify-the-distributed-replay-controller-service"></a>Pour modifier le service Distributed Replay Controller  
  
1.  Sur l'ordinateur contrôleur, arrêtez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller.  
  
2.  Sous **Services**, cliquez avec le bouton droit sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller**, puis sélectionnez **Propriétés**.  
  
3.  Dans la fenêtre **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller** , sous l'onglet **Ouvrir une session** , sélectionnez **Ce compte**, tapez ou cliquez sur **Parcourir** pour entrer le nouveau compte d'ouverture de session, puis cliquez sur **OK**.  
  
     **Important !** Lorsque vous configurez le contrôleur Distributed Replay, vous pouvez spécifier un ou plusieurs comptes d'utilisateurs qui seront utilisés pour exécuter les services clients Distributed Replay. Vous trouverez ci-dessous la liste des comptes pris en charge :  
  
    -   Compte d’utilisateur de domaine  
  
    -   Compte d'utilisateur local créé par l'utilisateur  
  
    -   Administrateur  
  
    -   Compte virtuel et Compte de service administré (MSA)  
  
    -   Services réseau, Services locaux et Système  
  
     Les comptes de groupe (locaux ou de domaine) et autres comptes intégrés (comme Tout le monde) ne sont pas acceptés.  
  
4.  Démarrez le service Distributed Replay Controller.  
  
### <a name="to-modify-the-distributed-replay-client-service"></a>Pour modifier le service Distributed Replay Client  
  
1.  Avant de modifier le service Distributed Replay Client, assurez-vous que le compte de service client que vous modifiez a été spécifié durant l'installation (dans le paramètre CTLRUSERS sur l'ordinateur contrôleur). Si le compte de service client que vous modifiez n'a pas été spécifié pendant l'installation, vous devez effectuer les étapes suivantes au préalable :  
  
    1.  Arrêtez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller.  
  
    2.  Sur l’ordinateur contrôleur sur lequel le service contrôleur est installé, depuis l’invite de commandes, tapez **dcomcnfg**.  
  
    3.  Dans la fenêtre **Services de composants**, accédez à **Racine de la console -> Services de composants -> Ordinateurs -> Poste de travail -> DCOM Config ->DReplayController**.  
  
    4.  Cliquez avec le bouton droit sur **DReplayController**, puis cliquez sur **Propriétés**.  
  
    5.  Dans la fenêtre **Propriétés de DReplayController** , sous l'onglet **Sécurité** , cliquez sur **Modifier** dans la section **Autorisations d'exécution et d'activation** .  
  
    6.  Accordez au nouveau compte d'ouverture de session du service client des autorisations **Activation locale et à distance** , puis cliquez sur **OK**.  
  
    7.  Cliquez sur **Modifier** dans la section **Autorisations d'accès** et accordez des autorisations **Accès local et distant** au nouveau compte d'ouverture de session du service client, puis cliquez sur **OK**.  
  
    8.  Cliquez sur **OK** pour fermer la fenêtre **Propriétés de DReplayController** .  
  
    9. Sur l'ordinateur contrôleur, ajoutez le compte d'ouverture de session du service client modifié au groupe **Utilisateurs du modèle COM distribué** .  
  
    10. Démarrez le service SQL Server Distributed Replay Controller.  
  
2.  Arrêtez le service client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client.  
  
3.  Dans la fenêtre **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client** , sous l'onglet **Ouvrir une session** , sélectionnez **Ce compte**, tapez ou cliquez sur **Parcourir** pour entrer le nouveau compte d'ouverture de session, puis cliquez sur **OK**.  
  
4.  Démarrez le service Distributed Replay Client.  
  
  
