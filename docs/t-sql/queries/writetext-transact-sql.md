---
description: WRITETEXT (Transact-SQL)
title: WRITETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WRITETEXT_TSQL
- WRITETEXT
dev_langs:
- TSQL
helpviewer_keywords:
- replacing data
- WRITETEXT statement
- updating data [SQL Server]
- nonlogged updating
- minimally logged updating [SQL Server]
- overwriting data
- data updates [SQL Server], WRITETEXT statement
ms.assetid: 80c252fd-a8b8-4a2e-888a-059081ed4109
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f1793b813e763df5cb8078a73fb1cd78c7fe7a6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193365"
---
# <a name="writetext-transact-sql"></a>WRITETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet la mise à jour interactive, avec une journalisation minimale, d’une colonne de type **text**, **ntext** ou **image** existante. WRITETEXT remplace totalement les données existantes de la colonne qu'elle affecte. Vous ne pouvez pas utiliser WRITETEXT sur les colonnes **text**, **ntext** et **image** des affichages.  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] À la place, utilisez les types de données de valeur élevée et la clause **.** WRITE de l’instruction [UPDATE](../../t-sql/queries/update-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
WRITETEXT [BULK]  
  { table.column text_ptr }  
  [ WITH LOG ] { data }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 BULK  
 Permet aux outils de téléchargement de télécharger un flux de données binaires. Le flux de données doit être fourni par l'outil au niveau du protocole TDS. Lorsque le flux de données n'est pas présent, le processeur de requêtes ignore l'option BULK.  
  
> [!IMPORTANT]  
>  Nous recommandons de ne pas utiliser l'option BULK dans les applications basées sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette option est susceptible d'être modifiée ou supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *table* **.column**  
 Nom de la table et de la colonne **text**, **ntext** ou **image** à mettre à jour. Les noms de la table et de la colonne doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). La spécification des noms de la base de données et du propriétaire est facultative.  
  
 *text_ptr*  
 Valeur qui stocke le pointeur des données **text**, **ntext** ou **image**. *text_ptr* doit être de type **binary(16)**. Pour créer un pointeur de texte, exécutez une instruction [INSERT](../../t-sql/statements/insert-transact-sql.md) ou [UPDATE](../../t-sql/queries/update-transact-sql.md) avec des données non NULL pour la colonne **text**, **ntext** ou **image**.  
  
 WITH LOG  
 Ignoré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'enregistrement dans un journal est déterminé par le mode de récupération en vigueur dans la base de données.  
  
 *data*  
 Données **text**, **ntext** ou **image** à stocker. *data* peut être un littéral ou un paramètre. La longueur maximale du texte que vous pouvez insérer de manière interactive au moyen de WRITETEXT est d’environ 120 Ko pour les données **text**, **ntext** et **image**.  
  
## <a name="remarks"></a>Notes  
 Utilisez WRITETEXT pour remplacer des données **text**, **ntext** et **image**, et UPDATETEXT pour modifier des données **text**, **ntext** et **image**. UPDATETEXT est plus souple, car il modifie uniquement une partie d’une colonne **text**, **ntext** ou **image**, au lieu de la colonne entière.  
  
 Pour obtenir des performances optimales, il est recommandé d’ajouter ou de mettre à jour les données **text**, **ntext** et **image** dans des tailles de bloc qui sont des multiples de 8040 octets.  
  
 Si le mode de récupération de base de données est le mode de récupération simple ou le mode de récupération utilisant les journaux de transactions, les opérations **text**, **ntext** et **image** qui utilisent WRITETEXT sont des opérations journalisées de façon minimale lorsque de nouvelles données sont insérées ou ajoutées.  
  
> [!NOTE]  
>  La journalisation minimale n'est pas utilisée lors de la mise à jour de valeurs existantes.  
  
 Pour que l'instruction WRITETEXT fonctionne correctement, la colonne doit déjà contenir un pointeur de texte valide.  
  
 Si la table ne contient pas de texte dans les lignes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] économise de l’espace en n’initialisant pas les colonnes **text** quand des valeurs NULL explicites ou implicites sont ajoutées à des colonnes **text** à l’aide de l’instruction INSERT, et aucun pointeur de texte ne peut être obtenu pour ces valeurs NULL. Pour initialiser les colonnes **text** avec la valeur NULL, utilisez l’instruction UPDATE. Si la table contient du texte dans ses lignes, vous n'avez pas besoin d'initialiser la colonne text pour les valeurs NULL et vous pouvez toujours obtenir un pointeur de texte.  
  
 La fonction ODBC SQLPutData est plus rapide et utilise moins de mémoire dynamique que WRITETEXT. Cette fonction peut insérer jusqu’à deux gigaoctets de données **text**, **ntext** ou **image**.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les pointeurs de texte dans la ligne vers les données **text**, **ntext** ou **image** sont possibles, mais ils peuvent ne pas être valides. Pour plus d’informations sur l’option text in row, consultez [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Pour plus d’informations sur l’invalidation des pointeurs de texte, consultez [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite une autorisation UPDATE sur la table spécifiée. L'autorisation est transférable lorsque l'autorisation UPDATE est transférée.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant place le pointeur de texte dans la variable locale `@ptrval`, puis `WRITETEXT` place la nouvelle chaîne de texte dans la ligne sur laquelle pointe `@ptrval`.  
  
> [!NOTE]  
>  Pour exécuter cet exemple, vous devez installer l'exemple de base de données pubs.  
  
```sql  
USE pubs;  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
DECLARE @ptrval BINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
FROM pub_info pr, publishers p  
WHERE p.pub_id = pr.pub_id   
   AND p.pub_name = 'New Moon Books'  
WRITETEXT pub_info.pr_info @ptrval 'New Moon Books (NMB) has just released another top ten publication. With the latest publication this makes NMB the hottest new publisher of the year!';  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)  
  
  
