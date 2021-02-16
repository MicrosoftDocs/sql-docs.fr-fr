---
title: Importer à partir d’Excel ou exporter vers Excel avec SSIS | Microsoft Docs
description: Découvrez comment importer ou exporter des données Excel avec SQL Server Integration Services (SSIS), ainsi que les prérequis, les problèmes connus et les limitations.
ms.date: 06/29/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e497e0f2781aedc2adccc4263cc3b37e756a86da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345700"
---
# <a name="import-data-from-excel-or-export-data-to-excel-with-sql-server-integration-services-ssis"></a>Importer des données à partir d’Excel ou exporter des données vers Excel avec SQL Server Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Cet article décrit les informations de connexion à fournir et les paramètres à configurer pour importer des données à partir d’Excel ou exporter des données vers Excel avec SQL Server Integration Services (SSIS).

Les sections suivantes donnent les informations nécessaires pour utiliser efficacement Excel avec SSIS, et comprendre et résoudre des problèmes courants :

1.  Les [outils](#tools) disponibles.

2.  Les [fichiers](#files-you-need) nécessaires.

3.  Les informations de connexion à fournir et les paramètres à configurer pour charger des données vers ou depuis Excel avec SSIS.
    -   [Spécifiez Excel](#specify-excel) comme source de données.
    -   Indiquez [le nom et le chemin du fichier Excel](#excel-file).
    -   Sélectionnez la [version d’Excel](#excel-version).
    -   Spécifiez si [la première ligne contient des noms de colonnes](#first-row).
    -   Indiquez la [feuille de calcul ou plage qui contient les données](#sheets-ranges).

4.  Les problèmes connus et les limitations.
    -   Problèmes avec les [types de données](#issues-types).
    -   Problèmes avec [l’importation](#issues-importing).
    -   Problèmes avec [l’exportation](#issues-exporting).

## <a name="tools-you-can-use"></a><a name="tools"></a> Outils disponibles

Vous pouvez utiliser les outils suivants pour importer des données à partir d’Excel ou exporter des données vers Excel avec SSIS :

-   **SQL Server Integration Services (SSIS)** . Créez un package SSIS qui utilise la source Excel ou la destination Excel avec le Gestionnaire de connexions Excel. (Cet article n’explique pas comment créer des packages SSIS.)

-   **L’Assistant Importation/exportation SQL Server**, basé sur SSIS. Pour plus d’informations, voir [Importer et exporter des données avec l’Assistant Importation/exportation SQL Server](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md) et [Se connecter à une source de données Excel (Assistant Importation/exportation SQL Server)](import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md).

## <a name="get-the-files-you-need-to-connect-to-excel"></a><a name="files-you-need"></a> Obtenir les fichiers nécessaires pour vous connecter à Excel

Avant de pouvoir importer des données à partir d’Excel ou exporter des données vers Excel avec SSIS, vous devrez peut-être télécharger les composants de connectivité pour Excel s’ils n’ont pas été installés. Les composants de connectivité pour Excel ne sont pas installés par défaut.

Téléchargez la dernière version des composants de connectivité pour Excel ici : [Microsoft Access Database Engine 2016 Redistributable](https://www.microsoft.com/download/details.aspx?id=54920). La dernière version des composants peut ouvrir les fichiers créés dans des versions antérieures d’Excel.

### <a name="notes-about-the-download-and-installation"></a>Notes sur le téléchargement et l’installation

-   Veillez à télécharger Access Database Engine 2016 *Redistributable* et non Microsoft Access 2016 *Runtime*.

-   Si une version 32 bits d’Office est déjà installée sur l’ordinateur, vous devez installer la version 32 bits des composants. Vous devez également exécuter le package SSIS en mode 32 bits ou exécuter la version 32 bits de l’Assistant Importation et Exportation.

-   Si vous avez un abonnement Microsoft 365, vous pouvez voir s’afficher un message d’erreur à l’exécution du programme d’installation. L’erreur indique que vous ne pouvez pas installer le téléchargement côte à côte avec les composants Office « Démarrer en un clic ». Pour contourner ce message d’erreur, exécutez l’installation en mode silencieux en ouvrant une fenêtre d’invite de commandes et en exécutant le fichier .EXE que vous avez téléchargé avec l’option `/quiet`. Par exemple :

    `C:\Users\<user_name>\Downloads\AccessDatabaseEngine.exe /quiet`

    Si vous ne parvenez pas à installer 2016 Redistribuable, installez 2010 Redistribuable à la place à partir de [Microsoft Access Database Engine 2010 Redistributable](https://www.microsoft.com/download/details.aspx?id=13255). (Il n’y a pas de version Redistribuable pour Excel 2013.)

## <a name="specify-excel-as-your-data-source"></a><a name="specify-excel"></a> Spécifier Excel comme source de données

La première étape consiste à indiquer que vous voulez vous connecter à Excel.

### <a name="in-ssis"></a>Dans SSIS
Dans SSIS, créez un gestionnaire de connexions Excel pour vous connecter au fichier source ou de destination Excel. Il existe plusieurs façons de créer le gestionnaire de connexions :

-   Dans la zone **Gestionnaires de connexions**, cliquez avec le bouton droit sur **Nouvelle connexion**. Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS**, sélectionnez **EXCEL**, puis **Ajouter**.
 
-   Dans le menu **SSIS**, sélectionnez **Nouvelle connexion**. Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS**, sélectionnez **EXCEL**, puis **Ajouter**.

-   Créez le gestionnaire de connexions en même temps que vous configurez la **Source Excel** ou la **Destination Excel** dans la page **Gestionnaire de connexions** de **l’Éditeur de source Excel** ou de **l’Éditeur de destination Excel**.

### <a name="in-the-sql-server-import-and-export-wizard"></a>Dans l’Assistant Importation et Exportation SQL Server
Dans l’Assistant Importation et Exportation, dans la page **Choisir une source de données** ou **Choisir une destination**, sélectionnez **Microsoft Excel** dans la liste **Source de données**.

Si vous ne voyez pas Excel dans la liste des sources de données, assurez-vous d’utiliser l’Assistant 32 bits. Les composants de connectivité Excel sont généralement des fichiers 32 bits et ne sont pas affichés dans l’Assistant 64 bits.

## <a name="excel-file-and-file-path"></a><a name="excel-file"></a> Nom et chemin du fichier Excel

Les premières informations à fournir sont le nom et le chemin du fichier Excel. Entrez cette information dans **l’Éditeur du gestionnaire de connexions Excel** dans un package SSIS, ou dans la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation.

Entrez le nom et le chemin du fichier au format suivant :

-   Pour un fichier sur un ordinateur local, entrez **C:\\TestData.xlsx**.

-   Pour un fichier sur un partage réseau, entrez **\\\\Sales\\Data\\TestData.xlsx**.

Vous pouvez aussi cliquer sur **Parcourir** pour rechercher la feuille de calcul à partir de la boîte de dialogue **Ouvrir**.  
  
> [!IMPORTANT]
> Vous ne pouvez pas vous connecter à un fichier Excel protégé par mot de passe.

## <a name="excel-version"></a><a name="excel-version"></a> Version d’Excel

La deuxième information à fournir est la version du fichier Excel. Entrez cette information dans **l’Éditeur du gestionnaire de connexions Excel** dans un package SSIS, ou dans la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation.

Sélectionnez la version de Microsoft Excel ayant été utilisée pour créer le fichier, ou une autre version compatible. Par exemple, si vous n’avez pas pu installer les composants de connectivité 2016, vous pouvez installer les composants 2010 et sélectionner **Microsoft Excel 2007-2010** dans cette liste.

Si vous avez uniquement d’anciennes versions des composants de connectivité, vous ne pourrez peut-être pas sélectionner des versions plus récentes d’Excel dans la liste. La liste des **versions Excel** inclut toutes les versions d’Excel prises en charge par SSIS. La présence d’éléments dans cette liste n’implique pas que les composants de connectivité nécessaires sont installés. Par exemple, **Microsoft Excel 2016** apparaît dans la liste même si vous n’avez pas installé les composants de connectivité 2016.

## <a name="first-row-has-column-names"></a><a name="first-row"></a> La première ligne contient des noms de colonnes

Si vous importez des données d’Excel, l’étape suivante consiste à indiquer si la première ligne de données contient des noms de colonnes. Spécifiez cette information dans **l’Éditeur du gestionnaire de connexions Excel** dans un package SSIS, ou dans la page **Choisir une source de données** de l’Assistant Importation et Exportation.

-   Si vous désactivez cette option parce que les données sources ne contiennent pas de noms de colonnes, l’Assistant utilise F1, F2 et ainsi de suite comme en-têtes de colonnes.
-   Si les données contiennent des noms de colonnes, mais que vous désactivez cette option, l’Assistant importe les noms de colonnes comme étant la première ligne de données.
-   Si les données ne contiennent aucun nom de colonne et que vous activez cette option, l’Assistant utilise la première ligne de données sources comme noms de colonnes. Dans ce cas, la première ligne de données sources n’est plus incluse dans les données elles-mêmes.

Si vous exportez des données à partir d’Excel et que vous activez cette option, la première ligne de données exportées contient les noms de colonnes.

## <a name="worksheets-and-ranges"></a><a name="sheets-ranges"></a> Feuilles de calcul et plages

Vous pouvez utiliser trois types d’objets Excel comme source ou destination de vos données : une feuille de calcul, une plage nommée ou une plage de cellules sans nom que vous spécifiez par son adresse.

-   **Feuille de calcul.** Pour spécifier une feuille de calcul, ajoutez le caractère `$` à la fin du nom de la feuille et ajoutez des délimiteurs autour de la chaîne. Par exemple, **[Sheet1$]** . Vous pouvez aussi rechercher un nom qui se termine par le caractère `$` dans la liste des tables et vues existantes.

-   **Plage nommée.** Pour spécifier une plage nommée, indiquez simplement le nom de la plage. Par exemple, **MyDataRange**. Vous pouvez aussi rechercher un nom qui ne se termine pas par le caractère `$` dans la liste des tables et vues existantes.
    
-   **Plage sans nom.** Pour spécifier une plage de cellules que vous n’avez pas nommée, ajoutez le caractère $ à la fin du nom de la feuille, ajoutez la spécification de plage ainsi que des délimiteurs autour de la chaîne. Par exemple, **[Sheet1$A1:B4]** .

Pour sélectionner ou spécifier le type d’objet Excel que vous souhaitez utiliser comme source ou destination de vos données, effectuez l’une des procédures suivantes :

### <a name="in-ssis"></a>Dans SSIS

Dans SSIS, dans la page **Gestionnaire de connexions** de **l’Éditeur de source Excel** ou de **l’Éditeur de destination Excel**, effectuez l’une des opérations suivantes :

-   Pour utiliser une **feuille de calcul** ou une **plage nommée**, sélectionnez **Table ou vue** comme **Mode d’accès aux données**. Ensuite, dans la liste **Nom de la feuille Excel**, sélectionnez la feuille de calcul ou la plage nommée.

-   Pour utiliser une **plage sans nom** que vous spécifiez par son adresse, sélectionnez **Commande SQL** comme **Mode d’accès aux données**. Puis, dans le champ **Texte de la commande SQL**, entrez une requête semblable à l’exemple suivant :

    ```sql
    SELECT * FROM [Sheet1$A1:B5]
    ```

### <a name="in-the-sql-server-import-and-export-wizard"></a>Dans l’Assistant Importation et Exportation SQL Server
Dans l’Assistant Importation et Exportation, effectuez l’une des procédures ci-dessous :

-   Dans le cas d’une **importation** à partir d’Excel, effectuez l’une des opérations suivantes :

    -   Pour utiliser une **feuille de calcul** ou une **plage nommée**, dans la page **Spécifier la copie ou l’interrogation de table**, sélectionnez **Copier les données à partir d’une ou plusieurs tables ou vues**. Puis, dans la page **Sélectionner les tables et les vues sources**, dans la colonne **Source**, sélectionnez les feuilles de calcul et les plages nommées sources.

    -   Pour utiliser une **plage sans nom** que vous spécifiez par son adresse, dans la page **Spécifier la copie ou l’interrogation de table**, sélectionnez **Écrire une requête pour spécifier les données à transférer**. Puis, dans la page **Fournir une requête source**, spécifiez une requête semblable à l’exemple suivant :

        ```sql
        SELECT * FROM [Sheet1$A1:B5]
        ```

-   Dans le cas d’une **exportation** vers Excel, effectuez l’une des opérations suivantes :

    -   Pour utiliser une **feuille de calcul** ou une **plage nommée**, dans la page **Sélectionner les tables et les vues sources**, dans la colonne **Destination**, sélectionnez les feuilles de calcul et les plages nommées de destination.

    -   Pour utiliser une **plage sans nom** que vous spécifiez par son adresse, dans la page **Sélectionner les tables et les vues sources**, dans la colonne **Destination**, entrez la plage au format suivant sans délimiteurs : `Sheet1$A1:B5`. L’Assistant ajoute les délimiteurs nécessaires.

Une fois que vous avez sélectionné ou entré les objets Excel à importer ou exporter, vous pouvez également effectuer les opérations suivantes dans la page **Sélectionner les tables et les vues sources** de l’Assistant :

-   Vérifiez les mappages de colonnes entre la source et la destination en sélectionnant **Modifier les mappages**.

-   Affichez un aperçu des exemples de données pour vous assurer que le résultat est conforme à vos attentes, en sélectionnant **Aperçu**.

## <a name="issues-with-data-types"></a><a name="issues-types"></a>Problèmes avec les types de données

### <a name="data-types"></a>Types de données

Le pilote Excel ne reconnaît qu'un ensemble limité de types de données. Par exemple, toutes les colonnes numériques sont interprétées comme doubles (DT_R8) et toutes les colonnes de type chaîne (autres que les colonnes mémo) comme des chaînes Unicode de 255 caractères (DT_WSTR). SSIS mappe les types de données Excel de la façon suivante :

-   Numérique - virgule flottante double précision (DT_R8)

-   Devise - devise (DT_CY)

-   Booléen - booléen (DT_BOOL)

-   Date/heure - datetime (DT_DATE)

-   Chaîne - chaîne Unicode, longueur 255 (DT_WSTR)

-   Mémo - flux de texte Unicode (DT_NTEXT)

### <a name="data-type-and-length-conversions"></a>Conversions des types de données et des longueurs

SSIS ne convertit pas implicitement les types de données. Vous devrez donc éventuellement utiliser des transformations Colonne dérivée ou Conversion de données pour convertir les données Excel explicitement avant de les charger dans une destination autre qu’Excel, ou pour convertir des données d’une source autre qu’Excel avant de les charger dans une destination Excel.

Voici des exemples de conversions susceptibles d’être nécessaires :  
  
-   conversion entre des colonnes Excel de type chaîne Unicode et des colonnes de type chaîne non-Unicode avec des pages de codes spécifiques ;
  
-   conversion entre des colonnes Excel de type chaîne de 255 caractères et des colonnes de type chaîne de longueurs différentes ;
  
-   conversion entre des colonnes numériques Excel à double précision et des colonnes numériques d'autres types.

> [!TIP]
> Si vous utilisez l’Assistant Importation et Exportation et que vos données nécessitent certaines de ces conversions, l’Assistant configure automatiquement les conversions nécessaires. Aussi, même si vous souhaitez utiliser un package SSIS, il peut être utile de créer le package initial à l’aide de l’Assistant Importation et Exportation. Laissez l’Assistant créer et configurer automatiquement les gestionnaires de connexions, les sources, les transformations et les destinations.

## <a name="issues-with-importing"></a><a name="issues-importing"></a> Problèmes avec l’importation

### <a name="empty-rows"></a>Lignes vides

Quand vous spécifiez une feuille de calcul ou une plage nommée comme source, le pilote lit le bloc de cellules *contigu* à partir de la première cellule non vide en haut à gauche de la feuille de calcul ou de la plage. Par conséquent, vos données peuvent ne pas commencer à la ligne 1, mais les données sources ne doivent pas contenir de lignes vides. Par exemple, vous ne pouvez pas avoir de ligne vide entre les en-têtes de colonnes et les lignes de données, ni avoir un titre suivi de lignes vides en haut de la feuille de calcul.

Si vos données sont précédées de lignes vides, vous ne pouvez pas interroger les données comme une feuille de calcul. Dans Excel, vous devez sélectionner la plage de données et lui attribuer un nom, puis interroger la plage nommée au lieu de la feuille de calcul.

### <a name="missing-values"></a>Valeurs manquantes

Le pilote Excel lit un certain nombre de lignes (par défaut, huit lignes) dans la source spécifiée pour déterminer le type de données de chaque colonne. Lorsqu'il s'avère qu'une colonne combine différents types de données, notamment des données numériques avec des données texte, le pilote porte son choix sur le type de données majoritaire et retourne des valeurs NULL dans les cellules qui contiennent des données de l'autre type. En cas d'égalité, le type numérique l'emporte. La plupart des options de mise en forme de cellule dans la feuille de calcul Excel n'affectent pas cette détermination du type de données.

Vous pouvez modifier ce comportement du pilote Excel en spécifiant le mode d’importation qui importe toutes les valeurs sous forme de texte. Pour spécifier le mode d’importation, ajoutez `IMEX=1` à la valeur de **Propriétés étendues** dans la chaîne de connexion du gestionnaire de connexions Excel dans la fenêtre Propriétés. 

### <a name="truncated-text"></a>Texte tronqué

Lorsque le pilote détermine qu'une colonne Excel contient des données texte, il sélectionne le type de données (string ou memo) en fonction de la valeur la plus longue qu'il échantillonne. Si le pilote ne découvre pas de valeurs comptant plus de 255 caractères dans les lignes échantillonnées, il traite la colonne comme une colonne de type string à 255 caractères et non comme une colonne de type memo. Par conséquent, les valeurs de plus de 255 caractères peuvent être tronquées.

Pour importer des données d’une colonne de type mémo sans troncation, vous avez deux options :

-   Assurez-vous que la colonne mémo dans au moins une des lignes échantillonnées contient une valeur de plus de 255 caractères.

-   Augmentez le nombre de lignes échantillonnées par le pilote pour inclure une ligne. Vous pouvez augmenter le nombre de lignes échantillonnées en augmentant la valeur de **TypeGuessRows** sous la clé de Registre suivante :

| Version des composants Redistributable | Clé de Registre |
|---|---|
| Excel 2016 | HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\16.0\Access Connectivity Engine\Engines\Excel |
| Excel 2010 | HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\14.0\Access Connectivity Engine\Engines\Excel |
| | |

## <a name="issues-with-exporting"></a><a name="issues-exporting"></a> Problèmes avec l’exportation

### <a name="create-a-new-destination-file"></a>Créer un fichier de destination

#### <a name="in-ssis"></a>Dans SSIS

Créez un gestionnaire de connexions Excel en indiquant le nom et le chemin du nouveau fichier Excel que vous souhaitez créer. Ensuite, dans **l’Éditeur de destination Excel**, pour **Nom de la feuille Excel**, sélectionnez **Nouveau** pour créer la feuille de calcul de destination. SSIS crée alors le fichier Excel avec la feuille de calcul spécifiée.

#### <a name="in-the-sql-server-import-and-export-wizard"></a>Dans l’Assistant Importation et Exportation SQL Server

Dans la page **Choisir une destination**, sélectionnez **Parcourir**. Dans la boîte de dialogue **Ouvrir**, accédez au dossier où vous souhaitez créer le fichier Excel, entrez un nom pour ce nouveau fichier, puis sélectionnez **Ouvrir**.

### <a name="export-to-a-large-enough-range"></a>Exporter vers une plage suffisamment grande

Quand vous spécifiez une plage comme destination, une erreur se produit si la plage a moins de *colonnes* que dans les données sources. Toutefois, si la plage que vous spécifiez a moins de *lignes* que dans les données sources, l’Assistant continue d’écrire des lignes sans erreur et étend la définition de la plage pour qu’elle corresponde au nouveau nombre de lignes.

### <a name="export-long-text-values"></a>Exporter des valeurs texte longues

Avant de pouvoir enregistrer des chaînes dépassant 255 caractères dans une colonne Excel, le pilote doit reconnaître le type de données de la colonne de destination comme **mémo** et non comme **chaîne**.

-   Si une table de destination existante contient déjà des lignes de données, les premières lignes échantillonnées par le pilote doivent contenir au moins une instance d’une valeur dépassant 255 caractères dans la colonne mémo.

## <a name="related-content"></a>Contenu connexe

Pour plus d’informations sur les composants et les procédures décrits dans cet article, consultez les articles ci-dessous :

### <a name="about-ssis"></a>À propos de SSIS
[Gestionnaire de connexions Excel](connection-manager/excel-connection-manager.md)  
[Source Excel](data-flow/excel-source.md)  
[Destination Excel](data-flow/excel-destination.md)  
[Effectuer une boucle dans des fichiers et des tables Excel en utilisant un conteneur de boucles Foreach](control-flow/loop-through-excel-files-and-tables-by-using-a-foreach-loop-container.md)  
[Utilisation de fichiers Excel avec la tâche de script](extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)

### <a name="about-the-sql-server-import-and-export-wizard"></a>À propos de l’Assistant Importation et Exportation SQL Server
[Établir une connexion à une source de données Excel](import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)  
[Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)

### <a name="other-articles"></a>Autres articles
[Importer des données d’Excel vers SQL Server ou Azure SQL Database](../relational-databases/import-export/import-data-from-excel-to-sql.md)  
