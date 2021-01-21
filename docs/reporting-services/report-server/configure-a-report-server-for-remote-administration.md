---
title: Configurer un serveur de rapports pour l’administration à distance | Microsoft Docs
description: Découvrez comment configurer des instances du service Report Server Reporting Services pour une configuration locale ou distante à l’aide de l’outil de configuration ou de l’écriture de code personnalisé.
ms.date: 09/14/2015
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services Configuration tool
- WMI provider [Reporting Services], remote configuration
- configuration management [WMI]
- report servers [Reporting Services], configuring
- remote server administration [Reporting Services]
ms.assetid: 8c7f145f-3ac2-4203-8cd6-2a4694395d09
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7232e6b5d89ae9c73b81df16a03ebcd00859d7ee
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597053"
---
# <a name="configure-a-report-server-for-remote-administration"></a>Configurer un serveur de rapports pour l'administration à distance
  Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vous pouvez configurer des instances de serveur de rapports localement ou à distance. Pour configurer une instance de serveur de rapports à distance, vous pouvez faire appel à l’outil de configuration de Reporting Services ou bien écrire un code personnalisé qui utilise le fournisseur WMI (Windows Management Instrumentation) de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . L'outil de configuration de Reporting Services offre une interface graphique avec le fournisseur WMI pour vous permettre de configurer un serveur de rapports sans avoir à écrire du code. Lorsque vous démarrez l'outil, vous pouvez spécifier un serveur distant auquel vous connecter.  
  
 Avant de pouvoir utiliser l'outil pour configurer un serveur de rapports distant, vous devez suivre les instructions de cette rubrique pour activer les ports du Pare-feu Windows, les connexions à distance et les demandes WMI distantes.  
  
 Une configuration appropriée vous aide à éviter l'erreur suivante :  
  
 `The machine could not be found.`  
  
 `"The RPC server is unavailable. (Exception from HRESULT: 0x800706BA)".`  
  
## <a name="prerequisites"></a>Prérequis  
 Pour cela, vous devez ouvrir une session localement et être membre du groupe Administrateurs local. Vous ne pouvez pas modifier les paramètres du pare-feu Windows d'un ordinateur distant sur une connexion distante.  
  
 Si vous souhaitez activer l'administration à distance pour un utilisateur non-administrateur, vous devez accorder des autorisations d'activation à distance DCOM (Distributed Component Object Model) au compte. Les instructions de configuration du serveur pour un accès non-administrateur sont fournies dans cette rubrique.  
  
 Certaines organisations disposent de stratégies de groupe qui empêchent l'administration serveur à distance pour certains systèmes d'exploitation ou certains utilisateurs. Avant de commencer à modifier les paramètres du pare-feu, vérifiez auprès de votre administrateur réseau si des restrictions s'appliquent dans le cadre de l'administration à distance.  
  
 Pour plus d'informations, consultez [Connecting Through Windows Firewall](/windows/win32/wmisdk/connecting-to-wmi-remotely-with-vbscript) dans la documentation du kit de développement Platform SDK sur MSDN.  
  
## <a name="tasks"></a>Tâches  
 Les tâches qui activent la configuration de serveur de rapports à distance incluent les éléments suivants :  
  
-   Activer les ports du Pare-feu Windows afin d'autoriser les demandes sur les ports utilisés par le serveur de rapports et par l'instance du moteur de base de données SQL Server.  Voir [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md) et [Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
-   Autoriser les connexions distantes à l'instance du moteur de base de données qui héberge la base de données du serveur de rapports. Une connexion distante est nécessaire pour la configuration de la connexion à la base de données du serveur de rapports et la gestion des clés de chiffrement.  
  
-   Activer les demandes WMI distantes pour franchir le Pare-feu [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
-   Si vous configurez un serveur de rapports distant pour l'administration par un utilisateur non-administrateur, vous devez définir les autorisations DCOM pour activer l'accès WMI distant à un compte d'utilisateur Windows standard. Du fait que WMI utilise DCOM en tant que moyen de transport pour les appels distants, vous devez définir les autorisations DCOM afin que les utilisateurs non connectés en tant qu'administrateur local puissent configurer le serveur.  
  
-   Si vous configurez un serveur de rapports distant pour l'administration par un utilisateur non-administrateur, vous devez également définir les autorisations WMI sur l'espace de noms WMI du serveur de rapports. Par défaut, tous les membres du groupe Administrateurs local ont accès à l'espace de noms WMI du serveur de rapports. Pour accorder un droit d'accès aux personnes qui ne sont pas administrateurs, vous devez définir des autorisations.  
  
 Les instructions relatives à l'exécution de ces tâches sont fournies dans cette rubrique.  
  
### <a name="to-configure-remote-connections-to-the-report-server-database"></a>Pour configurer les connexions distantes à la base de données du serveur de rapports  
  
1.  Cliquez sur **Démarrer**, pointez sur **Programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
2.  Dans le volet gauche, développez **Configuration du réseau SQL Server**, puis cliquez sur **Protocoles** pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Dans le volet d’informations, activez les protocoles TCP/IP et Canaux nommés, puis redémarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="to-enable-remote-administration-in-windows-firewall"></a>Pour activer l'administration à distance dans le pare-feu Windows  
  
1.  Ouvrez une session en tant qu'administrateur local sur l'ordinateur où vous souhaitez activer l'administration à distance.  
  
2.  Ouvrez une invite de commandes avec les privilèges d’administrateur.  
  
3.  Exécutez la commande suivante :  
  
    ```  
    netsh.exe firewall set service type=REMOTEADMIN mode=ENABLE scope=ALL  
    ```  
  
     Vous pouvez spécifier différentes options pour l'étendue. Pour plus d'informations, consultez la documentation produit du pare-feu Windows.  
  
4.  Vérifiez que l'administration à distance est activée. Vous pouvez exécuter la commande suivante pour afficher l'état :  
  
    ```  
    netsh.exe firewall show state  
    ```  
  
5.  Redémarrez l'ordinateur.  
  
### <a name="to-set-dcom-permissions-to-enable-remote-wmi-access-for-non-administrators"></a>Pour définir les autorisations DCOM en vue d'activer l'accès WMI à distance pour les non-administrateurs  
  
1.  Dans le menu Démarrer, pointez sur **Outils d'administration**, puis cliquez sur **Services de composants**.  
  
     Pour Windows Vista, dans le menu Démarrer, cliquez sur **Tous les programmes**, cliquez sur **Exécuter**, puis entrez **mmc comexp.msc**.  
  
2.  Ouvrez le dossier Services de composants.  
  
3.  Ouvrez le dossier Ordinateurs.  
  
4.  Sélectionnez Poste de travail.  
  
5.  Dans le menu **Action** , sélectionnez **Propriétés**.  
  
6.  Cliquez sur **Sécurité COM**.  
  
7.  Sous **Autorisations d'exécution et d'activation**, cliquez sur **Modifier les limites**.  
  
8.  Si votre nom n'apparaît pas dans la boîte de dialogue **Autorisation d'exécution**, cliquez sur **Ajouter**.  
  
9. Tapez le nom de votre compte d'utilisateur, puis cliquez sur **OK**.  
  
10. Dans la zone **Autorisation pour \<User or Group>** , dans la colonne **Autoriser**, sélectionnez **Exécution à distance** et **Activation à distance**, puis cliquez sur **OK**.  
  
### <a name="to-set-permissions-on-the-report-server-wmi-namespace-for-non-administrators"></a>Pour définir les autorisations sur l'espace de noms WMI du rapport de serveurs pour les non-administrateurs  
  
1.  Dans le menu Démarrer, pointez sur **Outils d'administration**, puis cliquez sur **Gestion de l'ordinateur**.  
  
2.  Ouvrez le dossier Services et applications.  
  
3.  Cliquez avec le bouton droit sur **Contrôle WMI**, puis sélectionnez **Propriétés**.  
  
4.  Cliquez sur **Sécurité**.  
  
5.  Ouvrez le dossier Root.  
  
6.  Ouvrez le dossier Microsoft.  
  
7.  Ouvrez le dossier SQLServer.  
  
8.  Ouvrez le dossier ReportServer.  
  
9. Ouvrez le dossier de l'instance. Si vous avez installé l'instance par défaut, le dossier est MSSQLSERVER.  
  
10. Ouvrez le dossier v10.  
  
11. Sélectionnez le dossier Admin, puis cliquez sur **Sécurité**.  
  
12. Cliquez sur **Ajouter**, puis tapez le compte d'utilisateur que vous utiliserez pour gérer le serveur.  
  
13. Dans la colonne **Autoriser** , sélectionnez **Activer le compte**, **Appel à distance autorisé** et **Sécurité de lecture**, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
  
