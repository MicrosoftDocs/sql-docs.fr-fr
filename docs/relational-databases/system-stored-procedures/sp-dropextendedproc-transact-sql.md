---
description: sp_dropextendedproc (Transact-SQL)
title: sp_dropextendedproc (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropextendedproc
- sp_dropextendedproc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropextendedproc
ms.assetid: dd93af2c-1b7d-4e39-af23-2d21d270a381
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bc877761f2e86ff63883c9f84ead94955044acbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209334"
---
# <a name="sp_dropextendedproc-transact-sql"></a>sp_dropextendedproc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime une procédure stockée étendue.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez l’ [intégration CLR](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) à la place.  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_dropextendedproc [ @functname = ] 'procedure'   
```  
  
## <a name="arguments"></a>Arguments  
`[ @functname = ] 'procedure'` Nom de la procédure stockée étendue à supprimer. la *procédure* est **nvarchar (517)**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 L’exécution de **sp_dropextendedproc** supprime le nom de la procédure stockée étendue définie par l’utilisateur de l’affichage catalogue [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) et supprime l’entrée de l’affichage catalogue [sys.extended_procedures](../../relational-databases/system-catalog-views/sys-extended-procedures-transact-sql.md) . Cette procédure stockée ne peut être exécutée que dans la base de données **Master** .  
  
**sp_dropextendedproc** ne supprime pas les procédures stockées étendues du système. Au lieu de cela, l’administrateur système doit refuser l’autorisation EXECUTe sur la procédure stockée étendue au rôle **public** .  
  
 **sp_dropextendedproc** ne peut pas être exécutée dans une transaction.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_dropextendedproc**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime la procédure stockées étendue `xp_hello`.  
  
> [!NOTE]  
>  Cette procédure doit déjà exister ; si ce n'est pas le cas, un message d'erreur est renvoyé.  
  
```  
USE master;  
GO  
EXEC sp_dropextendedproc 'xp_hello';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addextendedproc &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproc-transact-sql.md)   
 [sp_helpextendedproc &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpextendedproc-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
