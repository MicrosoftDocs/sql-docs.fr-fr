---
title: DTAXML, élément (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément DTAXML contient tous les éléments qui décrivent les entrées et sorties de paramétrage générées par l’Assistant Paramétrage du moteur de base de données.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DTAXML element
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 9224b5ef28cd891e0dac3d5fdeb60c210f02cd71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338714"
---
# <a name="dtaxml-element-dta"></a>DTAXML, élément (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Élément racine d'un fichier d'entrée ou de sortie de l'Assistant Paramétrage du moteur de base de données, **DTAXML** contient tous les éléments qui décrivent les entrées et les sorties de paramétrage générées par l'Assistant Paramétrage du moteur de base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<DTAXML   
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/dta">  
    ...code removed here...  
</DTAXML>  
```  
  
## <a name="element-attributes"></a>Attributs des éléments  
  
|Attribut|Description|  
|---------------|-----------------|  
|**xmlns:xsi**|Obligatoire. Identifie l'espace de noms de l'instance du schéma XML. Les attributs de cet espace de noms servent à référencer le schéma qui est utilisé pour valider le fichier XML de l'Assistant Paramétrage du moteur de base de données.<br /><br /> Valeur requise : [http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)|  
|**xmlns**|Obligatoire. Identifie l'espace de noms de l'Assistant Paramétrage du moteur de base de données.<br /><br /> Si vous modifiez le fichier XML de l'Assistant Paramétrage du moteur de base de données à l'aide de l'éditeur XML dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cette valeur est utilisée par l'aide obtenue par la touche F1 et l'aide dynamique pour rechercher les rubriques de référence dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Valeur obligatoire :<br /><br /> [Database Engine Tuning Advisor XML Schema](https://go.microsoft.com/fwlink/?LinkId=43100) Namespace|  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|Aucun.|  
|**Valeur par défaut**|Aucun.|  
|**Occurrence**|Obligatoire une fois par fichier XML de l'Assistant Paramétrage du moteur de base de données.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|None|  
|**Éléments enfants**|[DTAInput, élément &#40;DTA&#41;](../../tools/dta/dtainput-element-dta.md)<br /><br /> **DTAOutput**, élément (consultez l’article [Database Engine Tuning Advisor XML schema](https://schemas.microsoft.com/sqlserver/) pour plus d’informations)|  
  
## <a name="remarks"></a>Notes  
 Pour plus d’informations sur les espaces de noms XML, consultez [Espaces de noms dans un document XML](/dotnet/standard/data/xml/managing-namespaces-in-an-xml-document). 
  
## <a name="example"></a>Exemple  
 Pour obtenir des exemples d’éléments **DTAXML** caractéristiques, consultez [Exemples de fichiers d’entrée XML &#40;DTA&#41;](../../tools/dta/xml-input-file-samples-dta.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)   
 [Démarrer et utiliser l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)  
  
