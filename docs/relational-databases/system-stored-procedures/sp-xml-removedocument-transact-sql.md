---
description: sp_xml_removedocument (Transact-SQL)
title: sp_xml_removedocument (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_xml_removedocument_TSQL
- sp_xml_removedocument
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xml_removedocument
ms.assetid: f9dca50a-8baf-4170-90bc-e72783ce5b73
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 20a46241bce5784c3c5148378f3517098f556086
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201777"
---
# <a name="sp_xml_removedocument-transact-sql"></a>sp_xml_removedocument (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Supprime la représentation interne du document XML spécifié par le descripteur du document puis invalide le descripteur.  
  
> [!NOTE]  
>  Un document analysé est stocké dans le cache interne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'analyseur MSXML (Msxmlsql.dll) utilise un huitième de la mémoire totale disponible pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour éviter de manquer de mémoire, exécutez **sp_xml_removedocument** pour libérer de la mémoire.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_xml_removedocument hdoc  
```  
  
## <a name="arguments"></a>Arguments  
 *hdoc*  
 Descripteur du document nouvellement créé. Un descripteur incorrect renvoie une erreur. *hdoc* est un entier.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou >0 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 Le code exemple suivant supprime la représentation interne d'un document XML. Le descripteur du document est fourni en entrée.  
  
```  
EXEC sp_xml_removedocument @hdoc;  
```  
  
## <a name="see-also"></a>Voir aussi      
 <br>[Procédures stockées système (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
 <br>[Procédures stockées XML (Transact-SQL)](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)
 <br>[sys.dm_exec_xml_handles (Transact-SQL)](../system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)
 <br>[sp_xml_preparedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-preparedocument-transact-sql.md)
 <br>[OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)
  
  
