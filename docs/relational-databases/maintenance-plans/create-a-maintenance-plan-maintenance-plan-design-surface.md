---
title: Créer un plan de maintenance à avec Aire de conception
description: Découvrez comment créer un plan de maintenance à un ou plusieurs serveurs à l’aide de l’aire de conception de plan de maintenance dans SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Maintenance Plan Design Surface
ms.assetid: 2ef803ee-a9f8-454a-ad63-fedcbe6838d1
author: MikeRayMSFT
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: 931aa85fbb37d8dba1c9bb3e06e00760d3f16caf
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049151"
---
# <a name="create-a-maintenance-plan-maintenance-plan-design-surface"></a>Créer un plan de maintenance (aire de conception de plan de maintenance)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer un plan de maintenance de serveur unique ou multiserveur à l'aide de l'aire de conception de plan de maintenance de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. L' **Assistant Plan de maintenance** est conseillé pour créer des plans de maintenance de base, tandis que l'aire de conception permet d'utiliser un flux de travail optimisé.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   [Création d'un plan de maintenance à l'aide de l'aire de conception de plan de maintenance](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Pour créer un plan de maintenance multiserveurs, vous devez configurer un environnement multiserveurs contenant un serveur maître et un ou plusieurs serveurs cibles. Les plans de maintenance multiserveurs doivent être créés et conservés sur le serveur maître. Ces plans peuvent être consultés mais ne peuvent pas être conservés sur les serveurs cibles.  
  
-   Les membres du rôle **db_ssisadmin** et du rôle **dc_admin** peuvent être en mesure d’élever leurs privilèges à **sysadmin**. Cette élévation de privilège est possible, car ces rôles peuvent modifier les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , qui sont exécutables par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec le contexte de sécurité **sysadmin** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Pour empêcher cette élévation de privilège durant l’exécution de plans de maintenance, de jeux d’éléments de collecte de données et d’autres packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configurez les travaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent qui exécutent des packages de façon à utiliser un compte proxy doté de privilèges limités ou ajoutez uniquement des membres **sysadmin** aux rôles **db_ssisadmin** et **dc_admin** .  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Pour créer ou gérer des plans de maintenance, vous devez être membre du rôle serveur fixe **sysadmin** . L'Explorateur d'objets affiche uniquement le nœud **Plans de maintenance** pour les utilisateurs membres du rôle serveur fixe **sysadmin** .  
  
##  <a name="using-maintenance-plan-design-surface"></a><a name="SSMSProcedure"></a> Utilisation de l'aire de conception de plan de maintenance  
  
#### <a name="to-create-a-maintenance-plan"></a>Pour créer un plan de maintenance  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez créer un plan de maintenance.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez avec le bouton droit sur le dossier **Plans de maintenance** et sélectionnez **Nouveau plan de maintenance**.  
  
4.  Dans la boîte de dialogue **Nouveau plan de maintenance** , dans la zone **Nom** , tapez un nom pour le plan, puis cliquez sur **OK**. Vous ouvrez ainsi la boîte à outils et l’aire *nom_plan_maintenance* **[Conception]** avec le sous-plan **Sous-plan_1** créé dans la grille principale.  
  
     Les options suivantes sont disponibles dans l'en-tête de l'aire de conception.  
  
     **Ajouter le sous-plan**  
     Ajoute un sous-plan que vous pouvez configurer.  
  
     **Propriétés du sous-plan**  
     Affiche la boîte de dialogue **Propriétés du sous-plan** du sous-plan sélectionné dans la grille principale. Vous pouvez également double-cliquer sur un sous-plan dans la grille pour afficher la boîte de dialogue **Propriétés du sous-plan** . Pour plus d'informations sur cette boîte de dialogue, voir ci-dessous.  
  
     **Supprimer le sous-plan sélectionné**  
     Supprime le sous-plan sélectionné.  
  
     **Planification du sous-plan**  
     Affiche la boîte de dialogue **Nouvelle planification du travail** du sous-plan sélectionné.  
  
     **Supprimer la planification**  
     Supprime une planification du sous-plan sélectionné.  
  
     **Gérer les connexions**  
     Affiche la boîte de dialogue **Gérer les connexions** . Permet d'ajouter des connexions d'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supplémentaires au plan de maintenance. Pour plus d'informations sur cette boîte de dialogue, voir ci-dessous.  
  
     **Création de rapport et enregistrement**  
     Affiche la boîte de dialogue **Création de rapport et enregistrement** . Pour plus d'informations sur cette boîte de dialogue, voir ci-dessous.  
  
     **Serveurs**  
     Affichez la boîte de dialogue **Serveurs** qui permet de sélectionner les serveurs où sont exécutées les tâches du sous-plan. Cette option est activée uniquement sur des serveurs maîtres dans des environnements multiserveurs. Pour plus d’informations, consultez [Créer un environnement multiserveur](../../ssms/agent/create-a-multiserver-environment.md) et [Plan de maintenance &#40;Serveurs&#41;](../../relational-databases/maintenance-plans/maintenance-plan-servers.md).  
  
     **Nom**  
     Affichez le nom du plan de maintenance. Pour les nouveaux plans de maintenance, le nom est spécifié dans une boîte de dialogue avant l'ouverture du concepteur de plan de maintenance. Pour renommer un plan de maintenance, cliquez dessus avec le bouton droit dans l’Explorateur d’objets, puis cliquez sur **Renommer**.  
  
     **Description**  
     Permet d'afficher ou de spécifier une description du plan de maintenance. La longueur maximale de la description est 512 caractères.  
  
     **Aire du concepteur**  
     Conception et entretien des plans de maintenance. Utilisez l'aire du concepteur pour ajouter des tâches de maintenance à un plan, supprimer des tâches d'un plan, spécifier des liens de précédence entre des tâches et indiquer les branchements et le parallélisme des tâches.  
  
     Un lien de précédence entre deux tâches établit une relation entre ces tâches. La seconde tâche (la *tâche dépendante*) s’exécute seulement si le résultat d’exécution de la première tâche (la *tâche prioritaire*) correspond aux critères spécifiés. En général, le résultat d'exécution spécifié est **Succès**, **Échec** ou **À l'achèvement**. Pour plus d'informations, consultez l'étape **8** ci-dessous.  
  
5.  Dans l’en-tête de l’aire de conception, double-cliquez sur **Sous-plan_1** et entrez un nom et une description pour le sous-plan dans la boîte de dialogue **Propriétés du sous-plan** .  
  
     Les options suivantes sont disponibles dans la boîte de dialogue **Propriétés du sous-plan** .  
  
     **Nom**  
     Nom du sous-plan.  
  
     **Description**  
     Brève description du sous-plan.  
  
     **Planification**  
     Indique pour quelle planification le sous-plan sera exécuté. Cliquez sur **Planification du sous-plan** pour ouvrir la boîte de dialogue **Nouvelle planification du travail** . Cliquez sur **Supprimer la planification** pour supprimer la planification du sous-plan.  
  
     Liste **Exécuter en tant que**  
     Sélectionnez le compte à utiliser pour exécuter cette sous-tâche.  
  
6.  Cliquez sur l'icône **Planification du sous-plan** pour entrer les informations de la planification dans la boîte de dialogue **Nouvelle planification du travail** .  
  
7.  Pour générer le sous-plan, faites glisser les éléments de flux des tâches de la **Boîte à outils** vers l'aire de conception du plan. Double-cliquez sur des tâches pour ouvrir les boîtes de dialogue permettant de configurer les options des tâches.  
  
     Les tâches de plan de maintenance suivantes sont disponibles dans la **Boîte à outils**:  
  
    -   **Tâche Sauvegarder la base de données**  
  
    -   **Tâche Vérifier l'intégrité de la base de données**  
  
    -   **Tâche Exécuter le travail de SQL Server Agent**  
  
    -   **Tâche Exécuter l'instruction T-SQL**  
  
    -   **Tâche de nettoyage d'historique**  
  
    -   **Tâche de nettoyage de maintenance**  
  
    -   **Tâche Notifier l'opérateur**  
  
    -   **Tâche Reconstruire l'index**  
  
    -   **Tâche Réorganiser l'index**  
  
    -   **Tâche Réduire la base de données**  
  
    -   **Tâche Mettre à jour les statistiques**  
  
     Pour ajouter des tâches à la **Boîte à outils**:  
  
    1.  Dans le menu **Outils** , cliquez sur **Choisir des éléments de boîte à outils**.  
  
    2.  Sélectionnez les outils que vous souhaitez voir afficher dans la **Boîte à outils**, puis cliquez sur **OK**.  
  
     Les tâches de plan de maintenance que vous ajoutez à la **Boîte à outils** sont également disponibles dans l' **Assistant Plan de maintenance**. Pour plus d’informations sur les différentes tâches présentées ci-dessus, consultez [Utilisation de l’Assistant Plan de maintenance](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure) sous **Démarrer l’Assistant Plan de maintenance**.  
  
8.  Pour définir un flux de travail entre les tâches :  
  
    1.  Cliquez avec le bouton droit sur la tâche prioritaire et sélectionnez **Ajouter une contrainte de précédence**.  
  
    2.  Dans la boîte de dialogue **Flux de contrôle** , dans la liste **À** , sélectionnez la tâche dépendante et cliquez sur **OK**.  
  
    3.  Double-cliquez sur le connecteur entre les deux tâches pour ouvrir la boîte de dialogue **Éditeur de contrainte de précédence** .  
  
         Les options suivantes sont disponibles dans la boîte de dialogue **Éditeur de contrainte de précédence** .  
  
         **Option de contrainte**  
         Définit la manière dont une contrainte fonctionne entre deux tâches.  
  
         Liste **Opération d’évaluation**  
         Spécifiez l'opération d'évaluation utilisée par la contrainte de précédence. Ces opérations sont : **Contrainte**, **Expression**, **Expression et contrainte** et **Expression ou contrainte**.  
  
         Liste **Valeur**  
         Spécifiez la valeur de contrainte : **Réussite**, **Échec** ou **À l'achèvement**. **Réussite** est la valeur par défaut.  
  
        > [!NOTE]  
        >  La ligne de contrainte de précédence est verte pour **Réussite**, rouge pour **Échec** et bleue pour **À l’achèvement**.  
  
         **Expression**  
         Si vous utilisez les opérations **Expression**, **Expression et contrainte** ou **Expression ou contrainte**, tapez une expression. L'expression doit prendre une valeur de type Boolean.  
  
         **Test**  
         Validez l'expression.  
  
         **Contraintes multiples**  
         Définissez la manière dont plusieurs contraintes interopèrent pour contrôler l'exécution de la tâche contrainte.  
  
         **ET logique**  
         Sélectionnez cette option pour spécifier que plusieurs contraintes de précédence sur le même exécutable doivent être évaluées ensemble. Toutes les contraintes doivent prendre la valeur True. Cette option est celle par défaut.  
  
        > [!NOTE]  
        >  Ce type de contrainte de précédence s'affiche sous forme de ligne pleine verte, rouge ou bleue.  
  
         **OU logique**  
         Sélectionnez cette option pour spécifier que plusieurs contraintes de précédence sur le même exécutable doivent être évaluées ensemble. Au moins une contrainte doit prendre la valeur True.  
  
        > [!NOTE]  
        >  Ce type de contrainte de précédence s'affiche sous forme de ligne pointillée verte, rouge ou bleue.  
  
9. Pour ajouter un autre sous-plan qui contient les tâches exécutées sur une autre planification, cliquez sur **Ajouter un sous-plan** dans la barre d'outils pour ouvrir la boîte de dialogue **Propriétés du sous-plan** .  
  
10. Pour ajouter des connexions à d'autres serveurs :  
  
    1.  Dans la barre d'outils de l'aire de conception, cliquez sur **Gérer les connexions**.  
  
    2.  Dans la boîte de dialogue **Gérer les connexions** , cliquez sur **Ajouter**.  
  
    3.  Dans la boîte de dialogue **Propriétés de connexion** , dans la zone **Nom de la connexion** , entrez le nom de la connexion que vous créez.  
  
    4.  Sous **Spécifiez les éléments suivants pour vous connecter aux données de SQL Server**, dans la zone **Sélectionnez ou entrez un nom de serveur**, entrez le nom du serveur SQL Server à utiliser ou cliquez sur les points de suspension **(…)** et sélectionnez un serveur dans la boîte de dialogue **SQL Server**. Si vous sélectionnez un serveur dans la boîte de dialogue **SQL Server** , cliquez sur **OK**.  
  
    5.  Sous **Entrez des informations pour vous connecter au serveur**, sélectionnez **Utiliser la sécurité intégrée de Windows NT** ou **Utiliser un nom d'utilisateur et un mot de passe spécifiques**. Si vous choisissez d'utiliser un nom d'utilisateur et un mot de passe spécifiques, entrez ces informations dans les zones **Nom d'utilisateur** et **Mot de passe** , respectivement.  
  
    6.  Dans la boîte de dialogue **Propriétés de connexion** , cliquez sur **OK**.  
  
    7.  Dans la boîte de dialogue **Gérer les connexions** , cliquez sur **Fermer**.  
  
11. Pour spécifier des options de création de rapports :  
  
    1.  Dans la barre d'outils de l'aire de conception, cliquez sur **Création de rapport et enregistrement**.  
  
    2.  Dans la boîte de dialogue **Création de rapport et enregistrement** , sous **Création de rapports**, sélectionnez **Générer un rapport de fichier texte** ou **Envoyer le rapport à un destinataire de messagerie** ou les deux.  
  
        1.  Si vous sélectionnez **Générer un rapport de fichier texte**, sélectionnez **Créer un nouveau fichier** ou **Ajouter au fichier**.  
  
        2.  En fonction de la sélection ci-dessus, entrez le nom et le chemin complet du nouveau fichier ou du fichier à ajouter dans les zones **Dossier** ou **Nom de fichier** . Vous pouvez également cliquer sur les points de suspension **(...)** , puis sélectionner le chemin du dossier ou le nom de fichier à partir des boîtes de dialogue **Localiser le dossier -** _nom\_serveur_ ou **Rechercher les fichiers de la base de données -** _nom\_serveur_.  
  
        3.  Si vous sélectionnez **Envoyer le rapport à un destinataire de messagerie**, dans la liste **Opérateur d'agent** , sélectionnez le destinataire du rapport envoyé par messagerie électronique.  
  
            > [!NOTE]  
            >  L'Agent SQL Server doit être configuré pour utiliser la messagerie de base de données afin d'envoyer le message. Pour plus d'informations, consultez [Configure SQL Server Agent Mail to Use Database Mail](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
    3.  Pour enregistrer des informations plus détaillées, sous **Enregistrement**, sélectionnez **Enregistrer les informations étendues**.  
  
    4.  Pour écrire les informations de résultats de plan de maintenance sur un autre serveur, sélectionnez **Se connecter à un serveur distant** , puis sélectionnez une connexion au serveur dans la liste **Connexion** , ou cliquez sur **Nouveau** pour entrer les informations de connexion dans la boîte de dialogue **Propriétés de connexion** .  
  
    5.  Dans la boîte de dialogue **Création de rapport et enregistrement** , cliquez sur **OK**.  
  
12. Pour consulter les résultats dans la visionneuse du fichier journal, dans **l’Explorateur d’objets**, cliquez avec le bouton droit sur le dossier **Plans de maintenance** ou sur le plan de maintenance spécifique et sélectionnez **Afficher l’historique**.  

     Les options suivantes sont disponibles dans la boîte de dialogue **Visionneuse du fichier journal -** _nom\_serveur_.  
  
     **Charger le journal**  
     Ouvre une boîte de dialogue dans laquelle vous pouvez spécifier un fichier journal à charger.  
  
     **Export**  
     Ouvre une boîte de dialogue qui vous permet d’exporter les informations figurant dans la grille **Résumé du fichier journal** vers un fichier texte.  
  
     **Actualiser**  
     Actualise l'affichage des journaux sélectionnés. Le bouton **Actualiser** permet de relire les journaux sélectionnés à partir du serveur cible lors de l'application des paramètres de filtre.  
  
     **Filter**  
     Ouvre une boîte de dialogue qui vous permet de spécifier les paramètres utilisés pour filtrer le fichier journal, notamment **Connexion**, **Date** et d’autres critères de filtre **Général** .  
  
     **action**  
     Permet de rechercher un texte spécifique dans le fichier journal. La recherche des caractères génériques n'est pas prise en charge.  
  
     **Stop**  
     Arrête le chargement des entrées du fichier-journal. Par exemple, vous pouvez utiliser cette option si un fichier de journal distant ou hors connexion est long à charger, et que vous souhaitez seulement consulter les entrées les plus récentes.  
  
     **Résumé du fichier journal**  
     Ce volet d'informations affiche un résumé du filtrage du fichier journal. Si le fichier n'est pas filtré, le texte suivant s'affiche : **Aucun filtre appliqué**. Si un filtre est appliqué au journal, le texte suivant s’affiche : **Filtrer les entrées du journal pour :** \<filter criteria>.  
  
     **Date**  
     Affiche la date de l'événement.  
  
     **Source**  
     Affiche la fonctionnalité source à partir de laquelle l'événement est créé, par exemple le nom du service (comme MSSQLSERVER). Ceci n'apparaît pas pour tous les types de journaux.  
  
     **Message**  
     Affiche les messages associés à l'événement.  
  
     **Type de journal**  
     Affiche le type de journal auquel appartient l'événement. Tous les journaux sélectionnés s'affichent dans la fenêtre de résumé du fichier journal.  
  
     **Source du journal**  
     Affiche une description du journal source dans lequel l'événement est capturé.  
  
     **Détails de la ligne sélectionnée**  
     Sélectionnez une ligne pour afficher des détails supplémentaires sur la ligne d'événement sélectionnée en bas de la page. Vous pouvez changer l'ordre des colonnes en les faisant glisser sur la grille. Vous pouvez redimensionner les colonnes en faisant glisser les barres de séparation des colonnes dans l'en-tête de la grille vers la gauche ou la droite. Double-cliquez sur les barres de séparation des colonnes dans l'en-tête de la grille pour ajuster automatiquement la largeur de la colonne au contenu.  
  
     **Instance**  
     Nom de l'instance pour laquelle l'événement s'est produit. Il est affiché sous la forme *nom de l'ordinateur*\\*nom de l'instance*.  
  
  
