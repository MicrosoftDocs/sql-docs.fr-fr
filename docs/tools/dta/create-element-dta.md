---
title: Create, élément (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément Create contient des informations sur les index, les statistiques ou les structures de tas d’une configuration spécifiée par l’utilisateur.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Create element (DTA)
ms.assetid: 9d076c90-f933-45f4-b6d9-447793f6528b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 281fd59192deeedbc32d7e543fdb95875929801d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342082"
---
# <a name="create-element-dta"></a>Create, élément (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contient des informations sur les index, les statistiques ou les structures de segments d'une configuration spécifiée par l'utilisateur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<Recommendation>  
    <Create>  
    ...code removed here...  
    </Create>  
</Recommendation>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|Aucun.|  
|**Valeur par défaut**|Aucun.|  
|**Occurrence**|Obligatoire une fois pour chaque type de structure PDS (index, statistiques ou structures de segments).|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|[Recommendation, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/recommendation-element-dta.md)|  
|**Éléments enfants**|[Index, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/index-element-dta.md)<br /><br /> Élément **Statistics** (pour plus d’informations, consultez [Database Engine Tuning Advisor XML schema](https://schemas.microsoft.com/sqlserver/) (Schéma XML de l’Assistant Paramétrage du moteur de base de données))<br /><br /> Élément **Heap** (pour plus d’informations, consultez [Database Engine Tuning Advisor XML schema](https://schemas.microsoft.com/sqlserver/) (Schéma XML de l’Assistant Paramétrage du moteur de base de données))|  
  
## <a name="remarks"></a>Notes  
 Cet élément porte le nom **CreateTypecomplexType** dans le schéma XML de l’Assistant Paramétrage du moteur de base de données. Il est utilisé pour créer des index, des statistiques ou des structures de segments pour une configuration spécifiée par l'utilisateur. Ne confondez pas cet élément **Create** avec les autres types d’éléments qui peuvent être utilisés pour créer des vues (**CreateViewType**) ou des partitions (**CreatePType**). Pour plus d’informations sur les autres types d’éléments [Create](https://schemas.microsoft.com/sqlserver/) , consultez **Database Engine Tuning Advisor XML schema** (Schéma XML de l’Assistant Paramétrage du moteur de base de données).  
  
## <a name="example"></a>Exemple  
 Pour obtenir un exemple d’utilisation de cet élément, consultez l’[Exemple de fichier d’entrée XML avec une configuration spécifiée par l’utilisateur &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
