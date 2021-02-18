---
title: Rechercher dans les fichiers
description: Découvrez comment rechercher une chaîne ou une expression dans un ensemble de fichiers spécifié et comment définir l’emplacement des résultats de la recherche.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Find in Files dialog box
ms.assetid: bf92770a-33df-43ef-85ad-5a9223649b98
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 642f0a2f82d279357300d87db3821496d881eb11
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271658"
---
# <a name="find-in-files"></a>Rechercher dans les fichiers
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  L’onglet **Rechercher dans les fichiers** de la fenêtre Rechercher et remplacer vous permet de rechercher une chaîne ou une expression dans le code d’un ensemble de fichiers spécifié. Les concordances trouvées et les actions exécutées sont répertoriées dans la fenêtre Résultats de la recherche, sélectionnée dans **Options de résultat**.  
  
 Vous pouvez également ouvrir la boîte de dialogue **Rechercher et remplacer** à l’aide de boutons de la barre d’outils et de touches de raccourci.  
  
 Les sections ci-après répertorient les commandes disponibles sous l’onglet **Rechercher dans les fichiers** .  
  
## <a name="find-what"></a>Rechercher  
 Ces commandes sous l’onglet **Rechercher dans les fichiers** vous permettent de spécifier la chaîne ou l’expression qui doit concorder.  
  
 **Rechercher**  
 Tapez le texte à rechercher. La boîte de dialogue tente de fournir le texte recherché, à l'aide du texte sélectionné avec le curseur avant l'ouverture de la boîte de dialogue, d'un texte proche ou d'un texte qui a fait l'objet d'une recherche antérieure. Vous pouvez réutiliser une des 20 dernières chaînes de recherche en la sélectionnant dans cette liste déroulante.  
  
 **[chaînes contenant des caractères génériques]**  
 Si vous souhaitez inclure des caractères génériques tels que des astérisques (`*`) et des points d’interrogation (`?`) dans votre chaîne de recherche, cochez la case **Utiliser** sous **Options de recherche** , puis cliquez sur **Caractères génériques**.  
  
 **[expression régulière]**  
 Pour forcer le moteur de recherche à interpréter votre chaîne de recherche comme une expression régulière, cochez la case **Utiliser** sous **Options de recherche** , puis cliquez sur **Expressions régulières**.  
  
 **Générateur d’expressions**  
 Ce bouton triangulaire en regard de la zone **Rechercher** est disponible quand la case **Utiliser** est cochée sous **Options de recherche**. Cliquez sur ce bouton pour afficher la liste des caractères génériques ou des expressions régulières, selon l’option **Utiliser** qui est sélectionnée. Tout élément sélectionné dans cette liste est ajouté à la chaîne **Rechercher** .  
  
## <a name="look-in"></a>Regarder dans  
 L’option sélectionnée dans la liste déroulante **Regarder dans** détermine si l’option **Rechercher dans les fichiers** limite la recherche aux fichiers qui sont actuellement actifs ou l’étend à tous les fichiers stockés dans certains dossiers. Sélectionnez l’étendue de la recherche dans la liste, tapez le chemin d’un dossier ou cliquez sur le bouton **...** pour afficher la boîte de dialogue **Choisir des dossiers de recherche** et choisir un ensemble de dossiers pour la recherche.  
  
> [!NOTE]  
>  Si l’option **Regarder dans** sélectionnée applique la recherche à un fichier que vous avez extrait du contrôle du code source, celle-ci se limite à la version du fichier qui a été téléchargée sur votre ordinateur local.  
  
 **Look in**  
 Sélectionnez une étendue de recherche prédéfinie dans cette liste ou entrez votre propre ensemble de répertoires à l’aide de la boîte de dialogue **Choisir des dossiers de recherche** .  
  
 **Document actif**  
 Cette option est disponible lorsqu'un document est ouvert dans un éditeur. Elle recherche la chaîne spécifiée dans **Rechercher** dans le document actif uniquement.  
  
 **Tous les documents ouverts**  
 Effectue la recherche dans tous les fichiers actuellement ouverts à des fins d'édition.  
  
 **Projet en cours**  
 Effectue la recherche dans tous les fichiers du projet en cours.  
  
 **Solution complète**  
 Effectue la recherche dans tous les fichiers de la solution active.  
  
 **Inclure les sous-dossiers**  
 Indique que les sous-dossiers du dossier spécifié dans **Regarder dans** doivent être inclus dans la recherche. Cette option requiert un ensemble de répertoires personnalisé.  
  
 **Parcourir**  
 Cliquez sur ce bouton pour afficher la boîte de dialogue **Choisir des dossiers de recherche** , qui vous permet d’assembler, de modifier, d’enregistrer et de sélectionner des ensembles nommés de répertoires à entrer dans la zone **Regarder dans** .  
  
## <a name="find-options"></a>Options de recherche  
 Vous pouvez développer ou réduire la section **Options de recherche** . Les options ci-après peuvent être activées ou désactivées.  
  
 **Respecter la casse**  
 Quand cette case est cochée, les fenêtres de résultats de la recherche n’affichent que les instances de la chaîne spécifiée dans **Rechercher** qui concordent avec le contenu et la casse. Par exemple, une recherche de **MyObject** lorsque la case **Respecter la casse** est cochée retourne « MyObject », mais pas « myobject », ni « MYOBJECT ».  
  
 **Mot entier**  
 Quand cette case est cochée, les fenêtres de résultats de la recherche n’affichent que les instances de la chaîne spécifiée dans **Rechercher** qui concordent avec les mots entiers. Si vous recherchez **MonObjet** , par exemple, « MonObject » est retourné comme résultat, mais pas « CMonObjet » ou « MonObjetC ».  
  
 **Utilisation**  
 Indique comment interpréter les caractères spéciaux qui sont entrés dans les zones de texte **Rechercher** et **Remplacer par** . Les options comprennent **Caractères génériques** et **Expressions régulières**.  
  
 **Regular Expressions**  
 Des notations spéciales définissent des modèles de texte pour les concordances. Pour obtenir la liste, consultez [Rechercher du texte avec des expressions régulières](./search-text-with-regular-expressions.md).  
  
 **Caractères génériques**  
 Les caractères spéciaux tels que les astérisques (`*`) et les points d'interrogation (`?`) représentent un ou plusieurs caractères. Pour obtenir la liste, consultez [Rechercher du texte avec des caractères génériques](./search-text-with-wildcards.md).  
  
 **Examiner ces types de fichiers**  
 Cette liste indique les types de fichiers auxquels la recherche doit être appliquée dans les répertoires spécifiés dans **Regarder dans**. Si ce champ est vide, la recherche est étendue à tous les fichiers des répertoires spécifiés dans **Regarder dans** .  
  
```  
*.[ext]; *.[ext] (manual)  
```  
  
 Pour effectuer une recherche dans des fichiers d'un type particulier, entrez un astérisque (`*`) pour le nom de fichier, suivi d'un point (`.`) et de l'extension de fichier. Pour spécifier plusieurs types de fichiers, entrez plusieurs chaînes de recherche séparées par un point-virgule (`;`).  
  
```  
*.[ext]; *.[ext] (from list)  
```  
  
 Sélectionnez n'importe quel élément dans la liste pour entrer une chaîne de recherche préconfigurée qui retrouvera des fichiers de types particuliers.  
  
## <a name="result-options"></a>Options de résultat  
 Détermine l’emplacement des résultats lorsque vous cliquez sur **Rechercher tout**. Vous pouvez développer ou réduire la section **Options de résultat** . Les options ci-après peuvent être activées ou désactivées.  
  
 **Fen. Résultats de la recherche 1**  
 Lorsque cette case à cocher est activée, les résultats de la recherche actuelle sont ajoutés au contenu de la fenêtre Résultats de la recherche 1. Cette fenêtre s'ouvre automatiquement pour afficher vos résultats de recherche. Pour l’ouvrir manuellement, cliquez sur **Autres fenêtres** dans le menu **Affichage** , puis cliquez sur **Résultats de la recherche 1**.  
  
 **Fen. Résultats de la recherche 2**  
 Lorsque cette case à cocher est activée, les résultats de la recherche actuelle sont ajoutés au contenu de la fenêtre Résultats de la recherche 2. Cette fenêtre s'ouvre automatiquement pour afficher vos résultats de recherche. Pour l’ouvrir manuellement, cliquez sur **Autres fenêtres** dans le menu **Affichage** , puis cliquez sur **Résultats de la recherche 2**.  
  
 **Afficher les noms des fichiers seulement**  
 Affiche une entrée par fichier contenant une concordance plutôt qu'une entrée par concordance dans la fenêtre Résultats de la recherche 1 ou Résultats de la recherche 2. Cette option n'est pas disponible dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Conserver les fichiers modifiés ouverts après un remplacement global**  
 Lorsque cette option est activée, tous les fichiers dans lesquels des remplacements ont été effectués restent ouverts afin que vous puissiez annuler ou enregistrer ces modifications. Les contraintes de mémoire peuvent limiter le nombre de fichiers qui peuvent rester ouverts suite à une opération de remplacement.  
  
> [!CAUTION]  
>  Vous ne pouvez utiliser la commande **Annuler** qu’avec les fichiers qui restent ouverts à des fins d’édition. Si vous n’activez pas cette option, les fichiers qui n’étaient pas ouverts à des fins d’édition restent fermés et l’option **Annuler** n’est pas disponible pour ces fichiers.  
  
## <a name="find-and-replace-views"></a>Vues Rechercher et remplacer  
 Les onglets situés en haut de la fenêtre Rechercher et remplacer comprennent les menus **Affichage** . Ces menus vous permettent de choisir un ensemble de champs à afficher dans le volet actif. Vous pouvez laisser la fenêtre Rechercher et remplacer ancrée à un endroit qui vous convient, puis effectuer des modifications au niveau des onglets et des vues pour exécuter tout type d'opération de recherche ou de remplacement.  
  
 **Recherche rapide**  
 Cet onglet de barre d’outils remplace la boîte de dialogue par celle intitulée **Recherche rapide** .  
  
 **Rechercher dans les fichiers**  
 Cet onglet de barre d’outils remplace la boîte de dialogue par celle intitulée **Rechercher dans les fichiers** .  
  
 **Rechercher un symbole**  
 Cet onglet de barre d’outils remplace la boîte de dialogue par celle intitulée **Rechercher un symbole** .  
  
## <a name="see-also"></a>Voir aussi  
 [Raccourcis clavier dans SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)