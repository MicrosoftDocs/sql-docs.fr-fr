---
description: Destination Teradata
title: Destination Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 83453cfa36ae4b2e9f6596acc7206162c2b82b76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342271"
---
# <a name="teradata-destination"></a>Destination Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

La destination Teradata charge en masse les données dans la base de données Teradata.

La destination utilise le gestionnaire de connexions Teradata pour se connecter à une source de données. Pour plus d’informations, consultez [Gestionnaire de connexions Teradata](teradata-connection-manager.md).

## <a name="load-options"></a>Options de chargement

La destination Teradata prend en charge deux modes de chargement des données :

- **TPT Stream** : Ce mode utilise l’opérateur TPT API Stream (protocole Teradata TPump).

- **TPT Load** (chargement en masse rapide) : Ce mode utilise l’opérateur TPT API Load (protocole Teradata FastLoad) pour le chargement en masse rapide.

Le mode de chargement rapide présente les restrictions suivantes :

- Le nombre maximal de sessions pour la base de données Teradata est déterminé par le facteur suivant, rencontré en premier :
    - Limites de sessions définies à l’aide de la commande SESSIONS
    - Limite de la base de données Teradata d’une session par AMP
    - Limite de la plateforme pour le nombre maximal de sessions par application : Définie par la variable MaxSess dans le fichier logiciel de l’interface du processeur de communications (COP), CLISPB.DAT. Vous pouvez utiliser la commande TDP SET MAXSESSIONS pour spécifier une limite de plateforme. La limite par défaut correspond à la valeur MAXSESS sur le serveur.

- Les index de jointure ne sont pas pris en charge.
- Les références de clés étrangères dans les tables cibles ne sont pas prises en charge.
- Les tables cibles définies avec un index secondaire ne sont pas prises en charge.

Pour plus d’informations sur les restrictions de chargement rapide dans Teradata, consultez la référence de chargement rapide de Teradata.

Vous pouvez définir le mode dans l’[Éditeur de destination Teradata (page Gestionnaire de connexions)](#teradata-destination-editor-connection-manager-page).

## <a name="error-handling"></a>Gestion des erreurs

Les erreurs renvoyées pendant le processus de chargement sont inscrites dans des tables d'erreurs temporaires verrouillées pendant le processus de chargement.
La propriété **Nombre maximal d'erreurs (MaxErrors)** de l'Éditeur avancé définit le nombre maximal d'erreurs pouvant être inscrites dans ces tables.

Si le nombre maximum d'erreurs est supérieur à zéro, des tables d'erreurs avec des noms uniques sont générées, et un message d'information est consigné dans le journal des packages. Les erreurs peuvent être récupérées par la sortie d'erreur standard du composant SSIS.

Les tables temporaires sont supprimées à la fin du processus de chargement. Si les tables temporelles ne peuvent pas être lues par la destination Teradata, elles sont supprimées uniquement si la propriété **Toujours supprimer la table d'erreurs** est sélectionnée.  Si le processus de chargement est interrompu prématurément, vous devez supprimer manuellement ces tables si nécessaire. Ces tables se trouvent dans la même base de données que la table de destination.

Lorsque le **nombre maximal d'erreurs** est atteint, l'état de la table cible dépend du mode utilisé.

- En mode de chargement rapide, la table cible n'est pas utilisable. Pour l’exécuter à nouveau, vous devez tronquer ou supprimer puis recréer la table cible. La restauration n’est pas prise en charge.
- En mode opérateur TPT Steam, la destination Teradata s'exécute via un mécanisme de lignes avec mise en mémoire tampon. Si la tâche échoue, toutes les modifications effectuées (tampons envoyés) au moment de la défaillance sont conservées de façon permanente dans la ou les tables cibles. Il n’existe aucun concept de restauration. Les tables d’erreurs vont être supprimées.

La destination Teradata a une sortie d’erreur. Pour plus d’informations, consultez [Éditeur de destination Teradata (page Sortie d’erreur)](#teradata-destination-editor-error-output-page).

## <a name="parallelism"></a>Parallélisme

Le parallélisme étant limité en mode de chargement rapide, plusieurs tâches de chargement rapide indépendantes ne peuvent pas accéder simultanément à la même table. Par ailleurs, le nombre de tâches simultanées à charge rapide est limité par la variable de base de données **MaxLoadTasks**.

Il n’existe aucune restriction de parallélisme en mode TPT Stream. Vous pouvez exécuter plusieurs destinations Teradata simultanément sur une même table, mais cela peut réduire les performances de Teradata. Pour plus d’informations, consultez la documentation Teradata.

## <a name="troubleshooting-the-teradata-destination"></a>Résolution des problèmes liés à la destination Teradata

Vous pouvez consigner les appels que la source Teradata effectue vers l'API Teradata Parallel Transporter (TPT API). Vous pouvez activer la journalisation des packages puis sélectionner l'événement **Diagnostic** au niveau du package pour consigner les appels.

Vous pouvez consigner les appels ODBC que la source Teradata effectue vers le pilote ODBC Teradata en activant la trace du gestionnaire de pilotes ODBC. Pour plus d’informations, consultez la documentation Microsoft sur la *génération d’une trace ODBC avec l’administrateur de sources de données ODBC*.

## <a name="teradata-destination-custom-properties"></a>Propriétés personnalisées de la destination Teradata

Le tableau suivant décrit les propriétés personnalisées de la destination Teradata. Toutes les propriétés sont en lecture/écriture.

|Nom de la propriété|Type de données|Description|
|:-|:-|:-|
|AlwaysDropErrorTable|Booléen|La valeur par défaut est **False**. Supprimer toutes les tables d’erreurs si **True**, même en cas d’échec de lecture de la destination Teradata.|
|ArraySupport|Booléen|La valeur par défaut est **True**. Les groupes DML utilisent ArraySupport si **True**. Applicable uniquement à TPT Stream. Cette propriété apparaît dans l’**Éditeur avancé**.|
|Mémoires tampons|Entier|Nombre de mémoires tampon de requête à augmenter, la valeur peut être comprise entre 2 et 64. Applicable uniquement à TPT Stream. Cette propriété apparaît dans l’**Éditeur avancé**.|
|BufferMode|Booléen|La valeur par défaut est **True**. Doit être **True** si la fonctionnalité PutBuffer est utilisée. Cette propriété apparaît dans l’**Éditeur avancé**.|
|BufferSize|Entier|Taille de la mémoire tampon de sortie (en ko) utilisée pour l'envoi des paquets de chargement. La valeur par défaut est 1024. Applicable uniquement à TPT Load. Cette propriété apparaît dans l’**Éditeur avancé**.|
|DataEncryption|Booléen|La valeur par défaut est **False**. Un chiffrement de sécurité complet est utilisé si **True**.|
|DefaultCodePage|Integer|Page de codes à utiliser quand la source de données n’a pas d’informations de page de codes. <br>**Remarque** : Cette propriété apparaît dans l’**Éditeur avancé**.|
|DetailedTracingLevel|Entier (énumération)|Sélectionnez l’une des options suivantes pour le suivi avancé : <br> **Off** : Aucune journalisation avancée. <br> **Général** : Une journalisation du traçage général des activités spécifiques au pilote est effectuée. <br> **CLI** : Une journalisation du traçage des activités spécifiques à CLIv2 est effectuée. <br> **Méthode de notification** : Une journalisation du traçage des activités spécifiques à la fonctionnalité de notification est effectuée. <br> **Bibliothèque commune** : une journalisation du traçage des activités de la bibliothèque opcommon est effectuée. <br> **Tout** : Une journalisation du traçage de toutes les activités susmentionnées est effectuée. <br> Le fichier journal du traçage avancé est défini dans la propriété **DetailedTracingFile**. <br> La propriété **DetailedTracingFile** doit être définie si l'option n'est pas Off. <br> Cette propriété apparaît dans l’**Éditeur avancé**.|
|DetailedTracingFile|Chaîne|Chemin du fichier journal généré automatiquement lorsque **DetailedTracingLevel** n'est pas **Off**. Cette propriété apparaît dans l’**Éditeur avancé**.|
|DiscardLargeRow|Booléen|La valeur par défaut est **False**. Ignorer les grandes lignes (supérieures à 64 ko) si **True**|
|ErrorTableName|Chaîne|Nom de la table d’erreurs. Utilise par défaut le nom de la table cible|
|ExtendedStringColumnsAllocation|Booléen|**Maximal Transfer Character Allocation Factor** est utilisé si **True**. <br> Cette valeur doit être définie sur **True** si la propriété **Export Width Table ID** de la base de données Teradata est définie sur **Maximal Defaults**. <br> La valeur par défaut est **False**.|
|FastLoad|Booléen|Utiliser le chargement rapide si **True**. La valeur par défaut est **false**. Vous pouvez aussi définir cette propriété dans l’[Éditeur de destination Teradata (page Gestionnaire de connexions)](#teradata-destination-editor-connection-manager-page).|
|MaxErrors|Entier|Nombre d’erreurs qui peuvent se produire avant l’arrêt du flux de données. La valeur par défaut est **0**, ce qui signifie qu’il n’y a aucune limite quant au nombre d’erreurs.<br> Si **Rediriger le flux** est sélectionné dans la page **Gestion des erreurs**. Avant que la limite du nombre d’erreurs soit atteinte, toutes les erreurs sont retournées dans la sortie d’erreur. Pour plus d’informations, consultez [Éditeur de destination Teradata (page Sortie d’erreur)](#teradata-destination-editor-error-output-page).|
|MaxSessions|Entier|Nombre maximal de sessions connectées. Cette valeur doit être supérieure à 1. La valeur par défaut est une session pour chaque AMP disponible.|
|MinSessions|Entier|Nombre minimal de sessions connectées. Cette valeur doit être supérieure à 1. La valeur par défaut est une session pour chaque AMP disponible.|
|Pack|Entier|Nombre d’instructions à empaqueter dans une requête à instructions multiples. La valeur par défaut est 20, la valeur maximale autorisée est 2400. Applicable uniquement à TPT Stream. Cette propriété apparaît dans l’**Éditeur avancé**.|
|PackMaximum|Booléen|Déterminer dynamiquement le facteur d’empaquetage maximal pour la tâche de flux actuelle si **True**. Applicable uniquement à TPT Stream. Cette propriété apparaît dans l’**Éditeur avancé**.|
|QueryBandSessInfo|Varchar|Expression de bande de requête définie par l'utilisateur et basée sur une session, permettant la gouvernance et le contrôle de la rétrofacturation. Cette propriété doit être au format chaîne de connexion. Cette propriété apparaît dans l’**Éditeur avancé**.|
|ReplicationOveride|Integer (énumération)|Options : <br> **Par défaut** : Aucune instruction SET SESSION OVERRIDE REPLICATION n’est envoyée à la base de données. Les paramètres par défaut de la base de données sont utilisés. <br> **Le** : Les contrôles normaux du service de réplication sont remplacés. <br> **Off** : Les contrôles normaux du service de réplication sont utilisés. <br> Cette propriété s'applique uniquement à TPT Stream. <br> Cette propriété apparaît dans l’**Éditeur avancé**.|
|Robust|Booléen|Une logique de redémarrage Robust est utilisée pour les opérations de récupération et de redémarrage si **True**. Cette propriété s'applique uniquement à **TPT Stream**. Cette propriété apparaît dans l’**Éditeur avancé**.|
|TableName|String|Nom de la table contenant les données en cours d’utilisation.|
|TenacityHours|Entier|Nombre d’heures pendant lesquelles le pilote TPT tente de se connecter lorsque le nombre maximal d’opérations de chargement/exportation est déjà en cours d’exécution. La valeur par défaut est 4 heures. Cette propriété apparaît dans l’**Éditeur avancé**|
|TenacitySleep|Entier|Nombre de minutes pendant lesquelles le pilote TPT s’interrompt avant de tenter de se connecter lorsque la limite est atteinte. La limite est définie par les propriétés **MaxSessions** et **TenacityHours**. La valeur par défaut est six minutes. Cette propriété apparaît dans l’**Éditeur avancé**|
|UnicodePassThrough|Booléen|Off (valeur par défaut) : Désactiver la transmission directe Unicode. <br>On : Activer la transmission directe Unicode.|

## <a name="configuring-the-teradata-destination"></a>Configuration de la destination Teradata

La destination Teradata peut être configurée par programmation ou par le biais du concepteur SSIS.

L’Éditeur de destination Teradata est illustré ci-dessous. Il contient la page Gestionnaire de connexions, la page Mappages et la page Sortie d’erreur.

Pour plus d’informations, consultez l’une des rubriques suivantes :

- [Éditeur de destination Teradata (page Gestionnaire de connexions)](#teradata-destination-editor-connection-manager-page)
- [Éditeur de destination Teradata (page Mappages)](#teradata-destination-editor-mappings-page)
- [Éditeur de destination Teradata (page Sortie d’erreur)](#teradata-destination-editor-error-output-page)

![éditeur de destination](media/teradata-destination.png)

La boîte de dialogue **Éditeur avancé** contient les propriétés qui peuvent être définies par programme.
Pour ouvrir la boîte de dialogue **Éditeur avancé** :

- Sur l’écran **Flux de données** de votre projet Integration Services, cliquez avec le bouton droit sur la destination Teradata, puis sélectionnez **Afficher l’éditeur avancé**.

Pour plus d’informations sur les propriétés que vous pouvez définir dans la boîte de dialogue Éditeur avancé, consultez [Propriétés personnalisées de la destination Teradata](#teradata-destination-custom-properties).

## <a name="teradata-destination-editor-connection-manager-page"></a>Éditeur de destination Teradata (page Gestionnaire de connexions)

Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de destination Teradata** pour sélectionner le gestionnaire de connexions Teradata de la destination. Cette page vous permet également de sélectionner une table ou une vue à partir de la base de données.

Pour ouvrir la page Gestionnaire de connexions de l’Éditeur de destination Teradata

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la destination Teradata.

- Sous l’onglet Flux de données, double-cliquez sur la destination Teradata.

- Dans l’Éditeur de destination Teradata, cliquez sur Gestionnaire de connexions.

### <a name="options"></a>Options

**Connection manager**

Sélectionnez un gestionnaire de connexions existant dans la liste ou cliquez sur **Nouveau** pour créer un gestionnaire de connexions Teradata.

**Nouveau**

Cliquez sur **Nouveau**. La boîte de dialogue **Éditeur du gestionnaire de connexions Teradata** s’ouvre et vous permet de créer un nouveau gestionnaire de connexions.

**Mode d'accès aux données**

Spécifiez la méthode de sélection des données dans la source. Ces fonctions sont répertoriées dans le tableau suivant :

|Option|Description|
|:-|:-|
|Nom de la table - TPT Stream|Mode incrémentiel utilisant l'opérateur TPT Stream. <br>**Nom de la table ou de la vue** : Sélectionnez une table ou une vue existante dans la liste. Cette liste affiche uniquement les 1 000 premières tables. Vous pouvez saisir le préfixe du nom de la table ou utiliser n'importe quelle partie du nom avec le caractère générique (*) pour afficher la ou les tables à utiliser.|
|Nom de la table – TPL Load|Mode de chargement rapide (Direct Path) utilisant l'opérateur TPT API Load (protocole Teradata FastLoad), qui exige que la table cible soit vide. <br>**Nom de la table ou de la vue** : Sélectionnez une table ou une vue existante dans la liste. Cette liste affiche uniquement les 1 000 premières tables. Vous pouvez saisir le préfixe du nom de la table ou utiliser n'importe quelle partie du nom avec le caractère générique (*) pour afficher la ou les tables à utiliser.|

**Chiffrement des données** Case à cocher permettant d’activer le chiffrement des données. Par défaut, cette option n'est pas sélectionnée.

**Toujours supprimer la table d'erreurs** Case à cocher permettant de supprimer les tables d’erreurs dans toutes les instances.

**Table d’erreurs** Nom de la table om sont consignées les erreurs.

**Nombre minimal de sessions** Nombre minimal de sessions connectées. La valeur par défaut est une session pour chaque AMP disponible. La valeur doit être supérieure à 1.

**Nombre maximal de sessions** Nombre maximal de sessions connectées. La valeur par défaut est une session pour chaque AMP disponible. La valeur doit être supérieure à 1.

**Nombre maximal d'erreurs** Nombre maximal d’erreurs pouvant être renvoyées avant que le flux de données ne soit arrêté ou redirigé. 

## <a name="teradata-destination-editor-mappings-page"></a>Éditeur de destination Teradata (page Mappages)

La page **Mappages** de la boîte de dialogue **Éditeur de destination Teradata** vous permet de mapper les colonnes d’entrée aux colonnes de destination.

Pour ouvrir la page Mappages de l’Éditeur de destination Teradata

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la destination Teradata.

- Sous l’onglet Flux de données, double-cliquez sur la destination Teradata.

- Dans l’Éditeur de destination Teradata, cliquez sur Mappages.

### <a name="options"></a>Options

**Colonnes d'entrée disponibles**

Liste des colonnes d'entrée disponibles. Par glisser-déplacer, mappez une colonne d'entrée à une colonne de destination disponible.

**Colonnes de destination disponibles**

Liste des colonnes de destination disponibles. Par glisser-déplacer, mappez une colonne de destination à une colonne d'entrée disponible.

**Colonne d'entrée**

Affichez les colonnes d’entrée que vous avez sélectionnées. Vous pouvez supprimer des mappages en sélectionnant **< ignorer >** de manière à exclure des colonnes de la sortie.

**Colonne de destination**

Affiche toutes les colonnes de destination disponibles, mappées et non mappées.

>[!NOTE]
>
>Les colonnes de types de données non pris en charge seront supprimées du mappage avec un avertissement.

## <a name="teradata-destination-editor-error-output-page"></a>Éditeur de destination Teradata (page Sortie d’erreur)
Utilisez la page Sortie d’erreur de la boîte de dialogue Éditeur de destination Teradata pour sélectionner les options de gestion des erreurs.

**Pour ouvrir la page Sortie d’erreur de l’Éditeur de destination Teradata**

- Dans SQL Server Data Tools, ouvrez le package SQL Server Integration Services (SSIS) qui contient la destination Teradata.

- Sous l’onglet Flux de données, double-cliquez sur la destination Teradata.

- Dans l’Éditeur de destination Teradata, cliquez sur Sortie d’erreur.

### <a name="options"></a>Options

**Comportement d’erreur**

Sélectionnez la façon dont la destination Teradata doit gérer les erreurs dans un flux : ignorer l'échec, rediriger la ligne ou faire échouer le composant.

**Rubriques connexes :** [Gestion des erreurs dans les données](./error-handling-in-data.md)

**Troncation**

Sélectionnez la façon dont la destination Teradata doit gérer la troncation dans un flux : ignorer l'échec, rediriger la ligne ou faire échouer le composant.

## <a name="next-steps"></a>Étapes suivantes

- Configurer le [gestionnaire de connexions Teradata](teradata-connection-manager.md)
- Configurer la [source Teradata](teradata-source.md)
- Configurer la [destination Teradata](teradata-destination.md)
- Si vous avez des questions, rendez-vous sur [Tech Community](https://aka.ms/AA6iwdw).
