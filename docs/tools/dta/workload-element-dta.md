---
title: Workload, élément (Assistant Paramétrage de base de données)
description: Dans l’utilitaire dta, l’élément Workload spécifie la charge de travail à utiliser pour une session de paramétrage. Cet article décrit cet élément.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Workload element
ms.assetid: 68ffd473-6546-4015-98d0-3763165de65c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 60e673cf32849203c921afa41fa4af6df8f2023e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349486"
---
# <a name="workload-element-dta"></a>Workload, élément (Assistant Paramétrage de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Spécifie la charge de travail à utiliser pour une session de paramétrage.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<DTAInput>  
    <Server>  
...code removed...  
    <Workload>...</Workload>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|Aucun.|  
|**Valeur par défaut**|Aucun.|  
|**Occurrence**|Obligatoire une fois par élément **DTAInput** .|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|[Démarrer et utiliser l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)|  
|**Éléments enfants**|[Élément File &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/file-element-dta.md)<br /><br /> [Élément Database &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/database-element-for-workload-dta.md)<br /><br /> [Élément EventString &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/eventstring-element-dta.md)|  
  
## <a name="remarks"></a>Notes  
 Une charge de travail est un ensemble d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui s'exécute sur une ou plusieurs bases de données que vous souhaitez paramétrer. L'Assistant Paramétrage du moteur de base de données peut utiliser des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] , des fichiers de trace et des tables de trace en tant que charges de travail.  
  
 Si vous spécifiez une charge de travail dans un fichier d'entrée XML et une charge de travail dans une ligne de commande avec l'outil **dta** , la charge de travail spécifiée dans la ligne de commande est utilisée pour le paramétrage. Toutes les options de paramétrage de la ligne de commande remplacent celles spécifiées dans un fichier d'entrée XML. Une seule exception : configuration spécifiée par l'utilisateur entrée dans le mode évaluation dans le fichier d'entrée XML. Par exemple, si une configuration est entrée dans l'élément **Configuration** du fichier d'entrée XML et que l'élément **EvaluateConfiguration** est également configuré en tant qu'une des options de paramétrage, les options de paramétrage spécifiées dans le fichier d'entrée remplacent toute option de paramétrage saisie dans la ligne de commande.  
  
 Une charge de travail doit être spécifiée pour chaque session de paramétrage.  
  
## <a name="example"></a>Exemple  
 L'exemple de code suivant spécifie la table de trace **MyDatabase.MyDBOwner.TuningTable001** pour l'élément **Workload** . La table **TuningTable001** a été créée à l'aide du modèle de paramétrage (Tuning) utilisé dans le Générateur de profils SQL Server et dont le résultat de trace a été enregistré comme table.  
  
```  
<DTAXML ...>  
  <DTAInput>  
    <Server>  
...code removed here...  
    </Server>  
    <Workload>  
      <Database>  
        <Name>MyDatabase</Name>  
        <Schema>  
          <Name>MyDBOwner</Name>  
            <Table>  
              <Name>TuningTable001</Name>  
            </Table>  
        </Schema>  
      </Database>  
    </Workload>  
...code removed here...  
  </DTAInput>  
</DTAXML>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
