---
description: xp_sscanf (Transact-SQL)
title: xp_sscanf (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_sscanf_TSQL
- xp_sscanf
dev_langs:
- TSQL
helpviewer_keywords:
- xp_sscanf
ms.assetid: 619a9df1-7008-407e-a75a-bc6f851454a8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5f1152cc2cb918adaf8e51a5ceabb11f5bde2394
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187943"
---
# <a name="xp_sscanf-transact-sql"></a>xp_sscanf (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Lit les données de la chaîne dans les emplacements d'argument spécifiés par chaque argument de format.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
xp_sscanf { string OUTPUT , format } [ ,argument [ ,...n ] ]   
```  
  
## <a name="arguments"></a>Arguments  
 **string**  
 La chaîne de caractères dont il faut lire la valeur des arguments.  
  
 OUTPUT  
 Lorsqu’il est spécifié, la valeur de l' *argument* est placée dans le paramètre de sortie.  
  
 *format*  
 Est une chaîne de caractères mise en forme similaire à ce qui est pris en charge par la fonction **sscanf** du langage C. Pour l'instant, seul l'argument de format %s est pris en charge.  
  
 *argument*  
 Variable **varchar** définie sur la valeur de l’argument de *format* correspondant.  
  
 *n*  
 Emplacement réservé signalant qu'un nombre maximum de 50 arguments peut être spécifié.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 **xp_sscanf** renvoie le message suivant :  
  
 `Command(s) completed successfully.`  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 Cet exemple utilise `xp_sscanf` pour extraire deux valeurs d'une chaîne source sur la base de leur position dans le format de la chaîne source.  
  
```  
DECLARE @filename varchar (20), @message varchar (20);  
EXEC xp_sscanf 'sync -b -fproducts10.tmp -rrandom', 'sync -b -f%s -r%s',   
  @filename OUTPUT, @message OUTPUT;  
SELECT @filename, @message;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------- --------------------   
products10.tmp        random  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées étendues générales &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_sprintf &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-sprintf-transact-sql.md)  
  
  
