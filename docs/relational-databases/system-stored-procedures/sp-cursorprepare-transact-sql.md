---
description: sp_cursorprepare (Transact-SQL)
title: sp_cursorprepare (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_prepare
ms.assetid: 6207e110-f4bf-4139-b3ec-b799c9cb3ad7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df4b0ff31a5b8a1723b6c8f130aeab9b32b03166
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209748"
---
# <a name="sp_cursorprepare-transact-sql"></a>sp_cursorprepare (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Compile le lot ou l'instruction de curseur dans un plan d'exécution, mais ne crée pas le curseur. L'instruction compilée peut être utilisée ultérieurement par sp_cursorexecute. Cette procédure, couplée avec sp_cursorexecute, a la même fonction que sp_cursoropen, mais est fractionnée en deux phases. sp_cursorprepare est appelée en spécifiant ID = 3 dans un paquet tabular data stream (TDS).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursorprepare prepared_handle OUTPUT, params , stmt , options  
    [ , scrollopt[ , ccopt]]  
```  
  
## <a name="arguments"></a>Arguments  
 *prepared_handle*  
 Identificateur de *handle* préparé généré par SQL Server qui retourne une valeur entière.  
  
> [!NOTE]  
>  *prepared_handle* est ensuite fourni à une procédure de sp_cursorexecute afin d’ouvrir un curseur. Une fois un handle créé, il existe jusqu'à ce que vous vous déconnectiez ou que vous le supprimiez de façon explicite par le biais d'une procédure sp_cursorunprepare.  
  
 *params*  
 Identifie des instructions paramétrables. La définition *params* des variables est substituée aux marqueurs de paramètres dans l’instruction. *params* est un paramètre obligatoire qui requiert une valeur d’entrée **ntext**, **nchar** ou **nvarchar** . Entrez une valeur NULL si l'instruction n'est pas paramétrable.  
  
> [!NOTE]  
>  Utilisez une chaîne **ntext** comme valeur d’entrée lorsque *stmt* est paramétré et que la valeur de PARAMETERIZED_STMT *scrollopt* est on.  
  
 *Insert*  
 Définit le jeu de résultats de curseur. Le paramètre *stmt* est requis et appelle une valeur d’entrée **ntext**, **nchar** ou **nvarchar** .  
  
> [!NOTE]  
>  Les règles de spécification de la valeur *stmt* sont les mêmes que celles de sp_cursoropen, à l’exception près que le type de données chaîne *stmt* doit être **ntext**.  
  
 *options*  
 Paramètre optionnel qui retourne une description des colonnes du jeu de résultats du curseur. les *options* requièrent la valeur d’entrée **int** suivante.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
 *scrollopt*  
 Option de défilement. *scrollopt* est un paramètre facultatif qui requiert l’une des valeurs d’entrée **int** suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 Étant donné que la valeur demandée n’est peut-être pas appropriée pour le curseur défini par *stmt*, ce paramètre sert à la fois à l’entrée et à la sortie. Dans de tels cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte une valeur appropriée.  
  
 *ccopt*  
 Option de contrôle en matière d'accès concurrentiel. *ccopt* est un paramètre facultatif qui requiert l’une des valeurs d’entrée **int** suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS (précédemment appelé LOCKCC)|  
|0x0004|**Optimiste** (anciennement OPTCC)|  
|0x0008|OPTIMISTIC (précédemment appelé OPTCCVAL)|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 Comme avec *scrollpt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut affecter une valeur différente de celle demandée.  
  
## <a name="remarks"></a>Notes  
 Le paramètre d'état RPC prend l'une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|Opération réussie|  
|0x0001|Échec|  
|1FF6|Impossible de retourner des métadonnées.<br /><br /> Remarque : Ceci est dû au fait que l’instruction ne produit pas un jeu de résultats ; par exemple, il s’agit d’une instruction INSERT ou DDL.|  
  
## <a name="examples"></a>Exemples  
  Voici un exemple d’utilisation de sp_cursorprepare et sp_cursorexecute

```sql
declare @handle int , @p5 int, @p6 int
exec sp_cursorprepare @handle OUTPUT, 
    N'@dbid int', 
    N'select * from sys.databases where database_id < @dbid',
    1,
    @p5 output,
    @p6 output


declare @p1 int  
set @P1 = @handle 
declare @p2 int   
declare @p3 int  
declare @p4 int  
set @P6 = 4 
exec sp_cursorexecute @p1, @p2 OUTPUT, @p3 output , @p4 output, @p5 OUTPUT, @p6

exec sp_cursorfetch @P2

exec sp_cursorunprepare @handle
exec sp_cursorclose @p2
```
 
 Quand *stmt* est paramétré et que la valeur de PARAMETERIZED_STMT *scrollopt* est on, le format de la chaîne est le suivant :  
  
 { *\<local variable name>**\<data type>* } [ ,... *n* ]  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursorexecute &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorunprepare &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorunprepare-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
