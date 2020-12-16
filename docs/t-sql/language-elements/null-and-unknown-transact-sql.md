---
description: NULL et UNKNOWN (Transact-SQL)
title: NULL et UNKNOWN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 9d491846-4730-4740-a680-77c69fae4a58
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 045c124a25a7c5604ff7d848365c156b734ea1d0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460912"
---
# <a name="null-and-unknown-transact-sql"></a>NULL et UNKNOWN (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  NULL indique que la valeur est inconnue. Une valeur NULL est différente d’une valeur vide ou égale à zéro. Deux valeurs NULL ne sont pas égales. Les comparaisons de deux valeurs NULL, ou d’une valeur NULL et d’une autre valeur, renvoient le message « inconnu » car la valeur de chaque NULL est inconnue.  
  
 Les valeurs NULL indiquent généralement des données inconnues, non applicables ou à ajouter ultérieurement. Le montant moyen, par exemple, ne peut pas être connu à l'instant où le client effectue une commande.  
  
 Notez les points suivants sur les valeurs Null :  
  
-   Pour tester des valeurs NULL dans une requête, utilisez IS NULL ou IS NOT NULL dans la clause WHERE.  
  
-   Les valeurs Null peuvent être insérées dans une colonne en spécifiant explicitement NULL dans une instruction INSERT ou UPDATE ou en laissant une colonne en dehors d’une instruction INSERT.  
  
-   Les valeurs Null ne peuvent pas servir d’informations permettant de distinguer entre les lignes dans une table, comme des clés primaires, ni d’informations permettant de distribuer des lignes, comme des clés de distribution.  
  
 Lorsque des valeurs NULL sont présentes dans les données, les opérateurs logiques et les opérateurs de comparaison peuvent renvoyer la valeur UNKNOWN au lieu de TRUE ou de FALSE. Cette logique tri-valuée nécessaire est source de nombreuses erreurs dans les applications. Les opérateurs logiques dans une expression booléenne qui inclut des valeurs UNKNOWN renvoient UNKNOWN sauf si le résultat de l’opérateur ne dépend pas de l’expression UNKNOWN. Ces tableaux donnent des exemples de ce comportement.  
  
 Le tableau suivant présente les résultats de l’application d’un opérateur AND à deux expressions booléennes quand une seule expression retourne UNKNOWN.  
  
|Expression 1|Expression 2|Résultats|  
|---------------|---------------|------------|  
|true|UNKNOWN|UNKNOWN|  
|UNKNOWN|UNKNOWN|UNKNOWN|  
|FALSE|UNKNOWN|FALSE|  
  
 Le tableau suivant présente les résultats de l’application d’un opérateur OR à deux expressions booléennes quand une seule expression retourne UNKNOWN.  
  
|Expression 1|Expression 2|Résultats|  
|---------------|---------------|------------|  
|true|UNKNOWN|true|  
|UNKNOWN|UNKNOWN|UNKNOWN|  
|FALSE|UNKNOWN|UNKNOWN|  
  
## <a name="see-also"></a>Voir aussi  
 [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md)   
 [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md)   
 [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md)   
 [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md)  
  
  
