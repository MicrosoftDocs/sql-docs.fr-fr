---
description: sp_cursorfetch (Transact-SQL)
title: sp_cursorfetch (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorfetch
- sp_cursorfetch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorfetch
ms.assetid: 14513c5e-5774-4e4c-92e1-75cd6985b6a3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bd6ad166a48d259ab925ae9369abe0d73a73f13b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99126308"
---
# <a name="sp_cursorfetch-transact-sql"></a>sp_cursorfetch (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Extrait une mémoire tampon d'une ou de plusieurs lignes de la base de données. Le groupe de lignes dans cette mémoire tampon est appelé le *tampon d’extraction* du curseur. sp_cursorfetch est appelée en spécifiant ID = 7 dans un paquet tabular data stream (TDS).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursorfetch cursor  
    [ , fetchtype[ , rownum [ , nrows] ]]   
```  
  
## <a name="arguments"></a>Arguments  
 *cursor*  
 Valeur de *handle* générée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et retournée par sp_cursoropen. *Cursor* est un paramètre obligatoire qui requiert une valeur d’entrée **int** . Pour plus d'informations, consultez la section « Notes » plus loin dans cette rubrique.  
  
 *FetchType*  
 Spécifie la mémoire tampon de curseur à extraire. *FetchType* est un paramètre facultatif qui requiert l’une des valeurs d’entrée entières suivantes.  
  
|Valeur|Nom|Description|  
|-----------|----------|-----------------|  
|0x0001|FIRST|Extrait la première mémoire tampon de lignes *nrows* . Si *nrows* est égal à 0, le curseur est positionné avant le jeu de résultats et aucune ligne n’est retournée.|  
|0x0002|NEXT|Extrait la mémoire tampon suivante de lignes *nrows* .|  
|0x0004|PREV|Extrait la mémoire tampon précédente des lignes *nrows* .<br /><br /> Remarque : l’utilisation de PREV pour un curseur FORWARD_ONLY retourne un message d’erreur car FORWARD_ONLY prend en charge uniquement le défilement dans une direction.|  
|0x0008|LAST|Extrait la dernière mémoire tampon des lignes *nrows* . Si *nrows* est égal à 0, le curseur est positionné après le jeu de résultats et aucune ligne n’est retournée.<br /><br /> Remarque : l’utilisation de LAST pour un curseur FORWARD_ONLY retourne un message d’erreur car FORWARD_ONLY prend en charge uniquement le défilement dans une direction.|  
|0x10|ABSOLUTE|Extrait une mémoire tampon de lignes *nrows* à partir de la ligne *rowNum* .<br /><br /> Remarque : l’utilisation de l’absolu pour un curseur dynamique ou un curseur FORWARD_ONLY retourne un message d’erreur car FORWARD_ONLY prend en charge uniquement le défilement dans une direction.|  
|0x20|RELATIVE|Extrait la mémoire tampon des lignes *nrows* en commençant par la ligne spécifiée comme étant la valeur *rowNum* des lignes de la première ligne du bloc actif. Dans ce cas, *rowNum* peut être un nombre négatif.<br /><br /> Remarque : l’utilisation de RELATIVE pour un curseur FORWARD_ONLY retourne un message d’erreur car FORWARD_ONLY prend en charge uniquement le défilement dans une direction.|  
|0x80|REFRESH|Remplit la mémoire tampon à partir de tables sous-jacentes.|  
|0x100|INFO|Récupère des informations sur le curseur. Ces informations sont retournées à l’aide des paramètres *rowNum* et *nrows* . Par conséquent, quand INFO est spécifié, *rowNum* et *nrows* deviennent des paramètres de sortie.|  
|0x200|PREV_NOADJUST|Est utilisée comme PREV. Toutefois, si les premiers résultats du jeu sont trouvés prématurément, les résultats peuvent varier.|  
|0x400|SKIP_UPDT_CNCY|Doit être utilisé avec l’une des autres valeurs *FetchType* , à l’exception de info.|  
  
> [!NOTE]  
>  Il n'existe aucune prise en charge de la valeur 0x40.  
  
 Pour plus d'informations, consultez la section « Notes » plus loin dans cette rubrique.  
  
 *rownum*  
 Paramètre facultatif utilisé pour spécifier la position de ligne des valeurs ABSOLUEs et *FetchType* en utilisant uniquement des valeurs entières pour l’entrée ou la sortie, ou les deux. *rowNum* sert d’offset de ligne pour la valeur de bit *FetchType* relative. *rowNum* est ignoré pour toutes les autres valeurs. Pour plus d'informations, consultez la section « Notes » plus loin dans cette rubrique.  
  
 *nrows*  
 Paramètre optionnel utilisé pour spécifier le nombre de lignes à extraire. Si *nrows* n’est pas spécifié, la valeur par défaut est 20 lignes. Pour définir la position sans retourner de données, spécifiez la valeur 0. Quand *nrows* est appliqué à la requête *FetchType* info, elle retourne le nombre total de lignes de cette requête.  
  
> [!NOTE]  
>  *nrows* est ignoré par la valeur de bit Refresh *FetchType* .  
>   
>  Pour plus d'informations, consultez la section « Notes » plus loin dans cette rubrique.  
  
## <a name="return-code-values"></a>Codet de retour  
 Lorsque vous spécifiez la valeur de bit INFO, les valeurs qui peuvent être retournées sont affichées dans les tableaux suivants.  
  
> [!NOTE]  
>  :   Si aucune ligne n'est retournée, le contenu de la mémoire tampon reste inchangé.  
  
|*\<rownum>*|Paramètre à définir sur |  
|------------------|------------|  
|Si le curseur n'est pas ouvert|0|  
|Si le curseur est positionné avant le jeu de résultats|0|  
|Si le curseur est positionné après le jeu de résultats|-1|  
|Pour les curseurs KEYSET et STATIC|Numéro de ligne absolu de la position actuelle dans le jeu de résultats|  
|Pour les curseurs DYNAMIC|1|  
|Pour ABSOLUTE|-1 retourne la dernière ligne dans un jeu.<br /><br /> -2 retourne la deuxième à la dernière ligne dans un jeu, et ainsi de suite.<br /><br /> Remarque : si plusieurs lignes sont demandées pour extraction dans ce cas, les deux dernières lignes du jeu de résultats sont retournées.|  
  
|*\<nrows>*|Paramètre à définir sur |  
|-----------------|------------|  
|Si le curseur n'est pas ouvert|0|  
|Pour les curseurs KEYSET et STATIC|En général, taille du jeu de clés actuel.<br /><br /> **-m** si le curseur est en création asynchrone avec *m* lignes trouvées à ce point.|  
|Pour les curseurs DYNAMIC|-1|  
  
## <a name="remarks"></a>Notes  
  
## <a name="cursor-parameter"></a>Paramètre de curseur  
 Avant toute opération d'extraction, la position par défaut d'un curseur se situe avant la première ligne du jeu de résultats.  
  
## <a name="fetchtype-parameter"></a>Paramètre fetchtype  
 À l’exception de SKIP_UPD_CNCY, les valeurs *FetchType* s’excluent mutuellement.  
  
 Lorsque SKIP_UPDT_CNCY est spécifié, les valeurs de colonne timestamp ne sont pas écrites dans la table de jeux de clés lorsqu'une ligne est extraite ou actualisée. Si la ligne de jeu de clés est mise à jour, les valeurs des colonnes timestamp restent inchangées. Si la ligne de jeu de clés est insérée, les valeurs des colonnes timestamp sont indéfinies.  
  
 Pour les curseurs KEYSET, cela signifie que les valeurs de la table de jeux de clés sont définies pendant la dernière opération FETCH non ignorée, si celle-ci a été effectuée. Sinon, les valeurs sont définies pendant le remplissage.  
  
 Pour les curseurs DYNAMIC, cela signifie que si l'opération Skip est effectuée avec une actualisation, elle produit les mêmes résultats que KEYSET. Pour tout autre type d'extraction, la table de jeux de clés est tronquée. Cela signifie que les lignes sont insérées et que les valeurs de la ou des colonnes timestamp sont indéfinies. Par conséquent, lorsque vous exécutez sp_cursorfetch pour les curseurs DYNAMIC, évitez d'utiliser SKIP_UPDT_CNCY pour toute opération autre que REFRESH.  
  
 Si une opération d'extraction échoue parce que la position de curseur demandée se situe au-delà du jeu de résultats, la position de curseur est définie juste après la dernière ligne. Si une opération d'extraction échoue parce que la position de curseur demandée se situe avant le jeu de résultats, la position de curseur est définie avant la première ligne.  
  
## <a name="rownum-parameter"></a>Paramètre rownum  
 Lorsque vous utilisez *rowNum*, la mémoire tampon est remplie à partir de la ligne spécifiée.  
  
 La valeur *FETCHTYPE* absolue fait référence à la position de *rowNum* dans le jeu de résultats entier. Un nombre négatif avec ABSOLUTE spécifie que l'opération compte les lignes à partir de la fin du jeu de résultats.  
  
 La valeur *FETCHTYPE* relative fait référence à la position de *rowNum* par rapport à la position du curseur au début de la mémoire tampon actuelle. Un nombre négatif avec RELATIVE spécifie que le curseur retourne en arrière par rapport à sa position actuelle.  
  
## <a name="nrows-parameter"></a>Paramètre nrows  
 L’actualisation des valeurs *FetchType* et les informations ignorent ce paramètre.  
  
 Lorsque vous spécifiez une valeur *FetchType* d’abord qui a une valeur *nrow* de 0, le curseur est positionné avant le jeu de résultats qui n’a aucune ligne dans le tampon d’extraction.  
  
 Quand vous spécifiez une valeur *FetchType* de Last qui a une valeur *nrow* de 0, le curseur est positionné après le jeu de résultats qui n’a aucune ligne dans le tampon d’extraction actuel.  
  
 Pour les valeurs *FetchType* de Next, prev, Absolute, RELATIVE et PREV_NOADJUST, une valeur *nrow* de 0 n’est pas valide.  
  
## <a name="rpc-considerations"></a>Éléments RPC à prendre en considération  
 L'état de retour RPC indique si le paramètre de taille du jeu de clés est définitif ou pas, en d'autres termes si le jeu de clés ou la table temporaire est rempli de façon asynchrone.  
  
 L'une des valeurs affichées dans le tableau suivant est affectée au paramètre d'état RPC.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|La procédure a été correctement exécutée.|  
|0x0001|La procédure a échoué.|  
|0x0002|Une extraction dans une direction négative a provoqué la définition de la position du curseur au début du jeu de résultats, alors que l'extraction aurait dû se situer logiquement avant les résultats.|  
|0x10|Un curseur avance rapide a été automatiquement fermé.|  
  
 Les lignes sont retournées comme un jeu de résultats classique, à savoir format de colonne (0x2a), lignes (0xd1), suivi de l'indication de fin (0xfd). Les jetons de métadonnées sont envoyés sous le même format que celui spécifié pour sp_cursoropen, à savoir 0x81, 0xa5 et 0xa4 pour les utilisateurs SQL Server 7.0, et ainsi de suite. Les indicateurs d'état de ligne sont envoyés en tant que colonnes masquées, comme en mode BROWSE, à la fin de chaque ligne avec le rowstat du nom de la colonne et le type de données INT4. Cette colonne rowstat comporte l'une des valeurs affichées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0x0001|FETCH_SUCCEEDED|  
|0x0002|FETCH_MISSING|  
  
 Étant donné que le protocole TDS n'offre aucun moyen d'envoyer la colonne d'état de fin sans envoyer les colonnes précédentes, les données fictives sont envoyées à la place des lignes manquantes (champs Nullable avec une valeur Null, champs de longueur fixe avec une valeur 0, valeur vide, ou valeur par défaut pour cette colonne, selon le cas).  
  
 Le nombre de lignes DONE sera toujours égal à zéro. Le message DONE contient le nombre de lignes de jeu de résultats réel et des messages d'erreur ou d'information peuvent apparaître entre les messages TDS.  
  
 Pour demander que les métadonnées relatives à la liste de sélection du curseur soient retournées dans le flux TDS, affectez à l'indicateur d'entrée RPC RETURN_METADATA la valeur 1.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-prev-to-change-a-cursor-position"></a>R. Utilisation de PREV pour modifier une position de curseur  
 Imaginons qu'un curseur h2 produit un jeu de résultats avec le contenu suivant dont la position actuelle est indiquée ci-dessous :  
  
```  
row 1 contents      
row 2 contents  
row 3 contents  
row 4 contents  <-- current position  
row 5 contents   
row 6 contents  
```  
  
 Ensuite, une sp_cursorfetch PREV qui a une valeur *nrows* de 5 positionne logiquement le curseur deux lignes avant la première ligne du jeu de résultats. Dans ces cas, le curseur est ajusté pour démarrer à la première ligne et retourner le nombre de lignes demandé. Cela signifie souvent qu'il retourne des lignes qui étaient dans le tampon d'extraction PRIOR.  
  
> [!NOTE]  
>  Il s'agit précisément du cas où le paramètre d'état RPC a la valeur 2.  
  
### <a name="b-using-prev_noadjust-to-return-fewer-rows-than-prev"></a>B. Utilisation de PREV_NOADJUST pour retourner moins de lignes que PREV  
 PREV_NOADJUST n'inclut jamais aucune des lignes au niveau de la position de curseur actuelle ou après celle-ci dans le bloc des lignes qu'il retourne. Dans les cas où PREV retourne des lignes après la position actuelle, PREV_NOADJUST retourne moins de lignes que le nombre demandé dans *nrows*. En prenant la position actuelle dans l'exemple A plus haut, lorsque PREV est appliqué, sp_cursorfetch(h2, 4, 1, 5) extrait les lignes suivantes :  
  
```  
row1 contents   
row2 contents  
row3 contents  
row4 contents  
row5 contents  
```  
  
 Toutefois, lorsque PREV_NOADJUST est appliqué, sp_cursorfetch(h2, 512, 6, 5) extrait uniquement les lignes suivantes :  
  
```  
row1 contents   
row2 contents  
row3 contents   
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
