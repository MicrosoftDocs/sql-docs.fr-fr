---
description: Paramètres globaux (Journalisation) (OracleToSQL)
title: Paramètres globaux (journalisation) (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 12dbcd77-2b90-4fa1-9cf9-239231ea5773
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: fbc74e9129c642ea1b6655deb4e01ee04d92d0ee
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88480501"
---
# <a name="global-settings-logging-oracletosql"></a>Paramètres globaux (Journalisation) (OracleToSQL)
Utilisez la boîte de dialogue **paramètres globaux** pour spécifier les paramètres de journalisation pour SSMA. En règle générale, vous pouvez modifier ces paramètres uniquement lorsque vous travaillez avec le support technique.  
  
Pour accéder à cette boîte de dialogue, dans le menu **Outils** , sélectionnez **paramètres globaux** , puis cliquez sur le bouton **journalisation** en bas du volet gauche.  
  
## <a name="options"></a>Options  
**Niveau de messages**  
Les options suivantes sont disponibles sous **niveau des messages**:  
  
|Option|Description|  
|----------|---------------|  
|**[toutes les catégories]**|Utilisé pour définir le niveau de journalisation pour toutes les options suivantes.|  
|**Collecteur**|Collecte les métadonnées relatives au schéma source et les enregistre dans le projet.|  
|**Converter**|Convertit les structures des objets de base de données sources, tels que les tables et les procédures stockées, en structures correspondantes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Data Migrator**|Migre les données de la base de données source vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Formateur**|Sous-composant du convertisseur qui génère des scripts pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schéma.|  
|**Interface utilisateur graphique**|Messages qui s’affichent lorsque vous utilisez l’outil SSMA.|  
|**Éditeur de liens**|Résout les identificateurs SQL et fournit des informations à d’autres composants.|  
|**Autres**|Tous les messages qui ne se trouvent pas dans une autre catégorie.|  
|**Analyseur**|Analyse le schéma source.|  
|**Synchronisateur**|Charge les objets de base de données source dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**TreeConverter**|Convertit des objets dans les métadonnées sources en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] métadonnées.|  
|**Testeur**|Messages qui s’affichent lorsque vous utilisez le testeur SSMA.|  
  
Pour chaque option sous **niveau de messages**, configurez l’un des niveaux de journalisation suivants pour SSMA :  
  
|||  
|-|-|  
|**Erreur irrécupérable**|Écrit uniquement les messages d’erreur fatale dans le journal.|  
|**Error**|Erreur d’écriture et messages d’erreur irrécupérable dans le journal.|  
|**Avertissement**|Écrire les messages d’avertissement, d’erreur et d’erreur irrécupérable dans le journal.|  
|**Info**|Écrire les messages d’information, d’avertissement, d’erreur et d’erreur irrécupérable dans le journal.|  
|**Déboguer**|Écrit tous les messages, y compris les messages de débogage, dans le journal.|  
  
**Chemin du fichier journal**  
Chemin d’accès et nom de fichier des fichiers journaux SSMA. Pour spécifier un autre nom, cliquez sur le chemin d’accès actuel, puis cliquez sur le bouton de navigation (**...**).  
  
**Taille du fichier journal**  
Taille maximale du fichier journal, en Ko. La taille minimale est de 10 Ko. La taille par défaut est de 10240 Ko.  
  
**Nombre total de fichiers journaux**  
Quand un journal est rempli, SSMA renomme le fichier journal et en démarre un nouveau. En utilisant ce paramètre, spécifiez le nombre maximal de fichiers journaux à conserver. La valeur minimale est 2.  
  
