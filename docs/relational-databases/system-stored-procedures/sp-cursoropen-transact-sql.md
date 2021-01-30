---
description: sp_cursoropen (Transact-SQL)
title: sp_cursoropen (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursoropen
- sp_cursoropen_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursoropen
ms.assetid: 16462ede-4393-4293-a598-ca88c48ca70b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1e8e4025a31c92ec001bcb45bee4f8d26f3dc24e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206279"
---
# <a name="sp_cursoropen-transact-sql"></a>sp_cursoropen (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ouvre un curseur. sp_cursoropen définit l’instruction SQL associée au curseur et aux options de curseur, puis remplit le curseur. sp_cursoropen équivaut à la combinaison des [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions DECLARE_CURSOR et Open. Cette procédure est appelée en spécifiant ID = 2 dans un paquet TDS (Tabular Data Stream).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursoropen cursor OUTPUT, stmt  
    [, scrollopt[ OUTPUT ] [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,boundparam][,...n]]] ]]  
```  
  
## <a name="arguments"></a>Arguments  
 *cursor*  
 Identificateur de curseur généré par SQL Server. *Cursor* est une valeur de *handle* qui doit être fournie pour toutes les procédures suivantes impliquant le curseur, par exemple sp_cursorfetch. *Cursor* est un paramètre obligatoire avec une valeur de retour **int** .  
  
 le *curseur* permet à plusieurs curseurs d’être actifs sur une même connexion de base de données.  
  
 *Insert*  
 Paramètre obligatoire qui définit le jeu de résultats de curseur. Toute chaîne de requête valide (syntaxe et liaison) de tout type de chaîne (indépendamment de Unicode, taille, etc.) peut servir de type de valeur *stmt* valide.  
  
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
  
 En raison de la possibilité que la valeur demandée ne soit pas appropriée pour le curseur défini par *stmt*, ce paramètre sert à la fois d’entrée et de sortie. Dans de tels cas, SQL Server affecte une valeur appropriée.  
  
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
  
 Comme avec *scrollopt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut remplacer les valeurs *ccopt* demandées.  
  
 *stopp*  
 Nombre de lignes de tampon d'extraction à utiliser avec AUTO_FETCH. La valeur par défaut est 20 lignes. le *compteur de lignes* se comporte différemment lorsqu’il est assigné comme valeur d’entrée ou valeur de retour.  
  
|Comme une valeur d'entrée|Comme une valeur de retour|  
|--------------------|---------------------|  
|Lorsque la AUTO_FETCH valeur *scrollopt* est spécifiée, *RowCount* représente le nombre de lignes à placer dans le tampon d’extraction.<br /><br /> Remarque : >0 est une valeur valide lorsque AUTO_FETCH est spécifié, mais est sinon ignoré.|Représente le nombre de lignes dans le jeu de résultats, sauf lorsque la valeur de AUTO_FETCH *scrollopt* est spécifiée.|  
  
-  
  
 *boundparam*  
 Indique l'utilisation de paramètres supplémentaires. *boundparam* est un paramètre facultatif qui doit être spécifié si la valeur de PARAMETERIZED_STMT *scrollopt* est définie sur on.  
  
## <a name="return-code-values"></a>Codet de retour  
 Si aucune erreur n'est générée, sp_cursoropen retourne l'une des valeurs suivantes.  
  
 0  
 La procédure a été exécutée avec succès.  
  
 0x0001  
 Une erreur s'est produite pendant l'exécution (une erreur mineure, pas assez grave pour générer une erreur dans l'opération).  
  
 0x0002  
 Une opération asynchrone est en cours.  
  
 0x0002  
 Une opération FETCH est en cours.  
  
 A  
 Ce curseur a été libéré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et n'est pas disponible.  
  
 Lorsqu'une erreur est générée, les valeurs de retour peuvent être incohérentes et l'exactitude ne peut pas être garantie.  
  
 Lorsque le paramètre *RowCount* est spécifié en tant que valeur de retour, le jeu de résultats suivant se produit.  
  
 -1  
 Retourné si le nombre de lignes est inconnu ou non applicable.  
  
 -n  
 Retourné lorsqu'un remplissage asynchrone est appliqué. Représente le nombre de lignes qui ont été placées dans le tampon d’extraction lorsque la valeur de AUTO_FETCH *scrollopt* est spécifiée.  
  
 Si l'appel de procédure distante est utilisé, les valeurs de retour sont les suivantes.  
  
 0  
 La procédure est réussie.  
  
 1  
 La procédure a échoué.  
  
 2  
 Un curseur de jeu de clés est généré de façon asynchrone.  
  
 16  
 Un curseur avance rapide a été automatiquement fermé.  
  
> [!NOTE]  
>  Si la procédure sp_cursoropen s’exécute correctement, les paramètres de retour RPC et un jeu de résultats avec des informations de format de colonne TDS (0xA0 & messages 0xA1) sont envoyés. En cas d'échec, un ou plusieurs messages d'erreur TDS sont envoyés. Dans les deux cas, aucune donnée de ligne n’est retournée et le nombre de messages *terminés* est égal à zéro. Si vous utilisez une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à 7.0, 0xa0, 0xa1 (standard pour les instructions SELECT) sont retournés avec les flux de jetons 0xa5 et 0xa4. Si vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, 0x81 est retourné (standard pour les instructions SELECT) avec les flux de jetons 0xa5 et 0xa4.  
  
## <a name="remarks"></a>Notes  
  
## <a name="stmt-parameter"></a>Paramètre stmt  
 Si *stmt* spécifie l’exécution d’une procédure stockée, les paramètres d’entrée peuvent être définis en tant que constantes dans le cadre de la chaîne *stmt* , ou spécifiés en tant qu’arguments *boundparam* . Les variables déclarées peuvent être passées comme paramètres liés de cette façon.  
  
 Le contenu autorisé du paramètre *stmt* varie selon que la valeur de retour de la ALLOW_DIRECT *ccopt* a été ou non liée au reste des valeurs *ccopt* , à savoir :  
  
-   Si ALLOW_DIRECT n’est pas spécifié, une [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction SELECT ou Execute appelant pour une procédure stockée contenant une instruction SELECT unique doit être utilisée. En outre, l'instruction SELECT doit obtenir la qualification de curseur, c'est-à-dire qu'elle ne peut pas contenir les mots clés SELECT INTO ou FOR BROWSE.  
  
-   Si ALLOW_DIRECT est spécifiée, cela peut générer une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], notamment celles qui, à leur tour, exécutent d'autres procédures stockées avec plusieurs instructions. Les instructions autres que SELECT ou toute instruction SELECT qui contient les mots clés SELECT INTO ou FOR BROWSE seront simplement exécutées et n'aboutiront pas à la création d'un curseur. Cela s'applique aussi à toute instruction SELECT incluse dans un lot de plusieurs instructions. Dans les cas où une instruction SELECT contient des clauses qui concernent uniquement les curseurs, ces clauses sont ignorées. Par exemple, lorsque la valeur de *ccopt* est 0x2002, il s’agit d’une demande pour :  
  
    -   un curseur avec des arrêts de défilement, si une seule instruction SELECT obtient la qualification de curseur, ou  
  
    -   une exécution d'instruction directe s'il existe plusieurs instructions, une instruction autre que SELECT unique ou une instruction SELECT qui n'obtient pas la qualification de curseur.  
  
## <a name="scrollopt-parameter"></a>Paramètre scrollopt  
 Les cinq premières valeurs *scrollopt* (keyset, DYNAMIC, FORWARD_ONLY, STATIC et FAST_FORWARD) s’excluent mutuellement.  
  
 PARAMETERIZED_STMT et CHECK_ACCEPTED_TYPES peuvent être liés par OR à chacune des cinq premières valeurs.  
  
 AUTO_FETCH et AUTO_CLOSE peuvent être liés par OR à FAST_FORWARD.  
  
 Si CHECK_ACCEPTED_TYPES est activé, au moins une des cinq dernières valeurs *scrollopt* (KEYSET_ACCEPTABLE `,` DYNAMIC_ACCEPTABLE, FORWARD_ONLY_ACCEPTABLE, STATIC_ACCEPTABLE ou FAST_FORWARD_ACCEPTABLE) doit également être activée.  
  
 Les curseurs STATIC sont toujours ouverts comme READ_ONLY. Cela signifie que la table sous-jacente ne peut pas être mise à jour via ce curseur.  
  
## <a name="ccopt-parameter"></a>Paramètre ccopt  
 Les quatre premières valeurs *ccopt* (READ_ONLY, SCROLL_LOCKS et les deux valeurs optimistes) s’excluent mutuellement.  
  
> [!NOTE]  
>  Le choix de l’une des quatre premières valeurs de *ccopt* détermine si le curseur est en lecture seule, ou si les méthodes de verrouillage ou optimistes sont utilisées pour empêcher les pertes de mises à jour. Si aucune valeur *ccopt* n’est spécifiée, la valeur par défaut est OPTIMISTIC.  
  
 ALLOW_DIRECT et CHECK_ACCEPTED_TYPES peuvent être liés par OR à chacune des quatre premières valeurs.  
  
 UPDT_IN_PLACE peut être lié par OR à READ_ONLY, SCROLL_LOCKS, ou l'une des valeurs OPTIMISTIC.  
  
 Si CHECK_ACCEPTED_TYPES est activé, au moins une des quatre dernières valeurs *ccopt* (READ_ONLY_ACCEPTABLE, SCROLL_LOCKS_ACCEPTABLE et l’une des valeurs OPTIMISTIC_ACCEPTABLE) doit également être activée.  
  
 Les fonctions de mise à jour et de suppression positionnées peuvent être exécutées uniquement dans le tampon d’extraction et uniquement si la valeur *ccopt* est égale à SCROLL_LOCKS ou optimiste. Si SCROLL_LOCKS est la valeur spécifiée, la réussite de l'opération est garantie. Si OPTIMISTIC est la valeur spécifiée, l'opération échoue si la ligne a changé depuis la dernière extraction.  
  
 La raison de cet échec est que, lorsqu'OPTIMISTIC est la valeur spécifiée, une fonction de contrôle d'accès concurrentiel optimiste est exécutée en comparant les valeurs d'horodatage ou de somme de contrôle, comme déterminé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l'une de ces lignes ne correspond pas, l'opération échoue.  
  
 La spécification d'UPDT_IN_PLACE comme valeur de retour détermine les résultats suivants :  
  
-   S'il n'est pas défini lors de l'exécution d'une mise à jour positionnée sur une table avec un index unique, le curseur supprime la ligne de sa table de travail et l'insère à la fin de l'une des colonnes clés utilisées par le curseur, en modifiant ainsi ces colonnes.  
  
-   S'il a la valeur ON, le curseur met simplement à jour les colonnes clés dans la ligne d'origine de la table de travail.  
  
## <a name="bound_param-parameter"></a>Paramètre bound_param  
 Le nom du paramètre doit être *ParamDef* Lorsque PARAMETERIZED_STMT est spécifié, en fonction du message d’erreur dans le code. Lorsque PARAMETERIZED_STMT n'est pas spécifié, aucun nom n'est spécifié dans le message d'erreur.  
  
## <a name="rpc-considerations"></a>Éléments RPC à prendre en considération  
 Il est possible d'affecter à l'indicateur d'entrée RPC RETURN_METADATA la valeur 0x0001 pour demander que les métadonnées relatives à la liste de sélection du curseur soient retournées dans le flux TDS.  
  
## <a name="examples"></a>Exemples  
  
### <a name="bound_param-parameter"></a>Paramètre bound_param  
 Tous les paramètres après le cinquième sont passés au plan d'instruction comme paramètres d'entrée. Le premier paramètre de ce type doit être une chaîne sous la forme suivante :  
  
 *{nom de variable locale type de données} [,... n*  
  
 Les paramètres suivants sont utilisés pour passer les valeurs à remplacer par le nom de la *variable locale* dans l’instruction.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursorfetch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
