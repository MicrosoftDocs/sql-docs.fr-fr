---
description: Source Azure Data Lake Store
title: Source Azure Data Lake Store | Microsoft Docs
ms.custom: ''
ms.date: 08/16/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSSRC.F1
- sql14.dts.designer.afpadlssrc.f1
ms.assetid: f9c3311f-7316-48d6-bf10-d810e70b4304
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 38de021617ed0454048a3d8cd7f0c5732d09861f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127335"
---
# <a name="azure-data-lake-store-source"></a>Source Azure Data Lake Store

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le composant **Source Azure Data Lake Store** permet à un package SSIS de lire des données dans Azure Data Lake Store. Les formats de fichier pris en charge sont le format texte et Avro.
  
 **Source Azure Data Lake Store** est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
> [!NOTE]
> Pour que le gestionnaire de connexions Azure Data Lake Store et les composants qui l’utilisent, notamment la source Azure Data Lake Store et la destination Azure Data Lake Store, puissent se connecter aux services, veillez à télécharger la dernière version du Feature Pack Azure [ici](https://www.microsoft.com/download/details.aspx?id=49492). 
  
## <a name="configure-the-azure-data-lake-store-source"></a>Configurer la source Azure Data Lake Store
 1. Pour afficher l’éditeur de la source Azure Data Lake Store, faites glisser **Source Azure Data Lake Store** sur le concepteur de flux de données et double-cliquez dessus pour ouvrir l’éditeur.  
  
2.  Dans le champ **Gestionnaire de connexions Azure Data Lake Store** , spécifiez un gestionnaire de connexions Azure Data Lake Store existant ou créez-en un qui fait référence à un service Azure Data Lake Store.  
  
    1.  Dans le champ **Chemin d’accès au fichier** , spécifiez le chemin du fichier source dans Azure Data Lake Store.   
  
    2.  Dans le champ **Format de fichier** , spécifiez le format du fichier source.  
  
        Si le format de fichier est le format texte, vous devez renseigner le champ **Délimiteur de colonne** . Sélectionnez également l’option **Noms de colonne dans la première ligne de données** si la première ligne du fichier contient des noms de colonne.  
  
3.  Après avoir spécifié les informations de connexion, basculez vers la page **Colonnes** pour mapper les colonnes sources sur les colonnes de destination du flux de données SSIS.   

## <a name="text-qualifier"></a>Qualificateur de texte

La **source Azure Data Lake Store** ne prend pas en charge les qualificateurs de texte. Si vous devez spécifier un qualificateur de texte pour traiter vos fichiers correctement, téléchargez les fichiers sur votre ordinateur local et traitez les fichiers avec la **Source du fichier plat**. La Source du fichier plat vous permet de spécifier un qualificateur de texte. Pour plus d’informations, consultez [Source du fichier plat](flat-file-source.md).
