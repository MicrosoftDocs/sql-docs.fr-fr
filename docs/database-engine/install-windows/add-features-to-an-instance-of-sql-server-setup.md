---
title: Ajouter des fonctionnalités à une instance de SQL Server (programme d’installation)
description: Cet article fournit une procédure pas à pas pour ajouter des fonctionnalités dépendantes d’une instance à une instance de SQL Server 2019.
ms.prod: sql
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- feature adding [SQL Server]
- " SQL Server, features"
- adding features to  SQL Server
ms.assetid: 97931fdc-d943-48dd-81b9-ae8b8d2c6dad
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 09/07/2019
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 2f756136657d20a694cbaa2263500624eeaf8c27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347876"
---
# <a name="add-features-to-an-instance-of-sql-server-setup"></a>Ajouter des fonctionnalités à une instance de SQL Server (programme d’installation)

[!INCLUDE [ SQL Server - Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Cet article fournit une procédure pas à pas pour ajouter des fonctionnalités à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Certains composants ou services SQL Server sont spécifiques à une instance de SQL Server. Ils sont également définis comme étant dépendants de l'instance. Ils partagent la même version que l'instance qui les héberge et sont utilisés exclusivement pour cette instance. Vous pouvez ajouter les composants dépendants d'une instance à une instance SQL, avec des composants partagés s'ils ne sont pas déjà installés. Pour obtenir la liste des fonctionnalités prises en charge par différentes éditions de SQL Server, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) ou [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).

Pour ajouter des fonctionnalités à une instance de SQL Server à partir d’une invite de commandes, consultez [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md).

## <a name="prerequisites"></a>Prérequis

Avant de continuer, consultez les articles dans [Planification d’une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md).

> [!NOTE]
> Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez SQL Server à partir d'un partage distant, vous devez utiliser un compte de domaine qui dispose des autorisations de lecture sur le partage distant.  
  
> [!NOTE]
> Lorsque vous ajoutez des fonctionnalités à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], les paramètres de rapport d'utilisation existants sont appliqués aux fonctionnalités ajoutées récemment. Pour modifier ces paramètres, utilisez l'outil **Rapports d'erreurs et d'utilisation SQL Server** dans le menu **Outils de configuration** SQL Server.

## <a name="procedures"></a>Procédures

#### <a name="to-add-features-to-an-instance-of-sscurrent"></a>Pour ajouter des fonctionnalités à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]

1. Insérez le support d'installation SQL Server. Dans le dossier racine, double-cliquez sur setup.exe. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur setup.exe. Si la boîte de dialogue Installation de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] s'affiche, sélectionnez **OK** pour installer les composants requis, puis **Annuler** pour quitter l'installation de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  

2. L'Assistant Installation lancera le Centre d'installation SQL Server. Pour ajouter une nouvelle fonctionnalité à une instance existante de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], sélectionnez **Installation** dans la zone de navigation de gauche, puis cliquez sélectionnez **Nouvelle installation autonome SQL Server ou ajout de fonctionnalités à une installation existante**.

3. L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour afficher les détails de vérification, sélectionnez **Afficher les détails**. Pour continuer, sélectionnez **OK**.

4. Dans la page Mises à jour du produit, les dernières mises à jour du produit SQL Server disponibles s'affichent. Si vous ne souhaitez pas inclure les mises à jour, décochez la case **Inclure les mises à jour du produit SQL Server**. Si aucune mise à jour du produit n'est découverte, le programme d'installation SQL Server n'affiche pas cette page et passe automatiquement à la page **Installer les fichiers d'installation**.

5. Sur la page Installer les fichiers d'installation, le programme d'installation fournit la progression du téléchargement, de l'extraction et de l'installation des fichiers d'installation. Si une mise à jour de l'installation SQL Server est trouvée et est spécifiée pour être incluse, cette mise à jour est également installée. Sélectionnez **Installer** pour installer les fichiers de support du programme d’installation.  

6. L'outil d'analyse de configuration système vérifiera l'état système de votre ordinateur avant que le programme d'installation ne se poursuive.  

7. Dans la page Type d’installation, sélectionnez l’option **Ajouter des fonctionnalités à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]** , puis sélectionnez l’instance à mettre à jour.

8. Dans la page Sélection de fonctionnalités, sélectionnez les composants que vous voulez installer. Une description de chaque groupe de composants apparaît dans le volet droit après que vous avez sélectionné le nom de la fonctionnalité. Vous pouvez choisir n'importe quelle combinaison de cases à cocher. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) et [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md). Chaque composant ne peut être installé qu'une seule fois sur une instance donnée de SQL Server. Pour installer plusieurs composants, vous devez installer une autre instance de SQL Server.

    Les composants requis pour les fonctionnalités sélectionnées sont affichés dans le volet droit. Le programme d'installation de SQL Server installera les composants requis qui ne sont pas déjà installés pendant l'étape d'installation décrite ultérieurement dans cette procédure.

    L'outil d'analyse de configuration système vérifiera l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Sélectionnez **Suivant** pour continuer.

9. La page Espace disque nécessaire calcule l'espace disque requis pour les fonctionnalités que vous spécifiez et compare cet espace à l'espace disque disponible sur l'ordinateur où le programme d'installation s'exécute.

10. Le flux de travail du reste de cet article dépend des fonctionnalités que vous avez spécifiées pour votre installation. Il est possible que les pages ne soient pas toutes visibles, en fonction de vos sélections.

11. Dans la page Configuration du serveur - Comptes de service, spécifiez les comptes de connexion des services SQL Server. Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez sélectionnées.

    Vous pouvez attribuer le même compte de connexion à tous les services SQL Server ou configurer chaque compte de service individuellement. Vous pouvez également spécifier si les services démarrent automatiquement, sont démarrés manuellement ou sont désactivés. Microsoft vous recommande de configurer les comptes de service individuellement afin de fournir des privilèges moindres pour chaque service, sachant que les services SQL Server disposent des autorisations minimales requises pour effectuer leurs tâches. Pour plus d’informations, consultez [Configuration du serveur - Comptes de service](./install-sql-server.md) et [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

    Pour spécifier le même compte de connexion pour tous les comptes de service dans cette instance de SQL Server, fournissez les informations d'identification dans les champs en bas de page.

    **Remarque relative à la sécurité** : [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]

    Lorsque vous avez terminé de spécifier les informations de connexion pour les services SQL Server, sélectionnez **Suivant**.

12. Utilisez l’onglet **Configuration du serveur — Classement** pour spécifier les classements non définis par défaut pour le Moteur de base de données et Analysis Services. Pour plus d’informations, consultez [Configuration du serveur - Classement](./install-sql-server.md).

13. Utilisez la page Configuration du moteur de base de données – Mise en service du compte pour spécifier les éléments suivants :  

    - Mode de sécurité : sélectionnez l'authentification Windows ou l’Authentification en mode mixte pour votre instance de SQL Server. Si vous sélectionnez l’Authentification en mode mixte, vous devez fournir un mot de passe fort pour le compte administrateur système SQL Server intégré.

        Lorsque la connexion d'un périphérique à SQL Server est établie, le mécanisme de sécurité est le même pour l'Authentification Windows et le Mode mixte. Pour plus d’informations, consultez [Configuration du moteur de base de données - Configuration du serveur](./install-sql-server.md).  

    - Administrateurs SQL Server : vous devez spécifier au moins un administrateur système pour l'instance de SQL Server. Pour ajouter le compte sous lequel l'installation de SQL Server s'exécute, sélectionnez **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, sélectionnez **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposent des privilèges d'administrateur pour l'instance de SQL Server. Pour plus d’informations, consultez [Configuration du moteur de base de données - Configuration du serveur](./install-sql-server.md).

    Lorsque vous avez fini de modifier la liste, sélectionnez **OK**. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Si la liste est complète, sélectionnez **Suivant**.

14. Utilisez la page Configuration du moteur de base de données - Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur des répertoires par défaut, sélectionnez **Suivant**.  

    > [!IMPORTANT]
    > Si vous spécifiez des répertoires d'installation non définis par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de SQL Server. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de SQL Server.

    Pour plus d’informations, consultez [Configuration du moteur de base de données – Répertoires de données](./install-sql-server.md).

15. Utilisez la page Configuration du moteur de base de données - FILESTREAM pour activer FILESTREAM pour votre instance de SQL Server. Pour plus d’informations sur FILESTREAM, consultez [Configuration du moteur de base de données - Filestream](./install-sql-server.md). Pour continuer, sélectionnez suivant.

16. Utilisez la page Configuration de Analysis Services - Approvisionnement du compte pour spécifier le mode serveur et les utilisateurs ou comptes qui ont les autorisations d’administrateur pour Analysis Services. Le mode serveur détermine quel sous-systèmes de mémoire et de stockage sont utilisés sur le serveur. Différents types de solution s'exécutent dans différents modes serveur. Si vous envisagez d'exécuter les bases de données multidimensionnelles de cube sur le serveur, choisissez l'option par défaut, le mode serveur multidimensionnel et d'exploration de données. En ce qui concerne les autorisations d'administrateur, vous devez spécifier au moins un administrateur système pour Analysis Services. Pour ajouter le compte sous lequel l'installation de SQL Server s'exécute, sélectionnez **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, sélectionnez **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposent des privilèges d'administrateur pour Analysis Services. Pour plus d’informations sur le mode serveur et les autorisations d’administrateur, consultez [Configuration Analysis Services – Mise en service de compte](./install-sql-server.md).

    Lorsque vous avez fini de modifier la liste, sélectionnez **OK**. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Si la liste est complète, sélectionnez **Suivant**.

17. Utilisez la page Configuration de Analysis Services - Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur des répertoires par défaut, sélectionnez **Suivant**.

    > [!IMPORTANT]
    > Si vous spécifiez des répertoires d'installation non définis par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de SQL Server. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de SQL Server.

    Pour plus d’informations, consultez [Configuration Analysis Services – Répertoires de données](./install-sql-server.md).

18. Utilisez la page Configuration de Reporting Services pour spécifier le type d’installation de Reporting Services à créer. Pour plus d’informations sur les modes de configuration de Reporting Services, consultez [Options de configuration Reporting Services &#40;SSRS&#41;](./install-sql-server.md).

19. Utilisez la page Configuration de Distributed Replay Controller pour spécifier les utilisateurs auxquels vous voulez accorder des autorisations administratives sur le service Distributed Replay Controller. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Controller.

    Sélectionnez le bouton **Ajouter l’utilisateur actuel** pour ajouter les utilisateurs auxquels vous voulez accorder des autorisations d’accès au service du contrôleur Distributed Replay. Sélectionnez le bouton **Ajouter** pour ajouter des autorisations d’accès au service du contrôleur Distributed Replay. Sélectionnez le bouton **Supprimer** pour supprimer des autorisations d’accès au service du contrôleur Distributed Replay.

    Pour continuer, sélectionnez **suivant**.

20. Utilisez la page Configuration de Distributed Replay Client pour spécifier les utilisateurs auxquels vous voulez accorder des autorisations administratives sur le service Distributed Replay Client. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Client.

    **Nom du contrôleur** est un paramètre optionnel ; la valeur par défaut est \<*blank*>. Entrez le nom du contrôleur avec lequel l'ordinateur client communiquera pour le service Distributed Replay Client. Notez les points suivants :

    - Si vous avez déjà configuré un contrôleur, entrez son nom lors de la configuration de chaque client.

    - Si vous n'avez pas encore configuré de contrôleur, vous n'êtes pas obligé de fournir le nom du contrôleur. Toutefois, vous devez entrer manuellement le nom du contrôleur dans le fichier de **configuration du client** .

    Spécifiez le **Répertoire de travail** du service Distributed Replay Client. Le répertoire de travail par défaut est \<*drive letter*>:\Program Files\Microsoft SQL Server\DReplayClient\WorkingDir.

    Spécifiez le **Répertoire des résultats** du service Distributed Replay Client. Le répertoire des résultats par défaut est \<*drive letter*>:\Program Files\Microsoft SQL Server\DReplayClient\ResultDir.

    Pour continuer, sélectionnez **suivant**.

21. Dans la page Rapport d'erreurs, spécifiez les informations que vous souhaitez envoyer à Microsoft qui aideront à améliorer SQL Server. Les options de création de rapports d'erreurs sont activées par défaut.

22. L'outil d'analyse de configuration système exécutera un autre ensemble de règles pour valider la configuration de votre ordinateur avec les fonctionnalités SQL Server que vous avez spécifiées.  

23. La page Prêt pour l'installation affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Dans cette page, le programme d'installation indique si la fonctionnalité de mise à jour du produit est activée ou désactivée et la version de la mise à jour finale. Après avoir cliqué sur Installer, l'installation de SQL Server installera d'abord les composants obligatoires pour les fonctionnalités sélectionnées suivis par l'installation de fonctionnalités.  

24. Au cours de l'installation, la page Progression de l'installation fournit des informations sur l'état pour que vous puissiez contrôler la progression de l'installation au fil de l'avancement de l'exécution du programme d'installation.  

25. Après l'installation, la page Terminé fournit un lien vers le fichier journal résumé pour l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de SQL Server, sélectionnez **Fermer**.

26. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="next-steps"></a>Étapes suivantes

Configurer votre installation SQL Server

- Pour réduire la surface d'exposition attaquable d'un système, SQL Server installe et active de façon sélective les services et les fonctionnalités clés. Pour plus d'informations, consultez [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).

## <a name="see-also"></a>Voir aussi
- [Afficher et lire les fichiers journaux d'installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Valider une installation SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)
- [Réparer une installation défectueuse de SQL Server 2016](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
- [Mettre à niveau SQL Server à l’aide de l’Assistant Installation &#40;Installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
- [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md)
