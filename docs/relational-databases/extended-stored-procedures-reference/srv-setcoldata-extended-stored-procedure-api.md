---
title: srv_setcoldata (API de procédure stockée étendue) | Microsoft Docs
description: En savoir srv_setcoldata dans l’API de procédure stockée étendue spécifie l’adresse actuelle pour les données d’une colonne.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
apiname:
- srv_setcoldata
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_setcoldata
ms.assetid: 2e19205a-25ca-4d4a-916b-d591cf2c892b
author: rothja
ms.author: jroth
ms.openlocfilehash: 3c9d151ca52e52d550d7eba42cb6e53af1d8417e
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87248216"
---
# <a name="srv_setcoldata-extended-stored-procedure-api"></a>srv_setcoldata (API de procédure stockée étendue)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Spécifie l'adresse actuelle pour les données d'une colonne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int srv_setcoldata (  
SRV_PROC *  
srvproc  
,  
int   
column  
,  
void *  
data   
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Pointeur vers la structure SRV_PROC qui est le handle pour une connexion cliente particulière. La structure contient des informations que la bibliothèque d'API de procédure stockée étendue utilise pour gérer les communications et les données entre l'application et le client.  
  
 *column*  
 Indique le numéro de la colonne pour laquelle l'adresse est spécifiée. Les colonnes sont numérotées, en commençant par 1.  
  
 *data*  
 Pointeur pour les données d'une colonne. La mémoire allouée pour *data* ne doit pas être libérée tant que les données de la colonne ne sont pas remplacées par un autre appel à **srv_setcoldata**ou tant que **srv_senddone** n'est pas appelé.  
  
## <a name="returns"></a>Retours  
 SUCCEED ou FAIL.  
  
## <a name="remarks"></a>Notes  
 Chaque colonne de la ligne doit être au préalable définie avec **srv_describe**. Les adresses des données de colonne sont définies initialement avec **srv_describe**. En cas de modification de l'adresse des données de colonne, **srv_setcoldata** doit être appelé pour spécifier la nouvelle adresse des données et **srv_setcoldata** doit être appelé séparément pour chaque colonne modifiée.  
  
 Pour représenter des données Null, attribuez la valeur 0 à la longueur de la colonne avec **srv_setcollen**. L'adresse des données est alors ignorée.  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](https://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409https://msdn.microsoft.com/security/).  
  
## <a name="see-also"></a>Voir aussi  
 [srv_describe &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-describe-extended-stored-procedure-api.md)  
  
  
