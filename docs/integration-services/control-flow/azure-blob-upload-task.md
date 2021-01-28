---
description: Tâche de chargement d’objets blob Azure
title: Chargement d’objets blob Azure, tâche | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobuptask.f1
- sql14.dts.designer.afpblobuptask.f1
ms.assetid: 6ea068b0-4cd8-45b5-b89d-09b8f25040c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 588bb693d5bcf8561d7f697dc910c483d63160a7
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783076"
---
# <a name="azure-blob-upload-task"></a>Tâche de chargement d’objets blob Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


La **tâche de chargement d’objets blob Azure** permet à un package SSIS de charger des fichiers sur Stockage Blob Azure.
    
Pour ajouter une **tâche de chargement d’objets blob Azure**, faites-la glisser vers le concepteur SSIS, double-cliquez dessus ou cliquez dessus avec le bouton droit, puis cliquez sur **Modifier** pour afficher la boîte de dialogue de **l’Éditeur de tâche de chargement d’objets blob Azure** .  
  
 La **tâche de chargement d’objets blob Azure** est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
  
 Le tableau suivant décrit les champs de cette boîte de dialogue.  

|**Champ**|**Description**|  
|---|---|  
|AzureStorageConnection|Spécifiez un gestionnaire de connexions Azure Storage existant ou créez-en un qui fait référence à un compte Azure Storage et pointe vers l’emplacement où les fichiers d’objets blob sont hébergés.|  
|BlobContainer|Spécifie le nom du conteneur d’objets blob qui contient les fichiers chargés en tant qu’objets blob.|  
|BlobDirectory|Spécifie le répertoire d’objets blob dans lequel le fichier chargé est stocké en tant qu’objet blob de bloc. Le répertoire d’objet blob est une structure hiérarchique virtuelle. Si l’objet blob existe déjà, il est remplacé.|  
|LocalDirectory|Spécifie le répertoire local qui contient les fichiers à charger.|  
|SearchRecursively|Spécifiez s’il faut rechercher de manière récursive dans les sous-répertoires.|  
|FileName|Spécifie un filtre de nom pour sélectionner des fichiers obéissant à un schéma de nom spécifié. Par exemple, `MySheet*.xls\*` inclut des fichiers tels que `MySheet001.xls` et `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Spécifie une plage de temps pour appliquer un filtre. Les fichiers modifiés après **TimeRangeFrom** et avant **TimeRangeTo** sont inclus.|  
