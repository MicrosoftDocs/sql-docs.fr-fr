---
title: Filegroup, élément pour les index (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément Filegroup pour les index spécifie le groupe de fichiers sur lequel l’index doit être créé pour une configuration spécifiée par l’utilisateur.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Filegroup element [DTA]
ms.assetid: 7078d2fb-fa77-44fc-beb3-c095088fcb85
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 8c7c2e6dbecbe46232f2e194821ab1fc0e4937f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335963"
---
# <a name="filegroup-element-for-index-dta"></a>Filegroup, élément pour les index (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Spécifie le groupe de fichiers sur lequel l'index doit être créé pour une configuration spécifiée par l'utilisateur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<Index>  
  <Name>...</Name>  
  <Column>  
    <Name>...</Name>  
  <Filegroup>...</Filegroup>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|**string**, longueur illimitée.|  
|**Valeur par défaut**|Aucun.|  
|**Occurrence**|facultatif. Peut être utilisé une seule fois pour chaque élément **Index** . Cet élément ne peut pas être utilisé si les éléments **PartitionScheme** et **PartitionColumn** sont spécifiés pour l'élément **Index** .|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|[Index, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/index-element-dta.md)|  
|**Éléments enfants**|Aucun.|  
  
## <a name="example"></a>Exemple  
 Pour obtenir un exemple d’utilisation de cet élément, consultez l’[Exemple de fichier d’entrée XML avec une configuration spécifiée par l’utilisateur &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
