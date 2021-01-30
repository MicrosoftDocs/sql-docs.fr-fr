---
description: sp_cursorprepexec (Transact-SQL)
title: sp_cursorprepexec (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorprepexec_TSQL
- sp_cursorprepexec
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorprepexec
ms.assetid: 8094fa90-35b5-4cf4-8012-0570cb2ba1e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b59597048ba7edc1102c3151d720e9bc6ff36942
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209693"
---
# <a name="sp_cursorprepexec-transact-sql"></a>sp_cursorprepexec (Transact-SQL)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Compile un plan pour le lot ou l'instruction de curseur envoyé, puis crée et remplit le curseur. sp_cursorprepexec combine les fonctions de sp_cursorprepare et de sp_cursorexecute. Cette procédure est appelée en spécifiant ID = 5 dans un paquet tabular data stream (TDS).  
  
 ![Icône de lien](../../database-engine/configure-windows/media/topic-link.gif "Icône de lien") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursorprepexec prepared handle OUTPUT, cursor OUTPUT, params , statement , options  
    [ , scrollopt [ , ccopt [ , rowcount ] ] ]  
    [, '@parameter_name[,...n ]']
```  
  
## <a name="arguments"></a>Arguments  
 *handle préparé*  
 Identificateur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *handle* préparé généré. un *handle préparé* est requis et retourne **int**.  
  
 *cursor*  
 Identificateur de curseur généré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Cursor* est un paramètre obligatoire qui doit être fourni pour toutes les procédures suivantes qui agissent sur ce curseur, par exemple, sp_cursorfetch.  
  
 *params*  
 Identifie des instructions paramétrables. La définition *params* des variables est substituée aux marqueurs de paramètres dans l’instruction. *params* est un paramètre obligatoire qui requiert une valeur d’entrée **ntext**, **nchar** ou **nvarchar** .  
  
> [!NOTE]  
>  Utilisez une chaîne **ntext** comme valeur d’entrée lorsque *stmt* est paramétré et que la valeur de PARAMETERIZED_STMT *scrollopt* est on.  
  
 *instruction*  
 Définit le jeu de résultats de curseur. Le paramètre d' *instruction* est obligatoire et appelle une valeur d’entrée **ntext**, **nchar** ou **nvarchar** .  
  
> [!NOTE]  
>  Les règles de spécification de la valeur stmt sont les mêmes que celles de sp_cursoropen, à l’exception près que le type de données chaîne *stmt* doit être **ntext**.  
  
 *options*  
 Paramètre optionnel qui retourne une description des colonnes du jeu de résultats du curseur. * les options requièrent la valeur d’entrée **int** suivante.  
  
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
  
 En raison de la possibilité que l’option demandée ne soit pas appropriée pour le curseur défini par *\<stmt>* , ce paramètre sert à la fois d’entrée et de sortie. Dans de tels cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte un type approprié et modifie cette valeur.  
  
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
  
 *stopp*  
 Paramètre optionnel qui indique le nombre de lignes de tampon d'extraction à utiliser avec AUTO_FETCH. La valeur par défaut est 20 lignes. le *compteur de lignes* se comporte différemment lorsqu’il est assigné comme valeur d’entrée ou valeur de retour.  
  
|Comme une valeur d'entrée|Comme une valeur de retour|  
|--------------------|---------------------|  
|Lorsque AUTO_FETCH est spécifié avec FAST_FORWARD curseurs *RowCount* représente le nombre de lignes à placer dans le tampon d’extraction.|Représente le nombre de lignes dans le jeu de résultats. Lorsque la valeur de AUTO_FETCH *scrollopt* est spécifiée, *RowCount* retourne le nombre de lignes extraites dans le tampon d’extraction.|  

*parameter_name* Désignez un ou plusieurs noms de paramètres comme défini dans l’argument params.  Un paramètre doit être fourni pour chaque paramètre inclus dans les paramètres. Cet argument n’est pas obligatoire quand aucun paramètre n’est défini pour l’instruction ou le lot Transact-SQL dans params.
  
## <a name="return-code-values"></a>Codet de retour  
 Si params retourne une valeur NULL, l’instruction n’est pas paramétrable.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorexecute &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursorprepare &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorprepare-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
