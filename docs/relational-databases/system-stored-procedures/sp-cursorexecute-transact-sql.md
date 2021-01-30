---
description: sp_cursorexecute (Transact-SQL)
title: sp_cursorexecute (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorexecute
- sp_cursorexecute_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_execute
ms.assetid: 6a204229-0a53-4617-a57e-93d4afbb71ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6aab1670ae9d517ec60d3af572d15eaf12432f7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205155"
---
# <a name="sp_cursorexecute-transact-sql"></a>sp_cursorexecute (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée et remplit un curseur basé sur le plan d'exécution créé par sp_cursorprepare. Cette procédure, couplée avec sp_cursorprepare, a la même fonction que sp_cursoropen, mais est fractionnée en deux phases. sp_cursorexecute est appelée en spécifiant ID = 4 dans un paquet tabular data stream (TDS).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursorexecute prepared_handle, cursor  
    [ , scrollopt[ OUTPUT ]  
    [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,bound param][,...n]]]]]  
```  
  
## <a name="arguments"></a>Arguments  
 *prepared_handle*  
 Valeur de *handle* d’instruction préparée retournée par sp_cursorprepare. *prepared_handle* est un paramètre obligatoire qui requiert une valeur d’entrée **int** .  
  
 *cursor*  
 Identificateur de curseur généré par le SQL Server. *Cursor* est un paramètre obligatoire qui doit être fourni pour toutes les procédures suivantes qui agissent sur le curseur, par exemple sp_cursorfetch  
  
 *scrollopt*  
 Option de défilement. *scrollopt* est un paramètre facultatif qui requiert une valeur d’entrée **int** . Le paramètre sp_cursorexecute *scrollopt* a les mêmes options de valeur que celles pour sp_cursoropen.  
  
> [!NOTE]  
>  La valeur PARAMETERIZED_STMT n'est pas prise en charge.  
  
> [!IMPORTANT]  
>  Si aucune valeur *scrollopt* n’est spécifiée, la valeur par défaut est keyset, quelle que soit la valeur *scrollopt* spécifiée dans sp_cursorprepare.  
  
 *ccopt*  
 Option de contrôle de la devise. *ccopt* est un paramètre facultatif qui requiert une valeur d’entrée **int** . Le paramètre sp_cursorexecute *ccopt* a les mêmes options de valeur que celles pour sp_cursoropen.  
  
> [!IMPORTANT]  
>  Si aucune valeur *ccopt* n’est spécifiée, la valeur par défaut est OPTIMISTIC, quelle que soit la valeur *ccopt* spécifiée dans sp_cursorprepare.  
  
 *stopp*  
 Paramètre optionnel qui indique le nombre de lignes de tampon d'extraction à utiliser avec AUTO_FETCH. La valeur par défaut est 20 lignes. le *compteur de lignes* se comporte différemment lorsqu’il est assigné comme valeur d’entrée ou valeur de retour.  
  
|Comme une valeur d'entrée|Comme une valeur de retour|  
|--------------------|---------------------|  
|Lorsque AUTO_FETCH est spécifié avec FAST_FORWARD curseurs *RowCount* représente le nombre de lignes à placer dans le tampon d’extraction.|Représente le nombre de lignes dans le jeu de résultats. Lorsque la valeur de AUTO_FETCH *scrollopt* est spécifiée, *RowCount* retourne le nombre de lignes extraites dans le tampon d’extraction.|  
  
 *bound_param*  
 Indique l'utilisation facultative de paramètres supplémentaires.  
  
> [!NOTE]  
>  Tous les paramètres après le cinquième sont passés au plan d'instruction comme paramètres d'entrée.  
  
## <a name="code-return-value"></a>Valeur de retour de code  
 *RowCount* peut retourner les valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|-1|Nombre de lignes inconnues.|  
|-n|Un remplissage asynchrone est appliqué.|  
  
## <a name="remarks"></a>Notes  
  
## <a name="scrollopt-and-ccopt-parameters"></a>Paramètres scrollopt et ccopt  
 *scrollopt* et *ccopt* sont utiles lorsque les plans mis en cache sont anticipés pour le cache du serveur, ce qui signifie que le handle préparé qui identifie l’instruction doit être recompilé. Les valeurs des paramètres *scrollopt* et *ccopt* doivent correspondre à celles envoyées dans la demande d’origine à sp_cursorprepare.  
  
> [!NOTE]  
>  PARAMETERIZED_STMT ne doit pas être assigné à *scrollopt*.  
  
 L'incapacité à fournir des valeurs correspondantes entraînera la recompilation des plans, annulant ainsi les opérations de préparation et d'exécution.  
  
## <a name="rpc-and-tds-considerations"></a>Éléments RPC et TDS à prendre en considération  
 Il est possible d'affecter à l'indicateur d'entrée RPC RETURN_METADATA la valeur 1 pour demander que les métadonnées relatives à la liste de sélection du curseur soient retournées dans le flux TDS.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
