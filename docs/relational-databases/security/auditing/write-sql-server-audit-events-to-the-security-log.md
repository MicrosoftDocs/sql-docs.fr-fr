---
title: Écrire des événements d’audit SQL Server dans le journal de sécurité | Microsoft Docs
description: Apprenez comment écrire des événements d'audit SQL Server dans le journal de sécurité Windows. Découvrez les limitations et les restrictions liées à l’utilisation de ce journal.
ms.custom: ''
ms.date: 09/21/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], Security Log
- server audit [SQL Server]
- audits [SQL Server], writing to Security Log
- security logs [SQL Server]
ms.assetid: 6fabeea3-7a42-4769-a0f3-7e04daada314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abdd25c6166b40436584a1260d4b1b09d40900c4
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186056"
---
# <a name="write-sql-server-audit-events-to-the-security-log"></a>Écrire des événements d'audit SQL Server dans le journal de sécurité  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Dans un environnement extrêmement sécurisé, le journal de sécurité Windows est l'emplacement approprié pour consigner des événements d'accès aux objets. D'autres emplacements d'audit sont pris en charge mais ils sont plus exposés au risque de falsification.  
  
 L’écriture des audits du serveur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le journal de sécurité Windows est soumise à trois conditions clés :  
  
-   Le paramètre Auditer l'accès aux objets doit être configuré pour capturer les événements. L’outil de stratégie d’audit (`auditpol.exe`) expose différents paramètres de sous-stratégies dans la catégorie **Auditer l’accès aux objets** . Pour permettre à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d'auditer l'accès aux objets, configurez le paramètre **généré par une application** .  
-   Le compte sous lequel le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est exécuté doit posséder l’autorisation **Générer des audits de sécurité** pour écrire dans le journal de sécurité Windows. Par défaut, les comptes Service local et Service réseau disposent de cette autorisation. Cette étape n'est pas requise si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute sous l'un de ces comptes.  
-   Accordez une autorisation totale au compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur la ruche du Registre `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security`.  

  > [!IMPORTANT]  
  > [!INCLUDE[ssnoteregistry-md](../../../includes/ssnoteregistry-md.md)]   
  
La stratégie d'audit Windows peut affecter l'audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'il est configuré pour écrire dans le journal de sécurité Windows, avec la possibilité de perdre des événements si la stratégie d'audit est configurée incorrectement. En général, le journal de sécurité Windows est configuré pour remplacer les événements les plus anciens. Les événements les plus récents sont ainsi préservés. Toutefois, si le journal de sécurité Windows n'est pas configuré pour remplacer les événements les plus anciens, et si le journal de sécurité est plein, le système publie alors l'événement Windows 1104 (le journal est plein). À ce stade :  
-   Plus aucun événement de sécurité supplémentaire n'est consigné  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne sera pas en mesure de détecter que le système n'est pas capable d'enregistrer les événements dans le journal de sécurité, provoquant ainsi la perte potentielle d'événements d'audit  
-   Une fois le journal de sécurité reconfiguré par l'administrateur, le comportement de consignation retourne à la normale.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Les administrateurs de l'ordinateur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doivent savoir que les paramètres locaux du journal de sécurité peuvent être remplacés par une stratégie de domaine. Dans ce cas, la stratégie de domaine peut remplacer le paramètre de sous-catégorie (**auditpol /get /subcategory:"généré par application"** ). Cela peut affecter la capacité de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à consigner des événements sans avoir aucun moyen de détecter que les événements que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] essaie d'auditer ne vont pas être consignés.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Vous devez être un administrateur Windows pour configurer ces paramètres.  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-auditpol"></a><a name="auditpolAccess"></a> Pour configurer le paramètre Auditer l'accès aux objets dans Windows à l'aide de l'outil auditpol  
  
1.  Ouvrez une invite de commandes avec des autorisations d'administrateur.  
  
    1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur **Accessoires**, cliquez avec le bouton droit sur **Invite de commandes**, puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    2.  Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'ouvre, cliquez sur **Continuer**.  
  
2.  Exécutez l'instruction suivante pour activer l'audit à partir de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    ```  
    auditpol /set /subcategory:"application generated" /success:enable /failure:enable  
    ```  
  
3.  Fermez la fenêtre d'invite de commandes.  
  
##  <a name="to-grant-the-generate-security-audits-permission-to-an-account-using-secpol"></a><a name="secpolAccess"></a> Pour octroyer l'autorisation Générer des audits de sécurité à un compte à l'aide de l'outil secpol  
  
1.  Sur un système d'exploitation Windows, dans le menu **Démarrer** , cliquez sur **Exécuter**.  
  
2.  Tapez **secpol.msc** , puis cliquez sur **OK**. Si la boîte de dialogue **Contrôle d'accès d'utilisateur** s'affiche, cliquez sur **Continuer**.  
  
3.  Dans l'outil Stratégie de sécurité locale, développez **Paramètres de sécurité**, **Stratégies locales**, puis cliquez sur **Attribution des droits utilisateur**.  
  
4.  Dans le volet de résultats, double-cliquez sur **Générer des audits de sécurité**.  
  
5.  Sous l'onglet **Paramètre de sécurité locale** , cliquez sur **Ajouter un utilisateur ou un groupe**.  
  
6.  Dans la boîte de dialogue **Sélectionnez les utilisateurs, les ordinateurs ou les groupes** , tapez le nom du compte d’utilisateur, tel que **domaine1\utilisateur1** , puis cliquez sur **OK**, ou cliquez sur **Avancé** et recherchez le compte.  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
8.  Fermez l'outil Stratégie de sécurité locale.  
  
9. Redémarrez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour activer ce paramètre.  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-secpol"></a><a name="secpolPermission"></a> Pour configurer le paramètre Auditer l'accès aux objets dans Windows à l'aide de l'outil secpol  
  
1.  Si la version du système d'exploitation est antérieure à [!INCLUDE[wiprlhext](../../../includes/wiprlhext-md.md)] ou Windows Server 2008, dans le menu **Démarrer** , cliquez sur **Exécuter**.  
  
2.  Tapez **secpol.msc** , puis cliquez sur **OK**. Si la boîte de dialogue **Contrôle d'accès d'utilisateur** s'affiche, cliquez sur **Continuer**.  
  
3.  Dans l'outil Stratégie de sécurité locale, développez **Paramètres de sécurité**, **Stratégies locales**, puis cliquez sur **Stratégie d'audit**.  
  
4.  Dans le volet de résultats, double-cliquez sur **Auditer l’accès aux objets**.  
  
5.  Sous l'onglet **Paramètre de sécurité locale** , dans la zone **Auditer les tentatives des types suivants** , sélectionnez **Succès** et **Échec**.  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
7.  Fermez l'outil Stratégie de sécurité locale.  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Audit &#40moteur de base de données&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md)  
  
  
