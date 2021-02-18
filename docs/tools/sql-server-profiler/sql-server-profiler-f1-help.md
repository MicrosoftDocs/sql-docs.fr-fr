---
title: Boîtes de dialogue SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Explorez les commandes et les paramètres disponibles dans SQL Server Profiler pour une utilisation dans les traces, les modèles, les fichiers, les tables, les compteurs de performances et les relectures.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: reference
ms.assetid: e57b9160-4b78-4353-abb2-bfdbdf523d7a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/07/2017
ms.openlocfilehash: 06711f4975bb84163f58857759c5470f004c52e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338636"
---
# <a name="sql-server-profiler-dialog-boxes"></a>Boîtes de dialogue SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] de Microsoft est un outil qui capture les événements [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d’un serveur. Ces événements sont enregistrés dans un fichier de trace qui peut être analysé ou utilisé ultérieurement pour relire une série d'étapes spécifique lors d'une tentative de diagnostic d'un problème. Les commandes et paramètres suivants sont disponibles dans les boîtes de dialogue de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
## <a name="trace-properties"></a>Propriétés de la trace
### <a name="general-tab"></a>Onglet Général
Utilisez l’onglet **Général** de la boîte de dialogue **Propriétés de la trace** pour consulter ou spécifier les propriétés d’une trace.  

|Élément|Description
|---|---
|**Nom de la trace** |Spécifiez le nom de la trace.  
|**Nom du fournisseur de trace**|Affiche le nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui va être tracée. Ce champ est automatiquement rempli avec le nom du serveur que vous avez spécifié lorsque vous vous êtes connecté. Pour modifier le nom du fournisseur de la trace, cliquez sur **Annuler** pour fermer la boîte de dialogue et démarrez une nouvelle trace.  
|**Type de fournisseur de trace**|Affiche le type de serveur qui fournit la trace. Le champ **Type de fournisseur de trace** est rempli automatiquement par le fichier de définition de trace. Vous ne pouvez pas modifier ce champ.  
|**version**|Affiche la version du serveur qui fournit la trace. Le champ **Version** est rempli automatiquement par le fichier de définition de trace. Vous ne pouvez pas modifier ce champ.  
|**Utiliser le modèle**|Sélectionnez un modèle dans le répertoire des modèles. Le répertoire contient les modèles par défaut et tous les modèles définis par l'utilisateur créés pour le type de fournisseur de trace actuel.  
|**Enregistrer dans le fichier**|Cette option permet de capturer les données de trace dans un fichier .trc. L'enregistrement des données de trace vous permet de consulter et d'analyser ces données plus tard.  
|**Définir la taille de fichier maximale (Mo)**|Si vous décidez d'enregistrer les données de trace dans un fichier, vous devez indiquer la taille maximale de ce fichier. La valeur par défaut est 5 mégaoctets (Mo). La taille maximale est uniquement limitée par le système de fichiers (NTFS, FAT) dans lequel le fichier est enregistré.  
|**Enregistrer sous**|Une fois que vous avez choisi d'enregistrer les données dans un fichier, vous pouvez sélectionner cette icône pour modifier le nom du fichier.  
|**Activer la substitution de fichier**|Sélectionnez cette option pour autoriser la création de fichiers supplémentaires pour accueillir les données de trace lorsque la taille de fichier maximale est atteinte. Chaque nouveau nom de fichier correspond au nom du fichier .trc d'origine numéroté séquentiellement. Par exemple, une fois qu’il a atteint la taille de fichier maximale, **NouvelleTrace.trc** se ferme et un nouveau fichier, **NouvelleTrace_1.trc**, s’ouvre, suivi par **NouvelleTrace_2.trc**, et ainsi de suite. La substitution de fichier est activée par défaut lorsque vous enregistrez une trace dans un fichier.  
|**Le serveur traite les données de trace**|Cette option indique que le serveur exécutant la trace doit traiter les données de trace. L'utilisation de cette option réduit la charge de travail générée par le traçage. Lorsque cette option est activée, aucun événement n'est ignoré même en cas de charge importante. Si cette option n'est pas activée, c'est le Générateur de profils SQL Server qui traite les événements et il se peut que certains ne soient pas tracés en cas de conditions difficiles.  
|**Enregistrer dans la table**|Cette option permet de capturer les données de trace dans une table de base de données. L'enregistrement des données de trace vous permet de consulter et d'analyser ces données plus tard. Ce choix peut néanmoins provoquer un surcroît de charge important sur le serveur où les données de trace sont enregistrées. Si possible, n'enregistrez pas la table de trace sur le serveur qui est tracé.  
|**Table de destination**|Une fois que vous avez choisi d'enregistrer les données de trace dans une table de base de données, vous pouvez sélectionner cette icône pour modifier le nom de la table.  
| **Définir le nombre de lignes maximal (en milliers)**|Spécifiez le nombre maximal de lignes pour l'enregistrement des données. La valeur par défaut est 1000 lignes. 
|**Activer l'heure d'arrêt de la trace**|Définissez la date et l'heure auxquelles la trace doit s'arrêter et se fermer. 

### <a name="events-selection-tab"></a>Onglet Sélection des événements
Utilisez l'onglet **Sélection des événements** de la boîte de dialogue **Propriétés de la trace** pour afficher ou spécifier les événements à tracer et les colonnes de données.  

|Élément|Description
|---|---
|Colonne **Events**|Spécifiez les événements à tracer en activant ou en désactivant les cases à cocher dans la colonne des événements. Les **événements** sont organisés par catégorie. Les classes d'événements spécifiées dans le modèle sont automatiquement sélectionnées. Pour plus d'informations, consultez [Référence de classe d'événements SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Colonnes de données|Précisez les colonnes de données à tracer en activant la case à cocher correspondant à l'événement et à la colonne de données requis. Toutes les colonnes d'événements pertinentes sont cochées par défaut pour chaque événement inclus dans la trace.  
|Filtres|Spécifiez les filtres en cliquant sur les en-têtes de colonne de données et entrant les critères du filtre. Les colonnes de données filtrées sont indiquées par une icône de filtre à gauche de l'étiquette de colonne dans la boîte de dialogue **Modifier le filtre** . Pour plus d’informations, consultez [SQL Server Profiler - Modifier le filtre]().  
|**Afficher tous les événements**|Affiche tous les événements disponibles. Par défaut, seules les lignes sélectionnées dans la grille **Sélection des événements** sont affichées. Désactivez cette case à cocher pour masquer tous les événements non sélectionnés dans la grille **Sélection des événements** .  
|**Afficher toutes les colonnes**|Affiche toutes les colonnes de données disponibles. Par défaut, seules les colonnes de données sélectionnées sont affichées. Désactivez cette case à cocher pour masquer toutes les colonnes de données non sélectionnées dans la grille **Sélection des événements** .  
|**Filtres de colonnes**|Affiche la boîte de dialogue **Modifier le filtre** . Vous pouvez utiliser celle-ci pour modifier les filtres de colonnes de données.  
|**Organiser les colonnes**|Modifie l'ordre des colonnes dans la trace et regroupe les résultats suivant une ou plusieurs colonnes.  

## <a name="trace-template-properties"></a>Propriétés du modèle de trace 
### <a name="new-general-tab"></a>Nouveau (onglet Général)
Utilisez l'onglet **Général** de la boîte de dialogue **Propriétés du modèle de trace** pour créer de nouveaux modèles de trace à l'aide des options suivantes. Pour accéder à cette boîte de dialogue, dans le menu **Fichier** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], pointez sur **Modèles**, puis cliquez sur **Nouveau**.

|Élément|Description
|---|---
|**Sélectionnez le type de serveur**|Indiquez le type de serveur sur lequel ce modèle va être utilisé.  
|**Nouveau nom de modèle**|Tapez un nom descriptif pour le modèle.  
|**Baser le nouveau modèle sur un modèle existant**|Choisissez l'un des modèles de la liste pour servir de base à ce modèle. Tous les événements sélectionnés, colonnes de données et filtres correspondent initialement à ceux du modèle existant et peuvent ensuite être modifiés en fonction des besoins.  
|**Utiliser comme modèle par défaut pour le type de serveur sélectionné**|Utilisez ce modèle par défaut pour les traces créées pour ce type de serveur.  

### <a name="edit-general-tab"></a>Modifier (onglet Général)
 Utilisez l'onglet **Général** de la boîte de dialogue **Propriétés du modèle de trace** pour consulter ou modifier les modèles de trace existants en utilisant les options suivantes. Pour accéder à cette boîte de dialogue, dans le menu [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] **de** , pointez sur **Modèles**, puis cliquez sur **Modifier le modèle**.  

|Élément|Description
|---|---
|**Sélectionnez le type de serveur**|Indiquez le type de serveur sur lequel ce modèle va être utilisé.  
|**Sélectionnez le nom du modèle**|Sélectionnez le modèle que vous souhaitez modifier.  
|**Utiliser comme modèle par défaut pour le type de serveur sélectionné**|Utilisez ce modèle par défaut pour les traces créées pour ce type de serveur.  

### <a name="events-selection-tab"></a>Onglet Sélection des événements
L'onglet **Sélection des événements** de la boîte de dialogue **Propriétés du modèle de trace** vous permet d'afficher, de modifier ou de spécifier les classes d'événements et les colonnes de données à inclure dans un modèle de trace [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  

|Élément|Description
|---|---
|Colonne **Events**|Permet de spécifier les événements à tracer en activant ou en désactivant la case à cocher dans la colonne d'événement. Les événements sont organisés par catégorie d'événement. Si vous avez sélectionné **Baser le nouveau modèle sur un modèle existant** sous l'onglet **Général** , des événements sont sélectionnés automatiquement en fonction du modèle spécifié. Pour plus d'informations sur les classes d'événements, consultez [Référence de classe d'événements SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Colonnes de données|Permet de spécifier les colonnes de données à tracer en activant la case à cocher qui correspond à la colonne d'événement et de données dont vous avez besoin. Toutes les colonnes d'événements appropriées sont sélectionnées par défaut pour chaque événement inclus dans la trace, si la case à cocher correspondant à l'événement est activée. Si vous avez activé **Baser le nouveau modèle sur un modèle existant** sous l'onglet **Général** , les colonnes de données et les filtres sont sélectionnés automatiquement en fonction du modèle spécifié.  
|Filtres|Spécifiez les filtres en cliquant sur les en-têtes de colonne de données et entrant les critères du filtre. Les colonnes de données filtrées sont indiquées par une icône de filtre à gauche de l'étiquette de colonne dans la boîte de dialogue **Modifier le filtre** .  
|**Afficher tous les événements**|Affiche tous les événements disponibles. Cette option est activée par défaut si vous créez un nouveau modèle qui n'est pas basé sur un modèle existant. Désactivez l'option pour masquer tous les événements non sélectionnés dans la grille **Sélection des événements** .  
|**Afficher toutes les colonnes**|Affiche toutes les colonnes de données disponibles. Cette option est activée par défaut si vous créez un nouveau modèle qui n'est pas basé sur un modèle existant. Désactivez l'option pour masquer toutes les colonnes de données non sélectionnées dans la grille **Sélection des événements** .  
|**Filtres de colonnes**|Lance la boîte de dialogue **Modifier le filtre**, qui affiche une icône de filtre à gauche de l’étiquette d’une colonne de données. La boîte de dialogue **Modifier le filtre** vous permet de modifier les filtres des colonnes de données.  
|**Organiser les colonnes**|Modifie l'ordre des colonnes dans la trace et regroupe les résultats suivant une ou plusieurs colonnes. 

## <a name="trace-file-properties"></a>Propriétés du fichier de trace 
### <a name="general-tab"></a>Onglet Général
Utilisez l'onglet **Général** de la boîte de dialogue **Propriétés du fichier de trace** pour consulter les propriétés d'un fichier de trace.  
Pour afficher cette fenêtre, ouvrez un fichier de trace. Ensuite, dans le menu **Fichier** , cliquez sur **Propriétés**.  

|Élément|Description
|---|---
|**Nom de fichier**|Le chemin d'accès et le nom du fichier de trace ouvert.  
|**Nom du fournisseur de trace**|Affiche le nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a été tracée.  
|**Type de fournisseur de trace**|Affiche le type de serveur qui a fourni la trace.  
|**version**|Affiche la version du serveur qui a fourni la trace.  
|**Taille du fichier (Ko)**|La taille du fichier de trace en kilo-octets (Ko).  
|**Créé le**|La date et l'heure de création du fichier de trace.  
|**Modifié le** |La date et l'heure de modification du fichier de trace.  

### <a name="events-selection-tab"></a>Onglet Sélection des événements
Utilisez l'onglet **Sélection des événements** de la boîte de dialogue **Propriétés du modèle de trace** pour consulter les propriétés des colonnes de la trace ou pour supprimer des colonnes de données de la trace.  
Pour afficher cette fenêtre, ouvrez un fichier de trace. Ensuite, dans le menu **Fichier** , cliquez sur **Propriétés**, puis cliquez sur l'onglet **Sélection des événements** .  

|Élément|Description
|---|---
|Colonne **Events**|Affiche les événements tracés organisés par catégorie. Initialement, tous les événements de la trace sont sélectionnés. Sélectionnez les événements requis en activant ou en désactivant les cases à cocher correspondant à une colonne de données d'un événement. Si une case d'événement est cochée, toutes les colonnes de données de cet événement sont sélectionnées. Si la colonne de données d'un événement est cochée, l'événement l'est aussi, de même que toutes les autres colonnes requises. Lors de l'affichage d'une table ou d'un fichier de trace, la désactivation des cases à cocher de certains événements ou de colonnes de données réduit la quantité de données visibles dans la fenêtre de trace, ce qui facilite les analyses. Pour réduire la quantité d'informations visibles, vous pouvez également modifier les filtres de colonne. Pour plus d'informations sur les classes d'événements, consultez [Référence de classe d'événements SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Colonnes de données|Colonnes de données tracées. Toutes les colonnes de données pertinentes de la trace sont cochées par défaut pour chaque événement inclus dans la trace.  
|Filtres|Spécifiez les filtres en cliquant sur les en-têtes de colonne de données et entrant les critères du filtre. Les colonnes de données filtrées sont indiquées par une icône de filtre à gauche de l'étiquette de colonne dans la boîte de dialogue **Modifier le filtre** .  
|**Afficher tous les événements**|Affiche tous les événements disponibles. Par défaut, seules les lignes sélectionnées dans la grille **Sélection des événements** sont affichées. Désactivez cette case à cocher pour masquer tous les événements non sélectionnés dans la grille **Sélection des événements** . Si la case **Afficher tous les événements** est cochée et que vous visualisez un fichier ou une table de trace, tous les événements enregistrés dans la trace s'affichent dans la fenêtre de trace.  
|**Afficher toutes les colonnes**|Affiche toutes les colonnes de données disponibles. Par défaut, seules les colonnes de données sélectionnées sont affichées. Désactivez cette case à cocher pour masquer toutes les colonnes de données non sélectionnées dans la grille **Sélection des événements** .  
|**Filtres de colonnes**|Ouvre la boîte de dialogue **Modifier le filtre** , dans laquelle une icône de filtre apparaît à gauche de l’étiquette des colonnes de données filtrées. La boîte de dialogue **Modifier le filtre** vous permet de modifier les filtres des colonnes de données.  
|**Organiser les colonnes**|Après avoir sélectionné les colonnes **Événements** et de données à inclure dans la trace, cliquez sur **Organiser les colonnes** pour forcer la réorganisation dans la grille des colonnes de la fenêtre des résultats de la trace.  

## <a name="trace-table-properties"></a>Propriétés de la table de trace
### <a name="events-selection-tab"></a>Onglet Sélection des événements
Utilisez l'onglet **Sélection des événements** de la boîte de dialogue **Propriétés de la table de trace** pour afficher les propriétés des colonnes de données et d'événements dans la trace ou pour supprimer de tels événements ou colonnes.  
Pour afficher cette fenêtre, utilisez le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour ouvrir un fichier de trace. Dans le menu **Fichier** , cliquez sur **Propriétés** puis sur l'onglet **Sélection des événements** .  

|Élément|Description
|---|---
|Colonne **Events**|Affiche les événements tracés organisés par catégorie. Sélectionnez les événements requis en activant ou en désactivant les cases à cocher correspondant à une colonne de données d'un événement. Si une case d'événement est cochée, toutes les colonnes de données de cet événement sont sélectionnées. Si la colonne de données d'un événement est cochée, l'événement l'est aussi, de même que toutes les autres colonnes requises. Lors de l'affichage d'une table ou d'un fichier de trace, la désactivation des cases à cocher de certains événements ou de colonnes de données réduit la quantité de données visibles dans la fenêtre de trace, ce qui facilite les analyses. Pour réduire la quantité d'informations visibles, vous pouvez également modifier les filtres de colonne. Pour plus d'informations sur les classes d'événements, consultez [Référence de classe d'événements SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Autres colonnes de données|Colonnes de données tracées. Toutes les colonnes de données pertinentes de la trace sont cochées par défaut pour chaque événement inclus dans la trace.  
|Filtres|Spécifiez les filtres en cliquant sur les en-têtes de colonne de données et entrant les critères du filtre. Les colonnes de données filtrées sont indiquées par une icône de filtre à gauche de l'étiquette de colonne dans la boîte de dialogue **Modifier le filtre** .  
|**Afficher tous les événements**|Affiche tous les événements disponibles. Par défaut, seules les lignes sélectionnées dans la grille **Sélection des événements** sont affichées. Désactivez cette case à cocher pour masquer tous les événements non sélectionnés dans la grille **Sélection des événements** . Si la case **Afficher tous les événements** est cochée et que vous visualisez un fichier ou une table de trace, tous les événements enregistrés dans la trace s'affichent dans la fenêtre de trace.  
|**Afficher toutes les colonnes**|Affiche toutes les colonnes de données disponibles. Par défaut, seules les colonnes de données sélectionnées sont affichées. Désactivez cette case à cocher pour masquer toutes les colonnes de données non sélectionnées dans la grille **Sélection des événements** .  
|**Filtres de colonnes**|Affiche la boîte de dialogue **Modifier le filtre**, avec une icône de filtre à gauche de l’étiquette de colonne. Vous pouvez utiliser celle-ci pour modifier les filtres de colonnes de données.  
|**Organiser les colonnes** |Après avoir sélectionné les colonnes **Événements** et de données à inclure dans la trace, cliquez sur **Organiser les colonnes** pour forcer la réorganisation dans la grille des colonnes de la fenêtre des résultats de la trace.  

## <a name="performance-counters-limit"></a>Limite des compteurs de performances
Utilisez la boîte de dialogue Limite des compteurs de performances pour limiter les informations provenant d'un fichier journal de performances du Moniteur système lors de sa corrélation avec une trace du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . Cette boîte de dialogue vous permet de sélectionner les compteurs qui doivent être affichés et utilisés pour la corrélation.  
La boîte de dialogue **Limite des compteurs de performances** est remplie avec les objets et compteurs de performances contenus dans le fichier journal de performances.  
### <a name="to-select-performance-objects-and-counters-to-correlate-with-a-trace"></a>Pour sélectionner les objets et compteurs de performances à corréler avec une trace  
1.  Développez un objet de performance pour déterminer les compteurs qui sont inclus dans le fichier journal de performances.  
2.  Activez les compteurs que vous souhaitez corréler avec le fichier de trace du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  

Si vous souhaitez sélectionner tous les compteurs pour un objet de performance, activez la case à cocher adjacente à celui-ci. L'activation du nœud de premier niveau, qui indique l'ordinateur, entraîne la sélection de tous les objets et compteurs de performances contenus dans le fichier journal de performances. 
## <a name="toolsoptions-general-options-page"></a>Outils/options (page Options générales)
Utilisez la boîte de dialogue **Options générales** pour afficher ou spécifier les options ci-après.  
### <a name="display-options"></a>Options d’affichage  

|Élément|Description
|---|---
|**Nom de police**|Affiche le nom de la police utilisée dans la grille des résultats de trace pendant les traces.  
|**Taille de la police**|Affiche la taille de la police utilisée dans la grille des résultats de trace pendant les traces.  
|**Choisir la police**|Ouvre une boîte de dialogue permettant de modifier les paramètres de police.  
|**Utiliser des paramètres régionaux pour afficher les valeurs de date et d'heure**|Affiche les valeurs de date et d'heure selon les paramètres régionaux configurés sur votre ordinateur. Si vous ne sélectionnez pas cette option, les valeurs de date et d'heure sont affichées dans le format fixe de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], qui comprend les millisecondes. Notez qu’en cochant et en décochant cette case, vous modifiez le format d’affichage des colonnes de date/heure, comme **StartTime** et **EndTime**. Toutefois, vous ne modifiez pas les paramètres de valeur **DateTime** dans les événements de langage ou les appels de procédure distante (RPC).  
|**Afficher les valeurs dans la colonne Durée en microsecondes**|Affiche les valeurs en microsecondes dans la colonne de données **Durée** des traces. Par défaut, la colonne **Durée** affiche les valeurs en millisecondes.  

### <a name="tracing-options"></a>Options de suivi  

|Élément|Description
|---|---
|**Démarrer le suivi juste après avoir établi la connexion**|Commence une trace utilisant le modèle par défaut dès qu'une connexion est établie.  
|**Mettre à jour la définition de la trace lors de la modification de la version du fournisseur**|Applique la définition de trace la plus récente à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque le fournisseur est mis à jour. Cette option n'est pas activée par défaut. Elle force le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] à interroger le serveur sur la définition de trace et à recréer le fichier sur le disque le cas échéant.  

### <a name="file-rollover-options"></a>Options de remplacement des fichiers  

|Élément|Description
|---|---
|**Charger tous les fichiers de substitution en séquence sans invite**|Charge automatiquement les fichiers de substitution lorsqu'un fichier de trace est ouvert. Si plusieurs fichiers ont été créés pendant le suivi, la sélection de cette option charge automatiquement tous les fichiers de substitution.  
|**Demander confirmation avant le chargement des fichiers de substitution**|Le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] vous demande confirmation avant l'ajout d'un fichier de substitution lorsqu'un fichier de trace est ouvert.  
|**Ne jamais charger les fichiers de substitution suivants**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ne charge jamais les fichiers de substitution suivants quand un fichier de trace est ouvert.  

### <a name="replay-options"></a>Options de relecture  

|Élément|Description
|---|---
|**Nombre par défaut de threads de relecture**|Spécifiez le nombre de threads de relecture à utiliser simultanément. Un nombre élevé consomme davantage de ressources pendant la relecture, mais améliore la simultanéité de relecture.  
|**Délai d'attente du moniteur d'intégrité par défaut (sec)**|Spécifiez le délai d'attente de la relecture, en secondes. La valeur par défaut est 3 600 secondes (1 heure). Ce paramètre affecte la durée d'exécution d'un thread autorisée avant son interruption par le moniteur d'intégrité.  
|**Intervalle d'interrogation du moniteur d'intégrité par défaut (sec)**|Spécifiez, en secondes, l'intervalle d'interrogation du moniteur d'intégrité pendant la relecture. La valeur par défaut est 60 secondes. Cette valeur permet à l'utilisateur de configurer la fréquence à laquelle le moniteur d'intégrité interroge les candidats à l'arrêt.

## <a name="source-table-database-engine-tuning-advisor-select-workload-table"></a>Table source (Assistant Paramétrage du moteur de base de données - Sélectionner une table de charge de travail)
Microsoft SQL Server Profiler et l’Assistant Paramétrage utilisent cette boîte de dialogue pour sélectionner des tables.  
- Dans Profiler, utilisez la boîte de dialogue **Table source** pour spécifier une table source pour une table de trace. Ceci est une table à partir de laquelle une trace est chargée et dont le contenu est affiché ou utilisé pour relire la trace.  
- Dans l’Assistant Paramétrage, utilisez la boîte de dialogue **Sélectionner une table de charge de travail** pour sélectionner une table de base de données qui contient les informations de trace de Profiler à utiliser comme charge de travail de paramétrage, ou pour afficher l’aperçu du contenu de la table avant de commencer l’analyse du paramétrage.  

|Élément|Description
|---|---
|**SQL Server**|Spécifie l’instance de SQL Server actuellement connectée. Ce champ est rempli automatiquement et ne peut pas être mis à jour.  
|**Sauvegarde de la base de données**|Spécifie la base de données dans laquelle se trouve la table de trace.  
|**Propriétaire**|Spécifie le propriétaire de la table de trace. Ce champ est rempli automatiquement comme **dbo**.  
|**Table**|Spécifie le nom de la table de trace à partir de laquelle la trace doit être lue.  

## <a name="destination-table"></a>Table de destination
La boîte de dialogue **Table de destination** vous permet de spécifier la table dans laquelle vous souhaitez stocker la trace.  

|Élément|Description
|---|---
|**SQL Server**|Spécifie l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actuellement connectée. Ce champ est rempli automatiquement et ne peut pas être mis à jour. Pour modifier le serveur, cliquez sur **Annuler** et connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle vous voulez stocker la table de trace.  
|**Sauvegarde de la base de données**|Spécifiez la base de données dans laquelle vous voulez stocker la table de trace.  
|**Propriétaire**|Spécifie le propriétaire de la table de trace. Ce champ est rempli automatiquement comme **dbo**.  
|**Table**|Spécifiez le nom de la table dans laquelle vous voulez stocker la trace.  

## <a name="replay-configuration"></a>Configuration de la relecture
### <a name="basic-replay-options"></a>Options de relecture de base
Dans la boîte de dialogue **Configuration de la relecture**, utilisez la page **Options de relecture de base** pour spécifier la manière de relire un fichier ou une table de trace.  
Pour afficher cette fenêtre, utilisez le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour ouvrir un fichier de trace ou une table qui contient les événements appropriés pour la relecture. Pour plus d’informations, consultez [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md). Lorsque le fichier ou la table de trace est ouvert, dans le menu **Relire** , cliquez sur **Début**, puis connectez-vous à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle vous voulez relire la trace.  

|Élément|Description
|---|---
|**Serveur de relecture**|Affiche l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle se connecter pour la relecture.  
|**Modifier...**|Affiche la boîte de dialogue **Se connecter au serveur** pour se connecter à un autre serveur.  
|**Enregistrer dans le fichier** |Permet d'enregistrer les résultats de relecture dans un fichier. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche la boîte de dialogue de fichier standard, qui vous permet de spécifier l’emplacement où enregistrer le fichier.  
|**Enregistrer dans la table**|Permet d'enregistrer les résultats de relecture dans une table. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche la boîte de dialogue de sélection de table, qui vous permet de spécifier l’emplacement où enregistrer la table.  
|**Nombre de threads de relecture**|Spécifiez le nombre de threads de relecture à utiliser simultanément. Plus ce nombre est important, plus la relecture consomme de ressources, mais plus elle est rapide.  
|**Relire les événements selon leur ordre de suivi**|Permet de relire les événements de manière séquentielle. Utilisez cette option pour relire une trace pour un débogage.  
|**Relire les événements en utilisant plusieurs threads** |Permet de relire les événements de manière simultanée. Cette option est plus rapide que la relecture séquentielle des événements, mais elle désactive le débogage. Les événements sont ordonnés à l'aide de leurs identificateurs de processus système (SPID).  
|**Afficher les résultats de relecture**|Permet d'afficher les résultats de relecture dans [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. 

### <a name="advanced-replay-options"></a>Options de relecture avancées
Dans la boîte de dialogue **Configuration de la relecture** , utilisez l’onglet **Options de relecture avancées** pour spécifier le mode de relecture d’un fichier de trace.  
Pour afficher cette fenêtre, utilisez le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour ouvrir un fichier de trace ou une table qui contient les événements appropriés pour la relecture. Pour plus d’informations, consultez [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md). Après avoir ouvert le fichier de trace ou la table, dans le menu **Relecture** , cliquez sur **Démarrer**, connectez-vous à l’instance de SQL Server sur laquelle vous voulez relire la trace, puis cliquez sur l’onglet **Options de relecture avancées** .  

|Élément|Description
|---|---
|**Relire les SPID système**|Spécifie si le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] relit les SPID système.  
|**Relire un SPID uniquement**|Ne relit que l'activité du fichier de trace source qui est relative au SPID sélectionné.  
|**SPID à relire**|Spécifiez le SPID à relire.  
|**Limiter la relecture par date et heure**|Activez cette option pour ne relire qu'une partie du fichier de trace source.  
|**Heure de début**|Date et heure auxquelles la relecture doit commencer dans le fichier de trace source.  
|**Heure de fin**|Date et heure auxquelles la relecture doit se terminer dans le fichier de trace source.  
|**Délai d'attente du moniteur d'intégrité (sec.)**|Spécifiez le délai d'attente de la relecture, en secondes. La valeur par défaut est 3 600 secondes (1 heure). Ce paramètre affecte la durée d'exécution d'un processus autorisée avant son interruption par le moniteur d'intégrité.  
|**Intervalle d'interrogation du moniteur d'intégrité par défaut (sec)**|Spécifiez, en secondes, l'intervalle d'interrogation du moniteur d'intégrité pendant la relecture. La valeur par défaut est 60 secondes. Cette valeur permet à l'utilisateur de configurer la fréquence à laquelle le moniteur d'intégrité interroge les candidats à l'arrêt.  
|**Activer le moniteur de processus bloqués de SQL Server**|Active un processus qui recherche les processus bloqués ou les processus de blocage.  
|**Délai d'attente du moniteur de processus bloqués (s)**|Configure la fréquence à laquelle le moniteur de processus recherche les processus bloqués ou les processus de blocage.  

## <a name="find-dialog-box"></a>Rechercher (boîte de dialogue)
Utilisez la boîte de dialogue **Rechercher** pour rechercher une trace de caractères ou de mots spécifiques. Pour annuler une recherche en cours, appuyez sur ÉCHAP.  
 Dans le menu [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]Edition **, cliquez sur** Rechercher **pour ouvrir cette boîte de dialogue dans**.  

|Élément|Description
|---|---
|**Rechercher**|Entrez le texte à rechercher. La recherche retourne toute chaîne contenant la chaîne spécifiée. Par exemple, si vous recherchez « Completed », la chaîne « SQL:BatchCompleted » est retournée. Les caractères génériques (*, ?, etc.) ne sont pas pris en charge.  
|**Rechercher dans la colonne**|Cliquez sur la colonne de données sur laquelle la recherche doit porter ou cliquez sur **\<All columns>** pour effectuer une recherche sur l'ensemble des colonnes de données de la trace.  
|**Respecter la casse**|Recherche du texte présentant la même casse que celui spécifié dans la zone **Rechercher** . Désactivez cette case à cocher pour rechercher dans la trace des correspondances avec les caractères indiqués mais sans distinction entre les majuscules et les minuscules.  
|**Mot entier**|Restreint la recherche à des mots entiers. Décochez la case **Mot entier** pour rechercher des caractères spécifiques au sein d’un mot.  
|**Suivant**|Recherche la correspondance suivante avec les caractères spécifiés dans la zone **Rechercher** .  
|**Précédent**|Recherche dans la trace la correspondance précédente avec les caractères spécifiés dans la zone **Rechercher** .  

 ## <a name="organize-columns"></a>Organiser les colonnes
Utilisez la boîte de dialogue **Organiser les colonnes** pour sélectionner des colonnes de données en vue de regrouper ou d'agréger des événements affichés dans une trace et faciliter ainsi la consultation et l'analyse des tables ou des fichiers de trace volumineux.  
- L'agrégation déplace et réduit tous les événements de la trace sous leur type de classe d'événements respectif. Un signe plus ( **+** ) apparaît à gauche du nom de la classe d’événements. Si vous cliquez sur le signe plus, la classe d'événements se développe et affiche tous les événements de ce type.  
- Le regroupement regroupe toutes les classes d'événements d'un type spécifique dans l'affichage de la fenêtre de trace. Les événements ne sont toutefois pas réduits sous le type de classe d'événements.  

Lorsque vous regroupez ou agrégez des événements dans l'affichage d'une fenêtre de trace, les colonnes sélectionnées pour le regroupement ou l'agrégation restent fixes dans la fenêtre d'affichage, mais vous pouvez faire défiler l'affichage vers la droite ou la gauche pour visualiser toutes les autres colonnes de données.  
Pour accéder à cette boîte de dialogue, ouvrez une table ou un fichier de trace existant, puis cliquez sur **Propriétés** dans le menu **File** de[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Dans la boîte de dialogue **Propriétés de la trace** , cliquez sur l'onglet **Sélection des événements** , puis sur **Organiser les colonnes**. Vous pouvez également cliquer sur **Organiser les colonnes** sous l'onglet **Sélection des événements** lorsque vous créez une nouvelle trace.  
Déplacez des noms de colonne de données sous **Groupes** pour regrouper ou agréger des classes d’événements dans la fenêtre de trace.
- Pour agréger des événements, déplacez une colonne de données vers **Groupes**. Tous les événements d'un type spécifique sont alors réduits sous le nom du type de la classe d'événements dans l'affichage de la fenêtre de trace. Un signe plus ( **+** ) apparaît à gauche du nom de la classe d’événements. Cliquez sur ce signe plus pour développer le type de la classe d'événements et afficher tous les événements. Vous pouvez activer et désactiver l'agrégation et le regroupement en cliquant sur **Vue agrégée** ou sur **Vue groupée** dans le menu **Affichage** .
- Pour regrouper des événements, déplacez plusieurs colonnes de données vers **Groupes**. Tous les événements d'un type spécifique sont alors regroupés dans l'affichage de la fenêtre de trace, mais les événements ne sont pas réduits sous chaque nom de type de classe d'événements. Vous pouvez basculer entre une vue groupée et une vue non groupée en cliquant sur **Vue groupée** dans le menu Affichage. Lorsque plusieurs colonnes de données sont déplacées vers **Groupes**, l'option de basculement vers **Vue agrégée** n'est pas disponible.

|Élément|Description
|---|---
|**Colonnes**|Répertorie les colonnes de données qui peuvent être déplacées vers **Groupes**. Cliquez sur le signe plus ( **+** ) à gauche de l’option **Colonnes** pour développer la liste.  
|**Haut**|Lorsque vous avez sélectionné une colonne de données, cliquez sur **Haut** pour déplacer des colonnes de données vers le haut dans **Groupes**. Vous pouvez également cliquer sur **Haut** pour réorganiser l'affichage des colonnes dans l'affichage de la fenêtre de trace.  
|**Descendre**|Lorsque vous avez sélectionné une colonne de données, cliquez sur **Bas** pour déplacer des colonnes de données vers le bas dans **Groupes**. Vous pouvez également cliquer sur **Bas** pour réorganiser l'affichage des colonnes dans l'affichage de la fenêtre de trace.  

## <a name="edit-filter"></a>Modifier le filtre
Utilisez la boîte de dialogue **Modifier le filtre** pour créer et modifier des filtres de colonne de données dans une trace. Cliquez sur le nom d'une colonne de données dans la liste pour afficher dans le volet adjacent les critères de filtre disponibles pour cette colonne. Entrez les critères de filtre et cliquez sur **OK** pour les appliquer à la colonne de données sélectionnée. Si une icône de filtre apparaît à gauche du nom de la colonne de données dans la liste, cela signifie qu'un filtre est déjà configuré pour cette colonne.  
 >[!NOTE]
 >Pour les colonnes de données de type chaîne, les critères de filtre s'afficheront en tant que valeur de chaîne LIKE ou NOT LIKE.  

## <a name="select-template-name"></a>Sélectionnez le nom du modèle
Utilisez la boîte de dialogue **Sélectionner le nom du modèle** pour sélectionner un modèle de trace [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] existant à exporter vers un fichier du système d'exploitation. Vous pouvez également utiliser cette boîte de dialogue pour sélectionner ou entrer un autre nom afin d'enregistrer un modèle de trace lorsqu'un modèle de trace existant est modifié. Pour accéder à cette boîte de dialogue lorsque vous exportez un modèle, dans le menu **Fichier** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], pointez sur **Modèles**, puis cliquez sur **Exporter le modèle**. Pour accéder à cette boîte de dialogue lorsque vous changez le nom d'un modèle, dans le menu **Fichier** , pointez sur **Modèles**, sur **Modifier le modèle**, puis cliquez sur **Enregistrer sous**.  

|Élément|Description
|---|---
|**Type de serveur**|Sélectionnez le type de serveur dans lequel vous souhaitez choisir un modèle. Cette option est uniquement disponible lorsque vous exportez un modèle.  
|**Nom du modèle**|Tapez un nouveau nom de modèle ou sélectionnez un nom de modèle dans la liste. Si vous exportez un modèle, vous pouvez uniquement sélectionner un nom de modèle dans la liste. 

## <a name="see-also"></a>Voir aussi 
[SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
[Analyse des performances et surveillance de l'activité du serveur](../../relational-databases/performance/server-performance-and-activity-monitoring.md)  
  
