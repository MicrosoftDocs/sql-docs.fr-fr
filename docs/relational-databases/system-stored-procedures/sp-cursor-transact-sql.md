---
description: sp_cursor (Transact-SQL)
title: sp_cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_cursor_TSQL
- sp_cursor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor
ms.assetid: 41ade0ca-5f11-469d-bd4d-c8302ccd93b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 07dcf9bfc42705f2ad49e3ce476ffcd8ad70f052
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813282"
---
# <a name="sp_cursor-transact-sql"></a>sp_cursor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Demande des mises à jour positionnées. Cette procédure effectue des opérations sur une ou plusieurs lignes dans le tampon d'extraction d'un curseur. sp_cursor est appelée en spécifiant ID = 1 dans un paquet tabular data stream (TDS).  
  
||  
|-|  
|**S’applique à**: SQL Server ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu’à la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).|  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursor  cursor, optype, rownum, table  
    [ , value[...n]]]  
```  
  
## <a name="arguments"></a>Arguments  
 *cursor*  
 Handle de curseur. *Cursor* est un paramètre obligatoire qui requiert une valeur d’entrée **int** . *Cursor* est la valeur de *handle* générée par SQL Server et retournée par la procédure sp_cursoropen.  
  
 *optype*  
 Paramètre obligatoire qui désigne quelle opération le curseur effectuera. *optype* requiert l’une des valeurs d’entrée **int** suivantes.  
  
|Valeur|Nom|Description|  
|-----------|----------|-----------------|  
|0X0001|UPDATE|Utilisée pour mettre à jour une ou plusieurs lignes dans le tampon d'extraction.  Les lignes spécifiées dans *rowNum* sont à nouveau accessibles et mises à jour.|  
|0x0002|DELETE|Utilisée pour supprimer une ou plusieurs lignes dans le tampon d'extraction. Les lignes spécifiées dans *rowNum* sont réutilisées et supprimées.|  
|0X0004|INSERT|Insère des données sans générer d’instruction SQL **Insert** .|  
|0X0008|REFRESH|Utilisée pour remplir la mémoire tampon à partir de tables sous-jacentes et peut être utilisée pour actualiser la ligne si une mise à jour ou une suppression échoue en raison d'un contrôle d'accès concurrentiel optimiste, ou après une opération UPDATE.|  
|0X10|LOCK|Provoque l’acquisition d’une SQL Server U-Lock sur la page contenant la ligne spécifiée. Ce verrou est compatible avec les verrous S-Lock mais pas avec les verrous X-Lock ou autres verrous U-Lock. Permet d'implémenter le verrouillage à court terme.|  
|0X20|SETPOSITION|Est utilisé uniquement lorsque le programme va émettre une instruction DELETE ou UPDATE positionnée SQL Server suivante.|  
|0X40|ABSOLUTE|Peut uniquement être utilisée avec UPDATE ou DELETE.  ABSOLUTE est utilisée uniquement avec les curseurs KEYSET (est ignorée pour les curseurs DYNAMIC et les curseurs STATIC ne peuvent pas être mis à jour).<br /><br /> Remarque : si absolu est spécifié sur une ligne du keyset qui n’a pas été extraite, l’opération peut échouer au contrôle d’accès concurrentiel et le résultat de retour ne peut pas être garanti.|  
  
 *rownum*  
 Spécifie les lignes dans le tampon d'extraction sur lesquelles le curseur interviendra, qu'il mettra à jour ou supprimera.  
  
> [!NOTE]  
>  Cela n'affecte pas le point de départ de toute opération d'extraction RELATIVE, NEXT ou PREVIOUS, ni les mises à jour ou suppressions effectuées à l'aide de sp_cursor.  
  
 *rowNum* est un paramètre obligatoire qui requiert une valeur d’entrée **int** .  
  
 1  
 Indique la première ligne dans le tampon d'extraction.  
  
 2  
 Indique la deuxième ligne dans le tampon d'extraction.  
  
 3, 4, 5  
 Indique la troisième ligne, etc.  
  
 n  
 Indique la énième ligne dans le tampon d'extraction.  
  
 0  
 Indique toutes les lignes dans le tampon d'extraction.  
  
> [!NOTE]  
>  Est uniquement valide pour une utilisation avec des valeurs de mise à jour, de suppression, d’actualisation ou de verrouillage *optype* .  
  
 *table*  
 Nom de la table qui identifie la table à laquelle s’applique *optype* quand la définition de curseur implique une jointure ou si des noms de colonnes ambigus sont retournés par le paramètre *value* . Si aucune table spécifique n'est désignée, la valeur par défaut est la première table dans la clause FROM. *table* est un paramètre facultatif qui requiert une valeur d’entrée de chaîne. La chaîne peut être spécifiée comme tout caractère ou type de données UNICODE. la *table* peut être un nom de table en plusieurs parties.  
  
 *value*  
 Permet d'insérer ou de mettre à jour des valeurs. Le paramètre de chaîne de *valeur* est utilisé uniquement avec les valeurs *OPTYPE* Update et insert. La chaîne peut être spécifiée comme tout caractère ou type de données UNICODE.  
  
> [!NOTE]  
>  Les noms des paramètres de la *valeur* peuvent être assignés par l’utilisateur.  
  
## <a name="return-code-values"></a>Codet de retour  
 Lors de l’utilisation d’un RPC, une opération de suppression ou de mise à jour positionnée avec un numéro de tampon 0 retourne un message terminé avec un nombre de *lignes* égal à 0 (échec) ou 1 (réussite) pour chaque ligne du tampon d’extraction.  
  
## <a name="remarks"></a>Notes  
  
## <a name="optype-parameter"></a>Paramètre optype  
 À l’exception des combinaisons de la fonction SETPOSITION et de la mise à jour, de la suppression, de l’actualisation ou du verrouillage ; ou absolu avec UPDATE ou DELETE, les valeurs *optype* s’excluent mutuellement.  
  
 La clause SET de la valeur de mise à jour est construite à partir du paramètre *value* .  
  
 L’un des avantages de l’utilisation de la valeur INSERT *optype* est que vous pouvez éviter de convertir des données non-caractères au format caractère pour les insertions. Les valeurs sont spécifiées de la même façon qu'UPDATE. Si une colonne requise n'est pas incluse, INSERT échoue.  
  
-   La valeur SETPOSITION n'affecte pas le point de départ de toute opération d'extraction RELATIVE, NEXT ou PREVIOUS, ni les mises à jour ou suppressions effectuées à l'aide de l'interface sp_cursor. Tout numéro qui ne spécifie pas de ligne dans le tampon d'extraction entraînera l'affectation de la valeur 1 à la position, sans aucune erreur retournée. Une fois la fonction SETPOSITION exécutée, la position reste effective jusqu’à la prochaine opération de sp_cursorfetch, l’opération de **récupération** T-SQL ou l’opération sp_cursor SetPosition via le même curseur. Une opération sp_cursorfetch suivante définira la position du curseur sur la première ligne dans le nouveau tampon d'extraction alors que d'autres appels de curseur n'affecteront pas la valeur de la position. SETPOSITION peut être liée par une clause OR avec REFRESH, UPDATE, DELETE ou LOCK pour définir la valeur de la position sur la dernière ligne modifiée.  
  
 Si une ligne de la mémoire tampon d’extraction n’est pas spécifiée par le biais du paramètre *rowNum* , la position prend la valeur 1, sans qu’aucune erreur ne soit retournée. Une fois la position définie, elle reste effective jusqu'à l'exécution de l'opération sp_cursorfetch, T-SQL FETCH ou sp_cursor SETPOSITION suivante sur le même curseur.  
  
 SETPOSITION peut être liée par une clause OR avec REFRESH, UPDATE, DELETE ou LOCK pour définir la position du curseur sur la dernière ligne modifiée.  
  
## <a name="rownum-parameter"></a>Paramètre rownum  
 S’il est spécifié, le paramètre *rowNum* peut être interprété comme le numéro de ligne dans le keyset au lieu du numéro de ligne dans le tampon d’extraction. L'utilisateur est chargé de garantir que le contrôle d'accès concurrentiel est tenu à jour. Cela signifie que, pour les curseurs SCROLL_LOCKS, vous devez maintenir indépendamment un verrou sur la ligne donnée (cette opération peut être effectuée via une transaction). Pour les curseurs OPTIMISTIC, vous devez avoir au préalable extrait la ligne pour effectuer cette opération.  
  
## <a name="table-parameter"></a>Paramètre de table  
 Si la valeur *optype* est Update ou Insert et qu’une instruction UPDATE ou Insert complète est soumise en tant que paramètre *value* , la valeur spécifiée pour *table* est ignorée.  
  
> [!NOTE]  
>  En ce qui concerne les vues, une seule table qui participe à la vue peut être modifiée. Les noms des colonnes des paramètres de *valeur* doivent refléter les noms des colonnes dans la vue, mais le nom de la table peut être celui de la table de base sous-jacente (auquel cas sp_cursor remplacera le nom de la vue).  
  
## <a name="value-parameter"></a>Paramètre value  
 Il existe deux alternatives aux règles d’utilisation de la *valeur* , comme indiqué précédemment dans la section arguments :  
  
1.  Vous pouvez utiliser un nom qui est «'précédée du \@ nom de la colonne dans la liste de sélection pour tous les paramètres de *valeur* nommée. Un avantage de cette alternative est que la conversion des données peut ne pas être nécessaire.  
  
2.  Utilisez un paramètre pour envoyer une instruction UPDATE ou INSERT complète, ou utilisez plusieurs paramètres pour envoyer des parties d’une instruction UPDATE ou INSERT qui SQL Server sont ensuite générées dans une instruction complète. Vous trouverez des exemples dans la section Exemples plus loin dans cette rubrique.  
  
## <a name="examples"></a>Exemples  
  
### <a name="alternative-value-parameter-uses"></a>Autres utilisations de paramètres de valeurs  
 Pour UPDATE :  
  
 Lorsqu'un paramètre unique est utilisé, une instruction UPDATE peut être envoyée à l'aide de la syntaxe suivante :  
  
 `[ [ UPDATE <table name> ] SET ] {<column name> = expression} [,...n]`  
  
> [!NOTE]  
>  Si la mise à jour \<table name> est spécifiée, toute valeur spécifiée pour le paramètre de *table* sera ignorée.  
  
 Lorsque plusieurs paramètres sont utilisés, le premier paramètre doit être une chaîne sous la forme suivante :  
  
 `[ SET ] <column name> = expression  [,...n]`  
  
 et les paramètres suivants doivent être sous la forme :  
  
 `<column name> = expression  [,...n]`  
  
 Dans ce cas, le \<table name> dans l’instruction Update construite est celui spécifié ou défini par défaut par le paramètre *table* .  
  
 Pour INSERT :  
  
 Lorsqu'un paramètre unique est utilisé, une instruction INSERT peut être envoyée à l'aide de la syntaxe suivante :  
  
 `[ [ INSERT [INTO] <table name> ] VALUES ] ( <expression> [,...n] )`  
  
> [!NOTE]  
>  Si INSERT *\<table name>* est spécifié, toute valeur spécifiée pour le paramètre *table* sera ignorée.  
  
 Lorsque plusieurs paramètres sont utilisés, le premier paramètre doit être une chaîne sous la forme suivante :  
  
 `[ VALUES ( ] <expression>  [,...n]`  
  
 et les paramètres suivants doivent être sous la forme :  
  
 `expression [,...n]`  
  
 sauf lorsque VALUES a été spécifié, auquel cas une « ) » de fin est nécessaire après la dernière expression. Dans ce cas, le *\<table name>* dans l’instruction Update construite est celui spécifié ou défini par défaut par le paramètre *table* .  
  
> [!NOTE]  
>  Il est possible d'envoyer un paramètre comme paramètre nommé, c'est-à-dire « `@VALUES` ». Dans ce cas, aucun autre paramètre nommé ne peut être utilisé.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
