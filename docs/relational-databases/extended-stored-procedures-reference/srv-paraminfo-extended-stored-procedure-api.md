---
title: srv_paraminfo (API de procédure stockée étendue) | Microsoft Docs
description: Découvrez comment srv_paraminfo dans l’API de procédure stockée étendue retourne des informations sur un paramètre.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
apiname:
- srv_paraminfo
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_paraminfo
ms.assetid: ee2afd4e-0d91-462b-9403-98d481546330
author: rothja
ms.author: jroth
ms.openlocfilehash: 78057c092942455e41f48e0f9f12a9502c1a67ee
ms.sourcegitcommit: 75f767c7b1ead31f33a870fddab6bef52f99906b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87332336"
---
# <a name="srv_paraminfo-extended-stored-procedure-api"></a>srv_paraminfo (API de procédure stockée étendue)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Retourne des informations sur un paramètre. Cette fonction remplace les fonctions suivantes : [srv_paramtype](../../relational-databases/extended-stored-procedures-reference/srv-paramtype-extended-stored-procedure-api.md), [srv_paramlen](../../relational-databases/extended-stored-procedures-reference/srv-paramlen-extended-stored-procedure-api.md), [srv_parammaxlen](../../relational-databases/extended-stored-procedures-reference/srv-parammaxlen-extended-stored-procedure-api.md) et [srv_paramdata](../../relational-databases/extended-stored-procedures-reference/srv-paramdata-extended-stored-procedure-api.md). **srv_paraminfo** prend en charge les types de données dans [Data Types](../../relational-databases/extended-stored-procedures-reference/data-types-extended-stored-procedure-api.md) et les données de longueur nulle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int srv_paraminfo (  
SRV_PROC *  
srvproc  
,  
int  
n  
,  
BYTE *  
pbType  
,  
ULONG *  
pcbMaxLen  
,  
ULONG *  
pcbActualLen  
,  
BYTE *  
pbData  
,  
BOOL *  
pfNull  
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Handle d'une connexion cliente.  
  
 *n*  
 Nombre ordinal du paramètre à définir. Le premier paramètre est 1.  
  
 *pbType*  
 Type de données du paramètre.  
  
 *pcbMaxLen*  
 Pointeur vers la longueur maximale du paramètre.  
  
 *pcbActualLen*  
 Pointeur vers la longueur réelle du paramètre. La valeur 0 (\**pcbActualLen* == 0) indique des données de longueur nulle si **pfNull* a la valeur FALSE.  
  
 *pbData*  
 Pointeur vers la mémoire tampon des données de paramètre. Si *pbData* n’est pas NULL, l’API de procédure stockée étendue écrit \**pcbActualLen* octets de données dans \**pbData*. Si *pbData* est NULL, aucune donnée n’est écrite dans \**pbData*, mais la fonction retourne \**pbType*, \**pcbMaxLen*, \**pcbActualLen* et **pfNull*. Cette mémoire tampon doit être gérée par l'application.  
  
 *pfNull*  
 Pointeur vers un indicateur null. **pfNull* a la valeur TRUE si la valeur du paramètre est NULL.  
  
## <a name="returns"></a>Retours  
 Si les informations sur le paramètre ont été obtenues avec succès, la valeur SUCCEED est retournée ; sinon, FAIL. La valeur FAIL est retournée en l’absence de procédure stockée distante active ou en l’absence d’un *n*ième paramètre de procédure stockée.  
  
## <a name="remarks"></a>Notes  
 **Remarque relative à la sécurité** Il est recommandé de revoir en détail le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](https://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409https://msdn.microsoft.com/security/).  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence du programmeur sur les procédures stockées étendues](../../relational-databases/extended-stored-procedures-reference/database-engine-extended-stored-procedures-reference.md)  
  
  
