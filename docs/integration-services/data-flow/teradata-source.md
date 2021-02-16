---
description: Connexion à la source Teradata
title: Connexion à la source Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d0170a81d73edef1e893a16df93e1bc004415739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337802"
---
# <a name="connect-to-the-teradata-source"></a>Connexion à la source Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

La source Teradata extrait les données des bases de données Teradata en utilisant :
- Une table ou une vue.
- Les résultats d'une instruction SQL.

La source utilise le gestionnaire de connexions Teradata pour se connecter à la source Teradata. Pour plus d’informations, consultez [Utiliser le gestionnaire de connexions Teradata](teradata-connection-manager.md).

## <a name="troubleshoot-the-teradata-source"></a>Dépannage de la source Teradata

Vous pouvez consigner les appels que la source Teradata effectue vers l'API Teradata Parallel Transporter (TPT). Pour cela, activez la journalisation des packages, puis sélectionnez l'événement **Diagnostic** au niveau du package.

Vous pouvez consigner les appels ODBC (Open Database Connectivity) que la source Teradata effectue vers le pilote ODBC Teradata en activant la trace du gestionnaire de pilotes ODBC. Pour plus d’informations, voir [Comment générer une trace ODBC avec l'administrateur de source de données ODBC](../../odbc/admin/setting-tracing-options.md).

## <a name="parallelism"></a>Parallélisme

La source Teradata prend en charge le parallélisme, dans lequel les tâches d’exportation peuvent accéder simultanément à la même table ou à différentes tables. Une variable de base de données appelée `MaxLoadTasks` fixe la limite du nombre de tâches d'exportation qui peuvent être exécutées en même temps. Vous pouvez définir ce nombre maximum à l’aide de la variable `MaxLoadTasks`.

## <a name="teradata-source-custom-properties"></a>Propriétés personnalisées de la source Teradata

Les propriétés personnalisées de la source Teradata sont répertoriées dans le tableau suivant. Toutes les propriétés sont en lecture/écriture.

|Nom de la propriété|Type de données|Description|
|:-|:-|:-|
|AccessMode|Integer (énumération)|Mode utilisé pour accéder à la base de données. Les valeurs possibles sont *Nom de la table* et *Commande SQL*. La valeur par défaut est *Nom de la table*.|
|BlockSize|Entier|Taille de bloc, en octets, utilisée lors du retour de données au client. La valeur par défaut est 1048576 (1 Mo). La valeur minimale est 256 octets. La valeur maximale est 16775168 octets.<br> Cette propriété apparaît dans le volet **Éditeur avancé**.|
|BufferMaxSize|Entier|Taille totale maximale du tampon de données renvoyé par la fonction GetBuffer. Cette taille doit être suffisamment grande pour contenir au moins une ligne de données, y compris l'en-tête de la ligne, la ligne de données proprement dite et le code de fin de la mémoire tampon. La taille maximale totale par défaut du tampon de données est de 16 775 552 octets. <br>Pour plus d'informations, voir [Exporter des données d'une base de données Teradata en utilisant GetBuffer](https://docs.teradata.com/reader/TvVKKmxaBAoyETJZD8zz_g/oaxiwNJmnCa6UctY4k498w).|
|BufferMode|Booléen|La valeur par défaut est *True*. La valeur doit être *True* si la fonctionnalité PutBuffer est utilisée. Cette propriété apparaît dans le volet **Éditeur avancé**.|
|DataEncryption|Booléen|La valeur par défaut est *False*. Un chiffrement de sécurité complet est utilisé si la valeur est *True*.|
|DefaultCodePage|Integer|Page de codes à utiliser quand la source de données n’a pas d’informations de page de codes. Cette propriété apparaît dans le volet **Éditeur avancé**.|
|DetailedTracingLevel|Integer (énumération)|Sélectionnez l’une des options suivantes pour le suivi avancé : <br> *Off* : Aucune journalisation avancée. <br> *Général* : Une journalisation du traçage général des activités spécifiques au pilote est effectuée. <br> *CLI* : Une journalisation du traçage des activités spécifiques à CLIv2 est effectuée. <br> *Méthode de notification* : Une journalisation du traçage des activités spécifiques à la fonctionnalité de notification est effectuée. <br> *Bibliothèque commune* : une journalisation du traçage des activités de la bibliothèque opcommon est effectuée. <br> *Tout* : Tout le traçage des activités précédentes est consigné. <br> Le fichier journal du traçage avancé est défini dans la propriété `DetailedTracingFile`. <br> La propriété `DetailedTracingFile` doit être définie si l'option n'est pas *Off*. Cette propriété apparaît dans le volet **Éditeur avancé**.|
|DetailedTracingFile|Chaîne|Chemin du fichier journal généré automatiquement lorsque *DetailedTracingLevel* n'est pas *Off*. Cette propriété apparaît dans le volet **Éditeur avancé**.|
|DiscardLargeRow|Booléen|La valeur par défaut est *False*. Ignorer les grandes lignes (supérieures à 64 ko) si la valeur est *true*.|
|ExtendedStringColumnsAllocation|Booléen|*Maximal Transfer Character Allocation Factor* est utilisé si la valeur est *True*. <br> Cette valeur doit être définie sur *True* si la propriété `Export Width Table ID` de la base de données Teradata est définie sur *Maximal Defaults*. <br> La valeur par défaut est *False*.|
|JobMaxRowSize|Entier|Taille de ligne maximale prise en charge. Cette valeur est requise si `DiscardLargeRow` est *True*.<br>Valeurs valides : <br>*64* (valeur par défaut) : Longueurs de lignes à 2 octets prises en charge. <br>*1024* : Longueurs de lignes à 4 octets prises en charge.|
|MaxSessions|Entier|Nombre maximal de sessions connectées. Cette valeur doit être supérieure à 1. La valeur par défaut est une session pour chaque Access Module Processor (AMP) disponible.|
|MinSessions|Entier|Nombre minimal de sessions connectées. Cette valeur doit être supérieure à 1. La valeur par défaut est une session pour chaque AMP disponible.|
|QueryBandSessInfo|Varchar|Expression de bande de requête définie par l'utilisateur, basée sur une session, dans un format de chaîne de connexion. Vous utilisez cette propriété pour la gouvernance et le contrôle de la rétrofacturation. Cette propriété apparaît dans le volet **Éditeur avancé**.|
|SpoolMode|Varchar|Les valeurs autorisées sont : <br>*Spool* : Utiliser la valeur par défaut *Spool*. <br> *NoSpool* : Ne pas utiliser *Spool*. Cette valeur n'est valide que si le serveur de base de données (DBS) prend en charge *NoSpool*. <br>  *NoSpoolOnly* : N’utiliser *Spool* dans aucun cas. La tâche sera interrompue par une erreur si le DBS ne prend pas en charge le service *NoSpool*.|
|SqlCommand|String|Commande SQL à exécuter lorsque la valeur `AccessMode` est *Commande SQL*.|
|TableName|String|Nom de la table contenant les données à utiliser quand `AccessMode` a la valeur *Nom de la table*.|
|TenacityHours|Entier|Nombre d’heures pendant lesquelles le pilote TPT tente de se connecter lorsque le nombre maximal d’opérations de chargement/exportation est déjà en cours d’exécution. La valeur par défaut est *4 heures*. Cette propriété apparaît dans le volet **Éditeur avancé**.|
|TenacitySleep|Entier|Nombre de minutes pendant lesquelles le pilote TPT s’interrompt avant de tenter de se connecter lorsque la limite est atteinte. La limite est définie par les propriétés `MaxSessions` et `TenacityHours`. La valeur par défaut est 6 minutes. Cette propriété apparaît dans le volet **Éditeur avancé**.|
|UnicodePassThrough|Booléen|*Off* (valeur par défaut) : Désactiver la transmission directe Unicode. <br>*Le* : Activer la transmission directe Unicode.|

## <a name="configure-the-teradata-source"></a>Configurer la source Teradata

Vous pouvez configurer la source Teradata par programmation ou en utilisant SQL Server Integration Services (SSIS) Designer.

Le volet **Teradata Source Editor** est présenté dans l'image suivante. Pour plus d'informations, consultez chacune des sections suivantes de l’Éditeur de source Teradata :

- [Volet Gestionnaire de connexions](#the-connection-manager-pane)
- [Volet Colonnes](#the-columns-pane)
- [Volet Sortie d’erreur](#the-error-output-pane)

![Éditeur de source Teradata](media/teradata-source.png)

Le volet **Éditeur avancé** contient les propriétés qui peuvent être définies par programmation. Pour ouvrir le volet :
- Sur la page **Flux de données** de votre projet Integration Services, cliquez avec le bouton droit sur la source Oracle, puis sélectionnez **Afficher l’éditeur avancé**.

Pour plus d’informations sur les propriétés que vous pouvez définir dans le volet **Éditeur avancé** , consultez [Propriétés personnalisées des sources Teradata](#teradata-source-custom-properties).

## <a name="the-connection-manager-pane"></a>Volet Gestionnaire de connexions

Utilisez le volet **Gestionnaire de connexions** pour sélectionner l'instance de gestionnaire de connexions Teradata pour la source. Ce volet vous permet également de sélectionner une table ou une vue dans la base de données. Pour ouvrir le volet :

1. Dans SQL Server Data Tools, ouvrez le package SSIS contenant la source Teradata.

1. Sous l’onglet **Flux de donnée** s, double-cliquez sur la source Teradata.

1. Dans l’Éditeur de source Teradata, sélectionnez l’onglet **Gestionnaire de connexions**.

### <a name="options"></a>Options

**Connection manager**

* Sélectionnez un gestionnaire de connexions existant dans la liste, ou choisissez **Nouveau** pour créer une instance de gestionnaire de connexions Teradata.

**Nouveau**

* Sélectionnez **Nouveau**. Le volet **Éditeur du gestionnaire de connexions Teradata** s’ouvre. Ce volet vous permet de créer un gestionnaire de connexions.

**Mode d’accès aux données**

* Choisissez une méthode de sélection des données dans la source. Ces fonctions sont répertoriées dans le tableau suivant :

    |Option|Description|
    |:-|:-|
    |Nom de la table - TPT Export|Permet de récupérer les données d’une table ou d’une vue dans la source de données Teradata. Quand cette option est sélectionnée, choisissez une table ou une vue disponible dans la liste pour **Nom de la table ou de la vue**.|
    |Commande SQL - Exportation TPT|Extrayez les données de la source de données Teradata à l’aide d’une requête SQL. Quand cette option est sélectionnée, entrez une requête de l’une des manières suivantes : <ul><li>Entrez le texte de la requête SQL dans le champ **Texte de la commande SQL** .</li><li>Sélectionnez **Parcourir** pour charger la requête SQL à partir d'un fichier texte.</li><li>Pour vérifier la syntaxe du texte de la requête, sélectionnez **Analyser** .</li></ul>|

**Préversion**

* Sélectionnez **Aperçu** pour afficher les 200 premières lignes de données extraites de la table ou de la vue sélectionnée.

## <a name="the-columns-pane"></a>Volet Colonnes

Utilisez le volet **Colonnes** pour mapper une colonne de sortie à chaque colonne (source) externe. Pour ouvrir le volet :

1. Dans SQL Server Data Tools, ouvrez le package SSIS contenant la source Teradata.

1. Sous l’onglet **Flux de donnée** s, double-cliquez sur la source Teradata.

1. Dans l’Éditeur de source Teradata, sélectionnez l’onglet **Colonnes**.

### <a name="options"></a>Options

**Colonnes externes disponibles**

Ce tableau énumère les colonnes externes disponibles que vous pouvez sélectionner pour les ajouter à la liste **Colonne externe**. Vous pouvez énumérer les colonnes dans l'ordre de votre choix. Vous ne pouvez pas ajouter ou supprimer des colonnes à l'aide de cette table.

* Pour sélectionner toutes les colonnes, cochez la case **Sélectionner tout**.

**Colonnes externes**

Les colonnes externes (sources) que vous avez sélectionnées sont listées dans l’ordre. Pour changer l’ordre, désactivez d’abord la liste **Colonne externe disponible**, puis sélectionnez les colonnes dans un ordre différent.

**Colonne de sortie**

Même si le nom de la colonne externe (source) sélectionnée correspond au nom de sortie par défaut, vous pouvez saisir n'importe quel nom unique.

>[!NOTE]
>>Si certaines colonnes contiennent des types de données non pris en charge, vous recevrez un avertissement qui indiquera ces types de données non pris en charge, et les colonnes concernées seront supprimées des colonnes correspondantes.

## <a name="the-error-output-pane"></a>Volet Sortie d’erreur

Utilisez le volet **Sortie d’erreur** pour sélectionner les options de traitement des erreurs. Pour ouvrir le volet :

1. Dans SQL Server Data Tools, ouvrez le package SSIS contenant la source Teradata.

1. Sous l’onglet **Flux de donnée** s, double-cliquez sur la source Teradata.

1. Dans l’Éditeur de source Teradata, sélectionnez l’onglet **Sortie d’erreur**.

### <a name="options"></a>Options

**Comportement d’erreur**

* Choisissez la façon dont la source Teradata doit traiter les erreurs dans un flux : 
  * Ignorer l’erreur
  * Rediriger la ligne
  * Faire échouer le composant

**Rubriques connexes** : Voir [Gestion des erreurs dans les données](error-handling-in-data.md).

**Troncation**

* Choisissez la façon dont la source Teradata doit traiter la troncation dans un flux : 
  * Ignorer l’erreur
  * Rediriger la ligne
  * Faire échouer le composant

## <a name="next-steps"></a>Étapes suivantes

- Configurez la [destination Teradata](teradata-destination.md).
- Si vous avez des questions, rendez-vous sur [Tech Community](https://aka.ms/AA6iwdw).