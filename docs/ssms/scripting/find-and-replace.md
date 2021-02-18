---
title: Rechercher et remplacer
description: Découvrez comment utiliser la boîte de dialogue Rechercher et remplacer pour rechercher et remplacer du texte.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Find and Replace dialog box
ms.assetid: 09297893-d80b-4c88-86b4-52bfb639e521
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd45603e4254be161b6e80a59e969f61784cc987
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354658"
---
# <a name="find-and-replace"></a>Rechercher et remplacer
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Utilisez la boîte de dialogue **Rechercher et remplacer** pour rechercher du texte dans un fichier et éventuellement le remplacer. En fonction de la manière dont vous ouvrez la boîte de dialogue **Rechercher et remplacer** , vous pouvez accéder à plusieurs versions de cette même boîte de dialogue proposant des options légèrement différentes. Dans le menu **Edition** , pointez sur **Rechercher et remplacer**, puis cliquez sur **Recherche rapide** pour ouvrir la boîte de dialogue avec les options de recherche, mais sans les options de remplacement. Dans le menu **Edition** , pointez sur **Rechercher et remplacer**, puis cliquez sur **Remplacement rapide** pour ouvrir la boîte de dialogue avec les options de recherche et les options de remplacement.  
  
 Vous pouvez également ouvrir la boîte de dialogue **Rechercher et remplacer** à l’aide de boutons de la barre d’outils et de touches de raccourci.  
  
## <a name="find-what"></a>Rechercher  
 Ces contrôles vous permettent de spécifier la chaîne de caractères ou l'expression à rechercher.  
  
 **Rechercher**  
 Tapez le texte à rechercher. La boîte de dialogue tente de fournir le texte recherché, à l'aide du texte sélectionné avec le curseur avant l'ouverture de la boîte de dialogue, d'un texte proche ou d'un texte qui a fait l'objet d'une recherche antérieure. Vous pouvez réutiliser une des 20 dernières chaînes de recherche en la sélectionnant dans cette liste déroulante.  
  
 **[chaînes contenant des caractères génériques]**  
 Si vous souhaitez inclure des caractères génériques tels que des astérisques (`*`) et des points d’interrogation (`?`) dans votre chaîne de recherche, cochez la case **Utiliser** sous **Options de recherche** , puis cliquez sur **Caractères génériques**.  
  
 **[expression régulière]**  
 Pour forcer le moteur de recherche à interpréter votre chaîne de recherche comme une expression régulière, cochez la case **Utiliser** sous **Options de recherche** , puis cliquez sur **Expressions régulières**.  
  
 **Générateur d’expressions**  
 Ce bouton triangulaire en regard de la zone **Rechercher** est disponible quand la case **Utiliser** est cochée sous **Options de recherche**. Cliquez sur ce bouton pour afficher la liste des caractères génériques ou des expressions régulières, selon l’option **Utiliser** qui est sélectionnée. Choisir un élément dans cette liste permet de l’ajouter à la chaîne spécifiée dans la zone **Rechercher** .  
  
## <a name="replace-with"></a>Remplacer par  
 Ces contrôles vous permettent de spécifier ce qui doit être inséré à la place de la chaîne ou de l'expression trouvée.  
  
 **Replace with**  
 Pour remplacer les instances de la chaîne spécifiée dans **Rechercher** par une autre chaîne, entrez la chaîne de remplacement dans ce champ. Pour supprimer les occurrences du texte spécifié dans **Rechercher**, laissez ce champ vide. Sélectionnez la liste déroulante afin d'afficher les 20 dernières entrées. Pour inclure des expressions régulières dans la chaîne spécifiée dans la zone **Remplacer par** , cochez la case **Utiliser** , puis cliquez sur **Expressions régulières**. Cette zone s’affiche uniquement si vous avez ouvert la boîte de dialogue en cliquant sur **Remplacement rapide**.  
  
 **Replace with**  
 Pour remplacer les occurrences de la chaîne spécifiée dans la zone **Rechercher** par une autre chaîne de caractères, tapez la chaîne de remplacement dans ce champ. Pour supprimer les occurrences de la chaîne spécifiée dans la zone **Rechercher** , laissez ce champ vide. Sélectionnez la liste déroulante afin d'afficher les 20 dernières entrées. Pour inclure des expressions régulières dans la chaîne spécifiée dans la zone **Remplacer par** , cochez la case **Utiliser** , puis cliquez sur **Expressions régulières**.  
  
 **Générateur d’expressions**  
 Ce bouton triangulaire placé à côté de la zone **Remplacer par** devient disponible quand la case **Utiliser** est cochée dans **Options de recherche**. Cliquez sur ce bouton pour afficher la liste des caractères génériques ou des expressions régulières, selon l’option **Utiliser** qui est sélectionnée. Cliquer sur un élément dans cette liste permet de l’ajouter à la chaîne spécifiée dans la zone **Remplacer par** .  
  
 **Replace**  
 Cliquez sur ce bouton pour remplacer l’occurrence en cours de la chaîne spécifiée dans la zone **Rechercher** par la chaîne spécifiée dans la zone **Remplacer par** et pour rechercher l’occurrence suivante à l’intérieur de la zone de recherche spécifiée dans **Regarder dans**.  
  
 **Remplacer tout**  
 Cliquez sur ce bouton pour remplacer toutes les occurrences de la chaîne spécifiée dans la zone **Rechercher** par la chaîne spécifiée dans la zone **Remplacer par** , dans tous les fichiers se trouvant dans la zone de recherche spécifiée dans la zone **Regarder dans** .  
  
> [!CAUTION]  
>  Vérifiez que l’étendue définie dans **Rechercher dans** inclut seulement les fichiers que vous souhaitez modifier.  
  
 Un rappel avec une option **Conserver les fichiers modifiés ouverts** est affiché. Pour conserver l’option **Annuler** , vous devez sélectionner cette option. L’option **Annuler** est disponible uniquement dans les fichiers qui restent ouverts après avoir été modifiés.  
  
 **Ignorer le fichier**  
 Ce bouton est activé lorsque la valeur spécifiée pour **Regarder dans** inclut plusieurs fichiers. Cliquez sur ce bouton si vous ne voulez pas inspecter ni modifier le fichier en cours. La recherche continue dans le fichier suivant de la liste **Regarder dans**.  
  
## <a name="look-in"></a>Regarder dans  
 **Look in**  
 Sélectionnez l’emplacement dans lequel vous souhaitez rechercher le texte spécifié dans **Rechercher**. Les options disponibles incluent **Document actif**, qui permet d’effectuer la recherche dans la fenêtre de document qui était active au moment de l’ouverture de la boîte de dialogue, et **Tous les documents ouverts**, qui permet d’effectuer la recherche dans toutes les fenêtres qui sont actuellement ouvertes dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="find-options"></a>Options de recherche  
 Vous pouvez développer ou réduire la section **Options de recherche** . Les options ci-après peuvent être activées ou désactivées.  
  
 **Respecter la casse**  
 Quand cette case est cochée, les fenêtres de résultats de la recherche n’affichent que les instances de la chaîne spécifiée dans **Rechercher** qui concordent avec le contenu et la casse. Par exemple, si vous recherchez «**MonObjet**» en cochant la case **Respecter la casse** , la recherche va retourner « MonObjet », mais pas « monobjet » ni « MONOBJET».  
  
 **Mot entier**  
 Quand cette case est cochée, les fenêtres de résultats de la recherche n’affichent que les instances de la chaîne spécifiée dans **Rechercher** qui concordent avec les mots entiers. Si vous recherchez **MonObjet** , par exemple, « MonObject » est retourné comme résultat, mais pas « CMonObjet » ou « MonObjetC ».  
  
 **Rechercher vers le haut**  
 Effectuez la recherche à partir de l'emplacement du curseur jusqu'au début du document.  
  
 **Rechercher le texte masqué**  
 Recherchez les occurrences dans le texte qui est masqué ou réduit.  
  
 **Utilisation**  
 Indique comment interpréter les caractères spéciaux qui sont entrés dans les zones de texte **Rechercher** et **Remplacer par** . Les options comprennent **Caractères génériques** et **Expressions régulières**.  
  
 **Regular Expressions**  
 Des notations spéciales définissent des modèles de texte pour les concordances. Pour obtenir la liste, consultez [Rechercher du texte avec des expressions régulières](./search-text-with-regular-expressions.md).  
  
 **Caractères génériques**  
 Les caractères spéciaux tels que les astérisques (`*`) et les points d'interrogation (`?`) représentent un ou plusieurs caractères. Pour obtenir la liste, consultez [Rechercher du texte avec des caractères génériques](./search-text-with-wildcards.md).  
  
 **Suivant**  
 Commence à rechercher le texte spécifié dans la zone **Rechercher** .  
  
 **Replace**  
 Cliquez sur ce bouton pour remplacer l’occurrence en cours de la chaîne spécifiée dans **Rechercher** par la chaîne spécifiée dans **Remplacer par** et pour rechercher l’occurrence suivante à l’intérieur de la zone de recherche spécifiée dans **Regarder dans**.  
  
 **Replace All**  
 Cliquez sur ce bouton pour remplacer toutes les occurrences de la chaîne spécifiée dans **Rechercher** par la chaîne spécifiée dans **Remplacer par**, dans tous les fichiers se trouvant dans la zone de recherche spécifiée dans **Regarder dans**.  
  
> [!CAUTION]  
>  Vérifiez que l’étendue définie dans **Rechercher dans** inclut seulement les fichiers que vous souhaitez modifier.  
  
## <a name="find-and-replace-views"></a>Vues Rechercher et remplacer  
 Les onglets situés en haut de la fenêtre Rechercher et remplacer comprennent les menus **Affichage** . Ces menus vous permettent de choisir un ensemble de champs à afficher dans le volet actif. Vous pouvez laisser la fenêtre **Rechercher et remplacer** ancrée à un endroit qui vous convient, puis basculer d’un onglet à l’autre et d’une vue à l’autre pour exécuter tout type d’opération de recherche ou de remplacement.  
  
 **Recherche rapide**  
 Cet onglet de barre d’outils remplace la boîte de dialogue par celle intitulée **Recherche rapide** .  
  
 **Rechercher dans les fichiers**  
 Cet onglet de barre d’outils remplace la boîte de dialogue par celle intitulée **Rechercher dans les fichiers** .  
  
 **Remplacement rapide**  
 Cet onglet de barre d’outils remplace la boîte de dialogue par celle intitulée **Remplacement rapide** .  
  
 **Remplacer dans les fichiers**  
 Cet onglet de barre d’outils remplace la boîte de dialogue par celle intitulée **Remplacer dans les fichiers** .  
  
## <a name="see-also"></a>Voir aussi  
 [Raccourcis clavier dans SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)