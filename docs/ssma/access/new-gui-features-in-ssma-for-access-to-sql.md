---
description: Nouvelles fonctionnalités de l’interface utilisateur graphique dans SSMA pour l’accès à SQL
title: Nouvelles fonctionnalités de l’interface utilisateur graphique dans SSMA pour l’accès à SQL | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 95b95de2-db05-4422-825d-43968ecfd01c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 75bddda078465537e7d68047d6c9c690beafaea4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066574"
---
# <a name="new-gui-features-in-ssma-for-access-to-sql"></a>Nouvelles fonctionnalités de l’interface utilisateur graphique dans SSMA pour l’accès à SQL
Ce chapitre décrit les nouvelles fonctionnalités de l’interface utilisateur SSMA  
  
## <a name="layouts"></a>Dispositions  
Cette fonctionnalité vous permet de choisir l’un des deux déploiements Windows prédéfinis ou de créer votre propre disposition. Pour accéder au sous-menu disposition, dans le menu Affichage, pointez sur dispositions. Vous pouvez choisir l’une des dispositions existantes, ajouter la disposition actuelle ou gérer les dispositions.  
  
### <a name="add-current-layout"></a>Ajouter la disposition actuelle  
Pour enregistrer la disposition Windows active, dans le menu Affichage, pointez sur disposition, puis cliquez sur Ajouter la disposition actuelle.  
  
### <a name="choose-predefined-layout"></a>Choisir une disposition prédéfinie  
Pour choisir l’une des mises en page prédéfinies, dans le menu Affichage, pointez sur dispositions, puis cliquez sur disposition par défaut ou sans explorateurs. Vous pouvez également utiliser les raccourcis Ctrl + Alt + 1 ou CTRL + ALT + 2 pour les mises en page prédéfinies respectivement.  
  
### <a name="choose-user-defined-layout"></a>Choisir une disposition définie par l’utilisateur  
Pour choisir la disposition définie par l’utilisateur, dans le menu Affichage, pointez sur dispositions, puis cliquez sur l’une des dispositions définies par l’utilisateur. Vous pouvez également utiliser des raccourcis définis pour les dispositions.  
  
### <a name="manage-layouts"></a>Gérer les dispositions  
Pour ouvrir la boîte de dialogue gérer les dispositions, dans le menu Affichage, pointez sur dispositions, puis cliquez sur gérer les dispositions. Dans la boîte de dialogue gérer les dispositions, vous trouverez la liste des mises en page existantes sur le côté gauche de la boîte de dialogue. Vous pouvez sélectionner la disposition pour modifier ses paramètres. Vous pouvez également modifier l’ordre des dispositions dans la liste ou supprimer la disposition à l’aide des boutons situés en haut de la liste. Sur le côté droit de la boîte de dialogue, vous pouvez modifier les paramètres de disposition suivants :  
  
-   Nom de la disposition  
  
-   Synchronisation des explorateurs de métadonnées  
  
-   Visibilité et largeur des explorateurs de métadonnées sources et cibles  
  
-   Visibilité des fenêtres source ou cible et de leur taille  
  
-   Visibilité et hauteur des fenêtres auxiliaires  
  
## <a name="bookmarks"></a>Signets  
Cette fonctionnalité vous permet de définir un ou plusieurs signets dans le code source ou cible, de trouver rapidement un signet à l’aide de raccourcis, de gérer les signets avec une boîte de dialogue conviviale.  
  
### <a name="toggle-bookmark"></a>Activer/Désactiver le signet  
Vous pouvez définir/supprimer un signet des manières suivantes :  
  
-   Bouton Activer/désactiver le signet par-dessus la fenêtre SQL source ou cible  
  
-   Cliquez sur la zone grise à gauche de la fenêtre SQL.  
  
-   Utilisez CTRL + MAJ + &lt; 0.. 9 &gt; pour définir le signet numéroté  
  
### <a name="bookmark-navigation"></a>Navigation de signet  
Vous pouvez parcourir les signets de l’une des manières suivantes :  
  
-   Utilisez les boutons signet suivant, signet précédent en haut de la fenêtre SQL  
  
-   Utilisez Ctrl + &lt; 0.. 9 &gt; pour rechercher le signet numéroté  
  
-   Utilisez les boutons atteindre ou afficher la source dans la boîte de dialogue gérer les signets  
  
### <a name="removing-bookmark"></a>Suppression du signet  
Vous pouvez supprimer un signet des manières suivantes :  
  
-   Utilisez le bouton Effacer en haut de la fenêtre SQL pour supprimer tous les signets du document actif  
  
-   Utilisez les boutons supprimer ou supprimer tout dans la boîte de dialogue gérer les signets  
  
### <a name="manage-bookmarks"></a>Gérer les signets  
Pour ouvrir la boîte de dialogue gérer les signets, dans le menu Edition, cliquez sur gérer les signets. Dans la boîte de dialogue, vous verrez une liste de signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.  
  
## <a name="object-history"></a>Historique des objets  
L’historique de l’objet GUI vous offre les avantages suivants lorsque vous parcourez des objets :  
  
-   Vous pouvez utiliser les boutons revenir en arrière et déplacer vers l’avant pour parcourir les objets que vous avez déjà visités.  
  
-   Lorsque vous revenez à l’objet, vous revenez à l’onglet que vous avez laissé  
  
-   Lorsque vous revenez à l’objet et que l’onglet est SQL, vous revenez à la position de curseur que vous avez quittée  
  
## <a name="advanced-search-capabilities"></a>Fonctions de recherche avancées  
Les fonctionnalités de recherche avancées offrent des fonctionnalités de recherche puissantes et flexibles et vous permettent de rechercher des déclarations d’objet, d’obtenir des informations sur les objets, d’effectuer une recherche rapide, d’effectuer une recherche d’objets avancée dans des catégories à l’aide de modèles, etc.  
  
### <a name="get-quick-information"></a>Recevoir des informations rapides  
Vous pouvez vous procurer des informations rapides sur l’objet à la position du curseur de l’une des manières suivantes :  
  
-   Cliquez sur le bouton Info Express en haut de la fenêtre SQL.  
  
-   Sélectionnez Info Express dans le menu contextuel de clic droit  
  
-   Appuyez sur Ctrl + Maj + espace  
  
### <a name="find-declaration"></a>Rechercher une déclaration  
Vous pouvez accéder à la déclaration de l’objet à la position du curseur de l’une des manières suivantes :  
  
-   Cliquez sur le bouton atteindre la déclaration en haut de la fenêtre SQL  
  
-   Sélectionnez atteindre la déclaration dans le menu contextuel de clic droit  
  
-   Appuyez sur F12  
  
### <a name="quick-search"></a>Recherche rapide  
Vous pouvez effectuer une recherche en texte rapide à l’aide des fonctionnalités suivantes :  
  
-   Vous pouvez commencer la recherche à l’aide du raccourci Ctrl + F  
  
-   Vous pouvez répéter la dernière recherche vers l’avant à l’aide de F3  
  
-   Vous pouvez répéter la dernière recherche vers l’arrière en utilisant Maj + F3  
  
-   Vous pouvez rechercher l’occurrence suivante du mot à la position du curseur à l’aide de CTRL + F3  
  
-   Vous pouvez rechercher l’occurrence précédente du mot à la position du curseur à l’aide de Ctrl + Maj + F3  
  
-   Vous pouvez également effectuer toutes ces actions avec les éléments de menu.  
  
### <a name="advanced-search"></a>Recherche avancée  
Pour ouvrir la boîte de dialogue Recherche avancée, dans le menu Edition, cliquez sur Rechercher, puis sur recherche avancée. Dans la boîte de dialogue, vous pouvez rechercher un objet à l’aide du modèle. En haut de la boîte de dialogue, vous pouvez choisir zone de recherche et catégories d’objet.  
  
