---
description: Source Oracle
title: Source Oracle
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6d26daafb62777091cb962332d47d8c00620e4d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343204"
---
# <a name="oracle-source"></a>Source Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

La source Oracle extrait les données d’Oracle Database en utilisant les modes suivants :

- Une table ou une vue.

- Les résultats d'une instruction SQL.

La source utilise un gestionnaire de connexions Oracle pour se connecter à la source Oracle. Pour plus d’informations, consultez [Gestionnaire de connexions Oracle](oracle-connection-manager.md).

## <a name="error-output"></a>Sortie d’erreur

La sortie d’erreur comprend les colonnes suivantes :  

- **Code d'erreur** : numéro qui représente le type de l’erreur actuelle. Le code d’erreur peut provenir du :
    - Serveur Oracle. Consultez la description détaillée de l’erreur dans la documentation de la base de données Oracle.
    - Runtime SSIS. Pour obtenir la liste des codes d'erreur SSIS, consultez le Guide de référence des erreurs et des événements SSIS.
- **Colonne d’erreur** : numéro de la colonne source qui provoque les erreurs de conversion.

- **Colonnes de données d’erreur** : données à l’origine de l’erreur.

La source Oracle retourne les erreurs qui se sont produites lors du chargement et du processus d’extraction dans la sortie d’erreur. Pour plus d’informations, consultez [Éditeur de source Oracle (page Sortie d’erreur)](#oracle-source-editor-error-output-page).

## <a name="troubleshooting-the-oracle-source"></a>Résolution des problèmes liés à la source Oracle

Vous pouvez enregistrer dans le journal les appels ODBC effectués par la source Oracle vers des sources de données Oracle pour résoudre les problèmes d’exportation des données. Pour cela, activez la trace du gestionnaire de pilotes ODBC. Pour plus d’informations, consultez la documentation Microsoft intitulée *Génération d’une trace ODBC avec l’administrateur des source de données ODBC*.

## <a name="oracle-source-custom-properties"></a>Propriétés personnalisées de la source Oracle

Les propriétés personnalisées de la source Oracle sont les suivantes. Toutes les propriétés sont en lecture/écriture.

|Nom de la propriété|Type de données|Description|
|:-|:-|:-|
|AccessMode|Integer (énumération)|Mode utilisé pour accéder à la base de données. Les valeurs possibles sont **Nom de la table** et **Commande SQL**. La valeur par défaut est **Nom de la table**.|
|BatchSize|Integer|Taille du lot pour le chargement en bloc. Il s'agit du nombre d'enregistrements extraits en tant que table. <br>Cette propriété est définie uniquement par l’**Éditeur avancé**.|
|DefaultCodePage|Integer|Page de codes à utiliser quand la source de données n’a pas d’informations de page de codes. <br>Cette propriété est définie uniquement par l’**Éditeur avancé**.|
|PreFetchCount|Integer|Nombre de lignes prérécupérées. <br>Cette propriété est définie uniquement par l’**Éditeur avancé**.|
|SqlCommand|String|Commande SQL à exécuter lorsque la valeur d'AccessMode est Commande SQL.|
|TableName|String|Nom de la table contenant les données à utiliser quand AccessMode a la valeur Nom de la table.|

## <a name="configuring-the-oracle-source"></a>Configuration de la source Oracle

Vous pouvez définir la source Oracle par programmation ou par le biais du concepteur SSIS.

L’Éditeur de Source Oracle est illustré ci-dessous. Il contient la page Gestionnaire de connexions, la page Colonnes et la page Sortie d’erreur.

Pour plus d’informations, consultez l’une des sections suivantes :

- [Éditeur de source Oracle (page Gestionnaire de connexions)](#oracle-source-editor-connection-manager-page)
- [Éditeur de source Oracle (page Colonnes)](#oracle-source-editor-columns-page)
- [Éditeur de source Oracle (page Sortie d’erreur)](#oracle-source-editor-error-output-page)

![Source Oracle](media/oracle-source.png)

La **boîte de dialogue Éditeur avancé** contient les propriétés qui peuvent être définies par programmation.

Pour ouvrir la boîte de dialogue **Éditeur avancé** :

- Sur l’écran **Flux de données** de votre projet Integration Services, cliquez avec le bouton droit sur la source Oracle, puis sélectionnez **Afficher l’éditeur avancé**.

Pour plus d’informations sur les propriétés que vous pouvez définir dans la boîte de dialogue **Éditeur avancé**, consultez [Propriétés personnalisées de la source Oracle](#oracle-source-custom-properties).

## <a name="oracle-source-editor-connection-manager-page"></a>Éditeur de source Oracle (page Gestionnaire de connexions)

Dans la page **Gestionnaire de connexions**, la boîte de dialogue **Éditeur de source Oracle** permet de sélectionner Oracle Database en tant que source, table ou vue dans la base de données.

**Pour ouvrir la page Gestionnaire de connexions de l’Éditeur de source Oracle**

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la source Oracle.

- Sous l’onglet Flux de données, double-cliquez sur la source Oracle.
### <a name="options"></a>Options

**Connection manager**

Sélectionnez un gestionnaire de connexions existant dans la liste ou cliquez sur **Nouveau** pour créer un gestionnaire de connexions Oracle.

**Nouveau**

Cliquez sur **Nouveau**. La boîte de dialogue **Éditeur du gestionnaire de connexions Oracle** s’ouvre et vous permet de créer un nouveau gestionnaire de connexions.

**Mode d’accès aux données**

Spécifiez la méthode de sélection des données dans la source. Ces fonctions sont répertoriées dans le tableau suivant :

|Option|Description|
|:-|:-|
|Table ou vue|Permet de récupérer les données d’une table ou d’une vue dans la source de données Oracle. Quand cette option est sélectionnée, choisissez une table ou une vue disponible dans la liste pour **Nom de la table ou de la vue**.|
|Commande SQL|Extrayez les données de la source de données Oracle à l’aide d’une requête SQL. Quand cette option est sélectionnée, entrez une requête de l’une des manières suivantes : <br>Entrez le texte de la requête SQL dans le champ **Texte de la commande SQL** . <br>Cliquez sur **Parcourir** pour charger la requête SQL à partir d'un fichier texte. <br>Pour vérifier la syntaxe du texte de la requête, cliquez sur **Analyser** .|

**Préversion**

Cliquez sur **Aperçu** pour afficher les 200 premières lignes de données extraites de la table ou de la vue sélectionnée.

## <a name="oracle-source-editor-columns-page"></a>Éditeur de source Oracle (page Colonnes)

Dans la page **Colonnes**, la boîte de dialogue **Éditeur de source Oracle** permet de mapper une colonne de sortie à chaque colonne externe (source).

**Pour ouvrir la page Colonnes de l’Éditeur de source Oracle**

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la source Oracle.

- Sous l’onglet Flux de données, double-cliquez sur la source Oracle.

- Dans l’Éditeur de source Oracle, cliquez sur Colonnes.

### <a name="options"></a>Options

**Colonnes externes disponibles**

Liste de colonnes externes disponibles qui peuvent être sélectionnées pour être ajoutées à la liste **Colonnes externes** dans l’ordre dans lequel elles sont sélectionnées. Vous ne pouvez pas utiliser cette table pour ajouter ou supprimer des colonnes.

Cochez la case **Sélectionner tout** pour sélectionner toutes les colonnes.

**Colonnes externes**

Les colonnes externes (sources) que vous avez sélectionnées sont listées dans l’ordre.
Pour changer l’ordre, désactivez d’abord la liste « Colonne externe disponible », puis sélectionnez la ou les colonnes avec un ordre différent.

**Colonne de sortie**

Le nom de la colonne externe (source) sélectionnée correspond au nom de la sortie par défaut. Vous pouvez entrer n’importe quel nom unique.
> [!NOTE]
>
>S’il existe des colonnes avec des types de données non pris en charge, un avertissement s’affiche pour indiquer que les types de données ne sont pas pris en charge, et les colonnes associées sont supprimées des colonnes de mappage.

## <a name="oracle-source-editor-error-output-page"></a>Éditeur de source Oracle (page Sortie d’erreur)

Utilisez la page **Sortie d’erreur** de la boîte de dialogue **Éditeur de source Oracle** pour sélectionner les options de gestion des erreurs.

**Pour ouvrir la page Sortie d’erreur de l’Éditeur de source Oracle**

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la source Oracle.

- Sous l’onglet Flux de données, double-cliquez sur la source Oracle.

- Dans l’Éditeur de source Oracle, cliquez sur Sortie d’erreur.

### <a name="options"></a>Options

**Comportement d’erreur**

Sélectionnez la façon dont la source Oracle doit gérer les erreurs dans un flux : ignorer l’échec, rediriger la ligne ou faire échouer le composant.
**Section connexe** : [Gestion des erreurs dans les données](./error-handling-in-data.md)

**Troncation**

Sélectionnez la façon dont la source Oracle doit gérer la troncation dans un flux : ignorer l’échec, rediriger la ligne ou faire échouer le composant.

## <a name="next-steps"></a>Étapes suivantes

- Configurer la [Destination Oracle](oracle-destination.md).
- Si vous avez des questions, rendez-vous sur [TechCommunity](https://aka.ms/AA5u35j).
