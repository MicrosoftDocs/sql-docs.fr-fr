---
title: Extension de virtualisation de données
description: Extension de virtualisation de données pour Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: alayu, sstein, maghan
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: a428322867df8585160a3363d174b2cd92bd1f17
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052050"
---
# <a name="data-virtualization-extension-for-azure-data-studio"></a>Extension de virtualisation de données pour Azure Data Studio

L’extension de virtualisation de données pour Azure Data Studio assure la prise en charge de l’[Assistant table externe avec des source de données ODBC](../../relational-databases/polybase/data-virtualization.md).

## <a name="install-the-data-virtualization-extension"></a>Installer l’extension de virtualisation de données

Pour installer l’extension de virtualisation des données, visitez [Étendre les fonctionnalités d’Azure Data Studio](./add-extensions.md).

## <a name="changes-in-release-10"></a>Changements dans la version 1.0

* Extension renommée Virtualisation de données.
* Assistant Créer une table externe :
  * Inclut des notebooks guidés pour la virtualisation des sources MongoDB et Teradata.
  * Ajout d’une boîte de dialogue pour renseigner les variables dans les notebooks de virtualisation MongoDB et Teradata.

## <a name="changes-in-release-016"></a>Modifications dans la version 0.16

* Assistant Créer une table externe :
  * Amélioration de la gestion des erreurs lors du chargement de tables et de vues sur la page de mappage d’objets.

## <a name="changes-in-release-015"></a>Modifications dans la version 0.15

* Assistant Créer une table externe :
  *  Réduction du temps nécessaire pour charger les informations de table et de colonne dans la page de mappage d’objets.
  * Correction d’un bogue lors du chargement des informations d’identification incluses dans l’étendue de la base de données dans la page de détails de la connexion.
* Assistant Création d’une table externe à partir de fichiers CSV :
  * Augmentation de la taille de l’échantillon par défaut utilisée pour l’analyse PROSE.

## <a name="changes-in-release-0141"></a>Modifications dans la version 0.14.1

* Prise en charge de la prise en charge de la source de données CTP 3.1

## <a name="changes-in-release-0121"></a>Modifications dans la version 0.12.1

* Le type de connexion **Cluster Big Data SQL Server** a été supprimé dans cette version. Toutes les fonctionnalités précédemment disponibles à partir de la connexion Big Data SQL Server sont désormais disponibles dans la connexion SQL Server.
* La navigation HDFS se trouve dans le dossier **Data Services**
* Pour les notebooks, le noyau PySpark et d’autres noyaux Big Data fonctionnent quand ils sont connectés à l’instance principale SQL Server dans votre cluster Big Data SQL Server.
* Assistant Créer une table externe :
  * Prise en charge de la création d’une table externe à l’aide d’une source de données externe existante.
  * Améliorations des performances à travers l’assistant.
  * Amélioration de la gestion des noms d’objets avec des caractères spéciaux. Dans certains cas, ces derniers provoquaient l’échec de l’Assistant.
  * Améliorations de la fiabilité de la page Mappage d’objets.
  * Bases de données système supprimées - `DWConfiguration`, `DWDiagnostics`, `DWQueue` - dans la liste déroulante bases de données.
  * Prise en charge de la définition du nom de l’objet de format de fichier externe dans l'assistant **Création d’une table externe à partir de fichiers CSV**.
  * Ajout d’un bouton Actualiser à la première page de l’assistant **Créer une table externe à partir de fichiers CSV**.

## <a name="release-notes-v0110"></a>Notes de publication (v0.11.0)

* La prise en charge de Jupyter Notebook, en particulier la prise en charge des noyaux Python3 et Spark, a été déplacée dans Azure Data Studio. Cette extension n’est plus nécessaire pour pouvoir utiliser des notebooks.
* Plusieurs correctifs de bogues dans les assistants de données externes :
  * Les mappages de types Oracle ont été mis à jour pour correspondre aux modifications fournies dans SQL Server 2019 CTP 2.3.
  * Correction d’un problème entraînant la perte des nouveaux schémas saisis dans les contrôles de mappage de tables.
  * Correction d’un problème où la vérification d’un nœud de base de données dans les mappages de tables n’aboutissait pas à la vérification de toutes les tables et vues.

## <a name="release-notes-v0102"></a>Notes de publication (v0.10.2)

### <a name="sql-server-2019-support"></a>Prise en charge de SQL Server 2019

La prise en charge de SQL Server 2019 a été mise à jour. Après la connexion à une instance de cluster Big Data SQL Server, un nouveau dossier _Data Services_ s’affiche dans l’arborescence de l’explorateur. Ce dossier présente des points de lancement pour des actions telles que l’ouverture d’un nouveau notebook sur la connexion, l’envoi de travaux Spark et l’utilisation de HDFS. Pour certaines actions, telles que la _Création de données externes_ à partir d’un fichier/dossier HDFS, l’extension _SQL Server 2019_ doit être installée.

### <a name="notebook-support"></a>Prise en charge des notebooks

Nous avons apporté des mises à jour significatives à l’interface utilisateur du notebook. Notre objectif est de faciliter la lecture des notebooks partagés avec vous. Cela signifiait la suppression de toutes les zones de contour autour des cellules, sauf si elles sont sélectionnées ou survolées, l’ajout de la prise en charge du pointage pour les actions sur des cellules uniques sans avoir besoin de sélectionner une cellule, la clarification de l’état d’exécution en ajoutant le nombre d’exécutions, un bouton _arrêter l’exécution_ animé et bien plus encore. Nous avons également ajouté des raccourcis clavier pour _Nouveau notebook_ (`Ctrl+Shift+N`), _Exécuter la cellule_ (`F5`),_Nouvelle cellule de code_ (`Ctrl+Shift+C`), _Nouvelle cellule de texte_ (`Ctrl+Shift+T`). Nous allons faire en sorte que toutes les actions clés puissent être lancées par raccourci, faites-nous savoir ce qui vous manque !

Parmi les autres améliorations et correctifs figurent les suivants :

* Maintenant, l’extension _SQL Server 2019_ invite les utilisateurs à choisir un répertoire d’installation pour les dépendances Python. En outre, Python n’est plus inclus dans le `.vsix file`, ce qui réduit la taille globale de l’extension. Les dépendances Python prennent en charge les noyaux Spark et Python3.
* La prise en charge du lancement d’un nouveau notebook à partir de la ligne de commande a été ajoutée. Le lancement avec les arguments `--command=notebook.command.new --server=myservername` doit ouvrir un nouveau notebook et se connecter à ce serveur.
* Correctifs de performances pour les notebooks avec une longueur de code conséquente dans les cellules. Si les cellules de code se trouvent sur plus de 250 lignes, une barre de défilement est ajoutée.
* Prise en charge améliorée des fichiers `.ipynb`. Les versions 3 et ultérieures sont désormais prises en charge. 

    >[!Note]
    > Enregistrement des mises à jour de fichiers dans la version 4 ou ultérieure.

* Le paramètre utilisateur `notebook.enabled` a été supprimé maintenant que la visionneuse de notebooks intégrée est stable.
* Un thème à contraste élevé est désormais pris en charge avec un certain nombre de correctifs pour la disposition des objets dans ce cas.
* Correction du problème #3680 où les sorties affichaient parfois certains caractères `,,,` de manière incorrecte.
* Correction du problème #3602 qui entraînait la disparition de l’éditeur pour les cellules après avoir quitté Azure Data Studio.
* La prise en charge a été ajoutée pour utiliser des vues de grille pour le type MIME de sortie `application/vnd.dataresource+json`. Cela signifie que de nombreux notebooks qui l’utilisent (par exemple, en définissant `pd.options.display.html.table_schema` dans un Notebook Python) disposent de sorties tabulaires plus attrayantes.

#### <a name="known-issues"></a>Problèmes connus

* La boîte de dialogue d’installation de Python s’affiche à l’ouverture d’un notebook. Si vous annulez cette installation, les listes déroulantes Noyaux et Attacher à n’affichent pas les valeurs attendues. Une solution de contournement consiste à effectuer l’installation de Python.
* Quand un notebook est ouvert avec un noyau qui n’est pas pris en charge, les listes déroulantes Noyaux et _Attacher à_ amènent Azure Data Studio à cesser de répondre. Fermez Azure Data Studio et assurez-vous que vous utilisez un noyau pris en charge (Python3, Spark | R, Spark | Scala, PySpark, PySpark3).
* La liaison à l’interface utilisateur Spark échoue lors de l’utilisation de PySpark3 ou d’autres noyaux Spark sur le point de terminaison SQL Server. Pour résoudre ce problème, sélectionnez l’interface utilisateur Spark dans le tableau de bord ou connectez-vous à l’aide du type de connexion cluster Big Data SQL Server, car le lien hypertexte de l’interface utilisateur Spark est correct.

### <a name="extensibility-improvements"></a>Améliorations de l’extensibilité

Un certain nombre d’améliorations ont été apportées à cette version pour aider les extendeurs.

* Une nouvelle API `ObjectExplorerNodeProvider` permet aux extensions de contribuer aux dossiers sous SQL Server ou d’autres nœuds de connexion. C’est ainsi que le nœud `Data Services` est ajouté sous les instances SQL Server 2019, mais il peut être utilisé pour ajouter facilement la surveillance ou d’autres dossiers à l’interface utilisateur.
* Deux nouvelles valeurs de clé de contexte sont disponibles pour vous aider à afficher/masquer les contributions au tableau de bord.
  * `mssql:iscluster` indique s’il s’agit d’un cluster Big Data SQL Server 2019
  * `mssql:servermajorversion` possède la version serveur (15 pour SQL Server 2019, 14 pour SQL Server 2017, etc.). Cela peut s’avérer utile si les fonctionnalités ne doivent être affichées que pour SQL Server 2017 ou une version ultérieure, par exemple.

## <a name="release-notes-v080"></a>Notes de publication (v0.8.0)

*Notebooks* :

* L’ajout de cellules avant/après les cellules existantes est désormais pris en charge en cliquant sur le bouton de cellule « Autres actions »
* L’option **Ajouter une nouvelle connexion** a été ajoutée aux connexions dans la liste déroulante « Attacher à »
* Une commande **Réinstaller les dépendances du notebook** a été ajoutée pour faciliter les mises à jour des packages Python et résoudre les cas où l’installation a été interrompue par la fermeture de l’application. Elle peut être exécutée à partir de la palette de commandes (utilisez `Ctrl/Cmd+Shift+P` et saisissez `Reinstall Notebook Dependencies`)
* Le package Python PROSE a été mis à jour vers la version 1.1.0 et inclut un certain nombre de correctifs de bogues. Utilisez la commande **Réinstaller les dépendances du notebook** pour mettre à jour ce package
* Une commande **Effacer la sortie** est maintenant prise en charge en cliquant sur le bouton de cellule **Autres actions**
* Correction des problèmes signalés par les clients suivants :
  * La session du notebook ne peut pas démarrer sur Windows en raison de problèmes de chemin
  * Impossible de démarrer le notebook à partir du dossier racine d’un lecteur, par exemple C:\ ou D:\
  * [#2820](https://github.com/Microsoft/azuredatastudio/issues/2820) impossible de modifier les notebooks créés à partir d’ADS dans VS Code
  * La liaison de l’interface utilisateur Spark fonctionne maintenant lors de l’exécution d’un noyau Spark
  * « Packages gérés » renommés en « Packages d’installation »

*Créer des données externes* :

* Les messages d’erreur peuvent être copiés et ont été séparés entre un affichage résumé et une vue détaillée
* Amélioration de la disposition de l’interface utilisateur et amélioration de la fiabilité et de la gestion des erreurs
* Correction des problèmes signalés par les clients suivants :
  * Les tables avec des mappages de colonnes non valides sont indiquées comme étant désactivées et un avertissement explique l’erreur

## <a name="release-notes-v072"></a>Notes de publication (v0.7.2)

* Azure Resource Explorer est maintenant intégré à Azure Data Studio et a été supprimé de cette extension. Merci pour vos commentaires à ce sujet !
* Amélioration des performances des notebooks avec de nombreuses cellules Markdown.
* Dimensionnement automatiquement des cellules de code dans le notebook. Elles ont toujours une taille minimale basée sur la barre d’outils de la cellule.
* Notification à l’utilisateur lors de l’installation des dépendances du notebook. Sur Windows, en particulier, cela peut prendre beaucoup de temps, aussi les notifications s’affichent maintenant dans la vue Tâches.
* Prise en charge de la réinstallation des dépendances de notebook. Cela est utile si l’utilisateur a précédemment fermé Azure Data Studio lors de l’installation.
* Prise en charge de l’annulation de l’exécution des cellules dans le notebook.
* Amélioration de la fiabilité lors de l’utilisation de l’assistant Création de données externes, en particulier lorsque des erreurs de connexion se produisent.
* Blocage de l’utilisation de l’assistant Création de données externes si PolyBase n’est pas activé ou est en cours d’exécution sur le serveur cible.
* Correction de l’orthographe et de l’attribution des noms pour SQL Server 2019 et la création des données externes.
* Suppression d’un grand nombre d’erreurs dans la console de débogage d’Azure Data Studio.