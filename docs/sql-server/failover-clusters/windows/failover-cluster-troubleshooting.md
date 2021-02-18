---
title: Dépannage de clusters de basculement | Microsoft Docs
description: Découvrez comment résoudre les problèmes relatifs aux clusters de basculement, y compris comment effectuer une récupération après un échec, résoudre des problèmes courants et utiliser des procédures stockées étendues ou d’objets COM.
ms.custom: ''
ms.date: 10/21/2015
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- troublshooting, failover clustering
- failover clustering, troubleshooting
- cluster troubleshooting
ms.assetid: 84012320-5a7b-45b0-8feb-325bf0e21324
author: cawrites
ms.author: chadam
ms.openlocfilehash: 34fe360248235da3c9a647799a178d9c734ad027
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350044"
---
# <a name="failover-cluster-troubleshooting"></a>Dépannage de clusters de basculement
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique aborde les sujets suivants :  
  
-   Étapes de dépannage de base  
  
-   Récupération à partir d'une défaillance de cluster de basculement  
  
-   Résolution des principaux problèmes de clustering de basculement  
  
-   Utilisation de procédures stockées étendues et d'objets COM  
  
## <a name="basic-troubleshooting-steps"></a>Étapes de dépannage de base  
 La première étape de diagnostic consiste à exécuter une vérification de validation de cluster à jour. Pour plus d’informations sur la validation, consultez [Guide pas à pas des clusters de basculement : Validation matérielle d’un cluster de basculement](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)).  Vous pouvez le faire sans interruption de service, car cela n’affecte aucune des ressources de cluster en ligne. La validation peut être exécutée à tout moment une fois que la fonctionnalité Clustering de basculement a été installée, notamment avant le déploiement du cluster, lors de la création du cluster et lors de l’exécution du cluster. En fait, des tests supplémentaires sont exécutés une fois le cluster utilisé, pour vérifier que les bonnes pratiques sont respectées pour les charges de travail à haute disponibilité. Sur ces dizaines de tests, seuls quelques-uns affecteront les charges de travail de cluster en cours d’exécution, et tous concernent la catégorie stockage. Il suffit donc de passer cette catégorie entière pour éviter facilement les tests avec interruption.  
Le clustering de basculement est proposé avec une sécurité intégrée pour empêcher les temps d’arrêt accidentels lors de l’exécution des tests de stockage pendant la validation. Si le cluster comprend des groupes en ligne lorsque la validation est lancée, et que les tests de stockage restent sélectionnés, l’utilisateur est invité à confirmer s'il souhaite exécuter tous les tests (et provoquer des temps d'arrêt) ou ignorer les tests de disques appartenant à des groupes en ligne pour éviter les temps d’arrêt. Si la catégorie de stockage entière est exclue des tests, cette invite ne s’affiche pas. Cela permet d’activer la validation du cluster sans temps d’arrêt.  
  
#### <a name="how-to-revalidate-your-cluster"></a>Comment revalider votre cluster  
  
1.  Dans le composant logiciel enfichable Cluster de basculement de l’arborescence de la console, vérifiez que **Gestion du cluster de basculement** est bien sélectionné, puis, sous **Gestion**, cliquez sur **Valider une configuration**.  
  
2.  Suivez les instructions de l’assistant pour spécifier les serveurs et les tests, puis exécuter ces derniers. La page **Résumé** s'affiche après l'exécution des tests.  
  
3.  Tout en restant sur la **Résumé** , cliquez sur **Afficher le rapport** pour afficher les résultats des tests.  
  
     Pour afficher les résultats des tests après avoir fermé l’assistant, consultez **%SystemRoot%\Cluster\Reports\Validation Report date and time.html** , où **%SystemRoot%** est le dossier dans lequel le système d’exploitation est installé (par exemple, **C:\Windows**).  
  
4.  Pour afficher les rubriques d'aide pour l’interprétation des résultats, cliquez sur **En savoir plus sur les tests de validation de cluster**.  
  
 Pour afficher les rubriques d’aide sur la validation de cluster après avoir fermé l’Assistant, dans le composant logiciel enfichable Cluster de basculement, cliquez sur **Aide**, puis sur **Rubriques d’aide**, allez sous l’onglet **Contenu** , développez le contenu de l’aide relative au cluster de basculement, puis cliquez sur **Validation d’une configuration de cluster de basculement**.  Une fois l'assistant de validation terminé, le **Rapport de résumé** présente les résultats. Tous les tests doivent se terminer avec une encoche verte ou, dans certains cas, un triangle jaune (avertissement). Lorsque vous recherchez des problèmes à résoudre (X rouges ou points d’interrogation jaunes), cliquez sur un test individuel pour consulter ses détails dans la partie du rapport qui résume les résultats du test. Les problèmes marqués par un X rouge doivent être résolus avant de résoudre les problèmes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 **Installation des mises à jour**  
  
 L'installation des mises à jour est importante pour éviter les problèmes sur votre système. Liens utiles :  
  
-   [Correctifs logiciels et mises à jour recommandés pour les clusters de basculement basés sur Windows Server 2012 R2](https://support.microsoft.com/kb/2920151)  
  
-   [Correctifs logiciels et mises à jour recommandés pour les clusters de basculement basés sur Windows Server 2012](https://support.microsoft.com/kb/2784261)  
  
-   [Correctifs logiciels et mises à jour recommandés pour les clusters de basculement basés sur Windows Server 2008 R2](https://support.microsoft.com/kb/980054)  
  
-   [Correctifs logiciels et mises à jour recommandés pour les clusters de basculement basés sur Windows Server 2008](https://support.microsoft.com/kb/957311)  
  
## <a name="recovering-from-failover-cluster-failure"></a>Récupération à partir d'une défaillance de cluster de basculement  
 Généralement, une défaillance de cluster de basculement est due à l'une des deux causes suivantes :  
  
-   Défaillance matérielle dans un nœud d'un cluster composé de deux nœuds. Cette erreur peut être due à une défaillance de la carte SCSI ou du système d'exploitation.  
  
     Pour effectuer une récupération à partir de cette défaillance, supprimez le nœud défaillant du cluster de basculement à l’aide du programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , mettez l’ordinateur hors connexion et résolvez la défaillance matérielle, puis réinstallez l’ordinateur et ajoutez le nœud réparé à l’instance de cluster de basculement.  
  
     Pour plus d’informations, consultez [Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) et [Récupérer à partir d’une défaillance d’instance de cluster de basculement](../../../sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure.md).  
  
-   Échec du système d'exploitation. Dans ce cas, le nœud est hors ligne, mais il n'est pas rompu de manière irrémédiable.  
  
     Pour effectuer une récupération à partir d'une défaillance du système d'exploitation, récupérez le nœud et testez le basculement. Si l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne bascule pas correctement, vous devez utiliser le programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour supprimer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] du cluster de basculement, effectuer les réparations nécessaires, réinstaller l’ordinateur, puis ajouter le nœud réparé à l’instance de cluster de basculement.  
  
     Ce type de récupération à partir d'une défaillance du système d'exploitation peut prendre du temps. Si l'échec du système d'exploitation peut être facilement récupéré, évitez toutefois d'utiliser cette technique.  
  
     Pour plus d’informations, consultez [Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) et [Procédure : récupérer d’une défaillance de cluster de basculement dans le scénario 2](recover-from-failover-cluster-instance-failure.md).  
  
## <a name="resolving-common-problems"></a>Résolution des problèmes courants  
 La liste ci-dessous décrit les problèmes d'utilisation les plus courants et explique comment les résoudre.  
  
### <a name="problem-incorrect-use-of-command-prompt-syntax-to-install-sql-server"></a>Problème : Utilisation incorrecte de la syntaxe d’invite de commandes pour installer SQL Server  
 **Erreur 1** : Il est difficile de diagnostiquer les erreurs du programme d’installation lorsque vous utilisez le commutateur **/qn** à partir de l’invite de commandes, dans la mesure où le commutateur **/qn** supprime toutes les boîtes de dialogue et les messages d’erreur du programme d’installation. Si le commutateur **/qn** est spécifié, tous les messages d’installation, y compris les messages d’erreur, sont écrits dans les fichiers journaux de l’installation. Pour plus d’informations sur les fichiers journaux, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
 **Résolution 1 :** Utilisez le commutateur  **/qb** au lieu du commutateur  **/qn**. Si vous utilisez le commutateur **/qb** , l’interface utilisateur de base s’affichera à chaque étape, ainsi que les messages d’erreur.  
  
### <a name="problem-sql-server-cannot-log-on-to-the-network-after-it-migrates-to-another-node"></a>Problème : SQL Server ne peut pas se connecter au réseau après avoir fait l’objet d’une migration vers un autre nœud  
 **Erreur 1 :** les comptes de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne sont pas en mesure de contacter un contrôleur de domaine.  
  
 **Résolution 1 :** Dans vos journaux d’événements, recherchez des signes indiquant l’existence de problèmes réseau, tels que des défaillances d’adaptateur ou des problèmes affectant le service DNS. Vérifiez que vous pouvez exécuter une commande ping sur le contrôleur de domaine.  
  
 **Erreur 2 : les mots de passe du compte de service** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne sont pas identiques sur tous les nœuds du cluster ou le nœud ne redémarre pas un service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui a fait l’objet d’une migration depuis un nœud défaillant.  
  
 **Résolution 2 :** Changez les mots de passe des comptes de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si vous n'effectuez pas cette opération et que vous modifiez les mots de passe des comptes de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur un nœud, vous devez également modifier les mots de passe sur tous les autres nœuds. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] effectue cette opération automatiquement.  
  
### <a name="problem-sql-server-cannot-access-the-cluster-disks"></a>Problème : SQL Server ne peut pas accéder aux disques de cluster  
 **Problème 1 :** Les microprogrammes ou les pilotes ne sont pas mis à jour sur tous les nœuds.  
  
 **Résolution 1 :** Vérifiez que tous les nœuds utilisent les versions de microprogrammes adéquates et les mêmes versions de pilotes.  
  
 **Erreur 2 :** un nœud ne peut pas récupérer des disques de clusters qui ont fait l'objet d'une migration depuis un nœud ayant échoué sur un disque de clusters partagés utilisant une lettre de lecteur différente.  
  
 **Résolution 2 :** Les lettres de lecteur de disque pour les disques de cluster doivent être identiques sur les deux serveurs. Si ce n'est pas le cas, examinez l'installation d'origine du système d'exploitation et MSCS ( [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Cluster Service).  
  
### <a name="problem-failure-of-a-sql-server-service-causes-failover"></a>Problème : La défaillance d’un service SQL Server provoque un basculement  
 **Solution :** pour empêcher que la défaillance de services spécifiques provoque le basculement du groupe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , configurez ces services à l’aide de l’Administrateur de cluster dans Windows, comme suit :  
  
-   Désactivez la case à cocher **Affecter le groupe** sur l'onglet **Avancé** de la boîte de dialogue **Propriétés de texte intégral** . Cependant, si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provoque un basculement, le service de recherche en texte intégral redémarre.  
  
### <a name="problem-sql-server-does-not-start-automatically"></a>Problème : SQL Server ne démarre pas automatiquement  
 **Résolution :** Utilisez l’Administrateur de cluster dans MSCS pour démarrer un cluster de basculement. Le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être paramétré de manière à démarrer manuellement ; l’Administrateur de cluster doit être configuré dans MSCS de façon à démarrer le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d'informations, consultez [Gestion des services](https://msdn.microsoft.com/library/ms178096\(v=sql.105\).aspx).  
  
### <a name="problem-the-network-name-is-offline-and-you-cannot-connect-to-sql-server-using-tcpip"></a>Problème : Le nom de réseau est hors ligne et vous ne pouvez pas vous connecter à SQL Server avec TCP/IP  
 **Problème 1 :** Le service DNS échoue à cause d’une ressource de cluster définie de façon à exiger ce service.  
  
 **Résolution 1 :** Corrigez les problèmes liés au service DNS.  
  
 **Problème 2 :** Un nom en double existe sur le réseau.  
  
 **Résolution 2 :** Utilisez NBTSTAT pour rechercher le nom en double, puis corrigez le problème.  
  
 **Erreur 3 :** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne se connecte pas à l’aide de canaux nommés.  
  
 **Résolution 3 :** Pour vous connecter à l’aide de canaux nommés, créez un alias avec le Gestionnaire de configuration SQL Server pour vous connecter à l’ordinateur approprié. Par exemple, si vous disposez d’un cluster à deux nœuds (**Node A** et **Node B**) et d’une instance de cluster de basculement (**Virtsql**) avec une instance par défaut, vous pouvez vous connecter au serveur dont la ressource de nom réseau est hors connexion, en procédant comme suit :  
  
1.  Déterminez le nœud sur lequel s'exécute le groupe contenant l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , à l'aide de l'Administrateur de cluster. Dans cet exemple, il s'agit de **Node A**.  
  
2.  Démarrez le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur cet ordinateur à l’aide de **net start**. Pour plus d'informations sur l'utilisation de **net start**, consultez [Démarrage manuel de SQL Server](https://msdn.microsoft.com/library/ms191193\(v=sql.105\).aspx).  
  
3.  Démarrez le Gestionnaire de configuration SQL Server [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur **Node A**. Examinez le nom du canal sur lequel le serveur est à l'écoute. Il doit être similaire à \\\\.\\$$\VIRTSQL\pipe\sql\query.  
  
4.  Sur l'ordinateur client, démarrez le Gestionnaire de configuration SQL Server.  
  
5.  Créez un alias SQLTEST1 pour vous connecter à ce nom de canal via les canaux nommés. Pour cela, entrez **Node A** comme nom de serveur et modifiez le nom du canal de la manière suivante : \\\\.\pipe\\$$\VIRTSQL\sql\query.  
  
6.  Connectez-vous à cette instance à l'aide de l'alias SQLTEST1 comme nom de serveur.  
  
### <a name="problem-sql-server-setup-fails-on-a-cluster-with-error-11001"></a>Problème : L’installation de SQL Server échoue sur un cluster avec l’erreur 11001  
 **Problème :** Une clé de Registre orpheline dans [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.X\Cluster]  
  
 **Résolution :** Vérifiez que la ruche du Registre MSSQL.X n’est pas en cours d’utilisation, puis supprimez la clé de cluster.  
  
### <a name="problem-cluster-setup-error-the-installer-has-insufficient-privileges-to-access-this-directory-drivemicrosoft-sql-server-the-installation-cannot-continue-log-on-as-an-administrator-or-contact-your-system-administrator"></a>Problème : Erreur d’installation de cluster : « Le programme d’installation ne dispose pas des privilèges suffisants pour accéder à ce répertoire : \<drive>\Microsoft SQL Server. Impossible de poursuivre l'installation. Ouvrez une session en tant qu'administrateur ou contactez votre administrateur système. »  
 **Problème :** Cette erreur est due à un lecteur partagé SCSI mal partitionné.  
  
 **Résolution :** Recréez une partition unique sur le disque partagé en effectuant les étapes suivantes :  
  
1.  Supprimez la ressource disque du cluster.  
  
2.  Supprimez toutes les partitions du disque.  
  
3.  Dans les propriétés du disque, vérifiez que celui-ci est un disque de base.  
  
4.  Créez une partition sur le disque partagé, formatez-le, puis affectez-lui une lettre de lecteur.  
  
5.  Ajoutez le disque au cluster à l'aide de l'Administrateur de cluster (cluadmin).  
  
6.  Exécutez le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
### <a name="problem-applications-fail-to-enlist-sql-server-resources-in-a-distributed-transaction"></a>Problème : Les applications ne parviennent pas à inscrire les ressources SQL Server dans une transaction distribuée  
 **Erreur** : comme [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) n’est pas complètement configuré dans Windows, les applications peuvent échouer dans leur tentative d’inscription des ressources [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans une transaction distribuée. Ce problème peut affecter les serveurs liés, les requêtes distribuées et les procédures stockées distantes qui utilisent des transactions distribuées. Pour plus d’informations sur la façon de configurer MS DTC, consultez [Avant l’installation du clustering de basculement](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
 **Résolution :** Pour éviter ce type de problème, vous devez activer intégralement les services MS DTC sur les serveurs où [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est installé et où MS DTC est configuré.  
  
 Pour activer correctement MS DTC, procédez comme suit :  
  
1.  Dans le Panneau de configuration, ouvrez **Outils d'administration**, puis **Gestion de l'ordinateur**.  
  
2.  Dans le volet gauche de Gestion de l'ordinateur, développez **Services et applications**, puis cliquez sur **Services**.  
  
3.  Dans le volet droit Gestion de l’ordinateur, cliquez avec le bouton droit sur **Distributed Transaction Coordinator**, puis sélectionnez **Propriétés**.  
  
4.  Dans la fenêtre **Coordinateur de transactions distribuées** , cliquez sur l'onglet **Général** , puis sur **Arrêter** pour arrêter le service.  
  
5.  Dans la fenêtre **Distributed Transaction Coordinator** , cliquez sur l’onglet **Ouverture de session** , puis définissez le compte d’ouverture de session NT AUTHORITY\NetworkService.  
  
6.  Cliquez sur **Appliquer** , puis sur **OK** pour fermer la fenêtre **Coordinateur de transactions distribuées** . Fermez la fenêtre **Gestion de l'ordinateur** . Fermez la fenêtre **Outils d'administration** .  
  
## <a name="using-extended-stored-procedures-and-com-objects"></a>Utilisation de procédures stockées étendues et d'objets COM  
 Lorsque vous utilisez des procédures stockées étendues avec une configuration de clustering de basculement, toutes les procédures stockées étendues doivent être installées sur un disque de cluster dépendant de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ceci permet d'assurer que lors du basculement d'un nœud, les procédures stockées étendues peuvent toujours être utilisées.  
  
 Si les procédures stockées étendues utilisent des composants COM, l'administrateur doit enregistrer les composants COM sur chaque nœud du cluster. Les informations pour le chargement et l'exécution des composants COM doivent figurer dans le Registre du nœud actif, afin que les composants soient créés. Dans le cas contraire, les informations sont conservées dans le Registre de l'ordinateur sur lequel les composants COM ont été enregistrés en premier lieu.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Fonctionnement des procédures stockées étendues](../../../relational-databases/extended-stored-procedures-programming/how-extended-stored-procedures-work.md)   
 [Caractéristiques d'exécution des procédures stockées étendues](../../../relational-databases/extended-stored-procedures-programming/execution-characteristics-of-extended-stored-procedures.md)  
  
