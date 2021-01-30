---
description: xp_sprintf (Transact-SQL)
title: xp_sprintf (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_sprintf_TSQL
- xp_sprintf
dev_langs:
- TSQL
helpviewer_keywords:
- xp_sprintf
ms.assetid: 1eedd65c-03cc-4eab-b76e-04684fdfec52
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 75d36ece3d2875e7b6ae30a19626ee96bf85e0af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99122700"
---
# <a name="xp_sprintf-transact-sql"></a>xp_sprintf (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Formate et stocke une série de caractères et de valeurs dans le paramètre de sortie de type chaîne. Chaque argument de format est remplacé par l'argument correspondant.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
xp_sprintf { string OUTPUT , format }  
     [ , argument [ ,...n ] ]  
```  
  
## <a name="arguments"></a>Arguments  
 *string*  
 Variable **varchar** qui reçoit la sortie.  
  
 OUTPUT  
 Lorsqu'elle est spécifiée, cette option place la valeur de la variable dans le paramètre de sortie.  
  
 *format*  
 Chaîne de caractères de format avec des espaces réservés pour les valeurs d' *argument* , similaire à celle prise en charge par la fonction **sprintf** du langage C. Pour l'instant, seul l'argument de format %s est pris en charge.  
  
 *argument*  
 Chaîne de caractères qui représente la valeur de l'argument de format correspondant.  
  
 *n*  
 Emplacement réservé signalant qu'un nombre maximum de 50 arguments peut être spécifié.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 **xp_sprintf** renvoie le message suivant :  
  
 `The command(s) completed successfully.`  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées étendues générales &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_sscanf &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-sscanf-transact-sql.md)  
  
  
