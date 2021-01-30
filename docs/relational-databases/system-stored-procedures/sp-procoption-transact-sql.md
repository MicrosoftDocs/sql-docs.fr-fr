---
description: sp_procoption (Transact-SQL)
title: sp_procoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_procoption
- sp_procoption_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_procoption
ms.assetid: 6f0221bd-70b4-4b04-b15d-722235aceb3c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed9d1056a751633f208a38cbb03f6a0d50057c88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205117"
---
# <a name="sp_procoption-transact-sql"></a>sp_procoption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit ou désactive l'exécution automatique d'une procédure stockée. Une procédure stockée qui a la valeur exécution automatique s’exécute chaque fois qu’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est démarrée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_procoption [ @ProcName = ] 'procedure'   
    , [ @OptionName = ] 'option'   
    , [ @OptionValue = ] 'value'   
```  
  
## <a name="arguments"></a>Arguments  
`[ @ProcName = ] 'procedure'` Nom de la procédure pour laquelle définir une option. la *procédure* est **nvarchar (776)**, sans valeur par défaut.  
  
`[ @OptionName = ] 'option'` Nom de l’option à définir. La seule valeur pour l' *option* est **Startup**.  
  
`[ @OptionValue = ] 'value'` Indique s’il faut définir l’option sur (**true** ou **on**) ou sur OFF (**false** ou **off**). la *valeur* est de type **varchar (12)**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou numéro d'erreur (échec)  
  
## <a name="remarks"></a>Notes  
 Les procédures de démarrage doivent se trouver dans la base de données **Master** et ne peuvent pas contenir de paramètres d’entrée ou de sortie. L'exécution des procédures stockées démarre lorsque toutes les bases de données sont récupérées et le message « Récupération terminée » est enregistré au démarrage.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit une procédure en vue d'une exécution automatique.  
  
```  
EXEC sp_procoption @ProcName = N'<procedure name>'   
    , @OptionName = 'startup'   
    , @OptionValue = 'on';   
```  
  
 L'exemple suivant empêche une procédure de s'exécuter automatiquement.  
  
```  
EXEC sp_procoption @ProcName = N'<procedure name>'      
    , @OptionName = 'startup'
    , @OptionValue = 'off';   
```  
  
## <a name="see-also"></a>Voir aussi  
 [Exécuter une procédure stockée](../../relational-databases/stored-procedures/execute-a-stored-procedure.md)  
  
  
