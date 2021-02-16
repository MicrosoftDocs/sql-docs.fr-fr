---
description: Destination Oracle
title: Destination Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3b28d420d66e55565c882c9fa2ae41f86e1c0764
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343232"
---
# <a name="oracle-destination"></a>Destination Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

La destination Oracle charge les données en masse dans Oracle Database.

La destination utilise le gestionnaire de connexions Oracle pour se connecter à une source de données. Pour plus d’informations, consultez [Gestionnaire de connexions Oracle](oracle-connection-manager.md).

Une destination Oracle inclut des mappages entre les colonnes d’entrée et les colonnes de la source de données de destination. Vous n'avez pas besoin de mapper les colonnes d'entrée à toutes les colonnes de destination, mais en fonction des propriétés des colonnes de destination, des erreurs peuvent se produire si aucune colonne d'entrée n'est mappée aux colonnes de destination. Par exemple, si une colonne de destination n'autorise pas les valeurs null, une colonne d'entrée doit être mappée à cette colonne. En outre, si les données d’entrée ne sont pas compatibles pour le type de colonne de destination, une erreur se produit au moment de l’exécution. En fonction du comportement des erreurs configuré, l’erreur sera ignorée, entraînera un échec, ou la ligne sera redirigée vers la sortie d’erreur.

La destination Oracle comporte une entrée normale et une sortie d’erreur.

Les colonnes de types de données non pris en charge sont supprimées des colonnes avec un avertissement avant le mappage.
Pour plus d’informations, consultez [Prise en charge des types de données](oracle-data-type-support.md).

## <a name="load-options"></a>Options de chargement

Deux modes de chargement d’accès sont pris en charge. Le mode peut être défini dans l’[Éditeur de destination Oracle (page Gestionnaire de connexions)](#oracle-destination-editor-connection-manager-page). Les deux modes sont :

- Chargement par lot : ce mode permet de charger des données dans une table Oracle par lots. Le lot entier est inséré sous la même transaction.
Pour plus d’informations sur la façon de configurer ce mode, consultez [Éditeur de destination Oracle (page Gestionnaire de connexions)](#oracle-destination-editor-connection-manager-page) et [Propriétés personnalisées de la destination Oracle](#oracle-destination-custom-properties).

- Chargement rapide à l’aide de Direct Path : ce mode consiste à utiliser le mode de chemin direct du pilote pour le chargement de la table Oracle. Il existe des restrictions lors de l’utilisation de ce mode. Pour plus d’informations, consultez la documentation d’Oracle.  
Pour plus d’informations sur la façon de configurer ce mode, consultez [Éditeur de destination Oracle (page Gestionnaire de connexions)](#oracle-destination-editor-connection-manager-page) et [Propriétés personnalisées de la destination Oracle](#oracle-destination-custom-properties).

## <a name="error-handling"></a>Gestion des erreurs

La destination Oracle a une sortie d’erreur. La sortie d'erreur du composant contient les colonnes de sortie suivantes :

- **Code d'erreur** : numéro qui représente le type de l’erreur actuelle. Le code d’erreur peut provenir du :
    - Serveur Oracle. Consultez la description détaillée de l’erreur dans la documentation de la base de données Oracle.
    - Runtime SSIS. Pour obtenir la liste des codes d'erreur SSIS, consultez le Guide de référence des erreurs et des événements SSIS.
- **Colonne d’erreur** : numéro de la colonne source qui provoque les erreurs de conversion.

- **Colonnes de données d’erreur** : données à l’origine de l’erreur.

Les types d’erreurs de sortie pendant le processus de chargement pris en charge sont les suivants : conversion de données, troncation, violation de contrainte, et ainsi de suite. Voir [Éditeur de destination Oracle (page Sortie d’erreur)](#oracle-destination-editor-error-output-page).

La propriété **Nombre maximal d’erreurs (MaxErrors)** définit le nombre maximal d’erreurs qui peuvent se produire. L’exécution s’arrête et retourne des erreurs quand le nombre maximal est atteint. Seuls les enregistrements d’exécution avant que le nombre maximal d’erreurs soit atteint sont inclus dans la table cible. Pour plus d’informations sur la configuration, consultez [Éditeur de destination Oracle (page Gestionnaire de connexions)](#oracle-destination-editor-connection-manager-page).

## <a name="parallelism"></a>Parallélisme

En mode de chargement par lot, il n’existe aucune restriction concernant la configuration de l’exécution en parallèle, mais les performances peuvent être affectées par le mécanisme standard de verrouillage des enregistrements. Le volume de perte de performances dépend des données et de l’organisation de la table.

Dans le protocole de chemin direct (chargement rapide), une seule destination Oracle peut être configurée pour s’exécuter sur la même table en même temps, mais peut utiliser le mode parallèle.

Un chemin direct parallèle autorise plusieurs charges de chemin direct, avec lesquelles plusieurs destinations Oracle peuvent être configurées pour s’exécuter simultanément sur la même table en même temps. Oracle ne verrouille pas la table cible exclusivement pour une utilisation dans la session de chargement rapide, ce qui permet d’exécuter des composants de destination de chargement rapide supplémentaires afin de charger la même table cible en parallèle.
Le chemin direct parallèle est plus restrictif ; toute utilisation du parallélisme doit être planifiée à l’avance.

Il n’y a aucune raison d’utiliser une seule session parallèle.

Consultez la documentation Oracle relative aux restrictions lors de l’utilisation de chargements de chemins directs parallèles.

Pour plus d’informations, consultez [Propriétés personnalisées de la destination Oracle](#oracle-destination-custom-properties).

## <a name="troubleshooting-the-oracle-destination"></a>Résolution des problèmes liés à la destination Oracle

Vous pouvez enregistrer dans le journal les appels ODBC effectués par la source Oracle vers des sources de données Oracle pour résoudre les problèmes d’exportation des données. Pour cela, activez la trace du gestionnaire de pilotes ODBC. Pour plus d’informations, consultez la documentation Microsoft intitulée *Génération d’une trace ODBC avec l’administrateur des source de données ODBC*.

## <a name="oracle-destination-custom-properties"></a>Propriétés personnalisées de la destination Oracle

Le tableau suivant décrit les propriétés personnalisées de la destination Oracle. Toutes les propriétés sont en lecture/écriture.

|Nom de la propriété|Type de données|Description|Mode de chargement|
|:-|:-|:-|:-|
|BatchSize|Integer|Taille du lot pour le chargement en bloc. Il s’agit du nombre de lignes chargées dans un même lot.|Utilisée uniquement en mode batch.|
|DefaultCodePage|Integer|Page de codes à utiliser quand la source de données n’a pas d’informations de page de codes. <br>**Remarque** : Cette propriété est définie uniquement par l’**Éditeur avancé**.|Utilisée pour les deux modes.|
|FastLoad|Booléen|Indique si le chargement rapide est utilisé. La valeur par défaut est **false**. Vous pouvez aussi définir cette propriété dans l’[Éditeur de destination Oracle (page Gestionnaire de connexions)](#oracle-destination-editor-connection-manager-page). |Utilisée pour les deux modes.|
|MaxErrors|Integer|Nombre d’erreurs qui peuvent se produire avant l’arrêt du flux de données. La valeur par défaut est **0**, ce qui signifie qu’il n’y a aucune limite quant au nombre d’erreurs.<br> Si **Rediriger le flux** est sélectionné dans la page **Gestion des erreurs**. Avant que la limite du nombre d’erreurs soit atteinte, toutes les erreurs sont retournées dans la sortie d’erreur. Pour plus d’informations, consultez [Gestion des erreurs](#error-handling).|Utilisée uniquement en mode de chargement rapide.|
|NoLogging|Booléen|Indique si la journalisation de base de données est désactivée. La valeur par défaut est **false**, ce qui signifie que la journalisation est activée.|Utilisée pour les deux modes.|
|Parallèle|Booléen|Indique si le chargement parallèle est autorisé. **True** indique que d’autres sessions de chargement sont autorisées à s’exécuter sur la même table cible.<br> Pour plus d’informations, consultez [Parallélisme](#parallelism).|Utilisée uniquement en mode de chargement rapide.|
|TableName|String|Nom de la table contenant les données en cours d’utilisation.|Utilisée pour les deux modes.|
|TableSubName|String|Sous-nom ou sous-partition. Cette valeur est facultative.<br> **Remarque** : Cette propriété ne peut être définie que dans l’**Éditeur avancé**.|Utilisée uniquement en mode de chargement rapide.|
|TransactionSize|Integer|Nombre d’insertions pouvant être effectuées dans une même transaction. La valeur par défaut est la valeur **BatchSize**.|Utilisée uniquement en mode batch.|
|TransferBufferSize|Integer|Taille de la mémoire tampon de transfert. La valeur par défaut est 64 Ko.|Utilisée uniquement en mode de chargement rapide.|

## <a name="configuring-the-oracle-destination"></a>Configuration de la destination Oracle

La destination Oracle peut être configurée par programmation ou par le biais du concepteur SSIS.

L’Éditeur de destination Oracle est illustré ci-dessous. Il contient la page Gestionnaire de connexions, la page Mappages et la page Sortie d’erreur.

Pour plus d’informations, consultez l’une des sections suivantes :

- [Éditeur de destination Oracle (page Gestionnaire de connexions)](#oracle-destination-editor-connection-manager-page)
- [Éditeur de destination Oracle (page Mappages)](#oracle-destination-editor-mappings-page)
- [Éditeur de destination Oracle (page Sortie d’erreur)](#oracle-destination-editor-error-output-page)

![Destination Oracle](media/oracle-destination.png)

La boîte de dialogue **Éditeur avancé** contient les propriétés qui peuvent être définies par programme.
Pour ouvrir la boîte de dialogue **Éditeur avancé** :

- Sur l’écran **Flux de données** de votre projet Integration Services, cliquez avec le bouton droit sur la destination Oracle, puis sélectionnez **Afficher l’éditeur avancé**.

Pour plus d’informations sur les propriétés que vous pouvez définir dans la boîte de dialogue Éditeur avancé, consultez [Propriétés personnalisées de la destination Oracle](#oracle-destination-custom-properties).

## <a name="oracle-destination-editor-connection-manager-page"></a>Éditeur de destination Oracle (page Gestionnaire de connexions)

Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de destination Oracle** pour sélectionner le gestionnaire de connexions Oracle de la destination. Cette page vous permet également de sélectionner une table ou une vue à partir de la base de données.

**Pour ouvrir la page Gestionnaire de connexions de l’Éditeur de destination Oracle**

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la destination Oracle.

- Sous l’onglet Flux de données, double-cliquez sur la destination Oracle.

- Dans l’Éditeur de destination Oracle, cliquez sur Gestionnaire de connexions.

### <a name="options"></a>Options

**Connection manager**

Sélectionnez un gestionnaire de connexions existant dans la liste ou cliquez sur **Nouveau** pour créer un gestionnaire de connexions Oracle.

**Nouveau**

Cliquez sur **Nouveau**. La boîte de dialogue **Éditeur du gestionnaire de connexions Oracle** s’ouvre et vous permet de créer un nouveau gestionnaire de connexions.

**Mode d'accès aux données**

Spécifiez la méthode de sélection des données dans la source. Ces fonctions sont répertoriées dans le tableau suivant :

|Option|Description|
|:-|:-|
|Nom de la table|Configurez la destination Oracle pour fonctionner en mode batch. Options :<br><br> **Nom de la table ou de la vue**: sélectionnez dans la liste une table ou une vue disponible dans la base de données.<br><br> **Taille de la transaction** : entrez le nombre d’insertions qui peuvent se trouver dans une même transaction. La valeur par défaut est la valeur **BatchSize**.<br><br> **Taille du lot** : tapez la taille (nombre de lignes chargées) du lot pour le chargement en masse.
|Nom de la table – chargement rapide|Configurez la destination Oracle pour fonctionner en mode de chargement rapide (Direct Path). <br><br>Options disponibles :<br><br> **Nom de la table ou de la vue**: sélectionnez dans la liste une table ou une vue disponible dans la base de données.<br><br> **Chargement parallèle** : indique si le chargement parallèle est activé. Pour plus d’informations, consultez [Parallélisme](#parallelism).<br><br> **Aucune journalisation** : cochez cette case pour désactiver la journalisation de base de données. Cette journalisation est utilisée à des fins de récupération, et n’est pas liée au suivi.<br><br> **Nombre maximal d’erreurs** : nombre maximal d’erreurs pouvant se produire avant l’arrêt du flux de données. La valeur par défaut est 0, ce qui signifie qu’il n’y a aucune limite.<br><br> Toutes les erreurs sont retournées dans la sortie d’erreur.<br><br> **Taille de la mémoire tampon de transfert (Ko)**  : entrez la taille de la mémoire tampon de transfert. La taille par défaut est de 64 Ko.|

**Afficher les données existantes**

Cliquez sur **Afficher les données existantes** pour afficher jusqu’à 200 lignes de données pour la table sélectionnée.

## <a name="oracle-destination-editor-mappings-page"></a>Éditeur de destination Oracle (page Mappages)

La page **Mappages** de la boîte de dialogue **Éditeur de destination Oracle** vous permet de mapper les colonnes d’entrée aux colonnes de destination.

**Pour ouvrir la page Mappages de l’Éditeur de destination Oracle**

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la destination Oracle.

- Sous l’onglet Flux de données, double-cliquez sur la destination Oracle.

- Dans l’Éditeur de destination Oracle, cliquez sur Mappages.

### <a name="options"></a>Options

**Colonnes d'entrée disponibles**

Liste des colonnes d'entrée disponibles. Par glisser-déplacer, mappez une colonne d'entrée à une colonne de destination disponible.

**Colonnes de destination disponibles**

Liste des colonnes de destination disponibles. Par glisser-déplacer, mappez une colonne de destination à une colonne d'entrée disponible.

**Colonne d'entrée**

Affichez les colonnes d’entrée que vous avez sélectionnées. Vous pouvez supprimer des mappages en sélectionnant **< ignorer >** de manière à exclure des colonnes de la sortie.

**Colonne de destination**

Affiche toutes les colonnes de destination disponibles, mappées et non mappées.

> [!NOTE]
>
>Les colonnes de types de données non pris en charge seront supprimées du mappage avec un avertissement.

## <a name="oracle-destination-editor-error-output-page"></a>Éditeur de destination Oracle (page Sortie d’erreur)

Utilisez la page Sortie d’erreur de la boîte de dialogue Éditeur de destination Oracle pour sélectionner les options de gestion des erreurs.

**Pour ouvrir la page Sortie d’erreur de l’Éditeur de destination Oracle**

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la destination Oracle.

- Sous l’onglet Flux de données, double-cliquez sur la destination Oracle.

- Dans l’Éditeur de destination Oracle, cliquez sur Sortie d’erreur.

### <a name="options"></a>Options

**Comportement d’erreur**

Sélectionnez la façon dont la source Oracle doit gérer les erreurs dans un flux : ignorer l’échec, rediriger la ligne ou faire échouer le composant.
**Section connexe** : [Gestion des erreurs dans les données](./error-handling-in-data.md)

**Troncation**

Sélectionnez la façon dont la source Oracle doit gérer la troncation dans un flux : ignorer l’échec, rediriger la ligne ou faire échouer le composant.

## <a name="next-steps"></a>Étapes suivantes

- Configurer le [Gestionnaire de connexions Oracle](oracle-connection-manager.md).
- Configurer la [Source Oracle](oracle-source.md).
- Configurer la [Destination Oracle](oracle-destination.md).
- Si vous avez des questions, rendez-vous sur [TechCommunity](https://aka.ms/AA5u35j).
