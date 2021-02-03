---
description: CREATE WORKLOAD Classifier (Transact-SQL)
title: CREATE WORKLOAD CLASSIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/11/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- CREATE WORKLOAD CLASSIFIER
- CREATE_WORKLOAD_CLASSIFIER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD CLASSIFIER statement
ms.assetid: ''
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 5f592228f6570dfef5df91875ad60318806a1ad3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177861"
---
# <a name="create-workload-classifier-transact-sql"></a>CREATE WORKLOAD CLASSIFIER (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Crée un objet classifieur à utiliser dans la gestion des charges de travail.  Le classifieur affecte les requêtes entrantes à un groupe de charge de travail en fonction des paramètres spécifiés dans la définition d’instruction du classifieur.  Les classifieurs sont évalués avec chaque requête envoyée.  Si une requête ne correspond pas à un classifieur, elle est affectée au groupe de charge de travail par défaut.  Le groupe de charge de travail par défaut est la classe de ressources smallrc.

> [!NOTE]
> Le classifieur de charge de travail prend la place de l’affectation de la classe de ressources sp_addrolemember.  Une fois que vous avez créé les classifieurs de charge de travail, exécutez sp_droprolemember pour supprimer tous les mappages de classes de ressources redondants.

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE WORKLOAD CLASSIFIER classifier_name  
WITH  
    (   WORKLOAD_GROUP = 'name'  
    ,   MEMBERNAME = 'security_account' 
[ [ , ] WLM_LABEL = 'label' ]  
[ [ , ] WLM_CONTEXT = 'context' ]  
[ [ , ] START_TIME = 'HH:MM' ]  
[ [ , ] END_TIME = 'HH:MM' ]  
  
[ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]) 
[;]
```
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Arguments

 *classifier_name*  
 Spécifie le nom qui identifie le classifieur de charge de travail.  classifier_name est de type sysname.  Il peut comporter jusqu’à 128 caractères et doit être unique dans l’instance.

 *WORKLOAD_GROUP* =  *'name'*    
 Quand les conditions sont remplies par les règles du classifieur, le paramètre 'name' mappe la requête à un groupe de charge de travail.  name est de type sysname.  Il peut comporter jusqu’à 128 caractères et doit être un nom de groupe de charge de travail valide au moment de la création du classifieur.

 Les groupes de charge de travail disponibles se trouvent dans la vue de catalogue [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md).

 *MEMBERNAME* =  *’security_account’*     
 Compte de sécurité utilisé pour la classification.  security_account est de type sysname, sans valeur par défaut. security_account peut être un utilisateur de base de données, un rôle de base de données, une connexion Azure Active Directory ou un groupe Azure Active Directory.
 
 *WLM_LABEL*   
 Spécifie la valeur de l’étiquette utilisée pour classifier une requête.  L’étiquette est un paramètre facultatif de type nvarchar(255).  Ajoutez [OPTION (LABEL)](/azure/sql-data-warehouse/sql-data-warehouse-develop-label) dans la requête pour qu’elle corresponde à la configuration du classifieur.

Exemple :

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'  
 ,WLM_LABEL      = 'dimension_loads' )

SELECT COUNT(*) 
  FROM DimCustomer
  OPTION (LABEL = 'dimension_loads')
```

*WLM_CONTEXT*  
Spécifie la valeur du contexte de session utilisée pour classifier une requête.  Le contexte est un paramètre facultatif de type nvarchar(255).  Utilisez [sys.sp_set_session_context](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) avec le nom de variable `wlm_context` avant d’envoyer une requête pour définir le contexte de session.

Exemple :

```sql
CREATE WORKLOAD CLASSIFIER wcDataLoad WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'
 ,WLM_CONTEXT    = 'dim_load' )
 
--set session context
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = 'dim_load'

--run multiple statements using the wlm_context setting
SELECT COUNT(*) FROM stg.daily_customer_load
SELECT COUNT(*) FROM stg.daily_sales_load

--turn off the wlm_context session setting
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = null
```

*START_TIME* et *END_TIME*  
Spécifie les valeurs start_time et end_time utilisées pour classifier une requête.  Les deux valeurs start_time et end_time sont au format HH:MM dans le fuseau horaire UTC.  Elles doivent être spécifiées ensemble.

Exemple :

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoads'
 ,MEMBERNAME     = 'ELTRole'  
 ,START_TIME     = '22:00'
 ,END_TIME       = '02:00' )
```

*IMPORTANCE* = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
Spécifie l’importance relative d’une requête.  Elle prend l'une des valeurs suivantes :

- LOW
- BELOW_NORMAL
- NORMAL (valeur par défaut)
- ABOVE_NORMAL
- HIGH  

Si l’importance n’est pas spécifiée, le paramètre d’importance défini pour le groupe de charge de travail est appliqué.  L’importance par défaut du groupe de charge de travail est définie sur Normal.  L’importance impacte l’ordre dans lequel les requêtes sont planifiées, avec un accès prioritaire aux ressources et aux verrous.

## <a name="classification-parameter-weighting"></a>Pondération des paramètres de classification

Une requête peut être mise en correspondance avec plusieurs classifieurs.  Les paramètres de classifieur sont pondérés.  Le classifieur correspondant à la pondération la plus élevée est utilisé pour affecter un groupe de charge de travail et une importance.  La pondération se présente comme suit :

|Paramètre du classifieur |Poids   |
|---------------------|---------|
|Utilisateur                 |64       |
|ROLE                 |32       |
|WLM_LABEL            |16       |
|WLM_CONTEXT          |8        |
|START_TIME/END_TIME  |4        |

Examinez les configurations de classifieurs suivantes.

```sql
CREATE WORKLOAD CLASSIFIER classifierA WITH  
( WORKLOAD_GROUP = 'wgDashboards'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = HIGH
 ,WLM_LABEL      = 'salereport' )

CREATE WORKLOAD CLASSIFIER classifierB WITH  
( WORKLOAD_GROUP = 'wgUserQueries'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = LOW
 ,START_TIME     = '18:00'
 ,END_TIME       = '07:00' )
```

L’utilisateur `userloginA` est configuré pour les deux classifieurs.  Si userloginA exécute une requête ayant l’étiquette `salesreport` entre 18 h 00 et 7 h, la requête sera classifiée dans le groupe de charge de travail wgDashboards avec une importance élevée (HIGH).  Vous voudrez peut-être classifier cette requête dans le groupe wgUserQueries avec une faible importance (LOW) pour la création de rapports durant les heures creuses, mais la pondération de WLM_LABEL est supérieure à START_TIME/END_TIME.  La pondération de classifierA est de 80 (64 pour l’utilisateur, plus 16 pour WLM_LABEL).  La pondération de classifierB est de 68 (64 pour l’utilisateur, 4 pour START_TIME/END_TIME).  Dans ce cas, vous pouvez ajouter WLM_LABEL à classifierB.

 Pour plus d’informations, consultez [Pondération des charges de travail](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification#classification-weighting).

## <a name="permissions"></a>Autorisations

 Exige l’autorisation CONTROL DATABASE.  
  
## <a name="examples"></a>Exemples

 L'exemple suivant montre comment créer un classifieur de charge de travail appelé `wgcELTRole`. Il utilise le groupe de charge de travail staticrc20 et l’utilisateur `ELTRole`, et définit l’importance sur `above_normal`.

```sql
CREATE WORKLOAD CLASSIFIER wgcELTRole
  WITH (WORKLOAD_GROUP = 'staticrc20'
       ,MEMBERNAME = 'ELTRole'
      ,IMPORTANCE = above_normal);
```

## <a name="see-also"></a>Voir aussi

[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Classification](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)</br>
[DROP WORKLOAD CLASSIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-classifier-transact-sql.md)</br>
[sys.workload_management_workload_classifiers](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql.md)</br>
[sys.workload_management_workload_classifier_details](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql.md)

