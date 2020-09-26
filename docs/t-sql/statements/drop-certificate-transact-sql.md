---
description: DROP CERTIFICATE (Transact-SQL)
title: DROP CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/18/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP CERTIFICATE
- DROP_CERTIFICATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], removing
- removing certificates
- dropping certificates
- DROP CERTIFICATE statement
- deleting certificates
ms.assetid: 5704aa04-68a3-4b29-b62b-8868af487817
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6af8fdfe363f8edbebbd80213e467ec7b35514ad
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91380164"
---
# <a name="drop-certificate-transact-sql"></a>DROP CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-asdb-asa-pdw](../../includes/applies-to-version/sql-asdb-asa-pdw.md)]

  Supprime un certificat de la base de données.  
  
> [!IMPORTANT]  
>  Une sauvegarde du certificat utilisée pour le chiffrement de la base de données doit être conservée même si le chiffrement n'est plus activé sur une base de données. Même si la base de données n'est plus chiffrée, des parties du journal des transactions peuvent toujours être protégées, et le certificat peut être nécessaire pour certaines opérations tant que la sauvegarde complète de la base de données n'a pas été effectuée. Le certificat est également nécessaire pour pouvoir restaurer les sauvegardes créées lorsque la base de données a été chiffrée.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

  
> [!Note]
> [!INCLUDE [Synapse preview note](../../includes/synapse-preview-note.md)]
  
## <a name="syntax"></a>Syntaxe  
  
```synaxsql  
DROP CERTIFICATE certificate_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *certificate_name*  
 Nom unique sous lequel le certificat est connu dans la base de données.  
  
## <a name="remarks"></a>Notes  
 Les certificats ne peuvent être supprimés que si aucune entité ne leur est associée.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL sur le certificat.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime le certificat `Shipping04` de la base de données `AdventureWorks`.  
  
```sql  
USE AdventureWorks2012;  
DROP CERTIFICATE Shipping04;  
```  
  
## <a name="examples-sspdw"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant supprime le certificat `Shipping04`.  
  
```sql
USE master;  
DROP CERTIFICATE Shipping04;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
