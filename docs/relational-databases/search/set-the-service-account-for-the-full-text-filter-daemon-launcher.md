---
description: Définir le compte du service du Lanceur de démon de filtre de texte intégral
title: Définir le compte du service du Lanceur de démon de filtre de texte intégral
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], FDHOST Launcher (MSSQLFDLauncher) service account
- FDHOST Launcher (MSSQLFDLauncher) [SQL Server]
ms.assetid: 3ab1d101-7ae0-488f-9b57-468e2517b737
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 4904a5e907a026f0cd8a6b5a0936b9f9395c2453
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468620"
---
# <a name="set-the-service-account-for-the-full-text-filter-daemon-launcher"></a>Définir le compte du service du Lanceur de démon de filtre de texte intégral
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
 Cette rubrique explique comment définir ou changer le compte du service du Lanceur de démon de filtre de texte intégral SQL (MSSQLFDLauncher) à l’aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le compte de service par défaut utilisé par le programme d’installation de SQL Server est `NT Service\MSSQLFDLauncher`.
  
  
## <a name="about-the-sql-full-text-filter-daemon-launcher-service"></a>À propos du service du Lanceur de démon de filtre de texte intégral SQL
Le service du Lanceur de démon de filtre de texte intégral SQL est utilisé par la recherche en texte intégral SQL Server pour démarrer le processus hôte de démon de filtre, qui gère les césures de mots et le filtrage de recherche en texte intégral. Le service du Lanceur doit être en cours d’exécution pour que la recherche en texte intégral puisse être utilisée.  
  
Le service du Lanceur de démon de filtre de texte intégral SQL est un service dépendant associé à une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le service du Lanceur de démon de filtre de texte intégral SQL propage les informations du compte de service à chaque processus hôte de démon de filtre qu’il lance.  

##  <a name="set-the-service-account"></a><a name="setting"></a> Définir le compte de service  
  
1.  Dans le menu **Démarrer**, pointez sur **Tous les programmes**, développez [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], puis cliquez sur **Gestionnaire de configuration SQL Server 2016**.  
  
2.  Dans le **Gestionnaire de configuration SQL Server**, cliquez sur **Services SQL Server**, cliquez avec le bouton droit sur **Lanceur de démon de filtre de texte intégral SQL (**_nom de l’instance_**)**, puis cliquez sur **Propriétés**.  
  
3.  Cliquez sur l’onglet **Ouvrir une session** de la boîte de dialogue, puis sélectionnez ou entrez le nom du compte sous lequel les processus que le service du Lanceur de démon de filtre de texte intégral SQL démarre doivent s’exécuter.  
  
4.  Après avoir fermé la boîte de dialogue, cliquez sur **Redémarrer** pour redémarrer le service du Lanceur de démon de filtre de texte intégral SQL.  
  
![Propriétés des processus du Lanceur de démon de filtre de texte intégral SQL](../../relational-databases/search/media/sql-full-text-filter-daemon-launch-process-properties.png)
  
##  <a name="troubleshoot-the-sql-full-text-filter-daemon-launcher-service-if-it-doesnt-start"></a><a name="error"></a> Dépanner le service du Lanceur de démon de filtre de texte intégral SQL s’il ne démarre pas  
 Si le service du Lanceur de démon de filtre de texte intégral SQL ne démarre pas, passez en revue les causes possibles suivantes :  
  
### <a name="permissions-issues"></a>Problèmes d’autorisations
-   Le groupe de services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas autorisé à démarrer le service du Lanceur de démon de filtre de texte intégral SQL.  

     Vérifiez que le groupe de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispose d’autorisations sur le compte du service du Lanceur de démon de filtre de texte intégral SQL. Pendant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le groupe de services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se voit accorder une autorisation par défaut de gérer, d'interroger et de démarrer le service du Lanceur de démon de filtre de texte intégral SQL. Si les autorisations de groupe de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affectées au compte de service du Lanceur de démon de filtre de texte intégral SQL ont été supprimées après l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le service du Lanceur de démon de filtre de texte intégral SQL ne démarre pas et la recherche en texte intégral est désactivée.     

-   Le compte utilisé pour se connecter au service ne dispose pas de privilèges.  
  
     Il se peut que vous utilisiez un compte qui ne dispose pas de privilèges de connexion sur l’ordinateur où l’instance de serveur est installée. Vérifiez que vous vous connectez avec un compte qui dispose de droits et d'autorisations d'utilisateur sur l'ordinateur local.  

### <a name="service-account-and-password-issues"></a>Problèmes liés au compte de service et au mot de passe
-   Le compte ou le mot de passe d'utilisateur du compte de service est incorrect.  
  
     Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]2016, assurez-vous que le service utilise le compte de service et le mot de passe corrects.  
  
-   Le mot de passe associé au compte du service du Lanceur de démon de filtre de texte intégral SQL a expiré.  
  
     Si vous utilisez un compte d’utilisateur local pour le service du Lanceur de démon de filtre de texte intégral SQL et que le mot de passe expire, vous devez effectuer les actions suivantes :  
  
    1.  Définissez un nouveau mot de passe Windows pour le compte.  
  
    2.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016, mettez à jour le service du Lanceur de démon de filtre de texte intégral SQL pour qu’il utilise le nouveau mot de passe.  
  
### <a name="named-pipes-configuration-issues"></a>Problèmes de configuration des canaux nommés
-   Le service du Lanceur de démon de filtre de texte intégral SQL n'est pas configuré correctement.  
  
     Si la fonctionnalité des canaux nommés a été désactivée sur l'ordinateur local ou si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été configuré pour utiliser un autre canal nommé que celui défini par défaut, le service du Lanceur de démon de filtre de texte intégral SQL peut ne pas démarrer.  
  
-   Une autre instance du même canal nommé est déjà en cours d'exécution.  
  
     Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fait office de serveur de canaux nommés pour le client du service du Lanceur de démon de filtre de texte intégral SQL. Si le canal nommé a déjà été créé par un autre processus préalablement au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , une erreur est consignée dans le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et dans le journal des événements Windows, et la recherche en texte intégral n'est pas disponible.  Identifiez le processus ou l'application qui tente d'utiliser le même canal nommé et arrêtez l'application.  
  
## <a name="see-also"></a>Voir aussi  
 [Rubriques de procédures concernant la gestion des services &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-connect-to-another-computer.md)   
 [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md)  
  
