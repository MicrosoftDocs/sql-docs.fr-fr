---
title: Server, élément pour les configurations (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément Server pour les configurations contient les informations d’identification du serveur sur lequel vous souhaitez évaluer une configuration.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Server element
ms.assetid: da9ff870-9cfd-42fe-994b-7b9292681f7d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: b0aa1686a93cfac030785e7d7572eb2157d4d38f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354460"
---
# <a name="server-element-for-configuration-dta"></a>Server, élément pour les configurations (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contient les informations d’identification du serveur sur lequel vous souhaitez que l’Assistant Paramétrage du moteur de base de données évalue la configuration hypothétique (spécifiée par l’élément **Configuration** ).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<Configuration>  
    <Server>  
...code removed here...  
    </Server>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|Aucun.|  
|**Valeur par défaut**|Aucun.|  
|**Occurrence**|Obligatoire une fois par élément **Configuration** .|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|[Configuration, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/configuration-element-dta.md)|  
|**Éléments enfants**|[Name, élément pour les serveurs &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/name-element-for-server-dta.md)<br /><br /> [Database, élément pour les configurations &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/database-element-for-configuration-dta.md)|  
  
## <a name="remarks"></a>Notes  
 Vous ne pouvez spécifier qu’un seul élément **Server** pour l’élément **Configuration** . Cet élément porte le nom **ServerTypecomplexType** dans le [schéma XML de l’Assistant Paramétrage du moteur de base de données](https://go.microsoft.com/fwlink/?linkid=43100). Ne confondez pas cet élément **Server** avec l’élément enfant de l’élément **DTAInput** . Pour plus d’informations, consultez [Server, élément &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/server-element-dta.md).  
  
## <a name="example"></a>Exemple  
 Pour obtenir un exemple d’utilisation, consultez l’[Exemple de fichier d’entrée XML avec une configuration spécifiée par l’utilisateur &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
