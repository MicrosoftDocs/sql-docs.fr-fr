---
description: Boîte de dialogue Ouvrir un fichier
title: Boîte de dialogue Ouvrir un fichier
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.openfile
ms.assetid: 3e01b9f5-2b0a-4fb3-9da8-984d27d17b8a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: f6d9c0a9d83f0004c534f3548cb114f06de2861f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423043"
---
# <a name="open-file-dialog-box"></a>Boîte de dialogue Ouvrir un fichier
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 La boîte de dialogue **Ouvrir un fichier** permet d’ouvrir un fichier existant à partir du disque. Vous pouvez également utiliser cette boîte de dialogue pour ouvrir un fichier déjà ouvert en utilisant des options d'encodage linguistique différentes.  
  
Pour accéder à cette boîte de dialogue, sélectionnez **Ouvrir** dans le menu **Fichier** , puis choisissez **Fichier**. Cette boîte de dialogue s'affiche également lorsque vous ouvrez des fichiers à partir d'autres éléments, comme la boîte de dialogue **Outils externes** . Dans le menu **Fichier** , sélectionnez **Ouvrir**et choisissez **Projet/Solution** pour ouvrir la boîte de dialogue **Ouvrir un projet** .  
  
> [!NOTE]  
> Avant d'ouvrir un projet ou un composant dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], déterminez la fiabilité de son code. Le fait d'ouvrir le projet ou le composant dans un [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] peut entraîner l'exécution de son code dans un processus approuvé sur votre ordinateur local.  
  
## <a name="option"></a>Option  
**Look in**  
Recherchez le dossier de projet existant dans ce menu déroulant. Si vous sélectionnez un dossier dans cette liste, son contenu s'affiche dans le volet principal.  
  
## <a name="my-places-bar"></a>Barre Mon environnement  
**Bureau**  
Affiche les fichiers et les dossiers situés sur le bureau.  
  
**Mes projets**  
Affiche les fichiers et les dossiers dans le dossier **Projets** de l’utilisateur.  
  
**Poste de travail**  
Affiche le contenu de votre disquette, de votre disque dur et de votre CD-ROM.  
  
## <a name="folder-list"></a>Liste des dossiers  
**Nom de fichier**  
Utilisez cette option pour filtrer les fichiers et les dossiers affichés. Entrez le nom de fichier complet ou partiel sur lequel doit s'effectuer le filtrage. Vous pouvez utiliser l'astérisque (*) comme caractère générique.  
  
**Type de fichiers**  
Utilisez cette option pour filtrer le contenu du dossier ou du répertoire sélectionné dans Regarder dans pour un type de fichier particulier.  
  
**Options Ouvrir avec et Encodage**  
Pour utiliser la **boîte de dialogue Ouvrir avec** et indiquer l’éditeur du fichier cible, sélectionnez le petit rectangle à droite du bouton **Ouvrir** et choisissez **Ouvrir avec**. Si besoin est, vous pouvez également spécifier un schéma d'encodage linguistique à appliquer lors de l'ouverture du fichier sélectionné. Pour ce faire, sélectionnez un programme dans la liste qui contient «**avec encodage**» et choisissez **Ouvrir** pour afficher la **boîte de dialogue Avec encodage**. Ce bouton n'est pas toujours disponible.  
  
## <a name="toolbar"></a>Barre d’outils  
**Naviguer vers l'arrière**  
Retourne le dossier, le lecteur ou l'adresse Internet la plus récemment affichée.  
  
**Dossier parent**  
Atteint le niveau suivant supérieur de l'arborescence.  
  
**Rechercher sur le Web**  
Ce bouton n'est pas disponible.  
  
**Supprimer**  
Supprime les fichiers ou dossiers sélectionnés du support de stockage.  
  
**Nouveau dossier**  
Affiche la boîte de dialogue **Nouveau dossier** . Utilisez cette option pour créer un dossier enfant sous le dossier sélectionné dans la zone de liste déroulante **Regarder dans** .  
  
## <a name="views"></a>Les vues  
Fournit des options permettant d’organiser et d’afficher le contenu de l’élément sélectionné dans la zone de liste déroulante **Vues** .  
  
**Miniatures**  
Affiche les éléments sous la forme de miniatures dans le volet d'informations.  
  
**Vignettes**  
Affiche les fichiers et les dossiers sous la forme de grandes icônes.  
  
**Icônes**  
Affiche les fichiers et les dossiers sous la forme de petites icônes.  
  
**Liste**  
Affiche les fichiers et les dossiers sous la forme d'une liste.  
  
**Détails**  
Affiche le nom, la taille, le type et la date de la dernière modification des fichiers et des dossiers sous la forme d'une liste. Pour trier par un détail particulier, cliquez sur l'en-tête de sa colonne.  
  
**WebView**  
Cette commande n'est pas disponible.  
  
## <a name="tools"></a>Outils  
Sélectionnez un outil à appliquer à l'élément sélectionné dans le volet du sommaire.  
  
**Supprimer**  
Supprime le fichier ou dossier sélectionné du support de stockage.  
  
**Mapper un lecteur réseau**  
Ouvre la boîte de dialogue **Mapper un lecteur réseau** .  
