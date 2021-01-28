---
description: Tâche de téléchargement d’objet blob Azure
title: Téléchargement d’objets blob Azure, tâche | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobdltask.f1
- sql14.dts.designer.afpblobdltask.f1
ms.assetid: 8a63bf44-71be-456d-9a5c-be7c31aff065
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 21ae3810f5f52ab8d77760cc08a887b74ab65977
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783084"
---
# <a name="azure-blob-download-task"></a>Tâche de téléchargement d’objet blob Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


La tâche de téléchargement d’objet blob Azure permet à un package SSIS de télécharger des fichiers à partir de Stockage Blob Azure.

Pour ajouter une **tâche de téléchargement d’objet blob Azure**, faites-la glisser sur le concepteur SSIS, double-cliquez dessus ou cliquez dessus avec le bouton droit, puis cliquez sur **Modifier** pour afficher la boîte de dialogue **Éditeur de la tâche de téléchargement d’objet blob Azure** .  
  
 La **tâche de téléchargement d’objets blob Azure** est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
 Le tableau suivant décrit les champs de cette boîte de dialogue.  

|**Champ**|**Description**|  
|---|---|
|AzureStorageConnection|Spécifiez un gestionnaire de connexions Azure Storage existant ou créez-en un qui fait référence à un compte Azure Storage et pointe vers l’emplacement où les fichiers d’objets blob sont hébergés.|  
|BlobContainer|Spécifie le nom du conteneur d’objets blob qui contient les fichiers d’objets blob à télécharger.|  
|BlobDirectory|Spécifie le répertoire d’objets blob qui contient les fichiers d’objets blob à télécharger. Le répertoire d’objet blob est une structure hiérarchique virtuelle.|  
|SearchRecursively|Spécifie si la recherche doit être récursive dans les sous-répertoires.|  
|LocalDirectory|Spécifie le répertoire local dans lequel les fichiers d’objets blob téléchargés seront stockés.|  
|FileName|Spécifie un filtre de nom pour sélectionner des fichiers obéissant à un schéma de nom spécifié. Par exemple, `MySheet*.xls\*` inclut des fichiers tels que `MySheet001.xls` et `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Spécifie une plage de temps pour appliquer un filtre. Les fichiers modifiés après **TimeRangeFrom** et avant **TimeRangeTo** sont inclus.|  
