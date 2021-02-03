---
description: SET FIPS_FLAGGER (Transact-SQL)
title: SET FIPS_FLAGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FIPS_FLAGGER
- SET_FIPS_FLAGGER_TSQL
- FIPS_FLAGGER_TSQL
- SET FIPS_FLAGGER
dev_langs:
- TSQL
helpviewer_keywords:
- SET FIPS_FLAGGER statement
- FIPS 127-2 standard
- FIPS_FLAGGER option
ms.assetid: e82f6bee-6cf6-4061-be22-9ad2e8e9d3d6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bb266f6aadef4690df7ebb7f16a17e1c5ae2ca05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190386"
---
# <a name="set-fips_flagger-transact-sql"></a>SET FIPS_FLAGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Spécifie la vérification de conformité à la norme FIPS 127-2. Ce contrôle est basé sur la norme ISO. Pour plus d’informations sur la conformité aux normes FIPS SQL Server, consultez [How to use SQL Server 2016 in FIPS 140-2-compliant mode (Guide pratique pour utiliser SQL Server 2016 en mode compatible avec FIPS 140-2)](https://support.microsoft.com/help/4014354/how-to-use-sql-server-2016-in-fips-140-2-compliant-mode). 
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SET FIPS_FLAGGER ( 'level' |  OFF )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **'** *level* **'**  
 Niveau de conformité à la norme FIPS 127-2, vérifié dans toutes les opérations effectuées dans les bases de données. Si une opération de base de données entre en conflit avec le niveau des normes ISO choisi, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un avertissement.  
  
 *level* doit avoir l’une des valeurs suivantes.  
  
|Value|Description|  
|-----------|-----------------|  
|ENTRY|Vérification des normes pour la conformité ISO de base.|  
|FULL|Vérification des normes pour la conformité ISO complète.|  
|INTERMEDIATE|Vérification des normes pour la conformité ISO de niveau intermédiaire.|  
|OFF|Pas de vérification des normes.|  
  
## <a name="remarks"></a>Remarques  
 L’option `SET FIPS_FLAGGER` est appliquée lors de l’analyse, et non lors de l’exécution. Par conséquent, si l’instruction SET est présente dans la procédure stockée ou le traitement d’instructions, elle devient effective, que l’exécution du code ait réellement atteint ou non ce point ; l’instruction `SET` devient effective avant l’exécution de toute autre instruction. Par exemple, même si le `SET` instruction se trouve dans un `IF...ELSE` bloc d’instructions qui n’est jamais atteint pendant l’exécution, le `SET` prend quand même effet parce que le `IF...ELSE` bloc d’instructions est analysé.  
  
 Si `SET FIPS_FLAGGER` est définie dans une procédure stockée, la valeur de `SET FIPS_FLAGGER` est restauré une fois le contrôle est retourné à partir de la procédure stockée. Par conséquent, une instruction dynamique `SET FIPS_FLAGGER` n’a aucun effet sur les instructions exécutées après celle-ci.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
