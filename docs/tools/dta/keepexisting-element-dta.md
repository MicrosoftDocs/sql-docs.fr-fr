---
title: KeepExisting, élément (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément KeepExisting spécifie les structures PDS que l’Assistant Paramétrage du moteur de base de données conserve au moment de générer des suggestions.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- KeepExisting element
ms.assetid: e67aae61-d06d-4a03-85ba-6516c3502dce
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 829500f7335d1cb805b309ef642e6f70393b6912
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345901"
---
# <a name="keepexisting-element-dta"></a>KeepExisting, élément (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Spécifie les structures PDS (index, vues indexées ou partitions) que l'Assistant Paramétrage du moteur de base de données doit conserver lors de la génération de sa recommandation.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <KeepExisting>...</KeepExisting>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|**string**, limite de longueur appliquée par le serveur.|  
|**Valeurs autorisées**|**NONE**<br /> Aucune structure existante.<br /><br /> **ALL**<br /> Toutes les structures existantes.<br /><br /> **ALIGNED**<br /> Toutes les structures alignées sur les partitions.<br /><br /> **CL_IDX**<br /> Tous les index cluster sur les tables.<br /><br /> **IDX**<br /> Tous les index cluster et non cluster sur les tables.<br /><br /> Utilisez une seule de ces valeurs avec cet élément.|  
|**Valeur par défaut**|Aucun.|  
|**Occurrence**|facultatif. Ne peut être utilisé qu'une seule fois pour chaque élément **TuningOptions** .|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|[Élément TuningOptions &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)|  
|**Éléments enfants**|Aucun.|  
  
## <a name="example"></a>Exemple  
 Pour obtenir un exemple d’utilisation de cet élément, consultez [Exemple de fichier d’entrée XML simple &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/simple-xml-input-file-sample-dta.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
