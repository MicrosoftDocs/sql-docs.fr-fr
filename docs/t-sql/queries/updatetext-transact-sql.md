---
description: UPDATETEXT (Transact-SQL)
title: UPDATETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATETEXT
- UPDATETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- text updates [SQL Server]
- updating data [SQL Server]
- data updates [SQL Server], UPDATETEXT statement
- UPDATETEXT statement
ms.assetid: d73c28ee-3972-4afd-af8d-ebbbd9e50793
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5304394a75662d5cdc37196aab70444ebfbcbe41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186909"
---
# <a name="updatetext-transact-sql"></a>UPDATETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour un champ **text**, **ntext** ou **image** existant. Utilisez UPDATETEXT pour changer seulement une partie d’une colonne **text**, **ntext** ou **image**. Utilisez WRITETEXT pour mettre à jour et remplacer un champ **text**, **ntext** ou **image** entier.  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] À la place, utilisez les types de données de valeur élevée et la clause **.** WRITE de l’instruction [UPDATE](../../t-sql/queries/update-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
UPDATETEXT [BULK] { table_name.dest_column_name dest_text_ptr }  
  { NULL | insert_offset }  
     { NULL | delete_length }  
     [ WITH LOG ]  
     [ inserted_data  
    | { table_name.src_column_name src_text_ptr } ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 BULK  
 Permet aux outils de téléchargement de télécharger un flux de données binaires. Le flux de données doit être fourni par l'outil au niveau du protocole TDS. Lorsque le flux de données n'est pas présent, le processeur de requêtes ignore l'option BULK.  
  
> [!IMPORTANT]  
>  Nous recommandons de ne pas utiliser l'option BULK dans les applications basées sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette option est susceptible d'être modifiée ou supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *table_name* **.** *dest_column_name*  
 Nom de la table et de la colonne **text**, **ntext** ou **image** à mettre à jour. Les noms de table et les noms de colonne doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). La spécification des noms de la base de données et du propriétaire est facultative.  
  
 *dest_text_ptr*  
 Valeur de pointeur de texte (retournée par la fonction TEXTPTR) qui pointe vers les données **text**, **ntext** ou **image** à mettre à jour. *dest_text_ptr* doit être de type **binary(** 16 **)**.  
  
 *insert_offset*  
 Position de départ de la mise à jour (commence à zéro). Pour les colonnes **text** ou **image**, *insert_offset* correspond au nombre d’octets à ignorer depuis le début de la colonne existante, avant l’insertion de nouvelles données. Pour les colonnes **ntext**, *insert_offset* correspond au nombre de caractères (chaque caractère **ntext** utilise deux octets). Les données **text**, **ntext** ou **image** existantes commençant à cette position de départ basée sur zéro sont décalées vers la droite pour laisser de l’espace pour les nouvelles données. Une valeur 0 signifie que les nouvelles données seront insérées au début des données existantes. Une valeur NULL signifie que les nouvelles données seront ajoutées à la fin des données existantes.  
  
 *delete_length*  
 Longueur des données à supprimer dans la colonne **text**, **ntext** ou **image** existante, à partir de la position *insert_offset*. La valeur *delete_length* est spécifiée en octets pour les colonnes **text** et **image** et en caractères pour les colonnes **ntext**. Chaque caractère **ntext** utilise deux octets. La valeur 0 signifie aucune suppression de données. La valeur NULL supprime toutes les données entre la position *insert_offset* et la fin de la colonne **text** ou **image** existante.  
  
 WITH LOG  
 L'enregistrement dans un journal est déterminé par le mode de récupération en vigueur dans la base de données.  
  
 *inserted_data*  
 Données à insérer dans la colonne **text**, **ntext** ou **image** existante à la position *insert_offset*. Il s’agit d’une valeur unique de type **char**, **nchar**, **varchar**, **nvarchar**, **binary**, **varbinary**, **text**, **ntext** ou **image**. La valeur *inserted_data* peut être un littéral ou une variable.  
  
 *table_name.src_column_name*  
 Noms de la table et de la colonne **text**, **ntext** ou **image** utilisée comme source des données insérées. Les noms de tables et de colonnes doivent respecter les règles des identificateurs.  
  
 *src_text_ptr*  
 Valeur de pointeur de texte (retournée par la fonction TEXTPTR) qui pointe vers une colonne **text**, **ntext** ou **image** utilisée comme source des données insérées.  
  
> [!NOTE]  
>  La valeur *scr_text_ptr* doit être différente de la valeur *dest_text_ptr*.  
  
## <a name="remarks"></a>Remarques  
 Les nouvelles données insérées peuvent correspondre à une constante, un nom de table, un nom de colonne ou un pointeur de texte *inserted_data* unique.  
  
|Action de mise à jour|Paramètres UPDATETEXT|  
|-------------------|---------------------------|  
|Remplacement des données existantes|Spécifiez une valeur *insert_offset* non NULL, une valeur *delete_length* différente de zéro et les nouvelles données à insérer.|  
|Suppression des données existantes|Spécifiez une valeur *insert_offset* non NULL et une valeur *delete_length* différente de zéro. Ne spécifiez pas de nouvelles données à insérer.|  
|Insertion de nouvelles données|Spécifiez la valeur *insert_offset*, une valeur *delete_length* différente de zéro et les nouvelles données à insérer.|  
  
 Pour obtenir des performances optimales, nous vous recommandons d’insérer ou de mettre à jour les données **text**, **ntext** et **image** dans des tailles de bloc qui sont des multiples de 8 040 octets.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les pointeurs de texte dans la ligne vers les données **text**, **ntext** ou **image** sont possibles, mais ils peuvent ne pas être valides. Pour plus d’informations sur l’option text in row, consultez [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Pour plus d’informations sur l’invalidation des pointeurs de texte, consultez [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
 Pour initialiser les colonnes **text** avec la valeur NULL, utilisez WRITETEXT. UPDATETEXT initialise les colonnes **text** avec une chaîne vide.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite une autorisation UPDATE sur la table spécifiée.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant place le pointeur de texte dans la variable locale `@ptrval`, puis utilise `UPDATETEXT` pour corriger une faute d'orthographe.  
  
> [!NOTE]  
>  Pour exécuter cet exemple, vous devez installer la base de données pubs.  
  
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
UPDATETEXT pub_info.pr_info @ptrval 88 1 'b';  
GO  
ALTER DATABASE pubs SET RECOVERY FULL;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
  
