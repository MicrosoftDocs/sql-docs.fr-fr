---
description: Propriétés personnalisées ADO NET
title: Propriétés personnalisées ADO NET | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e062a9ab-1e6b-4061-845a-4f8a0552b09d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a44bc28763df210a16a7376408ff342b91ac6d9c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88495939"
---
# <a name="ado-net-custom-properties"></a>Propriétés personnalisées ADO NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  **Propriétés personnalisées des sources**  
  
 La source ADO NET comporte des propriétés personnalisées et les propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la source ADO NET. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|CommandTimeout|String|Valeur qui spécifie le nombre de secondes accordées comme délai d'exécution de la commande SQL. La valeur égale à 0 indique que la commande n'arrive jamais à expiration.|  
|SqlCommand|String|Instruction SQL que la source ADO NET utilise pour extraire des données.<br /><br /> Lorsque le package se charge, vous pouvez mettre à jour cette propriété de manière dynamique avec l'instruction SQL que la source ADO NET utilisera. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md) et [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md).|  
|AllowImplicitStringConversion|Booléen|Valeur qui indique si les cas de figure suivants se présentent :<br /><br /> -Il n’y a aucune génération d’erreur de validation s’il existe une discordance entre les types de métadonnées externes et les types de colonnes de sortie qui sont des chaînes (DT_WSTR ou DT_NTEXT).<br /><br /> -Il y a une conversion implicite des types de métadonnées externes vers le type de données String que la colonne de sortie utilise.<br /><br /> <br /><br /> La valeur par défaut est TRUE.<br /><br /> Pour plus d’informations, consultez [Source ADO NET](../../integration-services/data-flow/ado-net-source.md).|  
  
 La sortie et les colonnes de sortie de la source ADO NET ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, consultez [Source ADO NET](../../integration-services/data-flow/ado-net-source.md).  
  
 **Propriétés personnalisées des destinations**  
  
 La destination [!INCLUDE[vstecado](../../includes/vstecado-md.md)] dispose à la fois de propriétés personnalisées et des propriétés communes à tous les composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la destination [!INCLUDE[vstecado](../../includes/vstecado-md.md)] . Toutes les propriétés sont en lecture/écriture. Ces propriétés ne sont pas disponibles dans **l’Éditeur de destination ADO NET**mais peuvent être définies à l’aide de **l’Éditeur avancé**.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|BatchSize|Integer|Nombre de lignes d'un lot envoyé au serveur. Une valeur égale à **0** indique que la taille du lot correspond à la taille du tampon interne. La valeur par défaut de cette propriété est **0**.|  
|CommandTimeout|Integer|Nombre maximal de secondes pendant lesquelles la commande SQL peut être exécutée avant d'arriver à expiration. Une valeur égale à **0** indique une durée illimitée. La valeur par défaut de cette propriété est **0**.|  
|TableOrViewName|String|Nom de la table ou vue de destination.|  
  
 Pour plus d’informations, consultez [Destination ADO NET](../../integration-services/data-flow/ado-net-destination.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés communes](https://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
  
