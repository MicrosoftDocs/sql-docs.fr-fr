---
description: DROP WORKLOAD Classifier (Transact-SQL)
title: DROP WORKLOAD CLASSIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- DROP_WORKLOAD_CLASSIFIER_TSQL
- DROP WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DROP WORKLOAD CLASSIFIER statement
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 2509ee07decbfcc9ca97b247b78191fead413c6b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186443"
---
# <a name="drop-workload-classifier-transact-sql"></a>DROP WORKLOAD CLASSIFIER (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Supprime un classifieur de gestion des charges de travail défini par l’utilisateur existant.  Si des requêtes sont en cours d’exécution ou se trouvent dans la file d’attente des requêtes dans l’état suspendu, elles conserveront leur classification et le classifieur peut être supprimé immédiatement. La suppression et la recréation du classifieur avec une importance différente n’affectent pas une requête déjà classifiée.
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe

```syntaxsql
DROP WORKLOAD CLASSIFIER classifier_name;
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Arguments

*classifier_name*  
Spécifie le nom qui identifie le classifieur de charge de travail.
  
## <a name="permissions"></a>Autorisations

Exige l’autorisation CONTROL DATABASE.  
  
## <a name="examples"></a>Exemples

L’exemple suivant supprime le classifieur de charge de travail nommé `wgcELTROLE`.  

```sql
DROP WORKLOAD CLASSIFIER wgcELTRole;
```

> [!NOTE]
> Une requête envoyée sans classifieur correspondant est classifiée dans le groupe de charge de travail par défaut.  Le groupe de charge de travail par défaut est la classe de ressources smallrc.
  
## <a name="see-also"></a>Voir aussi

[CREATE WORKLOAD CLASSIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-classifier-transact-sql.md)</br>
[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Classification de charges de travail](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)
