---
title: Table, élément pour les schémas (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément Table pour les schémas spécifie la table pour le paramétrage. Cet article décrit cet élément.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Table element [DTA]
ms.assetid: a59e8319-05d1-47f3-af39-7d970ab8e7dc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: f4fb23f38cbcec741d2026aec8856184aff50786
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85732023"
---
# <a name="table-element-for-schema-dta"></a>Table, élément pour les schémas (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Spécifie la table pour le paramétrage.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<Schema>  
...code removed here...  
    <Table>...</Table>  
```  
  
## <a name="element-attributes"></a>Attributs des éléments  
  
|Attribut|Description|  
|---------------|-----------------|  
|**NumberOfRows**|facultatif. Entier qui vous permet de simuler des tables de différentes tailles.|  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|**string**, entre 1 et 255 caractères.|  
|**Valeur par défaut**|Aucun.|  
|**Occurrence**|facultatif. Répertoriez autant de tables que nécessaire pour votre charge de travail.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|[Schema, élément pour les bases de données &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/schema-element-for-database-dta.md)|  
|**Éléments enfants**|[Name, élément pour les tables &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-table-dta.md)|  
  
## <a name="remarks"></a>Notes  
 Si vous ne spécifiez pas d'élément **Table** , l'Assistant Paramétrage du moteur de base de données suppose que toutes les tables sur la base de données spécifiée peuvent être paramétrées.  
  
## <a name="example"></a>Exemple  
 Pour obtenir un exemple d’utilisation, consultez [Server, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/server-element-dta.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
