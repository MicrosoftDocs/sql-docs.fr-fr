---
title: Conseils de navigation dans la documentation de SQL Server
description: Conseils et astuces pour la navigation dans la documentation technique de SQL Server. Aborde des éléments tels que la page hub, la table des matières et l’en-tête, et explique comment utiliser la barre de navigation et le filtre de version.
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: 4c8852c9b2ce3f7bce027632b741511a008cb646
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595884"
---
# <a name="sql-server-docs-navigation-guide"></a>Guide de navigation dans la documentation de SQL Server

Cette rubrique fournit des conseils et des astuces pour naviguer dans l’espace de la documentation technique de SQL Server.  

## <a name="hub-page"></a>Page hub

La page hub de SQL Server, qui se trouve sur [https://aka.ms/sqldocs](../index.yml?WT.mc_id=akams), constitue le point d’entrée pour toute recherche de contenu sur SQL Server.

Vous pouvez revenir à cette page à tout moment en sélectionnant **Documentation SQL** dans l’en-tête situé en haut de chaque page de la documentation technique de SQL Server : 

![Documentation SQL dans l’en-tête](media/sql-server-docs-navigation-guide/sql-docs-in-header.png)

## <a name="offline-documentation"></a>Documentation hors connexion

Si vous souhaitez afficher la documentation SQL Server sur un système hors ligne, vous disposez de deux options. Vous pouvez créer un fichier PDF à n’importe quel endroit de la documentation technique SQL Server, ou vous pouvez télécharger le contenu hors connexion à l’aide de la [visionneuse d’aide hors connexion SQL Server](./sql-server-offline-documentation.md). 

Si vous souhaitez créer un fichier PDF, cliquez sur le lien **Télécharger le PDF** situé au bas de chaque table des matières.


![Télécharger le PDF](media/sql-server-docs-navigation-guide/download-pdf.png)

## <a name="toc-symbols"></a>Symboles de la table des matières 

Les entrées de la table des matières se terminant par `>`, vous dirigent vers une documentation technique comprenant une table des matières différente. 

![Symboles > dans la table des matières](media/sql-server-docs-navigation-guide/single-carrots-in-sql-docs-toc.png)

Les entrées de la table des matières se terminant par `>>`, vous font quitter le site docs.microsoft.com. 

![Marqueurs de navigation dans la table des matières](media/sql-server-docs-navigation-guide/double-carrots-in-sql-docs-toc.png)

Si vous accédez à l’une de ces pages, vous pouvez revenir à la page technique principale de SQL Server et à la table des matières, en sélectionnant l’entrée « Bienvenue dans SQL Server > » située en haut de chaque table des matières. 

![Revenir à la table des matières SQL](media/sql-server-docs-navigation-guide/navigate-back-to-sql-toc.png)

## <a name="toc-search"></a>Recherche dans la table des matières 
Sur docs.microsoft.com, vous pouvez rechercher du contenu dans la table des matières à l’aide de la zone de filtre située dans la partie supérieure : 

![Utiliser la zone de filtre](media/sql-server-docs-navigation-guide/sql-docs-toc-filter.gif)

## <a name="version-filter"></a>Filtre de version
La documentation technique de SQL Server propose du contenu pour plusieurs versions de SQL Server. Les fonctionnalités pouvant varier d’une version à l’autre de SQL Server, le contenu varie également. 

Utilisez le [filtre de version](versioning-system-monikers-ui-sql-server.md) pour vérifier que le contenu que vous voyez correspond bien à la version de SQL Server qui vous intéresse : 

![Filtre de version dans Documentation SQL](media/sql-server-docs-navigation-guide/sql-docs-version-filter.gif)

Sélectionnez **All SQL** \> **Hide nothing** (Tout SQL > Ne rien masquer) pour afficher tout le contenu et que rien ne soit masqué par le filtre de version. L’option **Hide nothing** peut révéler le contenu qui s’applique à plusieurs versions différentes de SQL Server au sein du même article, ce qui peut être contradictoire, non clair ou confus. Par conséquent, l’option [**Hide nothing** n’est pas recommandée pour une utilisation courante](versioning-system-monikers-ui-sql-server.md#anchor-allsql-hidenothing). 

## <a name="breadcrumbs"></a>Barre de navigation

La barre de navigation, qui se trouve sous l’en-tête et au-dessus de la table des matières, indique l’emplacement de l’article que vous consultez dans la table des matières.  Elle vous aide non seulement à voir le contexte du contenu que vous lisez, mais aussi à remonter l’arborescence de la table des matières :

![Barre de navigation de la documentation SQL](media/sql-server-docs-navigation-guide/sql-docs-bread-crumbs.gif)

## <a name="article-section-navigation"></a>Navigation dans la section d’un article

Le volet de navigation de droite vous permet d’accéder rapidement aux sections d’un article et d’identifier votre position dans l’article.  

![Navigation de droite](media/sql-server-docs-navigation-guide/sql-docs-right-hand-navigation.gif)


## <a name="submit-docs-feedback"></a>Envoyer des commentaires sur la documentation

Si vous trouvez un problème dans un article, vous pouvez envoyer des commentaires à l’équipe SQL en charge du contenu en faisant défiler la page jusqu’en bas et en sélectionnant les **commentaires sur le contenu**.

![Commentaires sur du contenu de problème GIT](media/sql-server-get-help/git-issues.png)

Vous pouvez également envoyer des commentaires et des suggestions d’ordre général concernant la documentation à l’adresse [https://aka.ms/sqldocsfeedback](https://aka.ms/sqldocsfeedback). 

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![Modifier la documentation SQL](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="next-steps"></a>Étapes suivantes

- Bien démarrer avec la [documentation technique de SQL Server](index.yml).
- Pour plus d’informations sur la procédure à suivre pour envoyer des commentaires ou obtenir de l’aide sur SQL Server, consultez la page [Obtenir de l’aide](sql-server-get-help.md). 
- Pour accéder rapidement à tous les didacticiels et guides de démarrage rapide, consultez [Ressources SQL éducatives](../sql-server/educational-sql-resources.yml).