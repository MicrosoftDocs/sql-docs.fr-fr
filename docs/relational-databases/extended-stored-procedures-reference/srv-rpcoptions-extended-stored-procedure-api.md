---
title: srv_rpcoptions (API de procédure stockée étendue) | Microsoft Docs
description: Découvrez comment srv_rpcoptions dans l’API de procédure stockée étendue retourne les options d’exécution pour la procédure stockée distante actuelle.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
apiname:
- srv_rpcoptions
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_rpcoptions
ms.assetid: dbcce5d1-d5a1-4379-9597-04e43af5923d
author: rothja
ms.author: jroth
ms.openlocfilehash: 3d4bd331b54b4bb555fcc6cdbe59155a553332eb
ms.sourcegitcommit: 75f767c7b1ead31f33a870fddab6bef52f99906b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87332468"
---
# <a name="srv_rpcoptions-extended-stored-procedure-api"></a>srv_rpcoptions (API de procédure stockée étendue)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Retourne les options d'exécution pour la procédure stockée distante actuelle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DBUSMALLINT srv_rpcoptions ( SRV_PROC *  
srvproc   
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Pointeur vers la structure SRV_PROC qui est le handle d'une connexion cliente particulière (dans ce cas, le handle qui a reçu la procédure stockée distante). La structure contient des informations que la bibliothèque d'API de procédure stockée étendue utilise pour gérer les communications et les données entre l'application et le client.  
  
## <a name="returns"></a>Retourne  
 Bitmap qui contient les indicateurs d'exécution joints dans un OR logique pour la procédure stockée distante actuelle. En l'absence de procédure stockée distante actuelle, 0 est retourné et un message est généré.  
  
## <a name="remarks"></a>Notes  
 Le tableau ci-dessous décrit chaque indicateur d'exécution.  
  
|Indicateur d'exécution|Description|  
|--------------------|-----------------|  
|SRV_NOMETADATA|Le client a demandé des résultats sans informations de métadonnées. Cet indicateur est utilisé uniquement lorsque le client communique avec une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Une application de l'API de procédure stockée étendue ne peut pas omettre les informations de métadonnées.|  
|SRV_RECOMPILE|Le client a demandé de recompiler la procédure stockée distante avant de l'exécuter. Cet indicateur peut ne pas s'appliquer à une application de l'API de procédure stockée étendue.|  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](https://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409https://msdn.microsoft.com/security/).  
  
  
