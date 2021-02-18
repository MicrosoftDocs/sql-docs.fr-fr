---
title: Information sur les paramètres (IntelliSense)
description: Découvrez comment utiliser l’option Informations sur les paramètres d’IntelliSense, qui fournit des informations à mesure que vous saisissez les paramètres requis par une fonction ou une procédure stockée.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Parameter Info option [IntelliSense]
- stored function parameter completion [Intellisense]
- language references [SQL Server]
- IntelliSense [SQL Server], Parameter Info option
ms.assetid: 56c2aac9-c65c-4679-b62c-d9f689876dde
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 622b3060df64c94d12ab1b7c5d602ccf1c602a4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270823"
---
# <a name="parameter-info-intellisense"></a>Information sur les paramètres (IntelliSense)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  L’option [!INCLUDE[msCoName](../../includes/msconame-md.md)] Informations sur les paramètres **de** IntelliSense ouvre une liste de paramètres qui fournit des informations sur le nombre, le nom et le type des paramètres requis par une fonction ou une procédure stockée. Le paramètre en gras indique le prochain paramètre requis lorsque vous tapez une fonction ou une procédure stockée.  
  
 La liste des paramètres est également affichée pour les fonctions imbriquées. Si vous tapez une fonction comme paramètre d'une autre fonction, la liste affiche dans un premier temps les paramètres de la fonction interne. Dans un second temps, la liste affiche les paramètres de la fonction externe.  
  
#### <a name="to-view-parameter-info-for-functions-or-stored-procedures"></a>Pour afficher l'option Informations sur les paramètres pour les fonctions ou les procédures stockées  
  
1.  À la suite du nom de la fonction, tapez une parenthèse ouvrante comme vous le feriez normalement pour ouvrir une liste de paramètres. Après avoir tapé le nom d'une procédure stockée, tapez un espace comme vous le feriez normalement pour obtenir des informations à propos des paramètres d'une procédure.  
  
     IntelliSense affiche la déclaration complète de la fonction ou les paramètres de la procédure stockée dans une fenêtre indépendante, juste sous le point d'insertion. Le premier paramètre de la liste est affiché en gras.  
  
2.  À mesure que vous tapez les paramètres, le paramètre suivant à entrer vous est indiqué en gras.  
  
3.  Appuyez sur ÉCHAP pour fermer la liste ou continuez à taper la fonction jusqu'à la fin.  
  
     Dans le cas d'une fonction, si vous tapez la parenthèse fermante, vous fermez également la liste des paramètres.  
  
#### <a name="to-manually-start-parameter-info"></a>Pour démarrer manuellement l'option Informations sur les paramètres  
  
1.  Dans le menu **Edition** , sélectionnez **IntelliSense** , puis **Informations sur les paramètres**.  
  
2.  Utilisez le raccourci clavier CTRL+MAJ+ESPACE.  
  
 Pour plus d’informations, consultez [Configurer IntelliSense &#40;SQL Server Management Studio&#41;](./configure-intellisense-sql-server-management-studio.md).  
  
> [!NOTE]  
>  L’option **Informations sur les paramètres** est uniquement disponible pour l’éditeur de requête [!INCLUDE[ssDE](../../includes/ssde-md.md)] et l’Éditeur de requête XML.  
  
