---
title: Élément Database pour les serveurs (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément Database pour les serveurs spécifie la base de données que vous voulez paramétrer sur un serveur spécifique.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Database element
ms.assetid: 5cd9a87a-af4b-45f3-8c18-f7fd7e7d3064
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 02696a87796ae95743c19261bc50ed36e2577c7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342025"
---
# <a name="database-element-for-server-dta"></a>Élément Database pour les serveurs (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Spécifie la base de données que vous souhaitez paramétrer sur un serveur spécifique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<Server>  
...code removed here...  
    <Database>...</Database>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Aucun.|  
|Valeur par défaut|Aucun.|  
|Occurrence|Obligatoire une ou plusieurs fois par élément **Server** .|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|Élément parent|[Server, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/server-element-dta.md)|  
|Éléments enfants|[Name, élément pour les bases de données &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-database-dta.md)<br /><br /> [Schema, élément pour les bases de données &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/schema-element-for-database-dta.md)|  
  
## <a name="remarks"></a>Notes  
 Cet élément porte le nom **DatabaseDetailsTypecomplexType** dans le schéma XML de l’Assistant Paramétrage du moteur de base de données. Ne confondez pas cet élément **Database** avec celui dont le parent racine est l’élément **Configuration** . Pour plus d’informations, consultez [Database, élément pour les configurations &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/database-element-for-configuration-dta.md).  
  
## <a name="example"></a>Exemple  
 Pour obtenir un exemple d’utilisation de l’élément **Database** , consultez [Server, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/server-element-dta.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
