---
title: srv_paramlen (API de procédure stockée étendue) | Microsoft Docs
description: Découvrez comment srv_paramlen dans l’API de procédure stockée étendue retourne la longueur des données d’un paramètre d’appel de procédure stockée distante.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
apiname:
- srv_paramlen
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_paramlen
ms.assetid: d1fe92ff-cad6-4396-8216-125e5642e81e
author: rothja
ms.author: jroth
ms.openlocfilehash: e76d1f4a68d0c15d1f0a0b33627d18ade97669cf
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87248413"
---
# <a name="srv_paramlen-extended-stored-procedure-api"></a>srv_paramlen (API de procédure stockée étendue)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Retourne la longueur de données d'un paramètre d'appel de procédure stockée distante. Cette fonction a été remplacée par la fonction **srv_paraminfo**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int srv_paramlen (  
SRV_PROC *  
srvproc  
,  
int  
n   
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Pointeur vers la structure SRV_PROC qui correspond au handle d'une connexion cliente particulière (dans ce cas, le handle qui a reçu l'appel de procédure stockée distante). La structure contient des informations que la bibliothèque d'API de procédure stockée étendue utilise pour gérer les communications et les données entre l'application et le client.  
  
 *n*  
 Indique le numéro du paramètre. Le premier paramètre est 1.  
  
## <a name="returns"></a>Retourne  
 Longueur réelle, en octets, des données de paramètre. En l’absence de *n*ième paramètre ou de procédure stockée distante, la valeur -1 est retournée. Si le *n*ième paramètre est NULL, la valeur retournée est 0.  
  
 Cette fonction retourne les valeurs suivantes, si le paramètre est l’un des [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] types de données système suivants.  
  
|Nouveaux types de données|Longueur de données d'entrée|  
|--------------------|-----------------------|  
|**BITN**|**NULL :** 1<br /><br /> **Zéro :** 1<br /><br /> **>= 255 :** NON APPLICABLE<br /><br /> **<255 :** NON APPLICABLE|  
|**BIGVARCHAR**|**NULL :** 0<br /><br /> **Zéro :** 1<br /><br /> **>= 255 :** 255<br /><br /> **<255 :***len* réel|  
|**BIGCHAR**|**NULL :** 0<br /><br /> **ZERO :** 255<br /><br /> **>= 255 :** 255<br /><br /> **<255:** 255|  
|**BIGBINARY**|**NULL :** 0<br /><br /> **ZERO :** 255<br /><br /> **>= 255 :** 255<br /><br /> **<255:** 255|  
|**BIGVARBINARY**|**NULL :** 0<br /><br /> **Zéro :** 1<br /><br /> **>= 255 :** 255<br /><br /> **<255 :***len* réel|  
|**NCHAR**|**NULL :** 0<br /><br /> **ZERO :** 255<br /><br /> **>= 255 :** 255<br /><br /> **<255:** 255|  
|**NVARCHAR**|**NULL :** 0<br /><br /> **Zéro :** 1<br /><br /> **>= 255 :** 255<br /><br /> **<255 :***len* réel|  
|**Text**|**Null :** -1<br /><br /> **ZERO :** -1<br /><br /> **>= 255 :** -1<br /><br /> ** \< 255 :** -1|  
  
 \**len* réel = longueur de chaîne de caractères multioctets (cch)  
  
## <a name="remarks"></a>Notes  
 Chaque paramètre de procédure stockée distante possède une longueur de données réelle et une longueur de données maximale. Pour les types de données de longueur fixe standard qui n'autorisent pas les valeurs NULL, les longueurs réelle et maximale sont les mêmes. Pour les types de données de longueur variable, les longueurs peuvent varier. Par exemple, un paramètre déclaré en tant que **varchar(30)** peut posséder des données longues de 10 octets seulement. La longueur réelle du paramètre est 10 et sa longueur maximale est 30. La fonction **srv_paramlen** obtient la longueur de données réelle, en octets, d’une procédure stockée distante. Pour obtenir la longueur de données maximale d’un paramètre, utilisez **srv_parammaxlen**.  
  
 Quand un appel de procédure stockée distante est effectué avec des paramètres, ceux-ci peuvent être passés par nom ou par position (sans nom). Si l'appel de procédure stockée distante est effectué avec certains paramètres passés par nom et certains passés par position, une erreur se produit. Le gestionnaire de SRV_RPC est toujours appelé, mais il apparaît comme s’il n’y avait aucun paramètre et **srv_rpcparams** retourne 0.  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](https://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409https://msdn.microsoft.com/security/).  
  
## <a name="see-also"></a>Voir aussi  
 [srv_paraminfo &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-paraminfo-extended-stored-procedure-api.md)   
 [srv_rpcparams &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-rpcparams-extended-stored-procedure-api.md)  
  
  
