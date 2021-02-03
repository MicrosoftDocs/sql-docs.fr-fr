---
description: Fonctions texte et image - TEXTVALID (Transact-SQL)
title: TEXTVALID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTVALID_TSQL
- TEXTVALID
dev_langs:
- TSQL
helpviewer_keywords:
- invalid text pointers [SQL Server]
- valid text pointers [SQL Server]
- TEXTVALID function
- checking text pointers
- text-pointer values
- verifying text pointers
ms.assetid: 9411c349-b59b-4740-a270-92f91d81ad23
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 17a2d30b57e5c60624509f5adaafc2fb94fc30d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181464"
---
# <a name="text-and-image-functions---textvalid-transact-sql"></a>Fonctions texte et image - TEXTVALID (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fonction **text**, **ntext** ou **image** qui vérifie si un pointeur de texte spécifique est valide.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Aucune fonctionnalité de remplacement n'est disponible.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TEXTVALID ( 'table.column' ,text_ ptr )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *table*  
 Nom de la table à utiliser.  
  
 *column*  
 Nom de la colonne à utiliser.  
  
 *text_ptr*  
 Pointeur texte à vérifier.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
## <a name="remarks"></a>Notes  
 Retourne 1 si le pointeur est valide et 0 si le pointeur n'est pas valide. Notez que l’identificateur de la colonne **text** doit inclure le nom de la table. Vous ne pouvez pas utiliser UPDATETEXT, WRITETEXT ou READTEXT sans pointeur de texte valide.  
  
 Les fonctions et instructions suivantes sont également utiles lorsque vous travaillez avec des données **text**, **ntext** et **image**.  
  
|Fonction ou instruction|Description|  
|---------------------------|-----------------|  
|PATINDEX **(** ' _%pattern%_ ' **,** _expression_ **)**|Retourne la position d’un caractère dans la chaîne de caractères spécifiée dans les colonnes **text** et **ntext**.|  
|DATALENGTH **(** _expression_ **)**|Retourne la longueur des données dans les colonnes de type **text**, **ntext** et **image**.|  
|SET TEXTSIZE|Retourne la limite, en octets, des données **text**, **ntext** ou **image** à retourner avec une instruction SELECT.|  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant indique si un pointeur de texte valide existe pour chaque valeur dans la colonne `logo` de la table `pub_info`.  
  
> [!NOTE]  
>  Pour exécuter cet exemple, vous devez installer la base de données **pubs**.  
  
```sql
USE pubs;  
GO  
SELECT pub_id, 'Valid (if 1) Text data'   
   = TEXTVALID ('pub_info.logo', TEXTPTR(logo))   
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id Valid (if 1) Text data   
------ ----------------------   
0736   1                        
0877   1                        
1389   1                        
1622   1                        
1756   1                        
9901   1                        
9952   1                        
9999   1                        
  
(8 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
 [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)   
 [Fonctions texte et image &#40;Transact-SQL&#41;](./text-and-image-functions-textptr-transact-sql.md)   
 [TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)  
  
