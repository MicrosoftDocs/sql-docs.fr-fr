---
title: Nouveautés de SSMA pour SAP ASE (SybaseToSQL) | Microsoft Docs
description: Découvrez les modifications apportées à Assistant Migration SQL Server (SSMA) pour Sybase (SybaseToSQL) pour chaque version.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2be0cf8d-6dbe-443a-abbd-036249922205
ms.author: alexiva
ms.openlocfilehash: e63eea932696ca0304443722b40518017ae43485
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065042"
---
# <a name="whats-new-in-ssma-for-sap-ase-sybasetosql"></a>Nouveautés de SSMA pour SAP ASE (SybaseToSQL)

Cet article répertorie les modifications apportées à Assistant Migration SQL Server (SSMA) pour SAP ASE (anciennement SSMA pour Sybase) dans chaque version.

## <a name="ssma-v816"></a>SSMA v 8.16

La version v 8.16 de SSMA pour SAP ASE contient les modifications suivantes :

* Supprimer la prise en charge de l’analyseur hérité
* Résoudre le problème lié à l’actualisation des objets à partir de la base de données

## <a name="ssma-v815"></a>SSMA v 8.15

En plus de plusieurs améliorations de l’accessibilité, la version v 8.15 de SSMA pour SAP ASE contient les modifications suivantes :

* Remodelez les rapports d’évaluation pour travailler dans des navigateurs modernes
* Utiliser l’autorité fournie par la base de données pour l’authentification Azure AD
* Améliorer la dénomination des instructions chargées à partir de fichiers

## <a name="ssma-v814"></a>SSMA v 8.14

En plus de plusieurs améliorations visant à garantir une plus grande accessibilité pour les personnes handicapées, la version v 8.14 de SSMA pour SAP ASE requiert une mise à niveau de projet, car elle stocke désormais la version complète du serveur source/cible dans les métadonnées du projet.

## <a name="ssma-v813"></a>SSMA v 8.13

La version v 8.13 de SSMA pour SAP ASE contient les modifications suivantes :

* Considérer les casts de type implicite lors de la conversion des appels de procédure et de fonction
* Améliorer la journalisation de la chaîne de connexion source pour aider à résoudre les problèmes de connexion

## <a name="ssma-v812"></a>SSMA v 8.12

La version v 8.12 de SSMA pour SAP ASE contient des améliorations mineures en matière de performances et des correctifs de bogues.

## <a name="ssma-v811"></a>SSMA v 8.11

La version 8.11 de SSMA pour SAP ASE contient les modifications suivantes :

* Correction de la conversion de tables temporaires
* Utiliser la bibliothèque MSAL.NET pour l’authentification Azure Active Directory interactive

## <a name="ssma-v810"></a>SSMA v 8.10

La version v 8.10 de SSMA pour SAP ASE contient des améliorations mineures en matière de performances et des correctifs de bogues.

## <a name="ssma-v89"></a>SSMA v 8.9

La version v 8.9 de SSMA pour SAP ASE contient les modifications suivantes :

* Améliorer la conversion de format de date et d’heure
* Correction du problème avec les caractères manquants dans les définitions SQL pour les objets

## <a name="ssma-v88"></a>SSMA v 8.8

La version v 8.8 de SSMA pour SAP ASE comprend les éléments suivants :

* Améliorations de la stabilité de la synchronisation des objets SQL Server
* Améliorations des performances de l’interface graphique lors de l’évaluation et de la conversion
* Correction du problème avec les caractères manquants dans les définitions SQL pour les objets

## <a name="ssma-v87"></a>SSMA v 8.7

La version v 8.7 de SSMA pour SAP ASE présente des correctifs mineurs et des améliorations des performances dans l’interface utilisateur graphique.

> [!IMPORTANT]
> Avec SSMA v 8.5 et versions ultérieures, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

En plus d’un ensemble ciblé de correctifs conçus pour améliorer la facilité d’utilisation et les performances, la version 8.6 de SSMA pour SAP ASE a été améliorée en ajoutant un paramètre qui permet aux utilisateurs d’omettre les propriétés étendues SSMA dans le code converti.

Pour tirer parti de ce paramètre, dans SSMA pour SAP ASE, accédez à **Outils**  >  **paramètres du projet**  >    >  **conversion** générale, puis sous **divers**, mettez à jour la valeur du paramètre **omettre les propriétés étendues** sur **Oui**.

![Paramètre d’omission des propriétés étendues](../sybase/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Avec SSMA v 8.5 et versions ultérieures, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La version v 8.5 de SSMA pour SAP ASE est améliorée grâce à la prise en charge de l’authentification Azure Active Directory et de la prise en charge de base des fonctionnalités JSON dans SQL Server, ainsi qu’à un ensemble ciblé de correctifs conçus pour améliorer l’utilisation et les performances.

En outre, SSMA pour SAP ASE vous permet désormais de masquer les tables et les vues système (excluez-les de la conversion).

> [!IMPORTANT]
> Avec SSMA v 8.5, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La version v 8.4 de SSMA pour SAP ASE a été améliorée avec des correctifs ciblés conçus pour résoudre des problèmes d’accessibilité et résoudre un bogue lié à des colonnes d’index max (pour autoriser 32 au lieu de 16) pour SQL Server 2016 et versions ultérieures.

> [!IMPORTANT]
> Avec les versions de SSMA 7,4 à 8,4, .NET 4.5.2 est une condition préalable à l’installation.

## <a name="ssma-v83"></a>SSMA v 8.3

La version 8.3 de SSMA pour SAP ASE a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion. En outre, cette version de SSMA pour SAP ASE fournit des correctifs qui :

* Résoudre les problèmes d’accessibilité
* Ajouter la prise en charge de base pour le `hierarchyid` type dans SQL Server

## <a name="ssma-v82"></a>SSMA v 8.2

La version 8.2 de SSMA pour SAP ASE a été améliorée avec un ensemble ciblé de correctifs conçus pour améliorer les mesures de qualité et de conversion, ainsi que les correctifs pour :

* Un problème avec les index non cluster désactivés après la migration des données.
* Détection des .NET Framework pendant l’installation sans assistance.
* Incident intermittent qui se produit lorsqu’une nouvelle version est téléchargée.

> [!NOTE]
> Un problème connu avec la mise à jour automatique peut entraîner l’échec d’une mise à jour de SSMA v 8.1 à v 8.2. Si vous rencontrez cette erreur, téléchargez la nouvelle version et installez-la manuellement.

## <a name="ssma-v81"></a>SSMA v 8.1

La version 8.1 de SSMA pour SAP ASE a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion.

> [!NOTE]
> Un problème connu avec la mise à jour automatique peut entraîner l’échec d’une mise à jour de SSMA v 8.0 à v 8.1. Si vous rencontrez cette erreur, téléchargez la nouvelle version et installez-la manuellement.

## <a name="ssma-v80"></a>SSMA v 8.0

La version v 8.0 de SSMA pour SAP ASE a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion. En outre, cette version offre les nouvelles fonctionnalités suivantes :

* Prise en charge d' **Azure SQL Managed instance** en tant que cible. Vous pouvez maintenant créer des projets ciblant Azure SQL Managed Instance :

  ![Projet SQL Database MI](../media/ssma-newproject-sqldbmi.png)

* **Conseiller de réparation** après conversion. En savoir plus à ce sujet [ici](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Sélection préliminaire de base de données/schéma.

  Lors de la connexion à la source, l’utilisateur peut désormais sélectionner les bases de données/schémas intéressants. La sélection de seuls les schémas que vous envisagez de migrer permet de gagner du temps lors de la connexion initiale et d’améliorer les performances globales de SSMA.

  ![Objets de filtre SSMA](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La version de SSMA pour SAP ASE a été améliorée avec des correctifs ciblés conçus pour fournir des protections de sécurité et de confidentialité supplémentaires afin de répondre aux changements d’exigences globales.

## <a name="ssma-v79"></a>SSMA v 7.9

La version v 7.9 de SSMA pour SAP ASE contient les modifications suivantes :

* Correctifs ciblés qui améliorent les mesures de qualité et de conversion.
* Prise en charge dans la ligne de commande SSMA pour modifier le mappage du type de données et les préférences du projet.
* Prise en charge de la migration de données à l’aide de SQL Server Integration Services (SSIS). Après la conversion du schéma, il est possible de créer un package SSIS à l’aide d’un clic droit de la bouton du menu contextuel.
* La boîte de dialogue de connexion Azure SQL Database dans SSMA a également été modifiée pour spécifier le nom de serveur complet. Dans les versions précédentes de SSMA, le préfixe Azure SQL Database devait être mentionné explicitement dans les paramètres des projets.

## <a name="ssma-v78"></a>SSMA v 7.8

La version 7.8 de SSMA pour SAP ASE contient les modifications suivantes :

* Modifiez le mappage de type mis en surbrillance dans les **paramètres du projet**.
* La possibilité pour les utilisateurs de désactiver la télémétrie.

## <a name="ssma-v77"></a>SSMA v 7.7

La version de SSMA pour SAP ASE du v 7.7 contient les modifications suivantes :

* SSMA pour SAP ASE a été amélioré avec des correctifs ciblés qui améliorent la qualité et les mesures de conversion.
* En fonction de la demande populaire, la version 32 bits de SSMA pour SAP ASE est de retour. Par rapport à l’implémentation précédente (antérieure à v 7.4), il existe deux packages d’installation, mais ils ne peuvent pas être installés côte à côte. Par conséquent, vous devez choisir la version la plus appropriée en fonction des composants de connectivité dont vous disposez. Il est toujours préférable d’utiliser la version 64 bits, si possible.

## <a name="ssma-v76"></a>SSMA v 7.6

La version v 7.6 de SSMA pour SAP ASE contient les modifications suivantes :

* Des correctifs ciblés qui améliorent les métriques de qualité et de conversion et la prise en charge de SQL Server 2017 (version préliminaire publique). La prise en charge de SQL Server 2017 sur Windows et Linux est en version préliminaire publique et ne doit pas être utilisée pour les migrations de production.
* Prise en charge de la conversion des fonctions Sybase.

## <a name="ssma-v75"></a>SSMA v 7.5

La version 7.5 de SSMA pour SAP ASE (anciennement SSMA pour Sybase) contient les modifications suivantes :

* Plusieurs améliorations pour garantir une meilleure accessibilité pour les personnes handicapées.
* Prise en charge de la `CREATE OR REPLACE` syntaxe.

## <a name="ssma-v74"></a>SSMA v 7.4

La version 7.4 de SSMA pour Sybase contient les modifications suivantes :

* L’option **délai de requête** est désormais disponible pendant la découverte d’objets de schéma à la source et à la cible.

  ![option délai d’expiration de la requête](../media/query-timeout_red.png)
* La mesure de qualité et de conversion a été améliorée avec les correctifs ciblés, en fonction des commentaires des clients.

  > [!IMPORTANT]
  > .NET 4.5.2 est une condition préalable à l’installation de SSMA v 7.4. En outre, à compter de la version 7.4, la version 32 bits de SSMA n’est plus disponible.

## <a name="ssma-v73"></a>SSMA v 7.3

La version 7.3 de SSMA pour Sybase contient les modifications suivantes :

* Amélioration de la qualité et de la mesure de conversion avec des correctifs ciblés basés sur les commentaires des clients.
* Infrastructure d’extensibilité SSMA exposée via les éléments suivants :
  * Exportez les fonctionnalités vers un projet SQL Server Data Tools (SSDT).
    * Vous pouvez maintenant exporter des scripts de schéma de SSMA vers un projet SSDT. Vous pouvez utiliser les scripts de schéma pour apporter des modifications de schéma supplémentaires et déployer votre base de données.

        ![Enregistrer en tant que projet SSDT, commande](../media/export-schema-scripts_red.png)
  * Bibliothèques pouvant être consommées par SSMA pour effectuer des conversions personnalisées.
    * Vous pouvez désormais construire du code capable de gérer les conversions de syntaxe personnalisées et les conversions qui n’ont pas été traitées précédemment par SSMA.
      * Des instructions sur la construction d’un convertisseur personnalisé sont disponibles dans ce billet de blog, qui [étend les fonctionnalités de conversion de Assistant Migration SQL Server](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/).
      * Téléchargez un exemple de projet pour la conversion à partir de ce billet de [blog](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/).

## <a name="ssma-v72"></a>SSMA v 7.2

La version 7.2 de SSMA pour Sybase contient les modifications suivantes :

* Amélioration de la qualité et de la mesure de conversion avec des correctifs ciblés basés sur les commentaires des clients.
* Améliorations de la télémétrie pour fournir de meilleurs points de données afin de résoudre les problèmes des clients et d’améliorer les taux de conversion de SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La version 7.1 de SSMA pour Sybase contient les modifications suivantes :

* SQL Server 2017 sur Windows et Linux CTP1 est désormais une plateforme cible prise en charge pour la migration. Cette fonctionnalité est en version d’évaluation technique et prend en charge le déplacement des schémas et des données vers les serveurs SQL cibles.
* Prise en charge des mises à jour automatiques pour télécharger la dernière version de SSMA dès qu’elle est disponible.
* Les fichiers binaires de SSMA installables sont désormais fournis via des fichiers de package Windows Installer (. msi).

## <a name="may-2016"></a>Mai 2016

La version 2016 de SSMA pour Sybase contient les modifications suivantes :

* Ajout de la prise en charge de SQL Server 2016.
* Vérification de l’installation de .NET 2,0 supprimée.
* Dépendance de Pack d’extension mise à jour entre .NET 3,5 et .NET 4,0.
* Correction `save-project` des `open-project` commandes et de la console SSMA.
* Correction `securepassword` de la commande pour la console SSMA.
* Correction du décompte des objets pour le chargement initial.
* Correction du bogue dans les paramètres globaux.

## <a name="march-2016"></a>Mars 2016

La version préliminaire de SSMA de mars 2016 de SSMA pour Sybase ajoute la prise en charge de la migration vers SQL Server 2016.

## <a name="january-2016"></a>Janvier 2016

La version de maintenance de SSMA pour Sybase de janvier 2016 contient les modifications suivantes :

* Ajout de l’élément de menu de l’affichage du journal à SSMA (RFC 5706203).
* Ajout de données de télémétrie.

## <a name="july-2014"></a>Juillet 2014

La version du 2014 juillet de SSMA pour Sybase contient les modifications suivantes :

* Amélioration de la conversion du code Azure SQL Database.
* La fonctionnalité du pack d’extension a été déplacée vers le schéma pour prendre en charge Azure SQL Database.
* Améliorations des performances améliorées testées pour les bases de données avec plus de 10 000 objets.
* Ajout d’améliorations de l’interface utilisateur pour le traitement d’un grand nombre d’objets.
* Ajout de la possibilité de mettre en évidence les schémas métier connus (afin qu’ils puissent être ignorés lors de la conversion).
* Amélioration de la vitesse de conversion ajoutée.
* Ajout de la possibilité d’afficher le nombre d’objets dans l’interface utilisateur.
* Réduction de la taille du rapport de plus de 25%.
* Amélioration des messages d’erreur pour les constructions non analysées.

## <a name="april-2014"></a>Avril 2014

La version d’avril 2014 de SSMA pour Sybase contient les modifications suivantes :

* Ajout de la prise en charge de MS SQL Server 2014.
* Correction des bogues concernant la conversion vers Azure.
* Correction des bogues concernant les pages de rapport invisibles dans IE 10.

## <a name="january-2012"></a>janvier 2012

La version de 2012 janvier de SSMA pour Sybase contient les modifications suivantes :

* Ajout de la prise en charge de la conversion du déclencheur de restauration.
* Correction de la conversion de `@@ROWCOUNT` et `@@ERROR` de dans la même `SET` instruction.

## <a name="july-2011"></a>juillet 2011

La version de juillet 2011 de SSMA pour Sybase fournit des rapports d’erreurs améliorés pendant la migration des données.

## <a name="april-2011"></a>2011 avril

La version d’avril 2011 de SSMA pour Sybase contient les modifications suivantes :

* Le produit « SSMA pour Sybase » consolidé, qui prend en charge [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/ssSQL11-md.md)] et Azure SQL.
* Ajout de la prise en charge de la connexion et de la migration vers [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Ajout d’une nouvelle fonctionnalité pour convertir et migrer des bases de données Sybase vers Azure SQL.
* Moteur de migration de données côté client amélioré, prenant en charge la migration parallèle des données.
* Amélioration des performances de migration des données avec des modes de récupération simple et journalisée en bloc.
* Ajout de la possibilité de convertir et de migrer correctement les bases de données Sybase sensibles à la casse vers les SQL Server sensibles à la casse.
* Ajout de la prise en charge de la conversion des instructions de jointure Sybase ASE non-ANSI en SQL Server instructions de jointure ANSI a été étendue pour supprimer et mettre à jour des instructions.
* Des options de connectivité supplémentaires ont été fournies pour la connexion aux serveurs Sybase ASE à l’aide des fournisseurs ODBC Sybase ASE et Sybase ASE ADO.NET.
* Suppression de la dépendance sur une base de données distincte appelée `SysDB` , qui contient les fonctions d’émulation Sybase (installées dans le cadre du pack d’extension).
* Ajout de la possibilité d’installer SSMA pour le pack d’extension Sybase sur des [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] clusters.
* Ajout de la compatibilité descendante des projets créés par des versions antérieures de SSMA (v 4.0 et v 4.2).
* Ajout de la possibilité d’installer le produit SSMA pour Sybase v 5.0 côte à côte (SxS) avec les versions antérieures de SSMA (v 4.0 et v 4.2).

## <a name="july-2010"></a>Juillet 2010

La version de SSMA 2010 de SSMA pour Sybase a été ajoutée :

* Prise en charge de la migration vers SQL Server 2008 R2.
* Nouvelle application console SSMA pour l’exécution à partir de la ligne de commande.
* La prise en charge de la migration des données à l’aide des Server-Side et des moteurs de migration de données Client-Side.
* Prise en charge de l’instruction « Custom SELECT » dans la migration de données.
* Prise en charge de la migration à partir de Sybase ASE 15.0.3 et 15,5.

## <a name="june-2008"></a>2008 juin

La version du 2008 juin de SSMA pour Sybase contient les modifications suivantes :

* Ajout de SSMA tester, qui teste automatiquement la conversion de l’objet de base de données et la migration des données effectuée par SSMA. Une fois toutes les étapes de migration de SSMA terminées, utilisez le testeur SSMA pour vérifier que les objets convertis fonctionnent de la même façon et que toutes les données ont été correctement transférées.
* Conversion pré-SQL ajoutée. L’utilisateur peut désormais spécifier des déclarations de table temporaire (et d’autres objets) pour chaque procédure source à utiliser lors de la conversion.
* Améliorations ajoutées à la conversion d’objets :
  * Conversion de jointures révisée.
  * Agrégats et non-agrégations sans clauses/regroupement.
  * La `IDENTITY` fonction avec une `SELECT INTO` instruction.
  * Contraintes et index en cluster sur les données uniquement-verrouillées.
  * Tables temporaires créées par `SELECT INTO` .
  * Contraintes/index pour les tables temporaires.
  * [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)]Les nouveaux types DateTime sont pris en charge.
  * Connectivité Sybase 15,0 et types de données pris en charge.

## <a name="may-2007"></a>2007 mai

La version 2007 de SSMA pour Sybase ajoutée est la suivante :

* Possibilité de charger plus rapidement le contenu de la base de données lors de l’enregistrement d’un projet.
* Prise en charge des commentaires entrés par l’utilisateur dans le mode SQL mis en forme SQL Server.
* Améliorations de la conversion d’objets.

## <a name="november-2006"></a>2006 novembre

La version de novembre 2006 de SSMA pour Sybase contient les modifications suivantes :

* Nouveaux paramètres globaux ajoutés :
  * Vous pouvez choisir d’afficher les numéros de ligne dans les fenêtres de l’éditeur.
  * Vous pouvez configurer SSMA pour inviter à remplacer des objets dupliqués, ou toujours ou jamais remplacer des objets en double lors de la conversion de schéma.
* Ajout d’une nouvelle option de conversion qui vous permet de configurer la façon dont SSMA gère les situations suivantes :
  * Une `CAST` `CONVERT` instruction ou qui contient une chaîne binaire.
  * Recherche les valeurs NULL dans les expressions d’égalité.
  * Tables proxy.
  * Numéros d’erreur des messages utilisateur pour `RAISERROR` .
  * `UPDATE` instructions qui contiennent des identificateurs non résolus.
* Ajout d’une nouvelle option de migration qui vous permet de spécifier comment SSMA doit gérer les dates qui sont en dehors de la [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] plage de dates.
* Ajout d’un paramètre **SQL mis en forme** sous l’onglet **SQL** , qui met en forme le code pour une meilleure lisibilité.
* Les correctifs de bogues, notamment :
  * SSMA convertit désormais `LOCK TABLE <table> IN { SHARED | EXCLUSIVE } MODE` les instructions en ajoutant un `TABLOCK` `TABLOCKX` indicateur ou à la `SELECT` requête suivante sur la table.
  * Les casts nécessaires sont maintenant ajoutés lorsque les types binaires sont utilisés dans les expressions de caractères.
  * Améliorations de la mémoire et des performances.

## <a name="july-2006"></a>2006 juillet

La version de juillet 2006 de SSMA pour Sybase était la version initiale.

## <a name="see-also"></a>Voir également

[Prise en main avec SSMA pour Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)
