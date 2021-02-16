---
title: 'Mise à niveau : Assistant Installation (Programme d’installation)'
description: L’Assistant Installation SQL Server fournit une arborescence de fonctionnalités unique pour mettre à niveau sur place les composants SQL Server vers la dernière version de SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- upgrading Database Engine
- Database Engine [SQL Server], upgrading
ms.assetid: cef118a5-a7ce-4bfa-8b9d-c81996284cfc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a5621b9444d56307d21855c9b07aefe736d382d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353856"
---
# <a name="upgrade-sql-server-using-the-installation-wizard-setup"></a>Effectuer une mise à niveau de SQL Server à l’aide de l’Assistant Installation (Installation)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

L’Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une arborescence de fonctionnalités unique pour mettre à niveau sur place les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers la dernière version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
>[!WARNING]  
>Quand vous effectuez une mise à niveau vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est remplacée et n’existe plus sur votre ordinateur. 
>
>Avant d’opérer la mise à niveau, sauvegardez les bases de données SQL Server et les autres objets associés à l’instance SQL Server précédente.  
  
> [!CAUTION]  
> Pour de nombreux environnements de production et certains environnements de développement, une nouvelle mise à niveau de l’installation ou une mise à niveau propagée est plus appropriée qu’une mise à niveau sur place.  Pour plus d’informations sur les différentes méthodes de mise à niveau, consultez :
> * [Choisir une méthode de mise à niveau du moteur de base de données](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)
> * [Mettre à niveau Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)
> * [Mettre à niveau Integration Services](../../integration-services/install-windows/upgrade-integration-services.md)
> * [Mettre à niveau Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md)
> * [Mettre à niveau et migrer Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)
> * [Mettre à niveau Analysis Services](../../database-engine/install-windows/upgrade-analysis-services.md)
> * [Mettez à niveau PowerPivot pour SharePoint](../../database-engine/install-windows/upgrade-power-pivot-for-sharepoint.md).  
  
## <a name="prerequisites"></a>Prérequis  
Vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine doté des autorisations de lecture et d'exécution sur le partage distant, et qui représente un administrateur local.  
  
> [!WARNING]  
>  N'oubliez pas que vous ne pouvez pas modifier les fonctionnalités à mettre à niveau, de même que vous ne pouvez pas ajouter de fonctionnalités pendant l'opération de mise à niveau. Pour plus d’informations sur l’ajout de fonctionnalités à une instance mise à niveau de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] une fois l’opération de mise à niveau terminée, consultez [Ajouter des fonctionnalités à une instance de SQL Server &#40;Installation&#41;](./add-features-to-an-instance-of-sql-server-setup.md).  
  
 Si vous procédez à une mise à niveau de [!INCLUDE[ssDE](../../includes/ssde-md.md)], passez en revue [Planifier et tester le plan de mise à niveau du moteur de base de données](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md) , puis effectuez les tâches suivantes, en fonction de votre environnement :  
  
-   Sauvegardez tous les fichiers de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de l'instance à mettre à niveau, afin de pouvoir les restaurer, si besoin est.  
  
-   Exécutez les commandes DBCC (Database Console Commands) appropriées sur les bases de données à mettre à niveau afin de vérifier leur cohérence.  
  
-   Estimez l’espace disque requis pour mettre à niveau les composants SQL Server ainsi que les bases de données utilisateur. Pour connaître l’espace disque requis par les composants de SQL Server, consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Vérifiez que les bases de données système SQL Server (master, model, msdb et tempdb) sont configurées pour s’accroître automatiquement et vérifiez qu’elles disposent pour cela d’un espace disque suffisant.  
  
-   Vérifiez que tous les serveurs de bases de données possèdent des informations d'ouverture de session dans la base de données master. Ce point est particulièrement important pour la restauration d'une base de données, car les informations d'ouverture de session système résident dans la base de données master.  
  
-   Désactivez toutes les procédures stockées de démarrage, car le processus de mise à niveau arrête et démarre les services sur l’instance SQL Server en cours de mise à niveau. Les procédures stockées traitées au moment du démarrage pourraient bloquer le processus de mise à niveau.  
  
-   Lors de la mise à niveau des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est inscrit dans les relations MSX/TSX, mettez à niveau les serveurs cibles avant de mettre à niveau les serveurs maîtres. Si vous mettez à niveau les serveurs maîtres avant les serveurs cibles, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sera pas en mesure de se connecter aux instances principales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Quittez toutes les applications, y compris tous les services ayant des dépendances SQL Server. La mise à niveau peut échouer si les applications locales sont connectées à l'instance en cours de mise à niveau.  
  
-   Assurez-vous que la réplication est activé et arrêtez la réplication.   
    Pour savoir comment effectuer une mise à niveau propagée dans un environnement répliqué, consultez [Mettre à niveau des bases de données répliquées](../../database-engine/install-windows/upgrade-replicated-databases.md).
  
## <a name="procedure"></a>Procédure  
  
### <a name="to-upgrade-ssnoversion"></a>Mise à niveau vers [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et, dans le dossier racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir d'un partage réseau, accédez au dossier racine sur le partage, puis double-cliquez sur Setup.exe.  
  
2.  L'Assistant d'installation démarre le Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour mettre à niveau une instance existante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cliquez sur **Installation** dans la zone de navigation à gauche, puis cliquez sur **Mise à niveau à partir de...** versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Dans la page Clé de produit, cliquez sur une option pour indiquer si vous effectuez une mise à niveau vers une édition gratuite de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou si vous disposez d'une clé PID pour une version de production du produit. Pour plus d’informations, consultez [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) et [Mises à niveau de version et d’édition prises en charge](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
4.  Dans la page Termes du contrat de licence, prenez connaissance du contrat de licence et, si vous en acceptez les termes, activez la case à cocher **J'accepte les termes du contrat de licence.** , puis cliquez sur **Suivant**. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
5.  Dans la fenêtre Règles globales, la procédure d'installation passera automatiquement à la fenêtre Mises à jour du produit s'il n'existe aucune erreur de règle.  
  
6.  La page [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update s'affiche si la case à cocher [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update n'est pas activée dans Panneau de configuration\Tous les éléments du Panneau de configuration\Windows Update\Modifier les paramètres. Une coche sur la page [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update permet de modifier les paramètres de l'ordinateur de façon à inclure les dernières mises à jour lorsque vous recherchez les mises à jour de Windows.  
  
7.  Dans la page Mises à jour du produit, les dernières mises à jour disponibles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'affichent. Si vous ne souhaitez pas inclure les mises à jour, décochez la case **Inclure les mises à jour du produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Si aucune mise à jour du produit n'est découverte, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'affiche pas cette page et passe automatiquement à la page **Installer les fichiers d'installation** .  
  
8.  Sur la page Installer les fichiers d'installation, le programme d'installation fournit la progression du téléchargement, de l'extraction et de l'installation des fichiers d'installation. Si une mise à jour de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est trouvée, et est spécifiée pour être incluse, cette mise à jour est également installée.  
  
9. Dans la fenêtre Règles de mise à niveau, la procédure d'installation passera automatiquement à la fenêtre Sélectionner une instance s'il n'existe aucune erreur de règle.  
  
10. Dans la page Sélectionner une instance, spécifiez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à mettre à niveau. Pour mettre à niveau les outils d'administration et les fonctionnalités partagées, sélectionnez **Mettre à niveau les fonctionnalités partagées uniquement**.  
  
11. Dans la page Sélectionner les composants, les fonctionnalités à mettre à niveau seront présélectionnées. Une description de chaque groupe de composants apparaît dans le volet droit après que vous avez sélectionné le nom de la fonctionnalité.  
  
     Les composants requis pour les fonctionnalités sélectionnées sont affichés dans le volet droit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure.  
  
    > [!NOTE]  
    >  Si vous avez choisi de mettre à niveau les fonctionnalités partagées en sélectionnant **\<Upgrade shared features only>** dans la page **Sélectionner une instance**, toutes les fonctionnalités partagées sont présélectionnées dans la page Sélection des fonctionnalités. Tous les composants partagés sont mis à niveau à la fois.  
  
12. Dans la page Configuration de l'instance, spécifiez l'ID d'instance pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     **ID d’instance** : Par défaut, le nom de l’instance est utilisé comme ID d’instance. Il permet d'identifier les répertoires d'installation et les clés de Registre de votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tel est le cas pour les instances par défaut et les instances nommées. Pour une instance par défaut, le nom de l'instance et l'ID d'instance sont MSSQLSERVER. Pour utiliser un ID d’instance non défini par défaut, indiquez une valeur dans la zone de texte **ID d’instance** .  
  
     Tous les Service Packs et mises à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'appliqueront à chaque composant d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     **Instances installées** - La grille affichera les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui se trouvent sur l’ordinateur où le programme d’installation s’exécute. Si une instance par défaut est déjà installée sur l'ordinateur, vous devez installer une instance nommée de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
13. Le flux de travail du reste de cet article dépend des fonctionnalités que vous avez spécifiées pour votre installation. Il est possible que les pages ne soient pas toutes visibles, en fonction de vos sélections.  
  
14. Dans la page Configuration du serveur - Comptes de service, les comptes de service par défaut s’affichent pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les services réels configurés dans cette page dépendent des fonctionnalités que vous mettez à niveau.  
  
     Les informations d'authentification et de connexion sont transférées à partir de l'instance antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez attribuer le même compte de connexion à tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou configurer chaque compte de service individuellement. Vous pouvez également spécifier si les services démarrent automatiquement, sont démarrés manuellement ou sont désactivés. [!INCLUDE[msCoName](../../includes/msconame-md.md)] vous recommande de configurer les comptes de service individuellement afin de fournir des privilèges moindres pour chaque service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour effectuer leurs tâches. Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Pour spécifier le même compte de connexion pour tous les comptes de service dans cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], fournissez les informations d'identification dans les champs en bas de page.  
  
     **Note de sécurité** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     Lorsque vous avez terminé de spécifier les informations de connexion pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Suivant**.  
  
15. Dans la page des options de mise à niveau de recherche en texte intégral, spécifiez les options de mise à niveau pour les bases de données mises à niveau. Pour plus d’informations, consultez [Options de mise à niveau de recherche en texte intégral](./install-sql-server.md).  
  
16. La fenêtre Règles de fonctionnalité avancera automatiquement si toutes les règles passent.  
  
17. La page Prêt pour la mise à niveau affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Installer**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités.  
  
18. Au cours de l'installation, la page de progression fournit des informations d'état de sorte que vous puissiez contrôler la progression de l'installation au fil de l'exécution du programme d'installation.  
  
19. Après l'installation, la page Terminé fournit un lien vers le fichier journal résumé pour l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**.  
  
20. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="next-steps"></a>Étapes suivantes  
 Après avoir effectué la mise à niveau vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], effectuez les tâches suivantes :  
  
-   **Inscrire vos serveurs** - La mise à niveau entraîne la suppression des paramètres du Registre pour l’instance antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Après la mise à niveau, vous devez réinscrire vos serveurs.  
  
-   **Mettre à jour les statistiques** - Afin d’optimiser les performances des requêtes, il est recommandé de mettre à jour les statistiques sur toutes les bases de données après une mise à niveau. Utilisez la procédure stockée **sp_updatestats** pour mettre à jour les statistiques des tables définies par l’utilisateur dans les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **Configurer votre nouvelle installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** - Pour réduire la surface d’exposition vulnérable aux attaques d’un système, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe et active de manière sélective les services et fonctionnalités clés. Pour plus d'informations sur la configuration de la surface d'exposition, consultez le fichier Lisezmoi de cette version.  
  
## <a name="see-also"></a>Voir aussi  
 [Mettre à niveau SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)   
 [Compatibilité descendante_supprimé](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))  
  
