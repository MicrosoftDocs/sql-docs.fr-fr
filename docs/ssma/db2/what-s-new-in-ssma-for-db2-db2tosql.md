---
title: Nouveautés de SSMA pour DB2 (DB2ToSQL) | Microsoft Docs
description: Découvrez les modifications apportées à Assistant Migration SQL Server (SSMA) pour DB2 (DB2ToSQL) pour chaque version.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1cc38f85-3caa-42d0-8c76-a380c1d15c67
ms.author: alexiva
ms.openlocfilehash: a12bb61546ed042a98a3cae521572ef55723cfdf
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186506"
---
# <a name="whats-new-in-ssma-for-db2-db2tosql"></a>Nouveautés de SSMA pour DB2 (DB2ToSQL)

Cet article répertorie les Assistant Migration SQL Server (SSMA) pour les modifications DB2 dans chaque version.

## <a name="ssma-v817"></a>SSMA v 8.17

La version v 8.17 de SSMA pour DB2 contient les modifications suivantes :

* Améliorer la conversion de la fonction TRANSLATE
* Correction de la migration des données pour les tables avec des colonnes calculées
* Mettre à jour les rapports d’évaluation HTML pour utiliser l’éditeur moderne pour afficher du texte SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La version v 8.16 de SSMA pour DB2 contient les modifications suivantes :

* Corriger la conversion des alias de colonne avec des caractères spéciaux
* Correction de la conversion pour la `SELECTIVITY` clause
* Améliorer la conversion pour la `WITH ROW MOVEMENT` clause
* Supprimer la prise en charge de l’analyseur hérité
* Résoudre le problème lié à l’actualisation des objets à partir de la base de données

## <a name="ssma-v815"></a>SSMA v 8.15

En plus de plusieurs améliorations de l’accessibilité, la version v 8.15 de SSMA pour DB2 contient les modifications suivantes :

* Corriger la conversion des `MIN` / `MAX` fonctions d’agrégation avec des arguments date/heure
* Corriger le bogue dans la `VARCHAR_FORMAT` fonction d’émulation lorsque l' `DD` espace réservé est utilisé
* Améliorer les mappages de type pour le `TIME` type de données
* Améliorer la conversion `ROUND` des `TRUNC` fonctions et avec des arguments numériques
* Remodelez les rapports d’évaluation pour travailler dans des navigateurs modernes
* Utiliser l’autorité fournie par la base de données pour l’authentification Azure AD
* Améliorer la dénomination des instructions chargées à partir de fichiers

## <a name="ssma-v814"></a>SSMA v 8.14

En plus de plusieurs améliorations visant à garantir une plus grande accessibilité pour les personnes handicapées, la version v 8.14 de SSMA pour DB2 requiert une mise à niveau de projet, car elle stocke désormais la version complète du serveur source/cible dans les métadonnées du projet.

## <a name="ssma-v813"></a>SSMA v 8.13

La version v 8.13 de SSMA pour DB2 contient les modifications suivantes :

* Prise en charge des index uniques filtrés
* Considérer les casts de type implicite lors de la conversion des appels de procédure et de fonction
* Améliorer la journalisation de la chaîne de connexion source pour aider à résoudre les problèmes de connexion

## <a name="ssma-v812"></a>SSMA v 8.12

La version v 8.12 de SSMA pour DB2 contient les modifications suivantes :

* Conversion de la `STRIP` fonction
* Amélioration de l’analyse des options de procédure

## <a name="ssma-v811"></a>SSMA v 8.11

La version 8.11 de SSMA pour DB2 contient les modifications suivantes :

* Prise en charge de DB2 pour i (v 7.1 et versions ultérieures)
* Traduction de `SQLSTATE` et `SQLCODE`
* Message d’erreur de conversion pour les opérateurs secondaires dans une fonction
* Utiliser la bibliothèque MSAL.NET pour l’authentification Azure Active Directory interactive

## <a name="ssma-v810"></a>SSMA v 8.10

La version v 8.10 de SSMA pour DB2 résout une régression dans la découverte de clés étrangères et contient des améliorations mineures des performances.

## <a name="ssma-v89"></a>SSMA v 8.9

La version v 8.9 de SSMA pour DB2 contient les modifications suivantes :

* Correctif pour la conversion de la `TIMESTAMPDIFF` fonction
* Correction de la détection d’index lorsque l’index partitionné est présent
* Correction de la découverte de clés étrangères lorsque l’index primaire est défini dans un autre schéma
* Conversion améliorée pour les colonnes qui correspondent aux noms de fonctions intégrées
* Correction du problème avec les caractères spéciaux dans le nom du projet

## <a name="ssma-v88"></a>SSMA v 8.8

La version v 8.8 de SSMA pour DB2 comprend les éléments suivants :

* Améliorations de la stabilité de la synchronisation des objets SQL Server
* Améliorations des performances de l’interface graphique lors de l’évaluation et de la conversion
* Mise à jour du mappage de `ROWID` à `varbinary(40)` pour faciliter la migration des données
* Amélioration de la conversion des `SELECT ... FROM NEW/OLD TABLE` instructions
* Nouvelle conversion des `ALTER` instructions pour les procédures et les fonctions
* Nouvelle conversion des assignations de déstructuration

## <a name="ssma-v87"></a>SSMA v 8.7

La version v 8.7 de SSMA pour DB2 comprend un nouvel analyseur de syntaxe DB2, ainsi que des correctifs mineurs et des améliorations de performances dans l’interface graphique utilisateur.

En outre, SSMA pour DB2 fournit désormais les éléments suivants :

* Correctif pour la découverte de clés étrangères lors de la migration à partir de DB2 sur LUW.
* Conversion améliorée de l' `SELECT ... FOR UPDATE` instruction.
* Conversion améliorée pour la `COUNT` fonction dans les tables mq.
* Conversion des `SAVEPOINT` instructions.
* Conversion en émulant le comportement DB2's pour les `NULL` valeurs de la `ORDER BY` clause.
* Analyse de la prise en charge de l' `ASSOCIATE RESULT SET` instruction.

> [!IMPORTANT]
> Avec SSMA v 8.5 et versions ultérieures, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

En plus d’un ensemble ciblé de correctifs conçus pour améliorer la facilité d’utilisation et les performances, la version 8.6 de SSMA pour DB2 a été améliorée en ajoutant un paramètre qui permet aux utilisateurs d’omettre les propriétés étendues SSMA dans le code converti.

Pour tirer parti de ce paramètre, dans SSMA pour DB2, accédez à **Outils**  >  **paramètres du projet**  >    >  **conversion** générale, puis sous **divers**, mettez à jour la valeur du paramètre **omettre les propriétés étendues** sur **Oui**.

![Paramètre d’omission des propriétés étendues](../db2/media/ssma-omit-extended-properties.png)

En outre, SSMA pour DB2 fournit désormais les éléments suivants :

* Correctif pour la conversion des fonctions qui utilisent des valeurs d’argument par défaut.
* Amélioration de l’analyse de la `PARAMETER` clause pour les fonctions.
* Possibilité de convertir l' `LEAVE` instruction.

> [!IMPORTANT]
> Avec SSMA v 8.5 et versions ultérieures, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La version v 8.5 de SSMA pour DB2 est améliorée grâce à la prise en charge de l’authentification Azure Active Directory et de la prise en charge de base des fonctionnalités JSON dans SQL Server, ainsi qu’à un ensemble ciblé de correctifs conçus pour améliorer l’utilisation et les performances.

En outre, SSMA pour DB2 a été amélioré avec :

* Prise en charge de l’ajout de la conversion pour l' `GET DIAGNOSTICS` instruction avec `ROW_NUMBER` .
* Correction d’un bogue lié à des espaces au début du nom de l’objet qui n’est pas respecté.

> [!IMPORTANT]
> Avec SSMA v 8.5, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La version v 8.4 de SSMA pour DB2 a été améliorée avec des correctifs ciblés conçus pour résoudre des problèmes d’accessibilité et corriger un bogue lié aux colonnes d’index max (pour autoriser 32 au lieu de 16) pour SQL Server 2016 et versions ultérieures.

> [!IMPORTANT]
> Avec les versions de SSMA 7,4 à 8,4, .NET 4.5.2 est une condition préalable à l’installation.

## <a name="ssma-v83"></a>SSMA v 8.3

La version 8.3 de SSMA pour DB2 a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion. En outre, cette version de SSMA pour DB2 fournit des correctifs qui :

* Résoudre les problèmes d’accessibilité.
* Ajoutez la prise en charge de base pour le `hierarchyid` type dans SQL Server.
* Remplacez utilisation des fonctions TRIM dans les requêtes de découverte z/OS par `RTRIM` / `LTRIM` .
* Autoriser l’utilisateur à spécifier la collection de packages lors de la connexion en mode standard (valeur par défaut `NULLID` ).
* Ajoutez la conversion pour `CREATE TABLE AS SELECT` .
* Améliorez les conversions pour les tables temporaires globales.
* Résolvez un problème avec la vérification de l’unicité des objets pour classer par ordre de priorité les tables sur les contraintes, si les noms sont en conflit.
* Résolvez un problème de chargement des valeurs de colonne par défaut pour `DATE` et `TIMESTAMP` pour z/OS.
* Prise en charge d’un caractère de saut de ligne Unicode (également appelé `NEL` ).
* Résolvez un problème avec la conversion de curseur avec clause manquante `RETURN TO` .
* Ajoutez la prise en charge des étiquettes et `GOTO` .

## <a name="ssma-v82"></a>SSMA v 8.2

La version 8.2 de SSMA pour DB2 a été améliorée avec pour résoudre les problèmes liés aux connexions à Azure SQL Database à partir de l’outil de la console SSMA et de la colonne COUNT_BIG manquante dans la déclaration views au cours de la conversion. En outre, cette version comprend un ensemble ciblé de correctifs conçus pour améliorer les mesures de qualité et de conversion, ainsi que des correctifs pour :

* Un problème avec les index non cluster désactivés après la migration des données.
* Détection des .NET Framework pendant l’installation sans assistance.
* Incident intermittent qui se produit lorsqu’une nouvelle version est téléchargée.

> [!NOTE]
> Un problème connu avec la mise à jour automatique peut entraîner l’échec d’une mise à jour de SSMA v 8.1 à v 8.2. Si vous rencontrez cette erreur, téléchargez la nouvelle version et installez-la manuellement.

## <a name="ssma-v81"></a>SSMA v 8.1

La version 8.1 de SSMA pour DB2 a été améliorée pour fournir des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion.

> [!NOTE]
> Un problème connu avec la mise à jour automatique peut entraîner l’échec d’une mise à jour de SSMA v 8.0 à v 8.1. Si vous rencontrez cette erreur, téléchargez la nouvelle version et installez-la manuellement.

## <a name="ssma-v80"></a>SSMA v 8.0

La version 8.0 de SSMA pour DB2 a été améliorée pour fournir des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion. Cette version offre également les nouvelles fonctionnalités suivantes :

* Prise en charge d' **Azure SQL Managed instance** en tant que cible. Vous pouvez maintenant créer des projets ciblant Azure SQL Managed Instance :

  ![Projet MI SQL](../media/ssma-newproject-sqldbmi.png)

* **Conseiller de réparation** après conversion. En savoir plus à ce sujet [ici](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Sélection préliminaire de base de données/schéma.

  Lors de la connexion à la source, l’utilisateur peut désormais sélectionner les bases de données/schémas intéressants. La sélection de seuls les schémas que vous envisagez de migrer permet de gagner du temps lors de la connexion initiale et d’améliorer les performances globales de SSMA.

  ![Objets de filtre SSMA](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La version v 7.10 de SSMA pour DB2 contient les modifications suivantes :

* Des correctifs ciblés conçus pour fournir des protections de sécurité et de confidentialité supplémentaires pour répondre aux modifications des exigences globales.
* Correctif pour la conversion de `BEGIN-END` blocs.

## <a name="ssma-v79"></a>SSMA v 7.9

La version v 7.9 de SSMA pour DB2 contient les modifications suivantes :

* Correctifs ciblés qui améliorent les mesures de qualité et de conversion.
* Prise en charge dans la ligne de commande SSMA pour modifier le mappage du type de données et les préférences du projet.
* Prise en charge de la migration de données à l’aide de SQL Server Integration Services (SSIS). Après la conversion du schéma, il est possible de créer un package SSIS à l’aide d’un clic droit de la bouton du menu contextuel.
* La boîte de dialogue de connexion Azure SQL Database dans SSMA a également été modifiée pour spécifier le nom de serveur complet. Dans les versions précédentes de SSMA, le préfixe Azure SQL Database devait être mentionné explicitement dans les paramètres des projets.

## <a name="ssma-v78"></a>SSMA v 7.8

La version 7.8 de SSMA pour DB2 contient les modifications suivantes :

* Modifiez le mappage de type mis en surbrillance dans les *paramètres du projet*.
* La possibilité pour les utilisateurs de désactiver la télémétrie.

## <a name="ssma-v77"></a>SSMA v 7.7

La version de SSMA pour DB2 du v 7.7 contient les modifications suivantes :

* Correctifs ciblés qui améliorent les mesures de qualité et de conversion.
* En fonction de la demande populaire, la version 32 bits de SSMA pour DB2 est de retour. Par rapport à l’implémentation précédente (antérieure à v 7.4), il existe deux packages d’installation, mais ils ne peuvent pas être installés côte à côte. Par conséquent, vous devez choisir la version la plus appropriée en fonction des composants de connectivité dont vous disposez. Il est toujours préférable d’utiliser la version 64 bits, si possible.

## <a name="ssma-v76"></a>SSMA v 7.6

La version v 7.6 de SSMA pour DB2 a été améliorée avec des correctifs ciblés qui améliorent les mesures de qualité et de conversion et prennent en charge SQL Server 2017 (version préliminaire publique). La prise en charge de SQL Server 2017 sur Windows et Linux est en version préliminaire publique et ne doit pas être utilisée pour les migrations de production.

## <a name="ssma-v75"></a>SSMA v 7.5

La version 7.5 de SSMA pour DB2 a été améliorée avec plusieurs améliorations pour garantir une meilleure accessibilité aux personnes handicapées.

## <a name="ssma-v74"></a>SSMA v 7.4

La version 7.4 de SSMA pour DB2 contient les modifications suivantes :

* L’option **délai de requête** est désormais disponible pendant la découverte d’objets de schéma à la source et à la cible.

  ![option délai d’expiration de la requête](../media/query-timeout_red.png)

* La mesure de qualité et de conversion a été améliorée avec les correctifs ciblés, en fonction des commentaires des clients.

  > [!IMPORTANT]
  > .NET 4.5.2 est une condition préalable à l’installation de SSMA v 7.4. En outre, à compter de la version 7.4, la version 32 bits de SSMA a été supprimée.

## <a name="ssma-v73"></a>SSMA v 7.3

La version 7.3 de SSMA pour DB2 contient les modifications suivantes :

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

La version 7.2 de SSMA pour DB2 contient les modifications suivantes :

* Amélioration de la qualité et de la mesure de conversion avec des correctifs ciblés basés sur les commentaires des clients.
* Améliorations de la télémétrie pour fournir de meilleurs points de données afin de résoudre les problèmes des clients et d’améliorer les taux de conversion de SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La version 7.1 de SSMA pour DB2 contient les modifications suivantes :

* SQL Server 2017 sur Windows et Linux CTP1 est désormais une plateforme cible prise en charge pour la migration. Cette fonctionnalité est en version d’évaluation technique et permet le déplacement des schémas et des données vers les serveurs SQL Server.
* Prise en charge des mises à jour automatiques pour télécharger la dernière version de SSMA dès qu’elle est disponible.
* Les fichiers binaires de SSMA installables sont désormais fournis via des fichiers de package Windows Installer (. msi).

## <a name="may-2016"></a>Mai 2016

La version 2016 de SSMA pour DB2 contient les modifications suivantes :

* Ajout de la prise en charge de SQL Server 2016.
* Ajout de la conversion des tables en mémoire et régulière DB2 pour SQL Server les fonctionnalités in-Memory et hekaton.
* Ajout de la conversion des contrôles d’accès DB2 en objets de stratégie SQL Server (Sécurité au niveau des lignes pour DB2).
* Ajout de la conversion de tables avec version gérée par le système DB2 en SQL Server tables temporelles.
* Amélioration de l’analyseur et du programme de résolution DB2.
* Vérification de l’installation de .NET 2,0 supprimée.
* Suppression \* de la dll inutile du programme d’installation DB2.
* Correction `save-project` des `open-project` commandes et de la console SSMA.
* Correction `securepassword` de la commande pour la console SSMA.
* Correction du décompte des objets pour le chargement initial.
* Correction du bogue dans les paramètres globaux.
  
## <a name="march-2016"></a>Mars 2016

La version d’évaluation de SSMA de mars 2016 de SSMA pour DB2 ajoute la prise en charge de la migration vers SQL Server 2016.

## <a name="january-2016"></a>Janvier 2016

La version de maintenance de SSMA pour DB2 de janvier 2016 contient les modifications suivantes :
  
* Ajout de la prise en charge d’un certain nombre de fonctions standard.
* Correction des erreurs de l’analyseur DB2.
* Correction de la prise en charge de DB2 v9 zOS (RFC 5690920).
* Correction des erreurs d’identificateur non résolu DB2 lors de la conversion.
* Ajout de l’élément de menu de l’affichage du journal à SSMA (RFC 5706203).
* Ajout de données de télémétrie.
  
## <a name="november-2014"></a>novembre 2014

La version de novembre 2014 de SSMA pour DB2 était la version initiale.
