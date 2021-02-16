---
title: Choisir un mode d’authentification | Microsoft Docs
description: Choisissez entre le mode d’authentification Windows et l’authentification en mode mixte pour le Moteur de base de données SQL Server au moment de l’installation.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.ins.instwizard.authenticationmode.f1
- sql13.swb.passwordexpired.f1
helpviewer_keywords:
- sa account
- authentication modes
- trusted connection
- SQL Server Installation Wizard, Authentication Mode page
- choose authentication mode
- authentication [SQL Server], choosing a mode
- Windows authentication [SQL Server]
- mixed mode authentication
- mixed authentication mode
- SQL authentication mode
- Password Expired dialog box
ms.assetid: ff7a6a48-3d38-4209-aa0f-7d6c0a8c64ef
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ebcf99ad025477a562f2a050eb1c2c3f3f63e12f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344406"
---
# <a name="choose-an-authentication-mode"></a>Choisir un mode d'authentification
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Pendant l’installation, vous devez sélectionner un mode d’authentification pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Il existe deux modes possibles : Mode d'authentification Windows et mode mixte. Le mode d’authentification Windows active l’authentification Windows et désactive l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le mode mixte active à la fois l’authentification Windows et l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'authentification Windows est toujours disponible et ne peut pas être désactivée.  
  
## <a name="configuring-the-authentication-mode"></a>Configuration du mode d'authentification  
 Si vous sélectionnez le mode d'authentification mixte au cours de l'installation, vous devez fournir, puis confirmer, un mot de passe fort pour le compte d'administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégré appelé sa. Le compte sa se connecte à l'aide de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Si vous sélectionnez l'authentification Windows au cours de l'installation, le programme d'installation crée le compte sa pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mais celui-ci est désactivé. Si vous passez ultérieurement en mode d'authentification mixte et que vous souhaitez utiliser le compte sa, vous devez d'abord l'activer. Tout compte Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être configuré en tant qu'administrateur système. Étant donné que le compte sa est connu et qu'il est souvent la cible d'utilisateurs malveillants, ne l'activez pas à moins que votre application n'en ait besoin. N'attribuez jamais de mot de passe vide ou faible au compte sa. Pour passer du mode d’authentification Windows au mode d’authentification mixte et utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Modifier le mode d’authentification du serveur](../../database-engine/configure-windows/change-server-authentication-mode.md).  
  
## <a name="connecting-through-windows-authentication"></a>Connexion via l'authentification Windows  
 Quand un utilisateur se connecte par le biais d'un compte d'utilisateur Windows, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide le nom et le mot de passe du compte à l'aide du jeton du principal Windows du système d'exploitation. Autrement dit, l'identité de l'utilisateur est confirmée par Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne demande pas le mot de passe et n’effectue pas de validation d’identité. L’authentification Windows est le mode d’authentification par défaut et offre une sécurité très supérieure à l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L’authentification Windows utilise le protocole de sécurité Kerberos, met en œuvre des stratégies de mot de passe portant sur la validation de la complexité des mots de passe forts et prend en charge le verrouillage des comptes et l’expiration des mots de passe. Une connexion établie à l'aide de l'authentification Windows est parfois appelée une connexion approuvée car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] approuve les informations d'identification fournies par Windows.  
  
 En utilisant l'authentification Windows, vous pouvez créer des groupes Windows au niveau du domaine, et une connexion sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le groupe entier. La gestion de l'accès au niveau du domaine peut simplifier l'administration du compte.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="connecting-through-sql-server-authentication"></a>Connexion via l'authentification SQL Server  
 Lors de l’utilisation de l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les connexions créées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont pas basées sur les comptes d’utilisateur Windows. Le nom d’utilisateur et le mot de passe sont créés à l’aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et stockés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les utilisateurs qui recourent à l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent fournir leurs informations d’identification (nom de connexion et mot de passe) chaque fois qu’ils se connectent. Lorsque vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez définir des mots de passe forts pour tous les comptes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour obtenir des conseils sur les mots de passe forts, consultez [Mots de passe forts](../../relational-databases/security/strong-passwords.md).  
  
 Trois stratégies de mot de passe facultatives sont disponibles pour les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   L'utilisateur doit changer de mot de passe à la prochaine connexion  
  
     Oblige l'utilisateur à changer de mot de passe la prochaine fois qu'il se connecte. La possibilité de changer de mot de passe est fournie par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Les développeurs de logiciels tiers doivent fournir cette fonctionnalité si cette option est utilisée.  
  
-   Conserver l'expiration du mot de passe  
  
     La stratégie relative à la durée de vie maximale des mots de passe de l'ordinateur est appliquée aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Conserver la stratégie de mot de passe  
  
     Les stratégies de mot de passe Windows de l'ordinateur sont appliquées aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Elles prennent notamment en compte la longueur et la complexité des mots de passe. Cette fonctionnalité dépend de l'API `NetValidatePasswordPolicy` disponible uniquement dans [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] et versions ultérieures.  
  
#### <a name="to-determine-the-password-policies-of-the-local-computer"></a>Pour déterminer les stratégies de mot de passe de l'ordinateur local  
  
1.  Dans le menu **Démarrer** , cliquez sur **Exécuter**.  
  
2.  Dans la boîte de dialogue **Exécuter** , tapez **secpol.msc**, puis cliquez sur **OK**.  
  
3.  Dans l’application **Paramètres de sécurité locale** , développez **Paramètres de sécurité**, **Stratégies de comptes**, puis cliquez sur **Stratégie de mot de passe**.  

     Les stratégies de mot de passe sont décrites dans le volet de résultats.  
  
### <a name="disadvantages-of-sql-server-authentication"></a>Inconvénients de l'authentification SQL Server  
  
-   Si un utilisateur est un utilisateur de domaine Windows qui dispose d’une connexion et d’un mot de passe pour Windows, il doit toujours fournir un autre nom de connexion et un autre mot de passe ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) pour se connecter. Pour de nombreux utilisateurs, il est difficile de gérer plusieurs noms et plusieurs mots de passe. Devoir fournir des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à chaque connexion à la base de données peut s’avérer fastidieux.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] L’authentification ne peut pas utiliser le protocole de sécurité Kerberos.  
  
-   Windows offre des stratégies de mot de passe supplémentaires qui ne sont pas disponibles pour les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Le mot de passe de connexion chiffré de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être passé sur le réseau au moment de la connexion. Certaines applications qui se connectent automatiquement conservent le mot de passe au niveau du client. Il s'agit de points d'attaque supplémentaires.  
  
### <a name="advantages-of-sql-server-authentication"></a>Avantages de l'authentification SQL Server  
  
-   Permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de prendre en charge des applications anciennes ou tierces qui nécessitent l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de prendre en charge des environnements contenant des systèmes d'exploitation mixtes, où tous les utilisateurs ne sont pas authentifiés par un domaine Windows.  
  
-   Permet aux utilisateurs de se connecter à partir de domaines inconnus ou non approuvés. Par exemple, une application où les clients établis se connectent avec des noms de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assignés pour recevoir l'état de leur commande.  
  
-   Permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de prendre en charge des applications Web où les utilisateurs créent leur propre identité.  
  
-   Permet aux développeurs de logiciels de distribuer leurs applications en utilisant une hiérarchie d'autorisations complexe, basée sur des connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connues et prédéfinies.  
  
    > [!NOTE]  
    >  L'utilisation de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne limite pas les autorisations des administrateurs locaux sur l'ordinateur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé.  
  
## <a name="see-also"></a>Voir aussi  
 [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
  
