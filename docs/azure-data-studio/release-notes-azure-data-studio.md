---
title: Notes de publication d’Azure Data Studio
description: Cet article contient des notes de publication pour les versions d’Azure Data Studio allant de novembre 2017 jusqu’à présent. Pour la plupart des problèmes résumés ici, il existe des liens menant à des informations supplémentaires.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 2/18/2021
ms.openlocfilehash: c666ae644200c1b2beefb7f73c8a819294831985
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836026"
---
# <a name="release-notes-for-azure-data-studio"></a>Notes de publication pour Azure Data Studio

**[Téléchargez et installez la dernière version !](./download-azure-data-studio.md)**

## <a name="february-2021"></a>Février 2021

18 février 2021 &nbsp; / &nbsp; version : 1.26.0

&nbsp;

| Modifier | Détails |
| ------ | ------- |
| Correctifs de bogues | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22February+2021+Release%22+is%3Aclosed). |
| Mise à jour d’extension(s) | [Dacpac](extensions/sql-server-dacpac-extension.md) <br/> [Kusto (KQL)](extensions/kusto-extension.md) </br> [MachineLearning](extensions/machine-learning-extension.md) </br> [Profiler](extensions/sql-server-profiler-extension.md) </br> [SchemaCompare](extensions/schema-compare-extension.md) </br> [SQLDatabaseProjects](extensions/sql-database-project-extension.md) |
| Nouvelles fonctionnalités Azure Arc | Plusieurs contrôleurs de données sont désormais pris en charge <br/> Nouvelles options de la boîte de dialogue de connexion comme le fichier de configuration Kube <br/> Améliorations du tableau de bord Postgres |
| Nouvelles fonctionnalités de Notebook | Heure de démarrage du serveur Jupyter amélioré de 50 % sur Windows <br/> Ajout de support pour modifier des livres Jupyter en cliquant avec le bouton de droite <br/> Ajout de support du paramétrage du notebook URI et [ajout de la documentation sur le paramétrage du notebook](https://docs.microsoft.com/sql/azure-data-studio/notebooks/notebooks-parameterization) |

## <a name="december-2020-hotfix"></a>Décembre 2020 (correctif logiciel)

10 février 2021 &nbsp; / &nbsp; version : 1.25.3

| Modifier | Détails |
| ------ | ------- |
| Correction du bogue [13899](https://github.com/microsoft/azuredatastudio/issues/13899) | Défilement vers les liens de renvoi appropriés dans Notebooks |
| Mise à niveau d’Electron pour incorporer des correctifs de bogues importants| N/A |

## <a name="december-2020"></a>Décembre 2020

9 décembre 2020 &nbsp; / &nbsp; version : 1.25.0

&nbsp;

| Modifier | Détails |
| ------ | ------- |
| Correctifs de bogues | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22December+2020+Release%22). |
| Mise à jour de l’extension Projets de base de données | Espaces de travail ajoutés et barre latérale améliorée |

## <a name="november-2020"></a>Novembre 2020

12 novembre 2020 &nbsp; / &nbsp; version : 1.24.0

&nbsp;

| Modifier | Détails |
| ------ | ------- |
| Correctifs de bogues | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2020+Release%22+is%3Aclosed). |
| Boîte de dialogue Connexion | Ajout d’un nouvel onglet Parcourir à la boîte de dialogue de connexion. |
| Mise à jour d’extension(s) | Publication de la mise à jour de l’extension Postgres. |
| Nouvelles fonctionnalités des notebooks | Ajout de nouvelles fonctionnalités à la prise en charge de SQL dans les notebooks. <br/> Ajout de nouvelles fonctionnalités à la prise en charge du paramétrage dans les notebooks. <br/>  Ajout de nouvelles fonctionnalités à la diffusion en continu des résultats pour les notebooks SQL. |
| Installation de Python | Le package PROSE a été supprimé de l’installation par défaut de Python. |

### <a name="known-issues-1240"></a>Problèmes connus (1.24.0)

| Nouvel élément | Détails | Solution de contournement |
|----------|---------|------------|
| Extension Azure Arc | [Problème connu :](https://github.com/microsoft/azuredatastudio/issues/13319) Le bouton « Exécuter un script sur un notebook » pour les déploiements Arc MIAA & PG n’effectue aucune validation de champ avant de créer les scripts du notebook. Cela signifie que si les utilisateurs entrent un mot de passe erroné dans les champs de confirmation du mot de passe, ils risquent de se retrouver avec un notebook affichant une valeur de mot de passe incorrecte.| Le bouton « Déployer » fonctionne comme prévu, ce qui signifie que les utilisateurs doivent l’utiliser à la place. |
| Explorateur d’objets | Les versions de ADS antérieures à 1.24.0 ont un changement cassant dans l’Explorateur d’objets en raison des modifications du moteur relatives au [pool SQL serverless Azure Synapse Analytics](/azure/synapse-analytics/sql/on-demand-workspace-overview). | Pour continuer à utiliser l’Explorateur d’objets dans Azure Data Studio avec le pool SQL serverless Azure Synapse Analytics, vous avez besoin d’Azure Data Studio 1.24.0 ou version ultérieure. |

Pour connaître les autres problèmes connus et pour envoyer vos commentaires à l’équipe produit, accédez à [Commentaires sur Azure Data Studio](https://github.com/microsoft/azuredatastudio).

## <a name="october-2020"></a>Octobre 2020

14 octobre 2020 &nbsp; / &nbsp; version : 1.23.0

&nbsp;

| Modifier | Détails |
| ------ | ------- |
| Azure SQL Edge | Prise en charge des objets Azure SQL Edge. |
| Correctifs de bogues | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is:issue+milestone:%22October+2020+Release%22+is:closed). |
| Bases de données| Prise en charge de la même référence de base de données. |
| Mises à jour des extensions | [Azure Arc](extensions/azure-arc-extension.md)</br>[azdata](../azdata/install/deploy-install-azdata.md)</br>[Machine Learning](extensions/machine-learning-extension.md)</br>[Kusto (KQL)](extensions/kusto-extension.md)</br>[Comparaison de schémas](extensions/schema-compare-extension.md)</br>SQL Assessment</br>[Projets SQL Database](extensions/sql-database-project-extension.md)</br>[SQL Server Import](extensions/sql-server-import-extension.md) |
| Nouvelles fonctionnalités de déploiement | Ajout de déploiements de machines virtuelles et de bases de données Azure SQL. |
| PowerShell | Ajout de la prise en charge de la diffusion en continu du noyau PowerShell. |

## <a name="september-2020-hotfix"></a>Septembre 2020 (correctif logiciel)

30 septembre 2020, &nbsp; / &nbsp; version : 1.22.1

&nbsp;

| Modifier | Détails |
| ------ | ------- |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub](https://github.com/microsoft/azuredatastudio/releases/tag/1.22.1). |

## <a name="september-2020"></a>Septembre 2020

22 septembre 2020, version &nbsp; / &nbsp; : 1.22.0

&nbsp;

| Modifier | Détails |
| ------ | ------- |
| Nouvelles fonctionnalités des notebooks | <br/> &bull; &nbsp; Prend en charge la nouvelle modification des cellules de texte en fonction de la mise en forme du texte enrichi et de la conversion transparente en la démarque, également appelée barre d’outils WYSIWYG (tel écrit, tel écran) <br/> &bull; &nbsp; Prend en charge le noyau Kusto <br/> &bull; &nbsp; Prend en charge l’épinglage des notebooks <br/> &bull; &nbsp; Ajout de la prise en charge de la nouvelle version des books Jupyter <br/> &bull; &nbsp; Raccourcis Jupyter améliorés <br/> &bull; &nbsp; Améliorations du chargement des performances |
| Extension des projets SQL Database | L’extension de projets de SQL Database apporte un développement de base de données basé sur le projet à Azure Data Studio. Dans cette version préliminaire, vous pouvez créer et publier des projets SQL à partir de Azure Data Studio. |
| Extension Kusto (KQL) | Offre des expériences Kusto natives dans Azure Data Studio pour l’exploration de données et l’analytique des données par rapport à une grande quantité de données de streaming en temps réel stockées dans Azure Data Explorer. Cette version préliminaire prend en charge la connexion et la navigation dans les clusters Azure Data Explorer, l’écriture de requêtes KQL et la création de notebooks avec le noyau Kusto. |
| Extension Azure Arc | Les utilisateurs peuvent essayer la version préliminaire publique d’Azure Arc via Azure Data Studio. notamment : <br/> &bull; &nbsp; Déployer un contrôleur de données <br/> &bull; &nbsp; Déployer Postgres <br/> &bull; &nbsp; Déployer Managed Instance pour Azure Arc <br/> &bull; &nbsp; Se connecter au contrôleur de données <br/> &bull; &nbsp; Accéder aux tableaux de bord de service de données <br/> &bull; &nbsp; Book Azure Arc Jupyter |
| Options de déploiement | <br/> &bull; &nbsp; Azure SQL Database Edge <br/> (Edge nécessite l’extension de déploiement Azure SQL Edge) |
| GA extension d’importation SQL Server | Annonce de GA de l’extension d’importation SQL Server, les fonctionnalités ne sont plus disponibles en version préliminaire. Cette extension facilite l’importation des fichiers csv/txt. En savoir plus sur l’extension dans [cet article](./extensions/sql-server-import-extension.md). |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2020+Release%22+is%3Aclosed) |

## <a name="august-2020"></a>Août 2020

12 août 2020 &nbsp; / &nbsp; version : 1.21.0

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Nouvelles fonctionnalités des notebooks | &bull; &nbsp; Déplacer des emplacements de cellules <br/> &bull; &nbsp; Convertir des cellules en cellule de texte ou cellule de code
| Sélecteur Jupyter Books | Les utilisateurs peuvent désormais choisir Jupyter Books dans les versions GitHub et y accéder facilement dans Azure Data Studio |
| Ajout d’une option de recherche à la section Notebooks | Les utilisateurs peuvent facilement rechercher du contenu dans l’ensemble de leurs notebooks et Jupyter Books |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22August+2020+Release%22+is%3Aclosed) |
| &nbsp; | &nbsp; |

## <a name="july-2020-hotfix"></a>Juillet 2020 (correctif)

17 juillet 2020 &nbsp; / &nbsp; version : 1.20.1

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Correction de bogue n°11372 : l’opération glisser-déposer sur une table dans l’Explorateur d’objets renvoie de manière incorrecte les noms de tables à la ligne | [N°11372](https://github.com/microsoft/azuredatastudio/issues/11372) |
| Correction de bogue n°11356 : le thème sombre est désormais le thème par défaut | [N°11356](https://github.com/microsoft/azuredatastudio/issues/11356) |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>Problème connu

- Certains utilisateurs ont signalé des erreurs de connexion à partir de la nouvelle version MMicrosoft.Data.SqlClient v2.0.0 incluse dans cette mise en production. Les utilisateurs ont constaté que [ces instructions](https://github.com/microsoft/azuredatastudio/issues/11367#issuecomment-659614111) permettaient d’établir une connexion

## <a name="july-2020"></a>Juillet 2020

15 juillet 2020 &nbsp; / &nbsp; version : 1.20.0

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Ajout d’une visite guidée des nouvelles fonctionnalités | À partir de la page d’accueil et de la palette de commandes, les utilisateurs peuvent maintenant lancer une visite guidée des fonctionnalités pour passer en revue les fonctionnalités couramment utilisées, notamment la section Connexions, la section Notebooks et la place de marché des extensions. |
| Nouvelles fonctionnalités des notebooks | &bull; &nbsp; Prise en charge des en-têtes dans la barre d’outils Markdown<br/> &bull; &nbsp; Aperçu du Markdown côte à côte dans les cellules de texte
| Glisser-déposer des colonnes et des tables dans l’Éditeur de requête | Les utilisateurs peuvent directement glisser-déposer des colonnes et des tables à partir de la section Connexions vers l’Éditeur de requête |
| Ajout de l’icône Compte Azure à la barre d’activités | Les utilisateurs peuvent désormais voir facilement où se connecter à Azure |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22July+2020+Release%22+is%3Aclosed) |
| &nbsp; | &nbsp; |

## <a name="june-2020"></a>Juin 2020

15 juin 2020 &nbsp; / &nbsp; version : 1.19.0

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Ajout d’Azure Data Studio à l’intégration du portail Azure | Les utilisateurs peuvent désormais démarrer directement dans le portail Azure à partir d’une connexion Azure SQL Database, d’Azure Postgres et bien plus encore. |
| Nouvelles fonctionnalités des notebooks | &bull;&nbsp; Nouvelle barre d’outils du notebook <br/> &bull; &nbsp; Nouvelle barre d’outils de modification de cellule <br/> &bull; &nbsp; Mises à jour de l’expérience utilisateur de l’Assistant des dépendances Python <br/> &bull; &nbsp; Meilleur espacement pour les notebooks |
| Annonce de l’extension de l’API SQL Assessment | Cette extension ajoute l’évaluation de bonnes pratiques SQL Server à ADS. Elle expose l’API SQL Assessment, auparavant uniquement disponible dans le module PowerShell SqlServer et SMO, pour vous permettre d’évaluer vos instances SQL Server et de recevoir des suggestions à leur sujet de la part de l’équipe SQL Server. Découvrez en plus sur l’API SQL Assessment et ce qu’elle peut faire [dans cet article.](../tools/sql-assessment-api/sql-assessment-api-overview.md) |
| [Améliorations de l’extension Machine Learning](./extensions/machine-learning-extension.md) | Prend désormais en charge Azure SQL Managed Instance. |
| Améliorations de l’extension de virtualisation de données | Prend désormais en charge MongoDB et Teradata |
| Correctifs de bogues d’extension Postgres | Azure MFA corrigée |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2020+Release%22+is%3Aclosed) |
| &nbsp; | &nbsp; |

## <a name="may-2020-hotfix"></a>Mai 2020 (correctif)

27 mai 2020 &nbsp; / &nbsp; version : 1.18.1

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Correction du bogue #10538 keybind « Exécuter la requête actuelle » ne se comporte plus comme prévu | [10538](https://github.com/microsoft/azuredatastudio/issues/10538)  |
| Correction du bogue #10537 Impossible d’ouvrir des fichiers sql existants ou nouveaux sur v1.18 | [10537](https://github.com/microsoft/azuredatastudio/issues/10537)  |
| &nbsp; | &nbsp; |

## <a name="may-2020"></a>Mai 2020

20 mai 2020 &nbsp; / &nbsp; version : 1.18.0

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Annonce de l’extension d’invite SQL Redgate | Cette extension vous permet de gérer les styles de formatage directement dans Azure Data Studio, afin de pouvoir créer et modifier vos styles sans quitter l’environnement de développement intégré (IDE). |
| Annonce de l’extension Machine Learning | Cette extension vous permet d’effectuer les opérations suivantes : <br/> &bull; &nbsp; Gérer des packages Python et R dans des services de machine Learning SQL Server avec Azure Data Studio.<br/> &bull; &nbsp; Utiliser le modèle ONNX pour faire des prédictions dans Azure SQL Edge.<br/> &bull; &nbsp; Afficher les modèles ONNX dans une base de données Azure SQL Edge. <br/> &bull; &nbsp; Importer des modèles ONNX à partir d’un fichier ou d’Azure Machine Learning dans une base de données Azure SQL Edge. <br/> &bull; &nbsp; Créer un notebook pour exécuter des expérimentations. |
| Nouvelles fonctionnalités des notebooks | &bull; &nbsp; Ajout du nouvel Assistant Dépendances Python pour faciliter l’installation des dépendances Python <br/> &bull; &nbsp; Ajout de la prise en charge du soulignement pour la barre d’outils Markdown |
| Paramétrage pour Always Encrypted | Vous permet d’exécuter des requêtes qui insèrent des colonnes de bases de données chiffrées, les mettent à jour ou filtrent en fonction de ces colonnes.|
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22May+2020+Release%22+is%3Aclosed) |
| &nbsp; | &nbsp; |

## <a name="april-2020-hotfix"></a>Avril 2020 (correctif)

30 avril 2020 &nbsp; / &nbsp; version : 1.17.1

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Correction du bogue 10197 : Impossible de se connecter avec MFA | [10197](https://github.com/microsoft/azuredatastudio/issues/10197)  |
| &nbsp; | &nbsp; |

## <a name="april-2020"></a>Avril 2020

27 avril 2020 &nbsp; / &nbsp; version : 1.17.0

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Page d’accueil améliorée | Mise à jour de l’interface utilisateur dans la page d’accueil pour rendre les actions courantes plus visibles et mettre en évidence les extensions. |
| Nouvelles fonctionnalités des notebooks | &bull; &nbsp; Ajout de la barre d’outils Markdown lors de la modification de cellules de texte pour faciliter l’écriture avec Markdown <br/> &bull; &nbsp; Restructuration du viewlet Jupyter Books en viewlet Notebooks où vous pouvez gérer à la fois des books et des notebooks Jupyter <br/>&bull; &nbsp; Ajout de la prise en charge des graphiques persistants lors de l’enregistrement d’un notebook <br/> &bull; &nbsp; Ajout de la prise en charge de KQL magic dans les notebooks Python|
| Tableaux de bord améliorés | Les tableaux de bord dans Azure Data Studio ont été mis à jour avec les derniers modèles de conception, notamment une barre d’outils actions. Ce changement s’applique également à de nombreuses extensions. |
| Ajout de l’intégration Cloud Shell dans la vue Azure. | |
| Prise en charge d’Always Encrypted et d’Always Encrypted avec enclaves sécurisées. | |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22) |
| &nbsp; | &nbsp; |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22) |
| &nbsp; | &nbsp; |

## <a name="march-2020"></a>Mars 2020

18 mars 2020 &nbsp; / &nbsp; version : 1.16.0 

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Ajout de la prise en charge des graphiques dans les notebooks SQL | Quand une requête SQL est en cours d’exécution dans une cellule de code, les utilisateurs peuvent désormais créer et enregistrer des graphiques. |
| Ajout de l’expérience de création de books Jupyter | Les utilisateurs peuvent désormais créer leurs propres books Jupyter à l’aide d’un notebook. |
| Ajout de la prise en charge d’Azure AD pour l’extension Postgres | |
| Correction de nombreux bogues d’accessibilité | [Liste des bogues d’accessibilité](https://github.com/microsoft/azuredatastudio/issues?page=1&q=is%3Aissue+is%3Aclosed+milestone%3A%22S360+-+Accessibility%22+label%3AA11y_AzureDataStudio) |
| Mise à jour de VS Code vers 1.42 | Cette version comprend les mises à jour de VS Code à partir des 3 versions précédentes. Lisez les [notes de publication](https://code.visualstudio.com/updates/v1_42) correspondantes pour en savoir plus. |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22March+2020%22+is%3Aclosed) |
| &nbsp; | &nbsp; |

## <a name="february-hotfix"></a>Février (correctif)

19 février 2020 &nbsp; / &nbsp; version : 1.15.1

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Correction du bogue n° 9149 : Afficher les connexions actives | [N° 9149](https://github.com/microsoft/azuredatastudio/issues/9149)  |
| Correction du bogue n° 9061 : La grille Modifier les données n’est pas correctement redimensionnée quand le volet SQL est affiché ou masqué. | [N° 9061](https://github.com/microsoft/azuredatastudio/issues/9061)  |
| &nbsp; | &nbsp; |

## <a name="february-2020"></a>Février 2020

13 février 2020 &nbsp; / &nbsp; version : 1.15.0

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Nouvelle amélioration de la connexion à Azure | L’expérience de connexion à Azure a été améliorée et est désormais plus fluide, notamment parce qu’il n’est plus nécessaire de copier et coller le code de l’appareil. |
| Prise en charge de la recherche dans les notebooks | Les utilisateurs peuvent désormais utiliser Ctrl + F dans un notebook. La recherche dans les notebooks prend en charge les recherches ligne par ligne dans le code et les cellules de texte. |
| Mise à jour de VS Code vers la version 1.42 à partir des versions 1.38 et ultérieures | Cette version comprend les mises à jour de VS Code à partir des 3 versions précédentes. Lisez les [notes de publication](https://code.visualstudio.com/updates/v1_42) correspondantes pour en savoir plus. |
| Correction du problème d’[« écran blanc/vide »](https://github.com/microsoft/azuredatastudio/issues/8775) signalé par de nombreux utilisateurs. | |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22February+2020%22) |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>Problème connu

- Les utilisateurs sur macOS Catalina devront cliquer avec le bouton droit sur Azure Data Studio, puis cliquer sur Ouvrir.

## <a name="december-2019-hotfix"></a>Décembre 2019 (correctif logiciel)

26 décembre 2019 &nbsp; / &nbsp; version : 1.14.1

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Correction du bogue n° 8747 (échec du développement de l’Explorateur d’objets) | [#8747](https://github.com/microsoft/azuredatastudio/issues/8747)  |
| &nbsp; | &nbsp; |

## <a name="december-2019"></a>Décembre 2019

19 décembre 2019 &nbsp; / &nbsp; version : 1.14.0 

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Modification de la liste déroulante de connexion Attacher à dans les notebooks pour que seule la connexion actuellement active soit indiquée | [#8129](https://github.com/microsoft/azuredatastudio/issues/8129) |
| Ajout du paramètre bigdatacluster.ignoreSslVerification pour que les erreurs de vérification TLS/SSL puissent être ignorées lors de la connexion à un BDC | [#8582](https://github.com/microsoft/azuredatastudio/pull/8582) |
| Autorisation de la modification de la saveur de langage par défaut pour les éditeurs de requêtes hors connexion | [#8419](https://github.com/microsoft/azuredatastudio/pull/8419) |
| Statut de disponibilité générale pour les fonctionnalités du cluster Big Data/SQL 2019 | [#8269](https://github.com/microsoft/azuredatastudio/issues/8269) |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/milestone/44?closed=1) |
| &nbsp; | &nbsp; |


## <a name="november-2019-hotfix"></a>Novembre 2019 (correctif logiciel)

15 novembre 2019 &nbsp; / &nbsp; version : 1.13.1

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Correction du bogue n° 8210 (résultats des opérations copier-coller désordonnés) |  |
| &nbsp; | &nbsp; |

## <a name="november-2019"></a>Novembre 2019

4 novembre 2019 &nbsp; / &nbsp; version : 1.13.0 

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Prise en charge de SQL Server 2019 | &bull; &nbsp; Déployer un cluster Big Data SQL Server 2019 avec l’Assistant Déploiement BDC <br/>&bull; &nbsp; Gérer l’intégrité du cluster avec le tableau de bord du contrôleur <br/>&bull; &nbsp; Gérer les listes de contrôle d’accès HDFS à l’aide de la boîte de dialogue des listes de contrôle d’accès de sécurité <br/> &bull; &nbsp; Ajouter des montages à l’aide de la boîte de dialogue Hiérarchisation HDFS <br/> &bull; &nbsp; Résoudre les problèmes à l’aide du book Jupyter intégré, guide SQL Server 2019 <br/> &bull; &nbsp; Extension de virtualisation de données renommée Extension SQL vNext <br/> &bull; &nbsp; Ajout de la prise en charge de Teradata et de Mongo dans l’Assistant Table externe|
| Nouvelles fonctionnalités des notebooks | &bull; &nbsp; Annonce des notebooks PowerShell <br/> &bull; &nbsp; Annonce des cellules de code réductibles <br/>&bull; &nbsp; Améliorations des performances dans Notebooks <br/> &bull; &nbsp; Consultez la liste complète des améliorations [ici](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed+label%3A%22Area+-+Notebooks%22) |
| Annonce des livres Jupyter  | Les livres Jupyter sont un ensemble de notebooks et de fichiers Markdown organisés dans une table des matières. |
| Nouvel Assistant Déploiement de SQL Server  | Prend désormais en charge le déploiement de : <br/> &bull; &nbsp; SQL Server 2019 sur Windows <br/> &bull; &nbsp; SQL Server 2017 sur Windows <br/> &bull; &nbsp; SQL Server 2019 sur Docker <br/> &bull; &nbsp; SQL Server 2017 sur Docker |
| Annonce de l’extension de comparaison de schémas en disponibilité générale| &bull; &nbsp; Mode SQLCMD <br/> &bull; &nbsp; Prise en charge de la localisation <br/> &bull; &nbsp; Correctifs d’accessibilité <br/> &bull; &nbsp; Bogues de sécurité  |
| Annonce de l’extension dacpac SQL Server en disponibilité générale| <br/> &bull; &nbsp; Prise en charge de la localisation <br/> &bull; &nbsp; Correctifs d’accessibilité <br/> &bull; &nbsp; Bogues de sécurité |
| Annonce de l’extension Visual Studio IntelliCode | Visual Studio IntelliCode prend désormais en charge SQL, ce qui permet d’obtenir des suggestions plus intelligentes de mots clés réservés. |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed) |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix-2"></a>Octobre 2019 (correctif logiciel 2)

11 octobre 2019 &nbsp; / &nbsp; version : 1.12.2

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Désactiver le démarrage automatique de la gestion des erreurs en mode inspection |  |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix"></a>Octobre 2019 (correctif logiciel)

8 octobre 2019 &nbsp; / &nbsp; version : 1.12.1

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Problèmes résolus pour que les guillemets et les barres obliques inverses dans Notebooks s’échappent correctement. |  |
| &nbsp; | &nbsp; |

## <a name="october-2019"></a>2 octobre 2019

2 octobre 2019 &nbsp; / &nbsp; version : 1.12.0

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Version de l’extension Historique des requêtes | L’extension Historique SQL enregistre toutes les requêtes passées exécutées dans une session Azure Data Studio et les liste dans l’ordre d’exécution. Les utilisateurs peuvent ouvrir la requête, l’exécuter, la supprimer, suspendre l’historique des requêtes ou supprimer toutes les entrées de l’historique des requêtes. |
| Nouveaux résultats d’opérations copier/coller | Nous avons ajouté des moyens supplémentaires d’effectuer des opérations copier/coller des résultats à partir de la grille des résultats. |
| Mise à jour de l’extension PowerShell |  |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/milestone/42?closed=1) |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problèmes connus

- Notebooks
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) Rare cas de sérialisation incorrecte du notebook

## <a name="september-2019"></a>Septembre 2019

10 septembre 2019 &nbsp; / &nbsp; version : 1.11.0 

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Activer le mode SQLCMD | L’éditeur de requête prend désormais en charge le basculement du mode SQLCMD pour écrire et modifier des requêtes en tant que scripts SQLCMD. |
| Extension de la communauté : Query Editor Boost | Query Editor Boost est une extension open source axée sur l’amélioration de l’éditeur de requête d’Azure Data Studio pour les utilisateurs qui écrivent fréquemment des requêtes. &bull; &nbsp; Enregistrer la requête actuelle comme extrait de code <br/>&bull; &nbsp; Basculer les bases de données à l’aide de Ctrl+U <br/> &bull; &nbsp; Nouvelle requête à partir d’un modèle <br/> &bull; &nbsp; Consultez la liste complète des améliorations [ici](https://github.com/dzsquared/query-editor-boost) |
| Améliorations des notebooks | &bull; &nbsp; Améliorations des performances pour la prise en charge de fichiers de notebook plus grands <br/> &bull; &nbsp; Consultez la liste complète des améliorations [ici](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2019+Release%22+label%3A%22Area%3A+Notebooks%22+is%3Aclosed) |
| Visual Studio Code version d’août fusionnée 1.38 | Vous trouverez les dernières améliorations [ici](https://code.visualstudio.com/updates/v1_38). |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1) |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problèmes connus

- Notebooks
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) Rare cas de sérialisation incorrecte du notebook

## <a name="august-2019"></a>Août 2019

15 août 2019 &nbsp; / &nbsp; version : 1.10.0 

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Publication de l’extension SandDance 1.3.1 | &bull; &nbsp; Détection intelligente des graphiques <br/>&bull; &nbsp; Visualisations 3D <br/> &bull; &nbsp; Filtrage des données |
| Améliorations des notebooks | &bull; &nbsp; Ajouter du code ou une cellule de texte inline <br/>&bull; &nbsp; Ajout de la possibilité de cliquer avec le bouton droit sur la grille de résultats SQL pour enregistrer les résultats aux formats CSV, JSON, etc. <br/> &bull; &nbsp; Amélioration des performances de chargement des notebooks pour accélérer le chargement JSON <br/> &bull; &nbsp; Consultez la liste complète des améliorations [ici](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+label%3A%22Area%3A+Notebooks%22+milestone%3A%22August+2019+Release%22+is%3Aclosed) |
| Prise en charge de SQL Server 2019 |  Cette version inclut la prise en charge d’autres fonctionnalités de cluster Big Data SQL Server 2019, notamment : <br/> &bull; &nbsp; Réduction du temps nécessaire pour charger les informations de table et de colonne dans la page de mappage d’objets. <br/> &bull; &nbsp; Correction d’un bogue lors du chargement des informations d’identification incluses dans l’étendue de la base de données dans la page de détails de la connexion. <br/> &bull; &nbsp; Augmentation de la taille de l’échantillon par défaut utilisée pour l’analyse PROSE. | 
| L’extension dacpac prend désormais en charge Azure AD | 
| Visual Studio Code version de juillet fusionnée 1.37 | Vous trouverez les dernières améliorations [ici](https://code.visualstudio.com/updates/v1_37). |
| Bogues et problèmes résolus | Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1) |
| &nbsp; | &nbsp; |

## <a name="july-2019"></a>Juillet 2019

11 juillet 2019 &nbsp; / &nbsp; version : 1.9.0 

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Publication de l’extension SentryOne Plan Explorer | Notre partenaire Microsoft estimé, SentryOne, va proposer son [extension SentryOne Plan Explorer pour Azure Data Studio](https://www.sentryone.com/products/sentryone-plan-explorer-extension-azure-data-studio). <br> Il s’agit d’une extension gratuite qui fournit des diagrammes de plan améliorés pour les requêtes exécutées dans Azure Data Studio, avec des algorithmes de disposition optimisés et un codage de couleurs intuitif pour aider à identifier rapidement les opérateurs les plus coûteux qui affectent les performances des requêtes. Pour en savoir plus sur l’extension, consultez le billet de blog de SentryOne [ici](https://sqlperformance.com/2019/07/sentryone/plan-explorer-extension-azure-data-studio). |
| Nouvelles fonctionnalités à venir pour la comparaison de schémas | &bull; &nbsp; Prise en charge des fichiers de comparaison de schémas (.SCMP) <br/>&bull; &nbsp; Prise en charge de l’annulation de comparaison de schémas <br/>&bull; &nbsp; Vous trouverez les modifications complètes [ici](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed+)|
| Améliorations des notebooks | &bull; &nbsp; Prise en charge de Plotly Python <br/>&bull; &nbsp; Ouvrir un notebook à partir du navigateur <br/> &bull; &nbsp; Boîte de dialogue de gestion des packages Python <br/> &bull; &nbsp; Améliorations des performances et du Markdown <br/> &bull; &nbsp; Mise à jour des raccourcis clavier <br/>  &bull; &nbsp; Vous trouverez des correctifs de bogues et des fonctionnalités mineures [ici](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22+) |
| Prise en charge de SQL Server 2019 |  Cette version inclut la prise en charge d’autres fonctionnalités de cluster Big Data SQL Server 2019, notamment : <br/> &bull; &nbsp; Le tableau de points de terminaison de service dans le tableau de bord de gestion qui liste tous les services clés du cluster. <br/> &bull; &nbsp; Le notebook d’état du cluster montre comment interroger et résoudre les problèmes d’état du cluster dans tous les services et pods.| 
| Modules linguistiques mis à jour disponibles| Il existe maintenant 10 modules linguistiques disponibles dans le marketplace du gestionnaire d’extensions. Il vous suffit de rechercher la langue en question à l’aide du marketplace d’extensions et de l’installer. Une fois que vous avez installé la langue sélectionnée, Azure Data Studio vous invite à redémarrer avec la nouvelle langue. |
| Mise à jour de SQL Server Profiler | L’extension de profil SQL Server a été mise à jour pour inclure de nouvelles fonctionnalités, notamment : <br/> &bull; &nbsp; Filtrage par nom de base de données <br/> &bull; &nbsp; Prise en charge du copier/coller <br/> &bull; &nbsp; Enregistrer/charger un filtre <br/>Vous trouverez une liste complète des améliorations apportées à l’extension SQL Server Profiler [ici](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aclosed+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+SQL+Profiler%22+).  |
| Visual Studio Code version de mai fusionnée 1.35 | Vous trouverez les dernières améliorations [ici](https://code.visualstudio.com/updates/v1_35). |
| Bogues et problèmes résolus | Dans les versions précédentes d’Azure Data Studio, si une base de données utilisateur était sélectionnée lors de la connexion à partir de la boîte de dialogue de connexion, l’entrée de l’Explorateur d’objets obtenue était entièrement limitée à cette base de données unique. À compter de cette version, ce comportement est modifié de sorte que les propriétés au niveau du serveur s’affichent également dans l’Explorateur d’objets. <br/> Pour obtenir la liste complète des correctifs, consultez [Bogues et problèmes sur GitHub.](https://github.com/microsoft/azuredatastudio/milestone/35?closed=1) |
| &nbsp; | &nbsp; |

## <a name="june-2019"></a>Juin 2019

6 juin 2019 &nbsp; / &nbsp; version : 1.8.0

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Publication de l’extension des serveurs de gestion centralisée (CMS) | Les serveurs de gestion centralisée stockent une liste d'instances de SQL Server. Cette liste est organisée en un ou plusieurs groupes de serveurs d'administration centralisée. Les utilisateurs peuvent se connecter à leurs propres serveurs CMS existants et gérer leurs serveurs, par exemple avec l’ajout et la suppression de serveurs. Pour en savoir plus, consultez [ceci](../relational-databases/administer-multiple-servers-using-central-management-servers.md) |
| Publication des extensions de l’outil d’administration de base de données pour Windows | Cette extension lance deux des expériences les plus utilisées dans SQL Server Management Studio en provenance de Azure Data Studio. Les utilisateurs peuvent cliquer avec le bouton droit sur de nombreux objets différents (tels que des bases de données, des tables, des colonnes, des vues, etc.) et sélectionner des propriétés pour afficher la boîte de dialogue Propriétés SSMS pour cet objet. En outre, les utilisateurs peuvent cliquer avec le bouton droit sur une base de données et sélectionner Générer des scripts pour lancer l’Assistant Génération de scripts SSMS. 
| Améliorations de la comparaison de schémas | &bull; &nbsp; Options d’exclusion/inclusion ajoutées <br/>&bull; &nbsp; Générer le script ouvre le script après sa génération <br/>&bull; &nbsp; Barres de défilement doubles supprimées  <br/>&bull; &nbsp; Améliorations de la mise en forme et de la mise en page <br/>&bull; &nbsp; Vous trouverez les modifications complètes [ici](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed)|
| Déplacement de la section Messages dans son propre onglet | Lorsque les utilisateurs exécutaient des requêtes SQL, les résultats et les messages s’affichaient dans des panneaux empilés. Ils se trouvent désormais dans des onglets séparés dans un panneau, comme dans SSMS. |
| Améliorations du notebook SQL | &bull; &nbsp; Les utilisateurs peuvent désormais choisir d’utiliser leurs propres installations Python 3 ou Anaconda dans les notebooks <br/>&bull; &nbsp; Plusieurs correctifs de stabilité + ajustements/finitions <br/> &bull; &nbsp; Consultez la liste complète des améliorations [ici](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22)|
| Visual Studio Code version d’avril fusionnée 1.34 | Vous trouverez les dernières améliorations [ici](https://code.visualstudio.com/updates/v1_34) |
| Bogues et problèmes résolus. | Consultez [Bogues et problèmes, sur GitHub](https://github.com/microsoft/azuredatastudio/milestone/32?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problèmes connus

- Extensions de l’outil d’administration de base de données pour Windows
    - Impossible de lancer les propriétés à partir du nœud de serveur déconnecté
    - Impossible de lancer les propriétés des serveurs Azure
    - Certains objets n’ont pas de boîtes de dialogue de propriétés
    - Le démarrage des dialogues prend beaucoup de temps
    - Erreurs lors du lancement de serveurs avec certains types de connexions (par exemple Azure AD)
- Notebooks
    - [5838](https://github.com/microsoft/azuredatastudio/issues/5838) Autoriser les utilisateurs à utiliser le Python du système pour les notebooks
- Comparaison de schémas
    - [5804](https://github.com/microsoft/azuredatastudio/issues/5804) Les tâches Comparer les schémas affichent le menu contextuel d’annulation par défaut, qui ne fait rien

## <a name="may-2019"></a>Mai 2019

8 mai 2019 &nbsp; / &nbsp; version : 1.7.0 

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Publication de l’extension de comparaison de schémas | La comparaison de schémas est une fonctionnalité bien connue de SQL Server Data Tools (SSDT), et son principal cas d’utilisation est la comparaison et la visualisation des différences entre les bases de données et les fichiers .dacpac, et l’exécution d’actions pour les rendre identiques. |
| Déplacement de la vue de tâche déplacée vers la fenêtre Sortie | Les utilisateurs peuvent désormais afficher l’état des tâches de longue durée telles que la sauvegarde, la restauration et la comparaison de schémas dans l’affichage des tâches de la fenêtre Sortie
| Page d’accueil ajoutée | &bull; &nbsp; Liens vers des actions courantes comme Nouvelle requête, Nouveau fichier ou Nouveau notebook <br/>&bull; &nbsp; Liens vers la documentation et GitHub |
| Améliorations du notebook SQL | &bull; &nbsp; Améliorations du rendu du Markdown, notamment avec une meilleure prise en charge des notes et des tables <br/>&bull; &nbsp; Améliorations de la convivialité de la barre d’outils <br/>&bull; &nbsp; Les liens Markdown pour les notebooks approuvés n’ont plus besoin de Cmd/Ctrl + clic ; vous pouvez directement cliquer dessus <br/>&bull; &nbsp; Améliorations du nettoyage des processus Jupyter après fermeture des notebooks et réduction des erreurs lors du démarrage simultané de plusieurs notebooks <br/>&bull; &nbsp; Améliorations des connexions aux notebooks SQL pour s’assurer que des erreurs ne se produisent pas lors de l’exécution de 2 notebooks sur la même base de données <br/>&bull; &nbsp; Améliorations apportées au défilement automatique des notebooks vers la cellule en cours d’exécution quand vous cliquez sur le bouton Exécuter les cellules de la barre d’outils <br/>&bull; &nbsp; Amélioration générale de la stabilité et des performances |
| Bogues et problèmes résolus. | Consultez [Bogues et problèmes, sur GitHub](https://github.com/microsoft/azuredatastudio/milestone/31?closed=1). |
| &nbsp; | &nbsp; |

## <a name="april-2019"></a>Avril 2019

18 avril 2019 &nbsp; / &nbsp; version : 1.6.0 

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Onglet **Serveurs** renommé en **Connexions** | |
| Déplacement d’Azure Resource Explorer en tant que viewlet Azure sous Connexions | Les utilisateurs peuvent désormais afficher leurs instances SQL Azure via la viewlet Azure dans la vue Connexions et développer pour afficher les objets sous chaque serveur ou base de données.|
| Améliorations du notebook SQL | &bull; &nbsp; Ajout d’un bouton dans la barre d’outils pour effacer la sortie de toutes les cellules <br/>&bull; &nbsp; Ajout d’un bouton sur la barre d’outils pour exécuter toutes les cellules <br/>&bull; &nbsp; Nom de connexion fixe au lieu du nom du serveur (si défini) dans la liste déroulante Attacher à <br/>&bull; &nbsp; Correctif pour les images dans le Markdown qui ne sont pas rendues lors de l’utilisation de chemins d’images relatifs <br/>&bull; &nbsp; Amélioration des fonctionnalités dans les grilles de notebook grâce à l’ajout du redimensionnement automatique de la taille des colonnes par double-clic et à une meilleure prise en charge de la molette de souris <br/>&bull; &nbsp; Améliorations apportées à la gestion des erreurs et à la résilience de l’installation lors de l’installation de Python par le biais de notebooks <br/>&bull; &nbsp; Améliorations apportées à la fonctionnalité « Sélectionner tout » lors de la sélection de cellules de notebook <br/>&bull; &nbsp; Améliorations des connexions de notebook pour empêcher la fermeture d’un notebook et l’impact sur une connexion de l’Explorateur d’objets <br/>&bull; &nbsp; Amélioration de l’expérience du notebook pour montrer un message à l’utilisateur quand le notebook est déconnecté et nécessite une connexion pour exécuter des cellules<br/>&bull; &nbsp; Amélioration de la prise en charge des notebooks non enregistrés à réalimenter dans ADS quand ADS est à nouveau démarré |
| Bogues et problèmes résolus. | Consultez [Bogues et problèmes, sur GitHub](https://github.com/Microsoft/azuredatastudio/milestone/26?closed=1). |
| &nbsp; | &nbsp; |

## <a name="march-2019-hotfix"></a>Mars 2019 (correctif logiciel)

22 mars 2019 &nbsp; / &nbsp; version : 1.5.2 &nbsp; / &nbsp; version de correctif logiciel

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Correction de certains problèmes détectés dans la version 1.5.1. | Consultez [la version de correctif logiciel de mars sur GitHub](https://github.com/Microsoft/azuredatastudio/milestone/28).<br/> <br/>&bull; &nbsp; Résolution du problème où l’utilisateur ne pouvait pas fermer un notebook ouvert à partir de la tâche « Ouvrir le notebook » dans le tableau de bord <br/>&bull; &nbsp; Correction du problème où le fichier JSON du notebook avait un } supplémentaire après enregistrement <br/>&bull; &nbsp; Résolution d’un problème où les grilles de notebook ne répondaient pas aux changements de thème <br/>&bull; &nbsp; Correction du problème où le chemin complet du notebook était affiché dans l’en-tête de l’onglet. Désormais, seul le nom de fichier est affiché. |
| &nbsp; | &nbsp; |

## <a name="march-2019"></a>Mars 2019

18 mars 2019 &nbsp; / &nbsp; version : 1.5.1

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Ajout de [l’extension PostgreSQL pour Azure Data Studio](./extensions/postgres-extension.md) | Fonctionnalités prises en charge : <br/>&bull; &nbsp; Boîte de dialogue Connexion <br/>&bull; &nbsp; Explorateur d’objets <br/>&bull; &nbsp; Éditeur de requête <br/>&bull; &nbsp; Graphiques <br/>&bull; &nbsp; Tableaux de bord <br/>&bull; &nbsp; Extraits de code <br/>&bull; &nbsp; Modification des données <br/>&bull; &nbsp; Notebooks |
| Ajout de notebooks SQL | Ajout de la prise en charge du noyau SQL à la visionneuse de notebook intégrée : <br/>&bull; &nbsp; Prise en charge de T-SQL <br/>&bull; &nbsp; Prise en charge de PGSQL |
| Ajout de l’extension PowerShell | Apporte l’expérience de [l'extension PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) de VS Code.  |
| Ajout de l’extension dacpac SQL Server  | Place l’assistant application de la couche Données de l’extension d’importation SQL Server dans une nouvelle extension.  |
| Ajout de l’extension de la communauté QueryPlan.show | Ajoute la prise en charge de l’intégration pour visualiser les plans de requête  |
| Extension SQL Server 2019 en préversion mise à jour | &bull; &nbsp; La prise en charge de Jupyter Notebook, en particulier les noyaux Python3 et Spark, a été déplacée dans l’outil Azure Data Studio principal. <br/>&bull; &nbsp; Résolution des bogues dans l’assistant de données externes |
| Bogues et problèmes résolus. | Consultez [Bogues et problèmes, sur GitHub](https://github.com/Microsoft/azuredatastudio/milestone/25?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problèmes connus
- [#4427](https://github.com/Microsoft/azuredatastudio/issues/4427) : Le fait de cliquer sur exécuter sur la cellule avant que le noyau soit prêt pour Spark génère une solution erreur irrécupérable **Solution de contournement** : Attendre que les noyaux soient chargés jusqu’à l’exécution des cellules
- [#4493](https://github.com/Microsoft/azuredatastudio/issues/4493) : ADS lancé à partir de SSMS à l’aide de l’authentification SQL demande à l’utilisateur un mot de passe **Solution de contournement :** Utilisez l’authentification Windows pour l’instant. 
- [#4494](https://github.com/Microsoft/azuredatastudio/issues/4494) : Impossible d’installer la fonctionnalité de notebook SQL <br/>
**Solution de contournement :** Suivez les étapes de la solution de contournement [ici](https://github.com/Microsoft/azuredatastudio/issues/4494#issuecomment-473043832). 
- [#4503](https://github.com/Microsoft/azuredatastudio/issues/4503) : Azure Data Studio ne peut pas être ouvert directement à partir du dossier Téléchargements (Mac) <br />
**Solution de contournement :** Redémarrez l’ordinateur après avoir décompressé l’application. Le problème sera étudié. 
- [#4539](https://github.com/Microsoft/azuredatastudio/issues/4539) :  Enregistrer sous pour un notebook fait perdre le contexte de connexion <br />
**Solution de contournement :** Sera résolu dans la prochaine version. 
- [#4458](https://github.com/Microsoft/azuredatastudio/issues/4458) : L’extraction de dacpac fait planter SqlToolsService si une version non valide est utilisée <br/>
**Solution de contournement :** Redémarrez Azure Data Studio et assurez-vous que la version correcte est utilisée.
- Les icônes du nouveau notebook et du notebook ouvert sont perdues <br/>
**Solution de contournement :** Le type de connexion hérité est déconseillé. Nous vous recommandons de vous connecter au point de terminaison SQL Server pour avoir toutes les actions (Nouveau notebook, Nouveau travail Spark) comme prévu. 

## <a name="february-2019"></a>Février 2019

13 février 2019 &nbsp; / &nbsp; version : 1.4.5

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Ajout du pack d’extension **Pack d’administration pour SQL Server**. | Il est ainsi plus facile d’installer les extensions SQL Server liées à l’administration. notamment :<br/>&bull; &nbsp; [SQL Server Agent](./extensions/sql-server-agent-extension.md)<br/>&bull; &nbsp; [SQL Server Profiler](./extensions/sql-server-profiler-extension.md)<br/>&bull; &nbsp; [SQL Server Import](./extensions/sql-server-import-extension.md) |
| Ajout de la prise en charge du filtrage des événements étendus dans l’extension Profiler. | &nbsp; |
| Ajout de la fonctionnalité Enregistrer au format XML qui permet d’enregistrer les résultats T-SQL au format XML. | &nbsp; |
| Ajout des améliorations apportées à l’assistant d’application de la couche Données. | &bull; &nbsp; Ajout du bouton Générer le script<br/>&bull; &nbsp; Ajout d’une vue pour signaler les pertes de données potentielles lors du déploiement. |
| Met à jour vers l’extension SQL Server 2019 (préversion). | Consultez [Extension de virtualisation de données](./extensions/data-virtualization-extension.md). |
| Diffusion en continu des résultats activée par défaut pour les requêtes de longue durée. | &nbsp; |
| Bogues et problèmes résolus. | Consultez [Bogues et problèmes, sur GitHub](https://github.com/Microsoft/azuredatastudio/milestone/23?closed=1). |
| &nbsp; | &nbsp; |

## <a name="january-2019-hotfix"></a>janvier 2019 (correctif logiciel)

16 janvier 2019 &nbsp; / &nbsp; version : 1.3.9 &nbsp; / &nbsp; version de correctif logiciel

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Correction de certains problèmes détectés dans la version 1.3.8. | Consultez [la version de correctif logiciel de janvier sur GitHub](https://github.com/Microsoft/azuredatastudio/milestone/24?closed=1).<br/><br/>Pour plus d'informations, consultez :<br/>&bull; &nbsp; [Journal des modifications, sur GitHub](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md).<br/>&bull; &nbsp; [Versions, sur GitHub](https://github.com/Microsoft/azuredatastudio/releases). |
| &nbsp; | &nbsp; |

## <a name="january-2019"></a>Janvier 2019

9 janvier 2019 &nbsp; / &nbsp; version : 1.3.8

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Ajout d’un nouveau programme d’installation utilisateur pour Windows. | Contrairement au programme d’installation système existant, le nouveau programme d’installation utilisateur ne nécessite pas de privilèges d’administrateur. Cela permet également une plus grande facilité de mise à niveau pour les non-administrateurs. |
| Ajout de la prise en charge de l’authentification Azure Active Directory. | &nbsp; |
| Annonce d’Idera SQL DM Performance Insights (préversion). | &nbsp; |
| Prise en charge de l’assistant d’application de la couche Données dans l’extension SQL Server Import. | &nbsp; |
| Faites une mise à jour vers l’extension SQL Server 2019 (préversion). | Consultez [Extension de virtualisation de données](./extensions/data-virtualization-extension.md). |
| Améliorations apportées à SQL Server Profiler. | &nbsp; |
| Les résultats sont diffusés en continu pour les requêtes volumineuses (préversion). | &nbsp; |
| Extensions de la communauté : sp_executesql sur sql et nouvelle base de données. | &nbsp; |
| Bogues et problèmes résolus. | Consultez [Bogues et problèmes, sur GitHub](https://github.com/Microsoft/azuredatastudio/milestone/19?closed=1). |
| &nbsp; | &nbsp; |

## <a name="november-2018"></a>Novembre 2018

6 novembre 2018 &nbsp; / &nbsp; version : 1.2.4

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Faites une mise à jour vers l’extension SQL Server 2019 (préversion). | Consultez [Extension de virtualisation de données](./extensions/data-virtualization-extension.md). |
| Présentation de l’extension Coller le plan. | &nbsp; |
| Présentation de l’extension de requêtes à couleurs, notamment le thème de l’éditeur SSMS. | &nbsp; |
| Correctifs dans les extensions SQL Server Agent, Profiler et Import. | &nbsp; |
| Correction du problème KeepAlive pour le socket .NET Core provoquant l’abandon des connexions inactives sur macOS. | &nbsp; |
| Mettez à niveau le service Outils SQL vers .NET Core 2.2 Preview 3 (pour la prise en charge d’Azure AD). | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-november-2018"></a>Correctifs de bogues, novembre 2018

- Correction du [problème #2933](https://github.com/Microsoft/azuredatastudio/issues/2933) : Connexion perdue à Azure SQL Database
- Correction du [problème #2914](https://github.com/Microsoft/azuredatastudio/issues/2914) : Exception « Argument non valide » lors du développement du nœud de base de données OE
- Correction du [problème #2935](https://github.com/Microsoft/azuredatastudio/pull/2935) : Affichage correct des messages sur plusieurs lignes dans les résultats de la requête
- Correction du [problème #2906](https://github.com/Microsoft/azuredatastudio/pull/2906) : Correction de la modification du nom du document de données lorsque le nom de la table contient des caractères spéciaux
- Correction du [problème #2929](https://github.com/Microsoft/azuredatastudio/issues/2929) : Le journal des modifications intégré de l’extension invite à consulter les notes de publication de VS Code pour les modifications
- Correction du [problème #2719](https://github.com/Microsoft/azuredatastudio/issues/2719) : Icônes du thème à contraste élevé en double/triple
- Correction du [problème #3047](https://github.com/Microsoft/azuredatastudio/pull/3047) : Ajout d’une interface de ligne de commande pour la connexion à SQL Server
- Correction du [problème #3031](https://github.com/Microsoft/azuredatastudio/pull/3031) : Ajout de la prise en charge d’un thème de plan de requête

## <a name="october-2018"></a>Octobre 2018

29 octobre 2018 &nbsp; / &nbsp; version : 1.1.4

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Présentation d’Azure Resource Explorer pour parcourir Azure SQL Database. | &nbsp; |
| Améliorez la robustesse de la connectivité de l’Explorateur d’objets et de l’éditeur de requête. | &nbsp; |
| Améliorations des extensions de l’agent SQL. | &nbsp; |
| Faites une mise à jour vers l’extension SQL Server 2019 (préversion). | Consultez [Extension de virtualisation de données](./extensions/data-virtualization-extension.md). |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-october-2018"></a>Résolution de bogues, octobre 2018

- Correction du [problème #2717](https://github.com/Microsoft/azuredatastudio/issues/2717) : Mise en forme des clics sur la colonne XML
- Correction du [problème #2993](https://github.com/Microsoft/azuredatastudio/issues/2993) : Fenêtres de résultats incomplètes
- Correction du [problème #2999](https://github.com/Microsoft/azuredatastudio/issues/2999) : Impossible de charger le fichier System.Diagnostics.Tracing sur Mac lors de la connexion à la base de données
- Correction du [problème #2851](https://github.com/Microsoft/azuredatastudio/issues/2851) : Le graphique TimeSeries n’était pas rendu correctement
- Correction du [problème #2996](https://github.com/Microsoft/azuredatastudio/issues/2996) : Perte de la table temporaire en raison d’une modification soudaine de la session

Pour plus d’informations, consultez le [journal des modifications](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md) et les [notes de version](https://github.com/Microsoft/azuredatastudio/releases).

## <a name="september-2018-ga-release"></a>Septembre 2018 (version GA)

24 septembre 2018 &nbsp; / &nbsp; version : 1.0 &nbsp; / &nbsp; version GA

Version de mise à la disposition générale d’Azure Data Studio (anciennement SQL Operations Studio).

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Améliorations aux performances de la grille de résultats de requête et à l’expérience utilisateur pour un grand nombre de jeux de résultats. | &nbsp; |
| Actualisation du code source de Visual Studio Code de 1.23 à 1.26.1 avec la disposition en grille et l’amélioration de l’éditeur de paramètres (préversion). | &nbsp; |
| Améliorations de l’accessibilité pour le lecteur d’écran, la navigation au clavier et le contraste élevé. | &nbsp; |
| Ajout de l’option `Connection name` permettant de fournir un autre nom d’affichage dans la viewlet Serveurs. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="announcing-the-sql-server-2019-preview-extension"></a>Annonce de l’extension SQL Server 2019 en préversion

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Prise en charge des fonctionnalités SQL Server 2019 en préversion, notamment la prise en charge des [clusters Big Data](../big-data-cluster/big-data-cluster-overview.md). | Connectez-vous à la passerelle HDFS/Spark fournie avec SQL Server 2019 en préversion.<br/><br/>Parcourez HDFS, téléchargez des fichiers, enregistrez des fichiers et lancez des actions utiles comme l’analyse dans le notebook pour les fichiers CSV.<br/><br/>Envoyez des travaux Spark à partir du tableau de bord ou cliquez avec le bouton droit sur une connexion HDFS/Spark dans l’Explorateur d’objets. |
| Notebooks Azure Data Studio. | Créez ou ouvrez des notebooks à l’aide d’une visionneuse de notebook intégrée. Dans cette version, la visionneuse de notebook prend en charge la connexion aux noyaux locaux et au cluster Big Data SQL Server 2019 uniquement.<br/><br/>Utilisez les bibliothèques d’accélérateur de code PROSE dans votre notebook pour apprendre le format de fichier et les types de données pour la préparation rapide des données. |
| Azure Resource Explorer. | La vue Azure Resource Explorer vous permet de parcourir les points de terminaison liés aux données pour vos comptes Azure et de créer des connexions à ceux-ci dans l’Explorateur d’objets. Dans cette version, Azure SQL Database est pris en charge. |
| Assistant Créer une table externe PolyBase de SQL Server. | Créez une table externe et ses structures de métadonnées de soutien avec un assistant facile à utiliser. Dans cette version, les serveurs SQL Server et Oracle à distance sont pris en charge. |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-september-2018"></a>Correctifs de bogues, septembre 2018

- Correction du [problème #2647](https://github.com/Microsoft/azuredatastudio/issues/143) : Les graphiques ont fait un considérable pas en arrière.
- Correction du [problème #2648](https://github.com/Microsoft/azuredatastudio/issues/143) : SELECT retournait un lien hypertexte JSON sur la colonne entière.

Pour plus d’informations, consultez le [journal des modifications](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md) et les [notes de version](https://github.com/Microsoft/azuredatastudio/releases).

## <a name="august-2018"></a>Août 2018

30 août 2018 &nbsp; / &nbsp; version : 0.32.8 &nbsp; / &nbsp; préversion publique

La *préversion publique d’août* est axée sur les correctifs de bogues, la stabilisation des produits et la résolution des lacunes des scénarios existants.

_0.32.8 contient des correctifs pour deux régressions trouvées dans 0.32.7 ([#1971](https://github.com/Microsoft/azuredatastudio/issues/1971), [#2372](https://github.com/Microsoft/azuredatastudio/issues/2372))_

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Annonce de l’extension SQL Server Import. | &nbsp; |
| Gestion des sessions SQL Server Profiler. | &nbsp; |
| Prise en charge des modèles de session SQL Server Profiler. | &nbsp; |
| Améliorations apportées à SQL Server Agent. | &nbsp; |
| Nouvelle extension de la communauté : Kit de premier intervenant. | &nbsp; |
| Améliorations de la qualité de vie : Chaînes de connexion | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-august-2018"></a>Correctifs de bogues, août 2018

- Analyse de SQL dans une fenêtre de l’éditeur de requête à l’aide de la commande `Parse Syntax`.
- Correction du [problème #143](https://github.com/Microsoft/azuredatastudio/issues/143) : Double-cliquer ne sélectionne pas @ dans le nom de la variable.
- Correction du [problème #387](https://github.com/Microsoft/azuredatastudio/issues/387) : L’icône de base de données de l’onglet SQL est rouge.
- Correction du [problème #825](https://github.com/Microsoft/azuredatastudio/issues/825) : Demande : Se connecter automatiquement au serveur actif après le script en tant que... 
- Résolution du [problème #1278](https://github.com/Microsoft/azuredatastudio/issues/1278) : valeur redondante de sqlops.desktop [entrée du bureau]pour le nom et le commentaire.
- Correction du [problème #1285](https://github.com/Microsoft/azuredatastudio/issues/1285) : La mise à jour entraîne la suppression et le remplacement de l’icône d’application dans Windows.
- Correction du [problème #1317](https://github.com/Microsoft/azuredatastudio/issues/1317) : Correction du séparateur décimal.
- Correction du [problème #1474](https://github.com/Microsoft/azuredatastudio/issues/1474) : L’annulation de la connexion de modification déconnecte la connexion actuelle.
- Correction du [problème #1497](https://github.com/Microsoft/azuredatastudio/issues/1497) : Les options Afficher en tant que graphique sont coupées en bas.
- Correction du [problème #1524](https://github.com/Microsoft/azuredatastudio/issues/1524) : Interpréteur de commandes/tableau de bord : Les icônes principales de viewlet peuvent être déplacées et bloquer l’application.
- Correction du [problème #1578](https://github.com/Microsoft/azuredatastudio/issues/1578) : Impossible de développer/réduire le dossier de l’Explorateur de fichiers distant en cliquant sur nom.
- Correction du [problème #1620](https://github.com/Microsoft/azuredatastudio/issues/1620) : Suggestion de fonctionnalité : Obtenir la chaîne de connexion pour une connexion existante.
- Correction du [problème #1624](https://github.com/Microsoft/azuredatastudio/issues/1624) : La SelectBox ne change pas de couleur quand elle est désactivée.
- Correction du [problème #1728](https://github.com/Microsoft/azuredatastudio/issues/1728) : L’enregistrement au format JSON/EXCEL/CSV ne fonctionne pas.
- Correction du [problème #1744](https://github.com/Microsoft/azuredatastudio/issues/1744) : Le volet Résultats perd ses positions de défilement lors du basculement entre onglets.
- Correction du [problème #1748](https://github.com/Microsoft/azuredatastudio/issues/1748) : Message d’erreur lors de l’enregistrement du fichier Excel la deuxième fois (et les fois suivantes).
- Correction du [problème #1782](https://github.com/Microsoft/azuredatastudio/issues/1782) : Modification des données : la cellule ne revient pas à la valeur d’origine lors de la pression de la touche ÉCHAP.
- Résolution du [problème #1836](https://github.com/Microsoft/azuredatastudio/issues/1836) : fichiers .sql non associés à SQL Operations Studio.
- Correction du [problème #1850](https://github.com/Microsoft/azuredatastudio/issues/1850) : La saisie automatique de N'' aboutit à N'''.
- Correction du [problème #1985](https://github.com/Microsoft/azuredatastudio/issues/1985) : La fonction Copier à partir de la grille des résultats de requête est décalée d’une colonne.
- Correction du [problème #1998](https://github.com/Microsoft/azuredatastudio/pull/1998) : Ajout de la version de VS Code à boîte de dialogue À propos.
- Correction du [problème #2042](https://github.com/Microsoft/azuredatastudio/pull/2042) : Agent : Bouton activé pour importer des requêtes à partir de fichiers sql.
- Correction du [problème #2091](https://github.com/Microsoft/azuredatastudio/issues/2091) : Impossible d’utiliser le raccourci Ctrl+C pour copier à partir du volet Résultats.
- Correction du [problème #2099](https://github.com/Microsoft/azuredatastudio/pull/2099) : Ajout d’options saveAsCsv supplémentaires.
- Correction du [problème #2107](https://github.com/Microsoft/azuredatastudio/issues/2107) : Mise à jour de l’icône de document pour les documents du profileur et du tableau de bord.
- Correction du [problème #2129](https://github.com/Microsoft/azuredatastudio/pull/2129) : Enregistrement de la position de défilement des données modifiées lors du basculement d’onglets.
- Correction du [problème #2152](https://github.com/Microsoft/azuredatastudio/issues/2152) : Indicateur de ligne de grille de résultats basé sur zéro.

### <a name="known-issues-august-2018"></a>Problèmes connus, août 2018

- [Problème #2371](https://github.com/Microsoft/azuredatastudio/issues/2371) Enregistrer sous Excel enregistre uniquement la première ligne de données
- [Problème #2150](https://github.com/Microsoft/azuredatastudio/issues/2150) : Impossible de se connecter à SQL dans un conteneur sur Ubuntu 16.04

## <a name="july-2018"></a>Juillet 2018

19 juillet 2018 &nbsp; / &nbsp; version : 0.31.4 &nbsp; / &nbsp; Préversion publique

La *préversion publique de juillet* est axée sur les éléments suivants :

- La version initiale des scénarios de configuration de SQL Server Agent.
- Améliorations apportées aux modèles de session et de vue SQL Server Profiler.
- Correctifs de bogues pour les problèmes GitHub signalés par les clients.

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Améliorations à [l’extension SQL Server Agent pour SQL Operations Studio](./extensions/sql-server-agent-extension.md). | Ajout d’une vue pour les alertes, opérateurs, proxies et icônes dans le volet gauche.<br/><br/>Ajout de boîtes de dialogue pour Nouveau travail, Nouvelle étape du travail, Nouvelle alerte et Nouvel opérateur.<br/><br/>Ajout de Supprimer tâche, Supprimer alerte et Supprimer opérateur (clic droit).<br/><br/>Ajout de la visualisation des exécutions précédentes.<br/><br/>Ajout de filtres pour chaque nom de colonne. |
| Améliorations à [l’extension SQL Server Profiler pour SQL Operations Studio](./extensions/sql-server-profiler-extension.md). | Ajout de 5 modèles par défaut pour afficher les événements étendus.<br/><br/>Ajout du nom de la connexion au serveur/à la base de données.<br/><br/>Ajout de la prise en charge des instances d’Azure SQL Database.<br/><br/>Ajout d’une suggestion pour quitter Profiler lorsque l’onglet est fermé alors que Profiler est toujours en cours d’exécution. |
| Publication de l’extension Combiner des scripts. | &nbsp; |
| Les points d’extensibilité de l’assistant et de la boîte de dialogue ont été ajoutés aux auteurs d’extensions. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-july-2018"></a>Correctifs de bogues, juillet 2018

- Correction du [problème 728](https://github.com/Microsoft/azuredatastudio/issues/728) : Aucune réponse pour l’ajout d’une connexion sur macOS
- Correction du [problème 1612](https://github.com/Microsoft/azuredatastudio/issues/1612) : L’affichage du texte de la grille de résultats est perturbé par les caractères internationaux
- Correction du [problème 1693](https://github.com/Microsoft/azuredatastudio/issues/1693) : Boîte de dialogue de sauvegarde : L’interface utilisateur de l’Explorateur de fichiers ne fonctionne pas
- Correction du [problème 1713](https://github.com/Microsoft/azuredatastudio/issues/1713) : Nombre de lignes affectées
- Correction du [problème 1718](https://github.com/Microsoft/azuredatastudio/issues/1718) : Impossible de se connecter à une source de données
- Correction du [problème 1719](https://github.com/Microsoft/azuredatastudio/issues/1719) : TypeError lors de la connexion au serveur
- Correction du [problème 1724](https://github.com/Microsoft/azuredatastudio/issues/1724) : Les boîtes de dialogue d’extension ont cessé de fonctionner
- Correction du [problème 1749](https://github.com/Microsoft/azuredatastudio/issues/1749) : BOGUE : Les données HTML d’une colonne sont interprétées
- Correction du [problème 1789](https://github.com/Microsoft/azuredatastudio/issues/1789) : Extensibilité : Si vous ajoutez un fournisseur de connexion, la désinstallation ne le supprimera jamais de la liste
- Correction du [problème 1791](https://github.com/Microsoft/azuredatastudio/issues/1791) : Extensions Sqlops : queryeditor.connect() se connecte à la base de données cible, mais l’interface utilisateur n’indique pas que l’éditeur est connecté
- Correction du [problème 1799](https://github.com/Microsoft/azuredatastudio/issues/1799) : Le graphique de Top 10 des tailles de base de données ne fonctionne pas sur les instances sensibles à la casse
- Résolution du problème [1814](https://github.com/Microsoft/azuredatastudio/issues/1814) : une faute de frappe dans sqlops.d.ts provoquait une définition de type « any » implicite
- Correction du [problème 1817](https://github.com/Microsoft/azuredatastudio/issues/1817) : Faute d’orthographe
- Correction du [problème 1830](https://github.com/Microsoft/azuredatastudio/issues/1830) : La définition d’iconPath dans ButtonComponent après l’appel de component() n’entraîne pas la modification de l’icône
- Correction du [problème 1843](https://github.com/Microsoft/azuredatastudio/issues/1843) : Meilleure organisation des tables

## <a name="june-2018"></a>Juin 2018

20 juin 2018 &nbsp; / &nbsp; version : 0.30.6 &nbsp; / &nbsp; préversion publique

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Publication initiale de l’extension **SQL Server Profiler pour SQL Operations Studio _(préversion)_**. | &nbsp; |
| La nouvelle extension **SQL Data Warehouse** comprend des widgets de tableau de bord enrichis personnalisables qui regroupent des insights dans votre entrepôt de données. | Cela permet de débloquer les principaux scénarios de gestion et de paramétrage de votre entrepôt de données afin de s’assurer qu’il est optimisé pour des performances cohérentes. |
| **Modification de la prise en charge du filtrage et du tri des données**. | &nbsp; |
| Améliorations de l’extension **SQL Server Profiler pour SQL Operations Studio _(préversion)_** pour les vues Travaux et Historique des travaux. | &nbsp; |
| Amélioration des API d’extensibilité **Framework de création d’assistants et d’interfaces de dialogue**. | &nbsp; |
| Mise à jour du code source de la plateforme VS Code. | Intégration des versions suivantes :<br/>&bull; &nbsp; [Mars 2018 (1.22)](https://code.visualstudio.com/updates/v1_22)<br/>&bull; &nbsp; [Avril 2018 (1.23)](https://code.visualstudio.com/updates/v1_23) |
| &nbsp; | &nbsp; |

### <a name="github-issues-fixes-june-2018"></a>Correctifs de problèmes GitHub, juin 2018

- Demande de fonctionnalité ([problème 1204](https://github.com/Microsoft/azuredatastudio/issues/1204)) : Assurez-vous que la grille de résultats ajuste automatiquement la largeur de colonne aux données et n’oubliez pas les modifications manuelles si la même requête est réexécutée.
- Correction du [problème 1398](https://github.com/Microsoft/azuredatastudio/issues/1398) : Doit afficher les boutons Ajouter un message et Ajouter un compte quand le compte lié est vide.
- Correction du [problème 1399](https://github.com/Microsoft/azuredatastudio/issues/1399) : L’onglet du compte lié ne fonctionne plus lorsque la vue est réduite.
- Correction du [problème 1374](https://github.com/Microsoft/azuredatastudio/issues/1374) : Le service Outils SQL se bloque lors de l’ouverture d’un fichier .sql à partir du disque.
- Correction du [problème 1372](https://github.com/Microsoft/azuredatastudio/issues/1372) : Mot clé SQL « BETWEEN » manquant.
- Correction du [problème 1395](https://github.com/Microsoft/azuredatastudio/issues/1395) : Le mot clé « MATCH » bloque le service Outils SQL.
- Correction du [problème 1496](https://github.com/Microsoft/azuredatastudio/issues/1496) : L’option de menu contextuel « Nouveau profileur » dans l’Explorateur d’objets ne fait rien.
- Correction du [problème 1495](https://github.com/Microsoft/azuredatastudio/issues/1495) : Le plan de requête « Expliquer » de l’éditeur de requête ne fonctionne pas.

## <a name="may-2018"></a>Mai 2018

7 mai 2018 &nbsp; / &nbsp; version : 0.29.3 &nbsp; / &nbsp; préversion publique

La *préversion publique de mai* se concentre sur la stabilisation et les correctifs de bogues.

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Annonce de l’extension de recherche SQL Redgate disponible dans le gestionnaire d’extensions. | &nbsp; |
| La localisation de la communauté est disponible pour 10 langues. | Allemand, espagnol, français, italien, japonais, coréen, portugais, russe, chinois simplifié et chinois traditionnel. |
| Modification de la collection de télémétrie. | &bull; &nbsp; Collecte de télémétrie réduite.<br/>&bull; &nbsp; Expérience de désactivation améliorée.<br/>&bull; &nbsp; Liens vers la déclaration de confidentialité intégrés au produit. |
| Le gestionnaire d’extensions a amélioré l’expérience du marketplace. | Découvrez plus facilement les extensions de la communauté. |
| Extension de l’agent SQL. | &bull; &nbsp; Travaux.<br/>&bull; &nbsp; Amélioration de l’affichage de l’historique des travaux. |
| Mises à jour pour les extensions whoisactive et Server Reports. | &nbsp; |
| Amélioration du défilement des propriétés du tableau de bord de gestion. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues"></a>Résolutions de problèmes GitHub

- Correction du [problème 703](https://github.com/Microsoft/azuredatastudio/issues/703) : Si vous entrez du texte de type HTML dans Modifier les données, la valeur s’affiche de manière incorrecte jusqu’à l’actualisation
- Résolution du [problème 821](https://github.com/Microsoft/azuredatastudio/issues/821) : dépendance de package azuredatastudio.deb
- Correction du [problème 1260](https://github.com/Microsoft/azuredatastudio/issues/1260) : Mot clé « distinct » non mis en surbrillance
- Correction du [problème 1332](https://github.com/Microsoft/azuredatastudio/issues/1332) : La ligne de restauration des données modifiées ne fonctionne pas
- Correction du [problème 1215](https://github.com/Microsoft/azuredatastudio/issues/1215) : Extension de l’agent SQL et barre d’état
- Correction du [problème 1316](https://github.com/Microsoft/azuredatastudio/issues/1316) : L’agent SQL ne se redimensionne pas après modification de la taille de fenêtre

## <a name="april-2018"></a>Avril 2018

25 avril 2018 &nbsp; / &nbsp; version : 0.28.6 &nbsp; / &nbsp; préversion publique

La *préversion publique d’avril* contient des correctifs de bogues et des améliorations.

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Améliorations apportées à l’extension d’agent SQL en préversion : | &nbsp; |
| &nbsp; &nbsp; &nbsp; Prise en charge améliorée des fichiers. | &bull; &nbsp; Fichiers volumineux.<br/>&bull; &nbsp; Fichiers protégés, pour l’enregistrement protégé par l’administrateur.<br/>&bull; &nbsp; Stockage des fichiers \>256 Mo dans SQL Operations Studio. |
| &nbsp; &nbsp; &nbsp; Fractionnement intégré des terminaux. | Utilisez simultanément plusieurs terminaux ouverts. |
| &nbsp; &nbsp; &nbsp; Temps d’installation et de démarrage plus rapides. | Installation réduite de l’empreinte en nombre de fichiers sur le disque. |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues-april-2018"></a>Résolution des problèmes GitHub, avril 2018

- Correction du [problème 37](https://github.com/Microsoft/azuredatastudio/issues/37) : Lorsque la visionneuse de graphiques génère une erreur, un comportement inattendu se produit.
- Correction du [problème 462](https://github.com/Microsoft/azuredatastudio/issues/462) : Demande de fonctionnalité : Option pour les groupes de serveurs à développer par défaut.
- Correction du [problème 606](https://github.com/Microsoft/azuredatastudio/issues/606) : IntelliSense - Suggestion erronée pour la commande « Update ».
- Correction du [problème 967](https://github.com/Microsoft/azuredatastudio/issues/967) : Un plan de requête est attendu lorsque vous sélectionnez Showplan XML dans la grille de résultats.
- Correction du [problème 1023](https://github.com/Microsoft/azuredatastudio/issues/1023) : Ajout de crochets pour l’appel ms_foreachdb à partir de flyfishingdba.
- Correction du [problème 1048](https://github.com/Microsoft/azuredatastudio/issues/1048) : Erreur de négociation SSL/TLS avant la connexion.
- Correction du [problème 1050](https://github.com/Microsoft/azuredatastudio/issues/1050) : Vue d’insights effacée avant d’afficher l’erreur.
- Correction du [problème 1057](https://github.com/Microsoft/azuredatastudio/issues/1057) : Les actions de restauration et de nouvelle requête dans l’explorateur de widgets ne fonctionnent pas.
- Correction du [problème 1068](https://github.com/Microsoft/azuredatastudio/issues/1068) : La fenêtre de sortie du tableau de bord affiche un message d’erreur pour Azure SQL Database.
- Correction du [problème 1069](https://github.com/Microsoft/azuredatastudio/issues/1069) : La boîte de dialogue de connexion affiche une erreur Serveur requis lors de l’affichage initial.
- Correction du [problème 1070](https://github.com/Microsoft/azuredatastudio/issues/1070) : Les groupes de serveurs requièrent désormais un double-clic pour se développer.
- Correction du [problème 1072](https://github.com/Microsoft/azuredatastudio/issues/1072) : La sélection de l’arrière-plan de contrôle est semi-transparente.
- Correction du [problème 1115](https://github.com/Microsoft/azuredatastudio/issues/1115) : Correction de tous les problèmes d’accessibilité à contraste élevé dans SQL Operations Studio.
- Correction du [problème 1101](https://github.com/Microsoft/azuredatastudio/issues/1101) : L’extension ne parvient pas à se mettre à niveau ; le lien « Téléchargement manuel » redirige vers un emplacement incorrect.
- Correction du [problème 1103](https://github.com/Microsoft/azuredatastudio/issues/1103) : Le défilement vertical ne fonctionne pas dans l’onglet Accueil.
- Correction du [problème 1104](https://github.com/Microsoft/azuredatastudio/issues/1104) : Les onglets d’extension SQL ont cessé de fonctionner.

### <a name="visual-studio-code-121-platform"></a>Plateforme Visual Studio Code 1.21

Un des points notables de la préversion publique d’avril est l’actualisation du code source pour la plateforme Visual Studio Code 1.21. Plusieurs mises à jour ont ainsi été apportées à l’éditeur principal et au workbench par rapport au point de synchronisation 1.19 précédent. Voici quelques exemples :

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| [Nouvelle interface de notifications](https://code.visualstudio.com/updates/v1_21#_new-notifications-ui). | Gérez et passez facilement en revue les notifications SQL Operations Studio. |
| [Fractionnement intégré des terminaux](https://code.visualstudio.com/updates/v1_21#_split-terminals). | Travaillez avec plusieurs terminaux ouverts simultanément. |
| [Enregistrez des fichiers volumineux et protégés](https://code.visualstudio.com/updates/v1_20#_save-files-that-need-admin-privileges). | Enregistrez les fichiers protégés et de \>256 Mo de l’administrateur dans SQL Operations Studio. |
| [Prise en charge améliorée des fichiers volumineux](https://code.visualstudio.com/updates/v1_21#_text-buffer-improvements). | Optimisations de la mémoire tampon de texte pour les fichiers volumineux. |
| [Amélioration de la recherche de paramètres](https://code.visualstudio.com/updates/v1_20#_settings-search). | Trouvez facilement le paramètre approprié avec la recherche en langage naturel. |
| [Extraits de code globaux](https://code.visualstudio.com/updates/v1_20#_global-snippets). | Créez des extraits de code que vous pouvez utiliser pour tous les types de fichiers. |
| [Sélection multiple dans l’explorateur](https://code.visualstudio.com/updates/v1_20#_multi-select-in-the-explorer). | Effectuez des actions sur plusieurs fichiers à la fois. |
| [Erreurs et avertissements dans l'Explorateur](https://code.visualstudio.com/updates/v1_20#_error-indicators-in-the-explorer). | Accédez rapidement aux erreurs dans votre base de code. |
| [Faites glisser et déplacez ou copiez et collez entre fenêtres](https://code.visualstudio.com/updates/v1_21#_better-drag-and-drop-support). | Déplacez les fichiers entre fenêtres ouvertes dans SQL Operations Studio. |
| [Prise en charge du sous-module Git](https://code.visualstudio.com/updates/v1_20#_git-submodules). | Effectuez des opérations Git sur les dépôts Git imbriqués. |
| [Prise en charge du lecteur d’écran de terminal](https://code.visualstudio.com/updates/v1_20#_screen-reader-support). | Le terminal intégré dispose désormais du mode **Optimisé pour lecteur d’écran**. |
| [Disposition de l’éditeur centrée](https://code.visualstudio.com/updates/v1_21#_centered-editor-layout). | Optimisez l’espace d’affichage de votre code à l’écran. |
| [Résultats de recherche horizontaux (préversion)](https://code.visualstudio.com/updates/v1_21#_horizontal-search). | Vous pouvez maintenant afficher les résultats de la recherche dans un volet horizontal. |
| &nbsp; | &nbsp; |

Pour plus d’informations, consultez les [notes de publication de Visual Studio Code version février](https://code.visualstudio.com/updates/v1_21) et les [notes de publication de Visual Studio Code version janvier](https://code.visualstudio.com/updates/v1_20).

Pour plus d’informations, consultez le [Journal des modifications](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md).

## <a name="march-2018"></a>Mars 2018

28 mars 2018 &nbsp; / &nbsp; version : 0.27.3 &nbsp; / &nbsp; préversion publique

La *préversion publique de mars* continue à répondre aux principaux problèmes de GitHub et se concentre sur l’amélioration de notre histoire d’extensibilité. Activation spécifique du gestionnaire d’extensions, amélioration de la gestion des tableaux de bord et fourniture d’extensions d’agents SQL et d’insights. Cette version inclut les améliorations suivantes :

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Amélioration du modèle d’extensibilité du tableau de bord pour prendre en charge des insights avec onglets et des volets de configuration. | Le gestionnaire d’extensions permet une acquisition simple des extensions.<br/><br/>Extensions du tableau de bord pour sp\_whoisactive à partir de [whoisactive.com](http://www.whoisactive.com).<br/><br/>Pour plus d’informations, consultez [Étendre les fonctionnalités de SQL Operations Studio](./extensions/add-extensions.md). |
| Ajout des [API d’extensibilité supplémentaires pour la gestion de la connexion et de l’Explorateur d’objets](https://github.com/Microsoft/azuredatastudio/wiki/Extensibility-API). | &nbsp; |
| Poursuite de la résolution des [problèmes GitHub](https://github.com/Microsoft/azuredatastudio/issues) importants qui ont un impact sur les clients. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="february-2018"></a>Février 2018

15 février 2018 &nbsp; / &nbsp; version : 0.26.7 &nbsp; / &nbsp; préversion publique

La *préversion publique de février* comprend des suggestions de fonctionnalités et des correctifs de bogues de haute priorité. Cette version inclut les améliorations suivantes :

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Ajout de l’installation automatique des mises à jour, qui fournit une notification lorsqu’une nouvelle version est disponible en téléchargement. | &nbsp; |
| Le champ **Base de données** de la boîte de dialogue de connexion est maintenant une liste déroulante dynamiquement renseignée qui contient la liste des bases de données remplies à partir du serveur spécifié. | &nbsp; |
| Ajout de l’API d’extensibilité de connexion. | &nbsp; |
| Intégration de l’éditeur VS Code 1.19. | &nbsp; |
| Mettez à jour le composant JustinPealing/html-query-plan pour récupérer plusieurs améliorations de la visionneuse de plan de requête. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fixed-issues-february-2018"></a>Résolution des problèmes, février 2018

- Correction du [problème 6](https://github.com/Microsoft/azuredatastudio/issues/6) : Conservation de la connexion et de la base de données sélectionnée lors de l’ouverture de nouveaux onglets de requête.
- Correction du [problème 22](https://github.com/Microsoft/azuredatastudio/issues/22) : « Nom du serveur » et « Nom de la base de données » peuvent-ils être des listes déroulantes plutôt que des zones de texte ?
- Correction du [problème 549](https://github.com/Microsoft/azuredatastudio/issues/549) : L’installation silencieuse/très silencieuse entraîne l’ouverture de l’application après l’installation.
- Correction du [problème 481](https://github.com/Microsoft/azuredatastudio/issues/481) : Ajout de l’option « Rechercher des mises à jour ».
- Colorisation de l’éditeur SQL et correctifs de saisie semi-automatique :
  - Correction du [problème 584](https://github.com/Microsoft/azuredatastudio/issues/584) : Mot clé « FULL » non mis en surbrillance par IntelliSense.
  - Correction du [problème 345](https://github.com/Microsoft/azuredatastudio/issues/345) : Colorez les fonctions SQL dans l’éditeur.
  - Résolution du [problème 300](https://github.com/Microsoft/azuredatastudio/issues/300) : Le dernier « ] » de [#tempData] s’affiche avec la couleur verte.
  - Correction du [problème 225](https://github.com/Microsoft/azuredatastudio/issues/225) : Incompatibilité de couleur des mots clés.
  - Correction du [problème 60](https://github.com/Microsoft/azuredatastudio/issues/60) : Mise en surbrillance des couleurs de syntaxe SQL non valide lors de l’utilisation d’une table temporaire dans la clause from.

## <a name="january-2018"></a>Janvier 2018

17 janvier 2018 &nbsp; / &nbsp; version : 0.25.4 &nbsp; / &nbsp; préversion publique

La *préversion publique de janvier* comprend des suggestions de fonctionnalités et des correctifs de bogues de haute priorité. Cette version inclut les améliorations suivantes :

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| Les connexions serveur enregistrées sont disponibles dans la boîte de dialogue de Connexion. | &nbsp; |
| Activez la sortie à chaud. La sortie à chaud est désactivée par défaut. Pour l’activer, consultez [Paramètre de sortie à chaud](settings.md#hot-exit). | &nbsp; |
| Couleur d’onglet basée sur le groupe de serveurs. La coloration d’onglet est désactivée par défaut. Pour l’activer, consultez [Paramètre de couleur d’onglet](settings.md#tab-color). | &nbsp; |
| Changement de *Nom du serveur* en *Serveur* dans la boîte de dialogue de connexion. | &nbsp; |
| Correction de la commande *Exécuter la requête actuelle* qui ne fonctionnait pas. | &nbsp; |
| Correction du bogue de script de la fonction glisser-déposer. | &nbsp; |
| Correction de l’icône du menu Démarrer mal épinglée. | &nbsp; |
| Correction de l’icône de personnalisation de compte Azure manquante. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="december-2017"></a>Décembre 2017

19 décembre 2017 &nbsp; / &nbsp; version : 0.24.1 &nbsp; / &nbsp; préversion publique

La *préversion publique de décembre* comprend plusieurs correctifs de bogues pour tous les domaines fonctionnels, ainsi que les améliorations suivantes :

&nbsp;

| Modifier | Détails |
| :----- | :------ |
| La boîte de dialogue Créer une règle de pare-feu est désormais disponible pour vous aider à vous connecter à Azure SQL Database et Azure Synapse Analytics. | &nbsp; |
| Ajout des packages d’installation de Windows, de Linux DEB et de RPM. | &nbsp; |
| Gestion de l’éditeur de disposition visuelle du tableau de bord. | &nbsp; |
| Commandes *Modifier script comme* et *Exécuter script comme*. | &nbsp; |
| Commande *Exécuter la requête actuelle avec le plan réel*. | &nbsp; |
| Intégration de la plateforme de l’éditeur VS Code 1.18.1. | &nbsp; |
| Activation du sideloading des fichiers d’extension VSIX. | &nbsp; |
| Prise en charge de la syntaxe d’itération par lot « GO N ». | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="november-2017"></a>Novembre 2017

15 novembre 2017 &nbsp; / &nbsp; version : 0.23.6

- Version initiale d’Azure Data Studio.

## <a name="next-steps"></a>Étapes suivantes

Pour commencer, consultez l’un des guides de démarrage rapide suivants :

- [Connexion & interrogation de SQL Server](quickstart-sql-server.md)
- [Connexion & interrogation d’Azure SQL Database](quickstart-sql-database.md)
- [Connexion et interrogation d’Azure Synapse Analytics](quickstart-sql-dw.md)

Contribuer à Azure Data Studio :

- [https://github.com/Microsoft/azuredatastudio](https://github.com/Microsoft/azuredatastudio)
