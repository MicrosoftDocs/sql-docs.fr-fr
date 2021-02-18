---
title: Gérer les signets
description: La fenêtre Signets d’un éditeur de code vous permet de créer des liens vers certains emplacements dans le code. Découvrez comment créer, supprimer, activer et désactiver des signets, et comment les utiliser pour naviguer dans votre code.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- bookmarks [SQL Server Management Studio]
ms.assetid: 67cc3fd6-3238-4c58-a3ec-2d3b0438143a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3828647bddd0755a5f537fe0158d07a67d3c13d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271726"
---
# <a name="manage-bookmarks"></a>Gérer les signets

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Lorsque vous travaillez dans un éditeur de code, la fenêtre **Signets** vous permet de créer des liens vers des lignes de code spécifiques au sein de votre document. Vous pouvez afficher cette fenêtre à partir du menu **Affichage** .  
  
 Pour créer des signets et naviguer parmi ceux-ci, cliquez sur les boutons situés dans la barre d’outils de l’ **Éditeur de texte** et en haut de la fenêtre **Signets** . Vous pouvez ajouter et supprimer des signets, activer ou désactiver des signets, ainsi qu'organiser les signets dans des dossiers. Certaines commandes sont également disponibles à partir du menu contextuel de la fenêtre **Signets** . Pour ajouter ou supprimer un signet, placez le point d’insertion sur la ligne de votre choix dans l’Éditeur, puis cliquez sur **Activer/Désactiver le signet**. Pour activer un signet, cochez la case correspondante dans la fenêtre **Signets** ; pour désactiver (sans supprimer) un signet, décochez sa case.  
  
## <a name="text-editor-toolbar"></a>Barre d'outils Éditeur de texte  
 Les boutons ci-dessous sont disponibles dans la barre d’outils **Éditeur de texte** quand un document texte est ouvert dans l’éditeur. Pour afficher la barre d’outils **Éditeur de texte** dans l’éditeur de requête, dans le menu **Affichage** , pointez sur **Barres d’outils**, puis cliquez sur **Éditeur de texte**.  
  
 **Activer/Désactiver un signet sur la ligne active**  
 Ajoute ou supprime un signet sur la ligne sélectionnée du document dans l'éditeur actif. Cela ne modifie pas la ligne de code contenant un signet.  
  
 **Placer le signe insertion sur le signet précédent**  
 Sélectionne le signet précédent, activé dans la fenêtre **Signets** . Lorsque le premier signet est atteint, le signe insertion passe au dernier signet. Si nécessaire, ouvre le fichier où le signet sélectionné apparaît dans l'éditeur. Fait défiler ce document jusqu'à la ligne marquée par un signet et y place le point d'insertion.  
  
 **Placer le signe insertion sur le signet suivant**  
 Sélectionne le signet suivant, activé dans la fenêtre **Signets** . Lorsque le dernier signet est atteint, le signe insertion passe au premier signet. Si nécessaire, ouvre le fichier où le signet sélectionné apparaît dans l'éditeur. Fait défiler ce document jusqu'à la ligne marquée par un signet et y place le point d'insertion.  
  
 **Efface tous les signets du document actif**  
 Affiche un message de confirmation, puis supprime tous les signets du document actif. Ne supprime pas les lignes de code qui contenaient des signets.  
  
> [!CAUTION]  
>  Vous ne pouvez pas annuler cette procédure. Ensuite, vous devez utiliser **Activer/Désactiver un signet sur la ligne active** pour créer de nouveaux signets. Pour désactiver des signets sans les supprimer, décochez les cases correspondantes dans la fenêtre **Signets** .  
  
## <a name="bookmarks-window"></a>Fenêtre Signets  
 Pour organiser des signets, créez des dossiers de signets dans la fenêtre **Signets** . Faites glisser les signets dans les dossiers. Les boutons ci-dessous sont disponibles en haut de la fenêtre **Signets** .  
  
 **Activer/Désactiver un signet sur la ligne active**  
 Ajoute ou supprime un signet sur la ligne sélectionnée du document dans l'éditeur actif. Cela ne modifie pas la ligne de code contenant un signet.  
  
 **Nouveau dossier**  
 Ajoute un dossier dans la fenêtre **Signets** .  
  
> [!TIP]  
>  Dans un fichier de code volumineux, il peut être utile d'organiser les signets en dossiers relatifs aux travaux. Sélectionner un dossier rend actif les boutons **Déplacer le signe d’insertion vers le signet précédent du dossier actif** et **Déplacer le signe d’insertion vers le signet suivant du dossier actif** .  
  
 **Placer le signe insertion sur le signet précédent**  
 Sélectionne le signet précédent, activé dans la fenêtre **Signets** . Lorsque le premier signet est atteint, le signe insertion passe au dernier signet. Si nécessaire, ouvre le fichier où le signet sélectionné apparaît dans l'éditeur. Fait défiler ce document jusqu'à la ligne marquée par un signet et y place le point d'insertion.  
  
 **Placer le signe insertion sur le signet suivant**  
 Sélectionne le signet suivant, activé dans la fenêtre **Signets** . Lorsque le dernier signet est atteint, le signe insertion passe au premier signet. Si nécessaire, ouvre le fichier où le signet sélectionné apparaît dans l'éditeur. Fait défiler ce document jusqu'à la ligne marquée par un signet et y place le point d'insertion.  
  
 **Déplacer le signe insertion vers le signet précédent du dossier actif**  
 Sélectionne le signet précédent, activé au sein du même dossier dans la fenêtre **Signets** . Lorsque le premier signet est atteint, le signe insertion passe au dernier signet dans le dossier. Si nécessaire, ouvre le fichier où le signet sélectionné apparaît dans l'éditeur. Fait défiler ce document jusqu'à la ligne marquée par un signet et y place le point d'insertion.  
  
 **Déplacer le signe insertion vers le signet suivant du dossier actif**  
 Sélectionne le signet suivant, activé au sein du même dossier dans la fenêtre **Signets** . Lorsque le dernier signet est atteint, le signe insertion passe au premier signet dans le dossier. Si nécessaire, ouvre le fichier où le signet sélectionné apparaît dans l'éditeur. Fait défiler ce document jusqu'à la ligne marquée par un signet et y place le point d'insertion.  
  
 **Désactiver/Activer tous les signets**  
 Coche ou décoche les cases de tous les signets dans la fenêtre **Signets** . Ne supprime pas les signets et ne modifie pas les lignes de code qu'ils marquent.  
  
 **Supprimer**  
 Supprime le signet actuellement sélectionné de la fenêtre **Signets** et du document où le signet est apparu. Ne supprime pas la ligne de code qui contenait le signet.  
  
 Cases à cocher des signets  
 Chaque signet a sa propre case à cocher. Pour activer un signet existant, cochez sa case dans la fenêtre **Signets** . Pour masquer (sans supprimer) un signet existant, décochez sa case dans la fenêtre **Signets** .  
  
## <a name="bookmarks-window-shortcut-menu"></a>Menu contextuel de la fenêtre Signets  
 Lorsque vous cliquez avec le bouton droit sur une entrée dans la fenêtre **Signets** , les commandes ci-dessous sont disponibles dans le menu contextuel.  
  
 **Supprimer**  
 Supprime le signet actuellement sélectionné de la fenêtre **Signets** et du document où le signet est apparu. Ne supprime pas la ligne de code qui contenait le signet.  
  
 **Renommer**  
 Vous permet d'affecter un nouveau nom complet à un signet ou un dossier.  
  
 **Désactiver/Activer le signet**  
 Coche ou décoche la case du signet sélectionné dans la fenêtre **Signets** . Ne supprime pas le signet et ne modifie pas la ligne de code qu'il marque.  
  
 **Désactiver/Activer tous les signets**  
 Coche ou décoche les cases de tous les signets dans la fenêtre **Signets** . Ne supprime pas les signets et ne modifie pas les lignes de code qu'ils marquent.  
  
## <a name="see-also"></a>Voir aussi  
 [Raccourcis clavier dans SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)  
