---
title: Fenêtre Liste d'erreurs
description: Découvrez comment utiliser la liste d’erreurs pour afficher les erreurs et les avertissements générés par IntelliSense lorsque vous modifiez des scripts Transact-SQL.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- error list window
- SQL Server Management Studio [SQL Server], error list window
ms.assetid: fae6327d-e268-44ae-a474-4a8f8f843129
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5045ff2aa128b9e1c34f3031ae1fcd487b511481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354512"
---
# <a name="transact-sql-debugger---error-list-window"></a>Débogueur Transact-SQL - Fenêtre Liste d’erreurs

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La **Liste d’erreurs** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] répertorie les erreurs syntaxiques et sémantiques générées par le code IntelliSense dans l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="features-of-the-error-list"></a>Fonctionnalités de la Liste d'erreurs  

La **Liste d'erreurs** fournit les fonctionnalités suivantes :  
  
-   Lorsque vous modifiez des scripts, la **Liste d’erreurs** répertorie les erreurs et les avertissements produits par IntelliSense dans l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
-   Vous pouvez double-cliquer sur une entrée de message d'erreur pour examiner l'onglet du fichier de script qui a généré l'erreur et vous rendre à l'endroit où l'erreur s'est produite.  
  
-   Vous pouvez filtrer les entrées à afficher, ainsi que les colonnes d'informations à afficher pour chaque entrée.  
  
-   Lorsque vous corrigez une erreur, l'entrée correspondante est supprimée de la **Liste d'erreurs**.  
  
-   Lorsque vous fermez l'onglet pour un fichier de script [!INCLUDE[tsql](../../includes/tsql-md.md)] , les erreurs relatives à ce fichier sont supprimées de la **Liste d'erreurs**.  
  
## <a name="working-with-the-error-list"></a>Utilisation de la Liste d'erreurs  
 Pour afficher la **Liste d'erreurs**, effectuez l'une des opérations suivantes :  
  
-   Dans le menu **Affichage** , cliquez sur **Liste d'erreurs**.  
  
-   Utilisez le raccourci clavier Ctrl+\\, Ctrl+E.  
  
 Après avoir ouvert la **Liste d'erreurs**, vous pouvez personnaliser l'affichage en effectuant les actions suivantes :  
  
-   Pour trier la liste, cliquez sur n'importe quel en-tête de colonne. Pour trier à nouveau sur une colonne supplémentaire, appuyez sur la touche MAJ et maintenez-la enfoncée tout en cliquant sur un autre en-tête de colonne.  
  
-   Pour sélectionner les colonnes qui sont affichées et celles qui sont masquées, sélectionnez **Afficher les colonnes** dans le menu contextuel.  
  
-   Pour modifier l'ordre dans lequel les colonnes sont affichées, faites glisser les en-têtes de votre choix vers la droite ou la gauche.  
  
 La **Liste d'erreurs** ne contient pas de liens vers des informations supplémentaires sur des erreurs spécifiques.  
  
## <a name="transact-sql-errors-in-management-studio"></a>Erreurs Transact-SQL dans Management Studio  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] affiche les erreurs pour les scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] aux emplacements suivants :  
  
-   La **Liste d’erreurs** contient toutes les erreurs syntaxiques et sémantiques détectées par IntelliSense dans l’éditeur du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Cette liste d'erreurs est mise à jour dynamiquement au fur et à mesure que vous modifiez des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] . La liste inclut toutes les erreurs que l’éditeur a identifiées dans chaque script [!INCLUDE[tsql](../../includes/tsql-md.md)] . L'éditeur n'arrête pas l'analyse d'un fichier lorsque des erreurs ont été identifiées dans un script. Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], IntelliSense dans l’éditeur du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne prend pas en charge tous les éléments syntaxiques [!INCLUDE[tsql](../../includes/tsql-md.md)] . La **Liste d'erreurs** contient uniquement les erreurs de la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] prise en charge par IntelliSense.  
  
-   L’onglet **Messages** en bas de la fenêtre de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] affiche les erreurs et les messages retournés par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] lorsqu’un script [!INCLUDE[tsql](../../includes/tsql-md.md)] est exécuté. Cette liste ne change pas jusqu'à ce que vous exécutiez à nouveau le script. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] arrête d’analyser un lot s’il détecte une ou deux erreurs de compilation ; par conséquent, l’onglet **Messages** peut ne pas répertorier toutes les erreurs d’un script.  
  
 Parfois, des erreurs sont répertoriées dans les deux emplacements. Par exemple, un fichier de script peut contenir une erreur de syntaxe qui est répertoriée dans la **Liste d'erreurs**. Si vous exécutez le script avant de corriger l’erreur, l’analyseur du [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut détecter la même condition et retourner une autre copie du message d’erreur sous l’onglet **Messages** .  
  
> [!NOTE]  
>  La **Liste d’erreurs** affiche uniquement les erreurs de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ; elle n’affiche pas les erreurs des éditeurs MDX, DMX ni XML/A. Toutes les erreurs MDX, DMX et XML/A sont affichées sous l’onglet **Messages** de ces éditeurs.  
  
## <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur  
 Lorsque la **Liste d'erreurs** est ouverte, les informations sont affichées dans les colonnes suivantes :  
  
 **Ordre par défaut**  
 Affiche un entier qui indique l'ordre dans lequel une entrée a été générée.  
  
 **Description**  
 Affiche le texte de l'entrée de l'erreur. Les descriptions longues s'étendent sur plusieurs lignes.  
  
 **File**  
 Affiche le nom du fichier de script qui a généré l'erreur.  
  
 **Ligne**  
 Affiche un entier qui indique la ligne du code contenant l'erreur.  
  
 **Colonne**  
 Affiche un entier qui indique la position de l'erreur dans la ligne de code.  
  
 **Projet**  
 Affiche le nom du projet qui comprend le fichier de script.  
