---
title: Configurer un serveur de rapports sur un cluster avec équilibrage de la charge réseau | Microsoft Docs
description: Découvrez comment configurer le scale-out d’un serveur de rapports pour qu’il s’exécute sur un équilibrage de la charge réseau. Implémentez une solution de cluster avec équilibrage de la charge réseau pour prendre en charge un déploiement scale-out Reporting Services.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.date: 03/09/2021
ms.openlocfilehash: 8d56f26c6661587cb13ff2962221a4a41c7da2ba
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622675"
---
# <a name="configure-a-report-server-on-a-network-load-balancing-cluster"></a>Configurer un serveur de rapports sur un cluster avec équilibrage de la charge réseau

  Si vous configurez la montée en puissance parallèle du serveur de rapports en vue de son exécution sur un cluster avec équilibrage de la charge réseau (NLB, Network Load Balancing), vous devez effectuer les opérations suivantes :  
  
- S'assurer que le cluster avec équilibrage de la charge réseau est accessible via un nom de serveur virtuel mappé avec l'adresse IP du serveur virtuel. Un nom de serveur virtuel est nécessaire pour configurer un point d'entrée unique au cluster avec équilibrage de la charge réseau. Lorsque vous configurerez une URL pour chaque instance du serveur de rapports, vous spécifierez le nom du serveur virtuel en tant qu'hôte.  
  
- Configurez la validation de l'état d'affichage pour prendre en charge la consultation des rapports interactifs. Les rapports interactifs sont en général restitués à plusieurs reprises au cours d'une session mono-utilisateur afin de permettre la visualisation de données nouvelles ou différentes en réponse aux actions de l'utilisateur. Configurer la validation de l'état d'affichage assure la continuité au cours de la session utilisateur quel que soit le serveur de rapports qui répond à la demande réelle.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne fournit pas de fonctionnalités pour l’équilibrage de charge d’un déploiement avec montée en puissance parallèle ni pour la définition d’un point unique d’accès via une URL partagée. Vous devez implémenter une solution logicielle ou matérielle distincte de cluster avec équilibrage de la charge réseau afin de prendre en charge un déploiement avec montée en puissance parallèle de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Vous pouvez installer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur les nœuds qui font déjà partie d'un cluster avec équilibrage de la charge réseau ou configurer d'abord un déploiement avec montée en puissance parallèle, puis installer le logiciel de cluster.  
  
## <a name="steps-for-report-server-deployment-on-an-nlb-cluster"></a>Étapes pour le déploiement du serveur de rapports sur un cluster avec équilibrage de la charge réseau

 Appuyez-vous sur les indications suivantes pour installer et configurer votre déploiement :  
  
|Étape|Description|Plus d’informations|  
|----------|-----------------|----------------------|  
|1|Avant d'installer Reporting Services sur les nœuds de serveurs d'un cluster avec équilibrage de la charge réseau, vérifiez les spécifications du déploiement avec montée en puissance parallèle.|[Configurer un déploiement scale-out de serveurs de rapports en mode natif](../install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)|  
|2|Configurez le cluster avec équilibrage de la charge réseau et vérifiez son bon fonctionnement.<br /><br /> Veillez à mapper un nom d'en-tête de l'hôte à l'adresse IP du serveur virtuel du cluster avec équilibrage de la charge réseau. Le nom d'en-tête de l'hôte est utilisé dans l'URL du serveur de rapports et présente l'avantage d'être plus facile à retenir et à taper qu'une adresse IP.|Pour plus d'informations, consultez la documentation Windows Server correspondant à la version du système d'exploitation Windows que vous exécutez.|  
|3|Ajoutez le nom NetBIOS et de domaine complet pour l’en-tête de l’hôte dans la liste des **BackConnectionHostNames** stockés dans le Registre Windows.<br /><br /> Par exemple, si le nom d’en-tête de l’hôte \<MyServer> est un nom virtuel pour le nom d’ordinateur Windows « contoso », vous pouvez probablement référencer le formulaire de nom de domaine complet sous la forme « contoso.domain.com ». Vous devrez ajouter le nom d’en-tête de l’hôte (MyServer) et le nom de domaine complet (contoso.domain.com) à la liste dans **BackConnectionHostNames**.  <br /><br /> Ensuite, redémarrez l'ordinateur pour vous assurer que les modifications prennent effet.|Cette étape est requise si votre environnement serveur implique l'authentification NTLM sur l'ordinateur local, créant une connexion de retour de boucle.<br /><br /> Si tel est le cas, les demandes entre le gestionnaire de rapports et le serveur de rapports vont échouer avec l'erreur 401 (Non autorisé).|  
|4|Installer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode fichiers uniquement sur les nœuds qui font déjà partie d'un cluster avec équilibrage de la charge réseau et configurer les instances du serveur de rapports pour un déploiement scale-out.<br /><br /> La montée en puissance parallèle que vous configurez ne répond pas aux demandes adressées à l'IP du serveur virtuel. La configuration de la montée en puissance parallèle pour l'utilisation de l'IP du serveur virtuel intervient à un stade ultérieur, après la configuration de la validation de l'état d'affichage.|[Configurer un déploiement par scale-out de serveurs de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)|  
|5|Configurez la validation de l'état d'affichage.<br /><br /> Pour de meilleurs résultats, effectuez cette étape après avoir configuré le déploiement avec montée en puissance parallèle et avant de configurer les instances du serveur de rapports pour l'utilisation de l'IP du serveur virtuel. En configurant d'abord la validation de l'état d'affichage, vous pouvez éviter les exceptions sur l'échec de la validation de l'état quand les utilisateurs tentent d'accéder aux rapports interactifs.|[Comment configurer la validation de l’état d’affichage](#ViewState) dans cette rubrique.|  
|6|Configurer **Hostname** et **UrlRoot** pour utiliser l’adresse IP de serveur virtuel du cluster avec équilibrage de la charge réseau.|[Comment configurer Hostname et UrlRoot](#SpecifyingVirtualServerName) dans cette rubrique.|  
|7|Vérifiez que les serveurs sont accessibles via le nom d'hôte que vous avez spécifié.|[Vérifier l’accès au serveur de rapports](#Verify) dans cette rubrique.|  
  
## <a name="how-to-configure-view-state-validation"></a><a name="ViewState"></a> Comment configurer la validation de l’état d’affichage

::: moniker range="=sql-server-2016"
Pour exécuter un déploiement avec montée en puissance parallèle sur un cluster avec équilibrage de la charge réseau, vous devez configurer la validation de l'état d'affichage afin que les utilisateurs puissent consulter les rapports HTML interactifs.  Vous devez procéder ainsi pour le service Web Report Server.
::: moniker-end

::: moniker range=">=sql-server-2017"
Pour exécuter un déploiement avec montée en puissance parallèle sur un cluster avec équilibrage de la charge réseau, vous devez configurer la validation de l'état d'affichage afin que les utilisateurs puissent consulter les rapports HTML interactifs.
::: moniker-end
  
 La validation de l'état d'affichage est contrôlée par ASP.NET. Par défaut, la validation de l'état d'affichage est activée et utilise l'identité du service Web pour effectuer la validation. Toutefois, dans un scénario de cluster avec équilibrage de la charge réseau, il existe plusieurs instances de services et plusieurs identités de services Web qui s'exécutent sur différents ordinateurs. L'identité de service étant différente pour chaque nœud, vous ne pouvez pas vous appuyer sur une seule identité de processus pour effectuer la validation.  
  
 Pour contourner ce problème, vous pouvez générer une clé de validation arbitraire pour prendre en charge la validation de l'état d'affichage, puis configurer manuellement chaque nœud de serveur de rapports de manière à ce qu'il utilise la même clé. Vous pouvez utiliser n'importe quelle séquence hexadécimale générée de façon aléatoire. L'algorithme de validation (tel que SHA1) détermine la longueur que doit avoir la séquence hexadécimale.  

**[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)]** [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

1. Générez une clé de validation et clé de déchiffrement en utilisant les fonctionnalités de création automatique fourni par le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. À la fin, vous devez avoir une entrée <`machineKey`> unique que vous pouvez coller dans le fichier Web.config pour chaque instance du serveur de rapports dans le déploiement scale-out.  
  
    L'exemple suivant illustre la valeur que vous devez obtenir. Ne copiez pas cet exemple dans vos fichiers de configuration ; les valeurs de clé ne sont pas valides.  
  
    ```xml
    <machineKey ValidationKey="123455555" DecryptionKey="678999999" Validation="SHA1" Decryption="AES"/>  
    ```  
  
2. Ouvrez le fichier Web.config pour Reportserver, puis, dans la section <`system.web`>, collez l'élément <`machineKey`> que vous avez généré. Par défaut, le fichier Web.config se trouve dans \Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\Reportserver\Web.config.  
  
3. Enregistrez le fichier .  
  
4. Répétez l'étape précédente pour chaque serveur de rapports impliqué dans le déploiement avec montée en puissance parallèle.  
  
5. Vérifiez que les fichiers Web.Config de tous les serveurs de rapports concernés par le déploiement avec scale-out contiennent tous des éléments <`machineKey`> identiques dans la section <`system.web`>.  

**[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)]** [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]


1. Générez une clé de validation et clé de déchiffrement en utilisant les fonctionnalités de création automatique fourni par le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. À la fin, vous devez avoir une seule entrée \<**MachineKey**> que vous pouvez coller dans le fichier RSReportServer.config pour chaque instance du serveur de rapports dans le déploiement scale-out.

    L'exemple suivant illustre la valeur que vous devez obtenir. Ne copiez pas cet exemple dans vos fichiers de configuration ; les valeurs de clé ne sont pas valides. La casse correcte doit être utilisée pour le serveur de rapports.

    ```xml
    <MachineKey ValidationKey="123455555" DecryptionKey="678999999" Validation="SHA1" Decryption="AES"/>
    ```

2. Ouvrez le fichier RSReportServer.config pour Reportserver puis, dans la section <`Configuration`>, collez l’élément \<**MachineKey**> que vous avez généré. Par défaut, le fichier RSReportServer.config se trouve dans \Program Files\Microsoft SQL Server Reporting Services\SSRS\ReportServer\RSReportServer.config pour Reporting Services et \Program Files\Microsoft Power BI Report Server\PBIRS\ReportServer\RSReportServer.config pour Power BI Report Server.  

3. Enregistrez le fichier .

4. Répétez l'étape précédente pour chaque serveur de rapports impliqué dans le déploiement avec montée en puissance parallèle.  

5. Vérifiez que les fichiers RSReportServer.Config de tous les serveurs de rapports concernés par le déploiement avec scale-out contiennent tous des éléments \<**MachineKey**> identiques dans la section <`Configuration`>.

## <a name="how-to-configure-hostname-and-urlroot"></a><a name="SpecifyingVirtualServerName"></a> Comment configurer Hostname et UrlRoot

 Pour configurer un déploiement avec montée en puissance parallèle du serveur de rapports sur un cluster avec équilibrage de la charge réseau, vous devez définir un seul nom de serveur virtuel qui fournit un point d'accès unique au cluster de serveurs. Puis, inscrivez le nom du serveur virtuel auprès du serveur de noms de domaine de votre environnement.  
  
 Après avoir défini le nom du serveur virtuel, vous pouvez configurer les propriétés **Hostname** et **UrlRoot** du fichier RSReportServer.config de façon à inclure le nom du serveur virtuel dans l’URL du serveur de rapports.  
  
 Configurez la propriété **Hostname** quand vous utilisez des réservations d’URL spécifiant des caractères génériques dans votre environnement de création de rapports. Quand vous spécifiez la propriété **Hostname** comme nom de serveur virtuel du serveur avec équilibrage de la charge réseau, le trafic réseau de l’environnement de création de rapports est dirigé vers le serveur avec équilibrage de la charge réseau. Le serveur NLB distribue ensuite les demandes parmi les nœuds du serveur de rapports.  
  
 En outre, configurez la propriété **UrlRoot** afin que les liens du rapport fonctionnent dans les rapports exportés vers des rapports statiques, tels que ceux au format Excel ou PDF, ou dans les rapports créés par abonnements, tels que les abonnements de messagerie électronique.  
  
 Si vous intégrez [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] 3.0 ou [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007, ou que vous hébergez vos rapports dans une application web personnalisée, il se peut que vous ayez uniquement besoin de configurer la propriété **UrlRoot** . Dans ce cas, configurez la propriété **UrlRoot** comme URL du site SharePoint ou de l’application web. Le trafic réseau de l'environnement de création de rapports est alors dirigé vers l'application qui gère les rapports, et non vers le serveur de rapports ou le cluster avec équilibrage de la charge réseau (NLB, Network Load Balancing).  
  
 Ne modifiez pas **ReportServerUrl**. Si vous modifiez cette URL, un aller-retour supplémentaire au serveur virtuel sera nécessaire pour chaque demande interne à gérer. Pour plus d’informations, consultez [URL des fichiers de configuration &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md). Pour plus d’informations sur la modification du fichier de configuration, consultez [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
1. Ouvrez RSReportServer.config dans un éditeur de texte.  
  
2. Recherchez la section **\<Service>** et ajoutez les informations suivantes au fichier de configuration, en remplaçant la valeur **Hostname** par le nom de serveur virtuel de votre serveur NLB :  
  
    ```xml
    <Hostname>virtual_server</Hostname>  
    ```  
  
3. Recherchez **UrlRoot**. L’élément n’est pas spécifié dans le fichier de configuration, mais la valeur par défaut utilisée est une URL au format https:// ou `https://<computername>/<reportserver>`, où \<*reportserver*> est le nom du répertoire virtuel du service web Report Server.  
  
4. Tapez une valeur pour **UrlRoot** qui inclut le nom virtuel du cluster au format https:// ou `https://<virtual_server>/<reportserver>`.  
  
5. Enregistrez le fichier .  
  
6. Répétez ces étapes dans chaque fichier RSReportServer.config de chaque serveur de rapports impliqué dans le déploiement par montée en puissance parallèle.  
  
## <a name="verify-report-server-access"></a><a name="Verify"></a> Vérifier l’accès au serveur de rapports

 Vérifiez que vous avez accès au déploiement avec montée en puissance parallèle par le biais du nom du serveur virtuel (par exemple, `https://MyVirtualServerName/reportserver` et `https://MyVirtualServerName/reports`).  
  
 Vous pouvez vérifier quel nœud traite effectivement les rapports en examinant les fichiers journaux du serveur de rapports ou en vérifiant le journal d’exécution Reporting Services (la table du journal d’exécution contient une colonne **InstanceName** qui indique quelle instance a traité une demande particulière). Pour plus d’informations, consultez [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).  
  
 Si vous ne pouvez pas vous connecter au serveur de rapports, vérifiez l'équilibrage de la charge réseau pour contrôler que les demandes sont envoyées au serveur de rapports et affichez le journal HTTP du serveur de rapports pour être certain que le serveur reçoit les demandes.  
  
### <a name="troubleshooting-failed-requests"></a>Dépannage des demandes ayant échoué

 Si les demandes n'atteignent pas les instances du serveur de rapports, consultez le fichier RSReportServer.config pour vérifier que le nom du serveur virtuel est spécifié comme nom d'hôte pour les URL du serveur de rapports :  
  
1. Ouvrez le fichier RSReportServer.config dans un éditeur de texte.  
  
2. Recherchez \<**Hostname**>, \<**ReportServerUrl**> et \<**UrlRoot**>, et vérifiez le nom d’hôte pour chaque paramètre. Si la valeur n'est pas le nom d'hôte attendu, remplacez-le par le nom d'hôte approprié.  
  
 Si vous démarrez l’outil de configuration Reporting Services après avoir effectué ces modifications, l’outil peut remplacer les paramètres \<**ReportServerUrl**> par la valeur par défaut. Conservez toujours une copie de sauvegarde des fichiers de configuration au cas où vous auriez besoin de les remplacer par la version qui contient les paramètres à utiliser.  
  
## <a name="see-also"></a>Voir aussi

 [Configurer une URL &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
 [Configurer un déploiement par scale-out de serveurs de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)   
 [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Gérer un serveur de rapports Reporting Services en mode natif](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)
