---
description: Propriétés personnalisées de la destination SQL Server
title: Propriétés personnalisées de la destination SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: b736aa6d-c154-44a0-be08-f25733fca1d9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: de7b5eb166ab200a0a5217cb4c95eed7216520d5
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194759"
---
# <a name="sql-server-destination-custom-properties"></a>Propriétés personnalisées de la destination SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispose à la fois de propriétés personnalisées et des propriétés communes à tous les composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|AlwaysUseDefaultCodePage|Booléen|Impose l’utilisation de la valeur de propriété DefaultCodePage. La valeur par défaut de cette propriété est **False**.|  
|BulkInsertCheckConstraints|Booléen|Valeur qui spécifie si l'insertion en bloc vérifie les contraintes. La valeur par défaut de cette propriété est **True**.|  
|BulkInsertFireTriggers|Booléen|Valeur qui spécifie si l'insertion en bloc exécute des déclencheurs dans les tables. La valeur par défaut de cette propriété est **False**.|  
|BulkInsertFirstRow|Entier|Valeur qui spécifie la première ligne à insérer. La valeur par défaut de cette propriété est **-1**, ce qui signifie qu’aucune valeur n’a été attribuée|  
|BulkInsertKeepIdentity|Booléen|Valeur qui spécifie si les valeurs peuvent être insérées dans des colonnes d'identité. La valeur par défaut de cette propriété est **False**.|  
|BulkInsertKeepNulls|Booléen|Valeur qui spécifie si l'insertion en bloc conserve les valeurs NULL. La valeur par défaut de cette propriété est **False**.|  
|BulkInsertLastRow|Entier|Valeur qui spécifie la dernière ligne à insérer. La valeur par défaut de cette propriété est **-1**, ce qui signifie qu’aucune valeur n’a été attribuée.|  
|BulkInsertMaxErrors|Entier|Valeur qui spécifie le nombre d'erreurs au-delà duquel l'insertion en bloc s'arrête. La valeur par défaut de cette propriété est **-1**, ce qui signifie qu’aucune valeur n’a été attribuée.|  
|BulkInsertOrder|Chaîne|Noms des colonnes de tri. Chaque colonne peut être triée par ordre croissant ou décroissant. En cas d'utilisation de plusieurs colonnes de tri, les noms des colonnes sont séparés par des virgules.|  
|BulkInsertTableName|Chaîne|Table ou vue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans la base de données dans laquelle les données sont copiées.|  
|BulkInsertTablock|Booléen|Valeur qui spécifie si la table est verrouillée lors de l'insertion en bloc. La valeur par défaut de cette propriété est **True**.|  
|DefaultCodePage|Integer|Page de codes à utiliser lorsque les informations de page de codes ne sont pas disponibles à partir de la source de données.|  
|MaxInsertCommitSize|Entier|Valeur qui spécifie le nombre maximal de lignes à insérer dans un lot. Lorsque la valeur est nulle, toutes les lignes sont insérées dans un lot unique.|  
|Délai d'expiration|Entier|Valeur qui spécifie le nombre de secondes pendant lesquelles la destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] patiente avant de s'arrêter si aucune donnée disponible ne peut être insérée. Une valeur égale à 0 signifie que la destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’expire pas. La valeur par défaut de cette propriété est 30.|  
  
 Les entrées et les colonnes d’entrée de la destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’ont pas de propriétés personnalisées.  
  
 Pour plus d’informations, consultez [Destination SQL Server](../../integration-services/data-flow/sql-server-destination.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés communes](./set-the-properties-of-a-data-flow-component.md)  
  
