---
description: catalog.check_schema_version
title: catalog.check_schema_version | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e0d5e9f5-59c6-4118-87b5-4aa5c37a7df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fe36fbd5d5a49889fb7a0a02c82f7ade489f2570
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339212"
---
# <a name="catalogcheck_schema_version"></a>catalog.check_schema_version 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Détermine si le schéma de catalogue SSISDB et les binaires [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (assembly ISServerExec et SQLCLR) sont compatibles.  
  
 ISServerExec.exc enregistre un message d'erreur lorsque le schéma et les fichiers binaires sont incompatibles.  
  
 La version du schéma SSISDB est incrémentée quand le schéma change lors de l'application des correctifs logiciels et lors des mises à niveau. Il est recommandé d'exécuter cette procédure stockée après qu'une sauvegarde SSISDB a été restaurée afin de garantir que le schéma et les fichiers binaires sont compatibles.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.check_schema_version [ @use32bitruntime = ] use32bitruntime  
```  
  
## <a name="arguments"></a>Arguments  
 [ @use32bitruntime= ] *use32bitruntime*  
 Lorsque le paramètre est défini sur **1**, c’est la version 32 bits de dtexec qui est appelée. *use32bitruntime* est de type **int**.  
  
## <a name="result-set"></a>Jeu de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'autorisation suivante :  
  
-   Appartenance au rôle de base de données **ssis_admin**.  
  
  
