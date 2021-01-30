---
description: sys.fn_check_object_signatures (Transact-SQL)
title: sys.fn_check_object_signatures (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_check_object_signatures_TSQL
- fn_check_object_signatures_TSQL
- fn_check_object_signatures
- sys.fn_check_object_signatures
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_check_object_signatures function
ms.assetid: 47509566-d3d7-46a9-89c1-91b4895d56b9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25c4774812d900bcbc0b9979b966f7ba7a3945bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206123"
---
# <a name="sysfn_check_object_signatures-transact-sql"></a>sys.fn_check_object_signatures (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Retourne une liste de tous les objets signables et indique si un objet est signé par un certificat spécifié ou une clé asymétrique. Retourne également une valeur indiquant si la signature d'un objet est valide si l'objet est signé par le certificat spécifié ou une clé asymétrique.  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_ check_object_signatures (   
    { '@class' } , { @thumbprint }   
  )   
```  
  
## <a name="arguments"></a>Arguments  
 { '\@ *classe*'}  
 Identifie le type d'empreinte numérique fourni :  
  
-   'certificate'  
  
-   'asymmetric key'  
  
 \@la *classe* est de **type sysname**.  
  
 { \@ *empreinte numérique* }  
 Hachage SHA-1 du certificat avec lequel la clé est chiffrée ou GUID de la clé asymétrique avec laquelle la clé est chiffrée. \@l' *empreinte numérique* est **varbinary (20)**.  
  
## <a name="tables-returned"></a>Tables retournées  
 Le tableau suivant répertorie les colonnes retournées par **fn_check_object_signatures** .  
  
|Colonne|Type|Description|  
|------------|----------|-----------------|  
|type|**nvarchar(120)**|Retourne une description de type ou un assembly.|  
|entity_id|**int**|Retourne l'ID de l'objet en cours d'évaluation.|  
|is_signed|**int**|Retourne la valeur 0 lorsque l'objet n'est pas signé par l'empreinte numérique fournie. Retourne la valeur 1 lorsque l'objet est signé par l'empreinte numérique fournie.|  
|is_signature_valid|**int**|Lorsque is_signed a la valeur 1, retourne la valeur 0 lorsque la signature n'est pas valide. Retourne la valeur 1 lorsque la signature est valide.<br /><br /> Lorsque is_signed a la valeur 0, retourne toujours la valeur 0.|  
  
## <a name="remarks"></a>Notes  
 Utilisez **fn_check_object_signatures** pour confirmer que les utilisateurs malveillants n’ont pas falsifié d’objets.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DEFINITION sur le certificat ou la clé asymétrique.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant recherche le certificat de signature de schéma pour la base de données `master` et retourne la valeur 1 pour `is_signed` et la valeur 1 pour `is_signature_valid` pour les objets signés par le certificat de signature de schéma et dont les signatures sont valides.  
  
```  
USE master;  
-- Declare a variable to hold the thumbprint.  
DECLARE @thumbprint varbinary(20) ;  
-- Populate the thumbprint variable with the master database schema signing certificate.  
SELECT @thumbprint = thumbprint   
FROM sys.certificates   
WHERE name LIKE '%SchemaSigningCertificate%' ;  
-- Evaluates the objects signed by the schema signing certificate  
SELECT type, entity_id, OBJECT_NAME(entity_id) AS [object name], is_signed, is_signature_valid  
FROM sys.fn_check_object_signatures ('certificate', @thumbprint) ;  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [IS_OBJECTSIGNED &#40;Transact-SQL&#41;](../../t-sql/functions/is-objectsigned-transact-sql.md)  
  
  
