---
description: Azure Blob Source
title: Source des objets blob Azure | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobsrc.f1
- sql14.dts.designer.afpblobsrc.f1
ms.assetid: 80645c5c-88c8-4fb0-8607-de1bb7bffcbb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ce39fed32923ae46bd499c32d5b58660db5dcd8b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88457481"
---
# <a name="azure-blob-source"></a>Azure Blob Source

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le composant **Azure Blob Source** permet à un package SSIS de lire les données provenant d’un blob Azure. Les formats de fichier pris en charge sont CSV et AVRO.
  
  Pour afficher l’éditeur d’Azure Bloc Source, faites glisser **Azure Blob Source** sur le concepteur de flux de données et double-cliquez dessus pour ouvrir l’éditeur.  
  
 **Source des objets blob Azure** est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
1.  Dans le champ **Gestionnaire de connexions du Stockage Azure**, spécifiez un gestionnaire de connexions du Stockage Azure existant ou créez-en un qui fera référence à un compte de Stockage Azure.  
  
2.  Dans le champ **Nom du conteneur d’objets blob** , spécifiez le nom du conteneur d’objets blob qui contient les fichiers sources.  
  
3.  Dans le champ **Nom de l’objet blob** , indiquez le chemin d’accès à l’objet blob.  
  
4.  Dans le champ **Format de fichier blob**, sélectionnez le format blob à utiliser : **Texte** ou **Avro**.  
  
5.  Si le format de fichier est **Texte**, vous devrez renseigner la valeur **Délimiteur de colonne**. (Les délimiteurs multicaractères ne sont pas pris en charge.)

    Sélectionnez également l’option **Noms de colonne dans la première ligne de données** si la première ligne du fichier contient des noms de colonne.

6.  Si le fichier est compressé, sélectionnez **Décompresser le fichier**.

7.  Si le fichier est compressé, sélectionnez le **Type de compression** : **GZIP**, **DEFLATE** ou **BZIP2**. Notez que le format ZIP n’est pas pris en charge.
  
8.  Après avoir spécifié les informations de connexion, basculez vers la page **Colonnes** pour mapper les colonnes sources sur les colonnes de destination du flux de données SSIS.  
  
  
