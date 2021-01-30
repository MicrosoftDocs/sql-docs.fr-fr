---
description: sp_cursoroption (Transact-SQL)
title: sp_cursoroption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursoroption_TSQL
- sp_cursoroption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursoroption
ms.assetid: 88fc1dba-f4cb-47c0-92c2-bf398f4a382e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 787c54be4c1030f9c091dbfead90c569a63bb270
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210475"
---
# <a name="sp_cursoroption-transact-sql"></a>sp_cursoroption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit des options de curseur ou retourne des informations de curseur créées par la procédure stockée sp_cursoropen. sp_cursoroption est appelée en spécifiant ID = 8 dans un paquet tabular data stream (TDS).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursoroption cursor, code, value  
```  
  
## <a name="arguments"></a>Arguments  
 *cursor*  
 Est une valeur de *handle* qui est générée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et retournée par la procédure stockée sp_cursoropen. le *curseur* requiert une valeur d’entrée **int** pour l’exécution.  
  
 *code*  
 Permet de stipuler différents facteurs des valeurs de retour de curseur. le *code* requiert l’une des valeurs d’entrée **int** suivantes :  
  
|Valeur|Nom|Description|  
|-----------|----------|-----------------|  
|0x0001|TEXTPTR_ONLY|Retourne le pointeur de texte, et non les données réelles, pour certaines colonnes text ou image désignées.<br /><br /> TEXTPTR_ONLY permet d’utiliser des pointeurs de texte comme *Handles* d’objets BLOB qui peuvent être récupérés ou mis à jour ultérieurement à l’aide des [!INCLUDE[tsql](../../includes/tsql-md.md)] fonctions ou de DBLIB (par exemple [!INCLUDE[tsql](../../includes/tsql-md.md)] READTEXT ou DBLIB dbwritetext).<br /><br /> Si une valeur « 0 » est affectée, toutes les colonnes text et image dans la liste de sélection retourneront des pointeurs de texte plutôt que des données.|  
|0x0002|CURSOR_NAME|Attribue le nom spécifié dans *valeur* au curseur. Cela permet à ODBC d’utiliser [!INCLUDE[tsql](../../includes/tsql-md.md)] des instructions UPDATE/DELETE positionnées sur les curseurs ouverts via sp_cursoropen.<br /><br /> La chaîne peut être spécifiée comme tout caractère ou type de données Unicode.<br /><br /> Étant donné que les [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions UPDATE/DELETE positionnées fonctionnent, par défaut, sur la première ligne d’un curseur FAT, SP_CURSOR SetPosition doit être utilisé pour positionner le curseur avant d’émettre l’instruction Update/Delete positionnée.|  
|0x0003|TEXTDATA|Retourne les données réelles, et non le pointeur de texte, pour certaines colonnes text ou image sur les extractions suivantes (autrement dit, cela annule l'effet de TEXTPTR_ONLY).<br /><br /> Si TEXTDATA est activé pour une colonne particulière, la ligne est à nouveau extraite ou actualisée, et TEXTPTR_ONLY peut ensuite lui être à nouveau affecté. Comme avec TEXTPTR_ONLY, le paramètre de valeur est un entier qui spécifie le numéro de colonne et une valeur zéro retourne toutes les colonnes text et image.|  
|0x0004|SCROLLOPT|Option de défilement. Pour plus d'informations, consultez « Valeurs des codes de retour » plus loin dans cette rubrique.|  
|0x0005|CCOPT|Option de contrôle en matière d'accès concurrentiel. Pour plus d'informations, consultez « Valeurs des codes de retour » plus loin dans cette rubrique.|  
|0x0006|ROWCOUNT|Nombre de lignes actuellement dans le jeu de résultats.<br /><br /> Remarque : le nombre de lignes peut avoir changé depuis la valeur retournée par sp_cursoropen si le remplissage asynchrone est utilisé. La valeur-1 est retournée si le nombre de lignes est inconnu.|  
  
 *value*  
 Désigne la valeur retournée par le *code*. la *valeur* est un paramètre obligatoire qui requiert une valeur d’entrée de *code* 0x0001, 0x0002 ou 0x0003.  
  
> [!NOTE]  
>  Une valeur de *code* de 2 est un type de données String. Toute autre entrée de valeur de *code* ou retournée par *valeur* est un entier.  
  
## <a name="return-code-values"></a>Codet de retour  
 Le paramètre *value* peut retourner l’une des valeurs de *code* suivantes.  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|0x0004|SCROLLOPT|  
|0X0005|CCOPT|  
|0X0006|ROWCOUNT|  
  
 Le paramètre *value* retourne l’une des valeurs SCROLLOPT suivantes.  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
  
 Le paramètre *value* retourne l’une des valeurs ccopt suivantes.  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS|  
|0x0004 ou 0x0008|OPTIMISTIC|  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursor-transact-sql.md)   
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)  
  
  
