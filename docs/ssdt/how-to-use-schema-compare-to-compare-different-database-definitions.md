---
title: Utiliser Comparer les schémas pour comparer différentes définitions de base de données
description: Découvrez comment comparer les schémas les définitions de base de données avec Comparer les schémas. Consultez comment exclure des différences spécifiques et mettre à jour la cible ou créer un script de mise à jour.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.schemacompare.SchemaCompareOptionsDialog
- sql.data.tools.schemacompare.watermark.f1
- sql.data.tools.schemacompare.f1
- sql.data.tools.schemacompare.connectiondialog.f1
- sql.data.tools.schemacompare.connectiondialog.error.f1
ms.assetid: 7f0905a4-081c-46e2-bd7d-325b63e5c675
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 414a6eb4f6e63d2bb0c02e831f60ebcff97dc590
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018029"
---
# <a name="how-to-use-schema-compare-to-compare-different-database-definitions"></a>Procédure : Utiliser Comparer les schémas pour comparer différentes définitions de base de données

SQL Server Data Tools (SSDT) inclut un utilitaire de comparaison de schémas que vous pouvez utiliser pour comparer deux définitions de base de données.  La source et la cible de la comparaison peuvent être n'importe quelle combinaison des éléments suivants : base de données connectée, projet de base de données SQL Server, instantané ou fichier .dacpac.  Les résultats de la comparaison s'affichent en tant qu'ensemble d'actions qui doivent être effectuées sur la cible de façon à ce qu'elle soit identique à la source.  Une fois la comparaison effectuée, vous pouvez mettre à jour la cible directement (s'il s'agit d'un projet ou d'une base de données) ou générer un script de mise à jour qui a le même effet.  
  
Les différences entre la source et la cible s'affichent dans une grille qui permet de les examiner facilement.  Vous pouvez extraire et passer en revue chaque différence dans la grille de résultats ou sous la forme d'un script.  Vous pouvez ensuite exclure des différences spécifiques de manière sélective.  
  
Vous pouvez enregistrer les comparaisons dans le cadre d'un projet de base de données SQL Server ou en tant que fichier autonome.  Vous pouvez également définir des options qui contrôlent l'étendue de la comparaison et les aspects de la mise à jour.  Vous pouvez ensuite enregistrer la comparaison afin de pouvoir la réutiliser ultérieurement ou l'utiliser comme point de départ d'une nouvelle comparaison.  
  
Dans la procédure suivante, vous comparez le schéma d'un projet de base de données à une base de données connectée.  
  
> [!WARNING]  
> Si un projet est spécifié en tant que cible pour la comparaison, la longueur maximale de chemin d'accès prise en charge (à l'exception de la lettre du lecteur, du signe deux-points et de la barre oblique inverse au début) pour le projet est de 256 caractères. Si le chemin d'accès de votre projet dépasse 256 caractères, vous serez toujours en mesure de comparer son schéma à une base de données ou à un autre projet. Toutefois, vous ne serez pas en mesure de mettre à jour son schéma.  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-compare-database-definitions"></a>Pour comparer des définitions de base de données  
  
1.  Dans le menu **Outils**, sélectionnez **SQL Server**, puis cliquez sur **Nouvelle comparaison de schémas**.  
  
    Dans l'**Explorateur de solutions** vous pouvez aussi cliquer avec le bouton droit sur le projet **TradeDev**, puis sélectionner **Comparaison de schémas**.  
  
    La fenêtre **Comparaison de schémas** s'ouvre et Visual Studio lui assigne automatiquement un nom, tel que `SqlSchemaCompare1`.  
  
    Deux menus déroulants avec une flèche verte entre eux s'affichent au-dessous de la barre d'outils de la fenêtre **Comparaison de schémas**. Ces menus vous permettent de sélectionner les définitions de base de données pour vos source et cible de comparaison.  
  
2.  Dans la boîte de dialogue **Sélectionner une source**, sélectionnez **Sélectionner une source** et la boîte de dialogue **Sélectionner le schéma source** s'ouvre.  
  
    Notez que si vous avez ouvert la fenêtre **Comparaison de schémas** en cliquant avec le bouton droit sur le nom du projet, le schéma source est déjà renseigné et vous pouvez passer à l'étape 4.  
  
3.  Sélectionnez la case d'option **Projet**, puis sélectionnez le projet de base de données **TradeDev** que vous avez créé au cours de la procédure précédente.  
  
4.  Dans le menu déroulant **Sélectionner une cible** de la fenêtre **Comparaison de schémas**, choisissez **Sélectionner une cible** et la boîte de dialogue **Sélectionner le schéma cible** s'ouvre. Dans la section **Schéma**, cliquez sur la case d'option **Base de données**, puis cliquez sur le bouton **Nouvelle connexion**.  
  
5.  Dans la boîte de dialogue **Propriétés de connexion**, entrez le nom du serveur sur lequel la base de données `TradeDev` réside et assurez-vous que les informations d'identification fournies sont correctes. Sélectionnez ensuite **TradeDev** sous **Connexion à une base de données** et cliquez sur **OK**.  
  
    Vous pouvez aussi cliquer sur le bouton **Options** dans la barre d'outils de la fenêtre **Comparaison de schémas** pour spécifier les objets à comparer, les types de différences à ignorer et d'autres paramètres.  
  
6.  Cliquez sur le bouton **Comparer** dans la barre d'outils de la fenêtre **Comparaison de schémas** pour démarrer la comparaison.  
  
    Lorsque la comparaison est terminée, les différences structurelles entre le projet et la base de données apparaissent dans le volet de **résultats** dans la partie supérieure de la fenêtre. Par défaut, les résultats de la comparaison regroupent toutes les différences par action (notamment, Supprimer, Modifier ou Ajouter). Le volet de **résultats** affiche une ligne pour chaque objet de base de données qui diffère dans les définitions de base de données. Chaque ligne identifie l'objet dans le schéma source ou cible (ou les deux) et l'action qui sera effectuée sur le schéma cible pour que l'objet cible soit identique à l'objet source.  Si un objet a été refactorisé et renommé ou déplacé vers un nouveau schéma, les noms source et cible sont différents et le nom source s'affiche en gras pour mettre en évidence la différence.  
  
    Par défaut la liste de résultats masque les objets qui sont identiques dans les deux schémas ou qui ne sont pas pris en charge pour la mise à jour (par exemple, les objets intégrés).  Cliquez sur les boutons de filtre dans la barre d'outils pour afficher ces objets.  
  
    Pour modifier la préférence de regroupement, cliquez sur la liste déroulante **Grouper les résultats** dans la barre d'outils.  Sélectionnez **Type** pour regrouper les résultats par type d'objet (par exemple, tables, affichages ou procédures stockées).  
  
7.  Recherchez la table `Products` dans le groupe `Tables`. Cliquez sur la ligne et notez que les définitions source et cible de la table s'affichent dans le volet **Définitions d'objet** avec les différences en surbrillance. Vous pouvez aussi développer la ligne de la table `Products` dans le volet de **résultats** pour examiner des éléments spécifiques de la table qui sont différents.  
  
8.  Par défaut, toutes les différences sont incluses dans l'étendue de l'action Mettre à jour la cible. Vous pouvez exclure les différences que vous ne souhaitez pas synchroniser. Pour cela, désactivez la colonne **Action** au centre de chaque ligne. Vous pouvez aussi cliquer avec le bouton droit sur une ligne dans le volet de schéma et sélectionner **Exclure**. Notez que la ligne apparaît immédiatement grisée. Lorsqu’il est temps de mettre à jour la base de données cible, cette ligne n’est pas prise en compte pour les modifications en attente.  
  
    Vous pouvez aussi cliquer avec le bouton droit sur une ligne de groupe et sélectionner **Exclure tout** ou **Inclure tout**, ce qui revient à désactiver ou activer toutes les différences dans ce groupe. Le regroupement des résultats par schéma s'avère utile pour inclure ou exclure toutes les modifications apportées à un schéma spécifique.  
  
    > [!WARNING]  
    > Si la ligne exclue possède des objets dépendants (par exemple, une ligne de **table** référencée par une ligne d'**affichage**), cette ligne est désactivée, mais pas sa case à cocher. Une fois toutes les lignes dépendantes de cette ligne désactivées, la ligne désactivée est désactivée. En outre, si une ligne est refactorisée (renommée ou déplacée vers un autre schéma), la case à cocher est désactivée pour cette ligne et ses lignes enfants dépendantes.  
    >   
    > Notez que si vous actualisez la comparaison, les différences que vous avez choisies d'ignorer seront ignorées.  
  
Pour mettre à jour le schéma de la cible, vous avez deux possibilités. Vous pouvez mettre à jour la cible directement à partir de la fenêtre **Comparaison de schémas** si la cible est une base de données ou un projet, ou générer un script de mise à jour si la cible est une base de données ou un fichier de base de données.  Un script généré s'affiche dans l'Éditeur Transact\-SQL, à partir duquel vous pouvez inspecter le script et l'exécuter par rapport à une base de données. Les procédures suivantes décrivent ces étapes en détail.  
  
> [!WARNING]  
> La mise à jour échoue, car la modification implique le changement d'une colonne NOT NULL en colonne NULL et provoque une perte de données. Si vous souhaitez continuer la mise à jour, cliquez sur le bouton **Options** (le cinquième à partir de la gauche) dans la barre d'outils Comparaison de schémas et désactivez l'option **Bloquer le déploiement incrémentiel en cas de perte de données**.  
  
### <a name="to-update-directly-in-the-schema-compare-window"></a>Pour mettre à jour directement dans la fenêtre Comparaison de schémas  
  
1.  Cliquez sur le bouton **Mettre à jour** dans la barre d'outils de la fenêtre Comparaison de schémas.  
  
2.  Examinez le script de modification généré. Vous pouvez enregistrer le script à l'aide du menu Fichier/Nouveau. Cela peut être pratique dans les situations où vous n'êtes pas autorisé à mettre à jour une base de données de production, auquel cas, vous pouvez donner le script à un administrateur de base de données en vue d'un déploiement ultérieur.  
  
3.  Si vous disposez des autorisations nécessaires pour mettre à jour la base de données, cliquez sur le bouton **Exécuter la requête** dans la barre d'outils du volet d'édition pour exécuter le script.  
  
### <a name="to-update-by-script"></a>Pour mettre à jour en exécutant un script  
  
1.  Cliquez sur le bouton **Générer un script** (le quatrième à partir de la gauche) dans la barre d'outils de la fenêtre Comparaison de schémas.  
  
    Le script généré s'affiche dans une nouvelle fenêtre de l'Éditeur Transact\-SQL.  
  
    > [!WARNING]  
    > Seuls les fichiers .dacpac générés par le processus d'instantané SSDT prennent en charge ce comportement.  Vous ne pouvez pas cibler un fichier .dacpac généré par les outils d'application de la couche Données SQL (DAC) ou l'infrastructure pour l'instant.  
  
2.  Examinez le script de modification généré. Vous pouvez enregistrer le script à l'aide de la commande de menu **Fichier/Enregistrer** ou Fichier/Enregistrer sous.  
  
    Un script enregistré peut être pratique dans les situations où vous n'êtes pas autorisé à mettre à jour une base de données de production. Dans ce cas, vous pouvez donner le script à un administrateur de base de données en vue d'un déploiement ultérieur.  
  
    Vous pouvez aussi connecter l'Éditeur Transact\-SQL à un serveur approprié et exécuter le script directement. Avant d'exécuter cette procédure, vous devez disposer des autorisations nécessaires pour créer ou mettre à jour la base de données. Si vous disposez des autorisations nécessaires pour mettre à jour la base de données, cliquez sur le bouton **Exécuter la requête** dans la barre d'outils du volet d'édition pour exécuter le script.  
  
3.  Cliquez sur le bouton **Connect** . Cette action établit la connexion au serveur actuel ou vous invite à entrer ou sélectionner un serveur dans la boîte de dialogue Se connecter au serveur.  Notez que le nom de la base de données est défini dans le script en tant que variable de commande.  
  
4.  Inspectez le script et, si nécessaire, apportez des modifications aux variables de commande qui définissent le nom de la base de données cible, le préfixe associé et les chemins d'accès de fichier.  
  
5.  Cliquez sur le bouton **Exécuter** dans la barre d'outil du volet d'édition pour exécuter cette requête.  
  
